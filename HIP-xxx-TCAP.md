# HIP-xxxx : Trustless Crypto Address Provider (TCAP)

```
Number:  HIP-xxxx
Title:   HIP-xxxx : Trustless Crypto Address Provider (TCAP)
Type:    Informational
Status:  Draft
Authors: befranz <befranz@gmx.net>
         0xstefan <support@niami.io>
Created: 2022-03-30
```

## Abstract

This HIP describes a trustless and verifiable way to get a crypto address associated to a specific Handshake name (TLD).

With HIP-xxxx addresses are stored with an additional signature verifiable by wallet that it's signed by the name included.

An address is fetched via http/https, either
- `static`  from any source, in this case the address is the same with every request or
- `dynamic` from a dedicated service which provides a single **unused** address with every request


## Motivation

At first glance it's similar to HIP-2 but there's big difference in requirements the name owner needs to meet. With HIP-2 addresses or the xpub (for HIP-2 Server) has to be stored on a secure system the name owner has control of, in reality it means name owner needs to set up and run an authoritative name server and a web server/hip-2 server to make sure nobody can change the addresses associated to a name.

With this HIP name owner only needs to set a TXT record on chain. Signed addresses can be stored anywhere. Additionally this makes service provider possible who can offer a service with additional features described below.

This HIP can work as a fallback for HIP-2.

## TXT Record on Chain

Name owner sets a record to associate name to a URL which provides the address (example for name `0xstefan`):

```TXT "HIPxxxx 0 https://tcap.sendme/hip3/0xstefan/%s"```

` 0 ` VALIDKEY allows to revoke/invalidate former data sets

`%s` is replaced by the asset symbol (`HNS` for Handshake)

## Address "File"

```0xstefan 1 0 hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf <Signature>```

`0xstefan` Name the address belongs to

` 1 0 ` First character of the VALIDKEY must be `0` otherwise line is invalid no matter if signature is right

`Signature` of `0xstefan 1 0 hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf` signed by `0xstefan`

The address file consists of one or many (for dynamic solution) lines in the same format.

## Basic Wallet Feature needed

The wallet is responsible to
- query the URL set on chain
- take the first line (in case there are more)
- check the `VALIDKEY` and to verify the signature. 

In this case an app is just telling the wallet (like Bob Extension) that x (`10.5`) of asset y (`HNS`) should be sent to name z (`0xstefan`).

The possible answers the app gets from the wallet are `nok - no address found` or `ok - transaction successful` - in a format still to define. App could let wallet check if address for name exists by telling the wallet to send amount `0`.

# Additional Wallet Features

A dynamic address provider service (tbd) can send two additional parameters to the wallet added after the signature. This can be

- `Unix Timestamp` till the provided address is reserved for usage
- `URL` the wallet can call when a transaction was made, so that the address provider can **mark the address as used** and **inform the name owner** about the transaction and that it runs low of unused addresses (if a contact is available).

File creation for many addresses at once needs to be a wallet function. There could even be an upload feature to the TCAP service.
