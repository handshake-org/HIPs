# HIP-xxxx : Trustless Crypto Address Provider (TCAP)

```
Number:  HIP-xxxx
Title:   HIP-xxxx : Trustless Crypto Address Provider (TCAP)
Type:    Informational
Status:  Draft
Authors: befranz <befranz@gmx.net>
         0xstefan <0xstefan@niami.io>
Created: 2022-03-30
```

## Abstract

This HIP describes a trustless and verifiable way to get a crypto address associated to a specific Handshake name (TLD).

With HIP-xxxx addresses are stored with an additional signature verifiable by wallet that it's signed by the name included.

An address is fetched via HTTPS, either
- `static`    from any source, in this case the address is the same with every request or
- `selective` from a dedicated service which provides a single selective address with every request

## Motivation

At first glance it's similar to HIP-2 but there's big difference in requirements the name owner needs to meet. With HIP-2 it's highly recommended to store addresses or the xpub (for HIP-2 Server) on a secure system the name owner has control of, in practice it means name owner needs to set up and run an authoritative name server and a web server/hip-2 server to make sure nobody can change the addresses associated to a name.

With this HIP name owner only needs to set a TXT record on chain. Signed addresses can be stored anywhere. Additionally this makes service provider possible who can offer a service with additional features described below.

This HIP can work as a fallback for HIP-2.

## TXT Record on Chain

Name owner sets a record to associate name to a URL which provides the address (example for name `0xstefan`):

```TXT "HIPxxxx https://tcap.sendme/hipxxxx/0xstefan/%s"```

`%s` is replaced by the asset symbol (`HNS` for Handshake)

In case a nameserver (NS) is configured for the name, this on-chain TXT record won't be available via DNS. If the name owner controls the nameserver it's preferred to set this TXT record in the nameserver.

## Address Data Set

An address data set consists either of one line for the `static` solution or of many lines (data set batch) for `selective` solution. All lines use the same syntax.

```0xstefan hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf <Signature>```

What does it mean?

`0xstefan` signing `0xstefan hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf` created `<Signature>` as result.

## Basic Wallet Features needed

The wallet is responsible to
- query the URL set on chain as TXT record (via DNS, optionally via node REST API)
- process the response (only the first line in case there are more)
- verify the signature

An app just needs to tell the wallet (like Bob Extension) that x (`10.5`) of asset y (`HNS`) should be sent to name z (`0xstefan`). 

We assume that HIP-xxxx is implemented in wallets which also support HIP-2. Since this HIP can work as a fallback for HIP-2 the communication between wallet and app is exactly the same. 

A `selective` address provider service can send additional parameters to the wallet. They are added to the address data set (after the signature). This can be

- `Unix Timestamp` the time till the provided address is reserved for usage
- `URL` the wallet can call when a transaction was made, so that the address provider can mark the address as used

Creating a data set batch (many addresses at once) needs to be a wallet function. There could even be a feature to upload this batch to the TCAP service. Just to remind, such a batch of new addresses doesn't need to come from the same wallet as the name. In some cases it's probably preferrable to create/use a different wallet for such use case.

## Selective Address Provider

### Basic Functions

A `selective` address provider service needs to offer following basic functions:
- Storing (adding) uploaded data set (via GUI and/or API) - already existing still unused addresses remain active (as default)
- Providing a single line data set per request via https link - as defined on chain
- Marking the provided address as `reserved` until `Unix Timestamp`
- Sending `Timestamp` and `URL` (with the data line) to the wallet which can call it to report the address was used
- API endpoint to receive the "address was used" info

For service safety and additional features an authentication method seems necessary. This can be done via signing a string issued by the service provider with the specific Handshake name.

### Optional Features
If the name owner provides a contact like email, the service can inform
- about the transaction
- that service runs low of unused addresses
