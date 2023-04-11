# HIP-xxxx : Declare allowed registrars for your names

```
Number:  HIP-xxxx
Title:   Declare allowed registrars for your names
Type:    Informational
Status:  Draft
Authors: Paul Anthony Webb <@NetOpWibby>
Created: 2022-12-12
```

## Abstract

This document specifies a new DNS record type that allows Handshake TLD
(top-level domain) owners to indicate which registrars are authorized to
register SLDs (second-level domains) for their TLDs.

## Motivation

Similarly to the CAA record, where one can dictate which CAs are allowed
to issue certificates for your domain, maybe there would be interest
among TLD owners to be selective or permissive about where SLDs on their
TLDs are registered from. Please note that this HIP doesn't detail
enforcement of this new resource record — that is up to the TLD owner.

Because the blockchain is public, prospective domain customers of a TLD
could look it up and discover where they can register. Unfortunately,
bad actors exist in every space and Handshake is no different. The RAA
record could also serve as peace of mind for intrepid customers. Learned
of a new registrar that claims it offers domains for every Handshake
name? It's now trivial to confirm.

## Record

Registrar Authority Authorization (RAA) is the tentative name of this
resource record. The format is fairly simple:

```
<NAME>. <CLASS> <TYPE> <DATA>
```

- **NAME**: the top-level domain name, followed by a period.
- **CLASS**: this is always "IN" (for "Internet")
- **TYPE**: this is always "RAA"
- **DATA**: this is a quoted value, delineated with semicolons if there
are zero or more than one value.

## Examples

To indicate that only a single registrar is authorized to register
domains for the "examplename" TLD ("app.beachfront" in this example),
one may use this RAA record:

```
examplename. IN RAA "app.beachfront"
```

To indicate a handful of registrars are authorized to register domains
for the "examplename" TLD, use a semicolon (";") as the separator. In
the below example, "app.beachfront," "register.encirca," and
"porkbun.com" are authorized registrars.

```
examplename. IN RAA "app.beachfront;register.encirca;porkbun.com"
```

To disallow any domain registration, set an RAA record to an empty list:

```
examplename. IN RAA ";"
```

## References

- https://en.wikipedia.org/wiki/Domain_Name_System#Resource_records
- https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization
