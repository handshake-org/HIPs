```
  HIP: xxx
  Layer: Applications
  Title: Non-Interactive Name Atomic Swaps
  Author: Mark Tyneway <mark.tyneway@gmail.com>
  Comments-URI: TODO
  Status: Draft
  Type: Standards Track
  Created: 2020-08-05
```

# Introduction

## Abstract

This document proposed a standard way for Handshake names to be traded on a secondary market
without the need of a trusted third party to act as an escrow.

## Motivation

A healthy secondary market for Handshake names will bring more activity to the Handshake economy.
Without a solution for a decentralized secondary market, it is likely that most names will
end up being managed on a custodial platform where there is liquidity for names.

After purchasing a name directly from the protocol, the name holder must rely on an escrow
to sell the name to a counterparty. This adds friction to a secondary market developing,
as escrows must be trusted and have the incentive to extract rent from the system. Transfering
a name requires two transactions, a `TRANSFER` where the name owner commits to the witness
version and witness program that the name will be transferred to, and a `FINALIZE` where the
name holder must wait a timeout after the `TRANSFER` before finalizing. A non-interactive
scheme is desired such that the name holder can simply publish a partially signed transaction
that the counterparty can fill in without needing any rounds of communication beforehand.
This scheme can be used to emulate a decentralized secondary market, as long as the necessary
data is made available.

# Specification

## Data Structures

The data structures and concepts defined below are used throughout the rest of this document.

### Locking Script

A locking script is defined as a tuple of the bech32 version and data. An encoded locking
script is an address and is specific to a particular network because of the bech32 human
readable part. The hrp for Handshake mainnet is `hs`.

### Covenant

Handshake introduces covenants to the Bitcoin UTXO model by adding an additional covenant field
to each output. The covenant field includes a type and covenant data. A UTXO with a particular
covenant type can only be spent to another covenant type when a spend path between those two types
is defined by the consensus rules. A state machine of valid spend paths is used to enable
the on chain auction system for names. The covenant data is used to persist information
between stages of the auction and includes a nonce to prevent signatures from being reused
between auctions for the same name.

A UTXO with the covenant type `NONE` can be thought of as being just like a Bitcoin UTXO.
Covenants involved in the auction process must be linked, meaning that the input index that references a covenant must be at the same output index that updates the covenant.

The covenant is represented as a varint prefixed bytearray of varint prefixed
covenant items. The number of items and types of the items depend on the covenant type.

### Witness Program

The Witness Program as defined in [BIP 141](https://github.com/bitcoin/bips/blob/master/bip-0141.mediawiki#Witness_program) with some slight modifications. Only native witness programs are supported, blake2b160 is used to hash public keys for p2wpkh and sha-3256 is used to hash scripts for p2wsh.

### Swap Script

Handshake builds on Bitcoin Script and introduces a new Opcode called `OP_TYPE` that pushes the type
of the covenant that is being spent to on to the stack. This can be used to construct a script that
has different execution paths depending on which covenant spend path the UTXO is being spent to.
For the counterparty to be able to trust that the name holder will not back out of the swap,
the ability to cancel the swap needs to be prevented. At the protocol level, this means that
`UPDATE`, `REVOKE` and `RENEW` must not be able to be spent to. A naive implementation of the script looks like:

```
OP_TYPE
0x07 // UPDATE
OP_EQUAL
OP_IF
OP_RETURN
OP_ENDIF
OP_TYPE
0x0b // REVOKE
OP_EQUAL
OP_IF
OP_RETURN
OP_ENDIF
OP_TYPE
0x08 // RENEW
OP_EQUAL
OP_IF
OP_RETURN
OP_ENDIF
OP_TYPE
0x09 // TRANSFER
OP_EQUAL
OP_IF
<public key>
OP_CHECKSIGVERIFY
OP_ENDIF
```

This script could be improved upon and improvements ought to be considered for future versions of this HIP. Improvements include using `OP_CSV` to timeout the swap, enabling the seller to lower
the price of the name over time and making the script itself smaller.

### Signature Hash Flags

The consensus rules dictate that the `TRANSFER` covenant must be spend to the same locking script
and must commit to the locking script that the `FINALIZE` is spent to. Since the counterparty
is unknown, the `TRANSFER` output cannot be committed to by the signature. The name holder
must commit to an output to themselves for the value that they are willing to sell the name at.
This is possible with `SIGHASH_SINGLEREVERSE` as it will commit to the output at the opposite
index as the input being signed. For example, if the first input is signed with 
`SIGHASH_SINGLEREVERSE`, then only the last output will be committed to. The seller would like
the counterparty to be able to fill in an input that has enough value to create a valid transaction,
so `SIGHASH_ANYONECANPAY` must be used as a modifier. This means that only the input being signed
will be committed to. This scheme must use the signature hash flag `SINGLEREVERSE | ANYONECANPAY`.

### Swap Proof

The Swap Proof includes all of the data that must be made available for a counterparty to trust
that the name is up for sale. Note that verification of this data structure depends on verifying
that the UTXO that holds the name exists.

A Swap Proof includes:

- Name
- Swap Script
- Signature
- Value

The name could be substituted for the outpoint for the name, as long as the verifier can
easily verify that the UTXO representing the name does exist. Ease of integration into user
interfaces should be taken into account here. The Swap Script must be hashed with sha3-256
and compared against the locking script for the name locking the UTXO.

### Signature

The signature in a swap proof is created over a transaction that looks like:

```
vin:
  0: name outpoint
vout:
  0: null
  1: Output(locking script, value)
```

Where the locking script is controlled by the name holder and the value is the asking price
for the name. It must be signed with `SINGLE_REVERSE | ANYONECANPAY`. Any additional fields
on the transaction that can be altered like `nSequence` must be the default value or added
to the swap proof. This would mean that the Swap Proof would have to be encoded similarly
to [BIP 174](https://github.com/bitcoin/bips/blob/master/bip-0174.mediawiki) and is left
to future revisions of this document.

### Partially Signed Transaction

A Swap Proof holds all of the information necessary to recreate a partially signed transaction
that the signature can be verified against.

## Design

### Setup Phase

The name holder must spend their name to the Swap Script and make the Swap Proof available.
This enables the protocol to be non-interactive. There are many options for making the proof
available and is out of scope for this document.

### Trade

The counterparty has the Swap Proof and must verify it. To do so, they must check that the
name exists on chain, reconstruct the partially signed transaction and verify the signature.
The Swap Script must be verified that it matches the template and prevents the buyer from
opting out of the atomic swap.

The counterparty adds an input that fulfills the value desired by the name seller, adds a
change output such that it is not the last output and produces a signature using `SIGHASH_ALL`.
This transaction can be broadcasted to the network. After the transfer timeout elapses, the
UTXO is in an anyone can spend state. The counterparty can follow up with the `FINALIZE` transaction
to themselves and take control of the name.

### Offer Discovery

Offer discovery is out of scope for this HIP but needs to be considered in its design.
Ideally there is a standard way to make offers available. This is possible with an additional
p2p protocol like [Bitmessage](https://wiki.bitmessage.org/) or [Swarm](https://ethersphere.github.io/swarm-home/).
This design hasn't been the most successful in the past. Potentially [Sia](https://sia.tech/)
could be leveraged or a simple open source website that aggregates offers, similar to a PGP
server.