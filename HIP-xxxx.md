# HIP-xxxx : Standard for TXT records

```
Number:  HIP-xxxx
Title:   Standard for TXT records
Type:    Standards
Status:  Draft
Authors: 0xStefan <https://twitter.com/0xStefan>
Created: 2022-02-22
```

## Abstract

A proposal for TXT record standards to provide on-chain data for a domain.

## Motivation

#### Domain Parking

There's currently no way to know if a domain is for sale by the owner or not.
A domain seller has no reliable way to provide that information.

#### Identity / Web3 Profile

Websites using Sign in with Handshake can use and render the TXT record data, e.g. as profile information. The domain owner is still in control of the data.

## TXT Standards Definition

For the above to work we need standards so any website can properly render the TXT records. The records should be flexible, short and easy to understand. Since it's impossible to validate the data via HSD. Therefore TXT records should be provided mostly without any prefix like "mailto:", "tel:" or "https://www.twitter.com/".

The goal is to let the app or website decide how to render the information. It also decreases the data saved on-chain.

#### Domain Parking

The parking TXT entry indicates that the domain is for sale.

```
TXT parking=[String]
```

#### Contact or Profile Information

```
TXT email=[Email]
TXT phone=[Tel]
```

```
TXT discord=[String]
TXT facebook=[String]
TXT github=[String]
TXT instagram=[String]
TXT linkedin=[String]
TXT telegram=[String]
TXT tiktok=[String]
TXT twitter=[String]
TXT weixin=[String]
TXT whatsapp=[Tel]
TXT youtube=[String]
```

#### Avatar

```
TXT avatar=[ URL | skylink | ipfsLink ]
```

#### Domain Caption

A caption is especially useful for identities or as a sales pitch. It can also be used to add more context to a domain (e.g. NFT's).

```
TXT caption=[String]
```

#### Payment / Wallet Alias

Although [HIP-0002](https://github.com/handshake-org/HIPs/blob/master/HIP-0002.md) is a much better approach, TXT entries could also be used to provide static wallet information.

```
TXT wallet=[Chain]:[Address]
```

## Valid TXT Standard Examples

#### Parking Examples

```
TXT parking
TXT parking=10000 HNS
TXT parking=USD 1000
TXT parking=USD 999.99
TXT parking=999,99 EUR
TXT parking=$199 (Old price $329)
TXT parking=Please send an offer via email
```

Every string is valid, but usually a price is provided. Data providers should be aware that prices could be devided by a dot or a comma and can have decimal points.

No matter what data is provided, as long as the parking TXT entry is added, the domain will be seen as "For Sale". It's up to the site owner or app to make sense of the data. Remember: It's impossible to validate the data via HSD.

#### Contact Information Examples

```
TXT email=hello@handshake.org
TXT phone=+1(310)3015800
```

The phone number should be prefixed with a plus sign and country code. Hyphens, dots and empty spaces are allowed.

```
TXT discord=handshake#1234
TXT facebook=handshake
TXT github=handshake
TXT instagram=handshake
TXT linkedin=handshake
TXT telegram=handshake
TXT tiktok=handshake
TXT twitter=handshake
TXT weixin=DDvRyffEabcdefGh123e
TXT whatsapp=907123987234
TXT youtube=handshake
```

For most social media profiles it's enough to provide the handle. WeChat (weixin) needs a chat ID. WhatsApp links will only work without a prefix, therefore it's best to omit them. We can always add or remove networks if needed.

#### Avatar Examples

```
TXT avatar=https://handshake.org/images/landing/logo-dark.svg
TXT avatar=sia:CABAB_1Dt0FJsxqsu_J4TodNCbCGvtFf1Uys_3EgzOlTcg
TXT avatar=ipfs:bafybeidd2gyhagleh47qeg77xqndy2qy3yzn4vkxmk775bg2t5lpuy7pcu
```

#### Domain Caption Example

```
TXT caption=The Wheel of Fortune represents the end of one cycle and the beginning of the next
```

#### Payment / Wallet Alias Examples

```
TXT wallet=hns:hs1qnkr3cwf9ldp7qpq2csuuyxf0znj0lwhmawhlal
TXT wallet=btc:12dRagNcdxK39288NjcDV4GX7rMsKCGn6B
TXT wallet=eth:0x28346f1ec065eea239152213373bb58b1c9fc93b
```

## Security Concerns

There is a slight chance of Cross Site Scripting (XSS). Therefore a site owner should always verify the provided data before adding it to the DOM.

#### Example

`<a href="javascript:alert('Hello World!');">Execute JavaScript</a>`

## References

[Linking Phone numbers in HTML](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a#linking_to_telephone_numbers)

[Bob Wallet Parking on GitHub](https://github.com/kyokan/bob-wallet/issues/183)

[Profile Information on Niami](https://niami/domain/0xstefan)
