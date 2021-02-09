# HIP-xxxx : Name data standard for alternative namespace resolution

```
Number:  HIP-xxxx
Title:   Name data standard for alternative namespace resolution
Type:    Informational
Status:  Draft
Authors: Mike Carson <https://impervious.com/>
Created: 2021-02-08
```

## Abstract

This HIP describes how to set the data for a Handshake name so that it resolves via an alternative namespace, for example via a sidechain.

## Motivation

There may be various reasons to resolve a name on an alternative namespace other than a domain name or an ip address.  One example is for decentralized subdomains on Ethereum.  Software could recognize the data on a name in a way to resolve it with Ethereum.

## Name data standard

We propose setting the NS records for the name with an underscore suffix which will resolve to an alternative protocol.  The prefix will be used as data for the protocol. For example:

```
{
  "records": [{
    "type": "NS",
    "ns": "0x36fc69f0983E536D1787cC83f481581f22CCA2A1._eth."
  }]
}
```

This would use a specific Ethereum contract address (0x36fc69f0983E536D1787cC83f481581f22CCA2A1) to resolve subdomains under the Handshake top-level domain.  In this example, the contract is an ENS fork.  Software would query a specific subdomain in the ENS contract, get the DNS data stored on the subdomain, and use this to resolve the subdomain.

## Registered protocols

data_suffix | plugin
--------------------------------|-----------------------------------
[ethereum contract address]._eth| Ethereum contract address


