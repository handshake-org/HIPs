# HIP-xxxx : TXT Record Naming Standard for Domain Listings

```
Number:  HIP-xxxx
Title:   TXT Record Naming Standard for Domain Listings
Type:    Standards
Status:  Draft
Authors: 0xStefan <https://twitter.com/0xStefan>
Created: 2022-02-22
```

## Abstract

A proposal to create a standard for the labels of string attributes in TXT records for domain listings — following the [RFC 1464](https://datatracker.ietf.org/doc/html/rfc1464) standard.

## Motivation

If a domain is not listet on an open marketplace like Namebase or Shakedex, it's hard to know if a domain is for sale by the owner or not. Furthermore a domain seller has no reliable way to provide that information.

## TXT Record Standard: Domain Listing

For the above to work we need standards so any website or app can properly render the TXT records. The records should be flexible, short and easy to understand. It is important to note, that it's impossible to validate the data via HSD. Therefore TXT records must be validated by the site or app that is rendering the TXT records.

The general syntax for domain listings is:

**TXT** `listing <attribute name>=<attribute value>`

There are currently two supported attributes: **price** and **url**.

### (1) Listing Attribute: Price (mandatory)

The listing price attribute is mandatory to create a domain listing. It enables a domain owner to provide a listing price for the domain. The expected value is the unit as a string, followed by the amount as a number. If the number contains decimals, a dot must be used as a seperator.

```
TXT listing price=<unit number>
```

#### Valid Listing Price Examples

```
TXT listing price=HNS 1000
TXT listing price=USD 50
TXT listing price=EUR 999.99
TXT listing price=BTC 0.000224
```

The value can also be 0 or null:

```
TXT listing price=0
TXT listing price=
```

Which means the domain is for sale and offers are accepted.

### (2) Listing Attribute: URL (optional)

The listing url attribute is optional. It enables a domain owner to provide a url, where visitors can find more information about the domain. It can also be used to point visitors to a parking website or a private marketplace. The expected value is a fully qualified URL that must start with http:// or https://.

```
TXT listing url=<URL>
```

#### Valid Listing Url Examples

```
TXT listing url=https://www.learnmore.com/about/my/domain
TXT listing url=https://learnmoreaboutmydomain/
```

## Security Concerns

There is a slight chance of Cross Site Scripting (XSS). Therefore a site owner should always verify the provided URL data before adding it to the DOM.

### Example

`<a href="javascript:alert('Hello World!');">Execute JavaScript</a>`

## References

[RFC 1464 Standards](<[https://niami/domain/0xstefan](https://datatracker.ietf.org/doc/html/rfc1464)>)