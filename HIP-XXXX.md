# HIP-XXXX : The Meta Account Key

```
Number:  HIP-XXXX
Title:   The Meta Account Key
Type:    Standards
Status:  Draft
Authors: Matthew Zipkin <pinheadmz@gmail.com>
Created: 2022-03-25
Requires: HIP-0008
```

# Abstract

There are some wallet operations that do not require addresses for receiving HNS
on chain. However, BIP32 recoverability is still desired for metadata that may
be involved in these operations. For these operations, we define a constant
account index of `0xffffffff` as "The Meta Account". Derivation paths from this
account key should be specified in this document to track future standard uses
for The Meta Account Key.

# Motivation

## Hardware Wallets

Hardware wallets like Ledger Nano S and Nano X support multiple coin types.
The Ledger wallet in particular has "apps" for each coin and the BIP32 paths
are partially [locked](https://github.com/LedgerHQ/app-handshake/commit/19d63b7ddc530ab96010afa5280f621b2e16e045). This means that wallet software
can only request data from the hardware in the `account`, `branch`, and `index`
ranges.

## Account Key Leakage

One example of recoverable wallet metadata usage is in the `generateNonce()`
function used by `hsd` when creating `BID` outputs. In `hsd` versions up to and
including `3.0.1` (as of this writing) the wallet incorporates a pseudo-random
factor to the blind that is partially derived from the current spending
account's xpub (Extended Public Key).

This means that if a user's `xpub` is leaked or given to a third party for any
reason (e.g. an accountant tracking wallet activity or other watch-only wallet
service) that party can "guess" the value of that user's blinded bids and will
be able to verify if they guessed correctly by regenerating the nonce used in
the BID. Since the lockup defines the upper limit of possible bid values, the
brute-force space is actually quite limited and easily accessible.

For example, if an attacker knows a user's `xpub` and they see that user has
placed a bid with a lockup of 10 HNS, they can use that `xpub` and the auction
nameHash to generate nonces for all values 0 - 10,000,000 until one nonce
matches the blind. The attacker then knows the secret bid value.

For this reason, The Meta Account Key is proposed as a REQUIRED feature of
[HIP-0008](https://hsd-dev.org/HIPs/proposals/0008/)

# Features

The meta-account key has the following properties:

- It can be restored from the wallet seed.

- It can remain private forever, there is never any need to share it.

- Users can share their `xpub` for accounting reasons but still keep their bid values secret.

- Watch-only wallets like those that construct transactions for hardware-singing
devices can request the `metaxpub` from the device and store it in a database. No
private key or password will ever be needed.

- Private keys from the `metaxpub` path are never used, so decrypting the master
private key is only needed to derive it once. Wallets that manage private keys
can choose not to store the `metaxpub` and derive it from the master private key
on the fly when users enter their password.

# Specification

1. Private keys from this account should NEVER be used

2. Child branches of The Meta Account Key therefore must always be non-hardened

Example:

```
const meta = wallet.master.key.deriveAccount(
  44,           // Purpose (BIP-44)
  5353,         // Coin Type (BIP-44)
  0xffffffff    // Meta Account
);

// Standardized purpose for HIP-8
const HIP8_master = meta.derive(0x00000008);

// Derive local spending wallet account for HIP-8
const HIP8_account = HIP8_master.derive(account_index);

// Continue using HIP-8, incorporating nameHash and auction height
const publicKey = metaxpub.derive(0x00000000)
                          .derive(0x11111111)
                          .derive(0x22222222)
                          .derive(0x33333333)
                          .derive(0x44444444)
                          .derive(0x55555555)
                          .derive(0x66666666)
                          .derive(0x77777777)
                          .derive(0x00002710); // height

const hash = Blake2b(bidAddress.hash || publicKey);

// Used to XOR secret bid amount into nulldata output
const key = hash.slice(0, 8);
```

# Implementation

https://github.com/pinheadmz/hsd/commit/6f3122108bc7a9561dced9e26cf15702f7e48f6f


