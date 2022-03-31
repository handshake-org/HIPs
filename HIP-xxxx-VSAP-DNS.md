# HIP-xxxx : Verifiable (Selective) Address Provider (VAP/VSAP) via DNSSEC

```
Number:  HIP-xxxx
Title:   HIP-xxxx : Verifiable (Selective) Address Provider (VAP/VSAP) via DNSSEC
Type:    Informational
Status:  Draft
Authors: befranz <befranz@gmx.net>
         0xstefan <0xstefan@niami.io>
Created: 2022-03-31
```

## Abstract

This HIP describes a verifiable way to get an address for an asset like HNS, BTC or USD which the owner of a Handshake domain has signed to use for payments.

Payment systems as trusted parts (wallets or fiat systems) using this feature are able to verify that the address provided is signed correctly. Users of such payment systems can be sure the right address is used for the specific domain name (ICANN or Handshake).

An address is fetched via HTTPS, either
- `static (VAP)`     from any source which always provides the same address (applicable for ETH or USD)
- `selective (VSAP)` from a dedicated service which provides a single selective address with every request (for BTC, HNS)

## Motivation

Currently there's no universal truely secure and verifiable way to translate domain names to addresses (both crypto and fiat) to send payments via domain name.

## TXT Records in DNS secured via DNSSEC

The owner of a domain (ICANN or Handshake) sets two TXT records. The first record defines the URL where the address is available:

```TXT "HIPxxxxURL=<URL>"```

The second TXT record contains the public key to verify the signed data delivered from the `<URL>` in the first TXT record.

```TXT "HIPxxxxKey=<Public Key>"```

Example:

TXT records for the domain `stefan.niami`, public key of Handshake name `0xstefan` which will be used to sign the address(es).

```
TXT "HIPxxxxURL=https://vsap.sendme/hipxxxx/stefan.niami.io/"
TXT "HIPxxxxKEY=7583ee39e6c66f346dec4dd4b12fac628c2cb74ee7458f895f30e1aeff96c34f"
```

The asset symbol (SLIP-0044 for crypto, ISO 4217 for fiat) is added to the `<HIPxxxxURL>`.

## Address Data Set

The data set is created by a Handshake wallet which supports Handshake Signature scheme to be able to sign data with a Handshake name (TLD). This Handshake name doesn't need to be the domain name the data set is created for - like the example above where Handshake name `0xstefan` is used to sign addresses for `stefan.niami`.

An address data set consists either of one line for the `static` solution (VAP) or of many lines (data set batch) for `selective` solution (VSAP). All lines use the same syntax.

```<Address> <Signature>```

Example (is signed by `0xstefan`):

```hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf WNIuvS...xPXA==```

`0xstefan` signing `hs1qlat47aa9m2k7pg2k0yylmqgy9h0me0jyf9rgwf` created `WNIuvS...xPXA==` as result.

## Basic Wallet Features which supports this HIP

The wallet is responsible to
- query the URL fetched via DNS
- process the response (only the first line in case there are more)
- verify the signature based on the Handshake Signature scheme and the public key provided via DNS

An app just needs to tell the payment system (like Bob Extension) that x (`10.5`) of asset y (`HNS`) should be sent to name z (`stefan.niami`). 

A `selective` address provider service (VSAP) can send additional parameters to the wallet. They are added to the data set (after the signature). This can be

- `Unix Timestamp` the time till the provided address is reserved for usage
- `URL` the wallet can call when a transaction was made, so that the address provider can mark the address as used

Creating a data set batch (many addresses at once) needs to be a Handshake wallet function. There could even be a feature to upload this batch to the VSAP service.

The Handshake wallet needs to offer a way to sign a single or a batch of addresses of  assets other than HNS, like BTC, ETH or USD.

## Verifiable Selective Address Provider (VSAP)

### Basic Functions

A `selective` address provider service needs to offer following basic functions:
- Storing (adding) uploaded data set (via GUI and/or API) - already existing still unused addresses remain active (as default). Data sets are preferrably already verified be storing.
- Providing a single line data set per request via https link - as defined in the DNS TXT record
- Marking the provided address as `reserved` until `Unix Timestamp`
- Sending `Timestamp` and `URL` (with the data line) to the wallet which can call it to report the address was used
- API endpoint to receive the "address was used" info

For service safety and additional features an authentication method seems necessary. This can be done via signing a string issued by the service provider which then is verified with the public key also available via DNS. To prevent service abuse and mitigate attacks the amount of API calls should be limited per source IP and time unit.

### Optional Features
If the name owner provides a contact like email, the service can inform
- about the transaction
- that service runs low of unused addresses

## References

[SLIP-0044](https://github.com/satoshilabs/slips/blob/master/slip-0044.md) can be used as a symbol reference.
[ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)
