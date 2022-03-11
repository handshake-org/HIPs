# HIP-xxx : TXT Record Naming Standard for Profile Data

```
Number:  HIP-xxx
Title:   TXT Record Naming Standard for Profile Data
Type:    Standards
Status:  Draft
Authors: 0xStefan <https://twitter.com/0xStefan>
Created: 2022-02-22
```

## Abstract

A proposal to create a standard for the labels of string attributes in TXT records for profile data — following the [RFC 1464](https://datatracker.ietf.org/doc/html/rfc1464) standard.

## Motivation

Handshake domains can also be used as usernames. Domains are used to sign in to websites and to identify users in the Handshake ecosystem. Therefore it's useful to be able to attach profile information like an email, phone, avatar, social profiles or wallet information to a Domain.

This Standard can also be used to provide contact information for domain listings according to [HIP-XXXX](https://github.com/handshake-org/HIPs/pull/46/files/18bed187abbbcf2846f4fea78df1f7f8f1348608?short_path=1d2937e#diff-1d2937eacaeeb0fe53fcad8e70f406714fb6359c5fef0f47b1aef97e1cdfcbed).

## TXT Record Standard: Profile Data

For the above to work we need standards so any website or app can properly render the TXT records. The records should be flexible, short and easy to understand. It is important to note, that it's impossible to validate the data via HSD. Therefore TXT records must be validated by the site owner or app that is rendering the TXT records.

The general syntax for profile data is:

**TXT** `profile <attribute name>=<attribute value>`

There are currently five supported attributes: **[email](#email)**, **[phone](#phone)**, **[avatar](#avatar)**, **[social](#social)** and **[wallet](#wallet)**. All attributes are optional but at least one attribute needs to be present, for a Domain to count as a profile.

### <a id="email"></a> (1) Profile Attribute: Email

The email attribute is optional. It enables a domain owner to provide an email for the domain. The expected value is an email address.

```
TXT profile email=<email-address>
```

#### Valid Email Examples

```
TXT profile email=hello@niami.io
TXT profile email=hello@niami
```

### <a id="phone"></a> (2) Profile Attribute: Phone

The phone attribute is optional. It enables a domain owner to provide a phone number for the domain. The expected value is a phone number.

Ideally the phone number is provided with a plus icon (+) and the country code.

```
TXT profile phone=<phone-number>
```

#### Valid Phone Examples

```
TXT profile phone=+1(536)8886253
TXT profile phone=+15368886253
TXT profile phone=+1536-888-6253
TXT profile phone=1536-888-6253
```

### <a id="avatar"></a> (3) Profile Attribute: Avatar

The avatar attribute is optional. It enables a domain owner to provide an avatar for the domain. The expected value is a fully qualified URL that must start with http:// or https:// that points to an image file.

Allowed image formats are: APNG, AVIF, GIF, JPEG, PNG, SVG, WebP according to [Common image file types](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types).

```
TXT profile avatar=<URL>
```

#### Valid Avatar Examples

```
TXT profile avatar=https://mywebsite.com/avatar.png
TXT profile avatar=https://siasky.net/MABn4I5M6yPT0aCjIMU2OjlTWmKysaqoCeP0gSJw5moVrg
```

### <a id="social"></a> (4) Profile Attribute: Social

The social attribute is optional. It enables a domain owner to provide social media usernames for the domain. The expected value is the name of the social platform followed by a username.

The name of the social platform can be gathered by taking the hostname (e.g. www.twitter.com), then removing the www. and the TLD (.com). If the social platform is hosted on a handshake Domain, the handshake Domain is used as the name for the social platform.

Site owners and apps using that data can decide which social platforms they want to support and how to display them.

```
TXT profile social=<social-media-name username>
```

#### Valid Social Username Examples

```
TXT profile social=twitter 0xstefan
TXT profile social=github 0xstefan
TXT profile social=discord 0xstefan#4697
TXT profile social=hnschat 0xstefan
```

### <a id="wallet"></a> (5) Profile Attribute: Wallet

The wallet attribute is optional. It enables a domain owner to provide one or more wallet addresses for the domain. The expected value is the token followed by a wallet address.

```
TXT profile wallet=<token wallet-address>
```

#### Valid Wallet Examples

```
TXT profile wallet=HNS hs1qnkr3cwf9ldp7qpq2csuuyxf0znj0lwhmawhlal
TXT profile wallet=BTC 12dRagNcdxK39288NjcDV4GX7rMsKCGn6B
TXT profile wallet=ETH 0x28346f1ec065eea239152213373bb58b1c9fc93b
```

If more than one wallet with the same token is provided, site owners or apps can use a random one to provide their services.

## Security Concerns

There is a slight chance of Cross Site Scripting (XSS) when using URL's in attributes. Therefore a site owner should always verify the provided URL data before adding it to the DOM.

### Example

`<a href="javascript:alert('Hello World!');">Execute JavaScript</a>`

## References

[RFC 1464 Standards](https://datatracker.ietf.org/doc/html/rfc1464)

[Common image file types by mozilla](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types)
