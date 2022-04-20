# HIP-xxxx : Verifiable Address Provider (VAP)

```
Number:  HIP-xxxx
Title:   HIP-xxxx : Verifiable Address Provider (VAP)
Type:    Informational
Status:  Draft
Authors: befranz <befranz@gmx.net>
         0xstefan <0xstefan@niami.io>
Created: 2022-04-20
```

## Abstract

This HIP describes a verifiable way how a Handshake name can be used as alias for a crypto receiving address. Following wording and examples refer to this use case but basically this protocol can be used to provide any key/value pair in a verifiable fashion.

Like [HIP-2](https://github.com/handshake-org/HIPs/blob/master/HIP-0002.md) an address is provided as single line, but with an additional signature created by the [signmessagewithname](https://hsd-dev.org/api-docs/#signmessagewithname) Handshake function.

Value and signature is fetched via URL defined on chain as TXT record.

## Motivation

At first glance it's similar to HIP-2 but there's big difference in requirements the name owner needs to meet. With HIP-2 it's highly recommended to store data on a secure system the name owner has control of, in practice it means name owner needs to set up and run an authoritative name server and a web server/HIP-2 server to make sure nobody can change the addresses associated to a name.

With this HIP name owner only needs to set a TXT record on chain. Signed addresses can be stored anywhere. Service provider like niami can facilitate the steps of signing, storing and retrieving data.

This HIP can work as a fallback for HIP-2.

## TXT Record on Chain

Name owner sets a URL as base for all her addresses:

```TXT "VAPURL=<URL>"```

Example:

```TXT "VAPURL=https://api.niami.io/vap/0xstefan/"```

The wallet will add the asset symbol (like `hns` for Handshake) to the URL to get associated address and signature.

## Message to sign/verify

Data is signed by Handshake function [signmessagewithname](https://hsd-dev.org/api-docs/#signmessagewithname), URL and address are separated by white space.

```
<VAPURL><Asset> <Address>
https://api.niami.io/vap/0xstefan/hns hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf
```

## Data to store online

Following data is stored online at `<VAPURL><Asset>`

```
<Address> <Signature>
hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf8rgwf A+D6OaTysDA90PxDLXaTx4jV/+hPkpWzhZqDbCVq9IE4B7lc/50X+jB/H9kTBYWkyGf1Bb862vS/0aQGF9dOmg==
```

## References

[SLIP-0044](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) can be used as a symbol reference.
