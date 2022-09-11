# HIP-xxxx : Stateless DANE clients
```
Number:  HIP-XXXX
Title:   Stateless DANE clients
Type:    Standards Track
Status:  Draft
Authors: Hasan Adams <@buffrr>
Created: 2022-09-11
 ```

## Abstract

This document proposes a standard for a new form of light clients that use extremely minimal resources suitable for embedding in web browser applications, especially on mobile devices. Clients only store tiny 32-byte Merkle tree roots and can use them to verify TLS trustlessly for any Handshake site completely off-chain, without doing any extra network lookups compared to traditional sites and without maintaining a stateful connection to a p2p network. The client only needs a simple forwarding DNS resolver to do untrusted A/AAAA lookups. Urkel proofs, TLSA records, and DNSSEC authentication chain are sent by the server itself embedded in an x509 server certificate. Verifying certificates is an entirely offline process that requires zero network lookups. The proposed client can encrypt all DNS queries and does not leak any requests to full nodes.

To obtain secure tree roots, clients should connect to the Handshake network at most once a day to sync but this standard permits clients to stay off the Handshake network for up to a week (in extreme conditions) while still being able to verify records included in the lastest cached tree roots. Clients should ideally verify proof of work to confirm the tree roots. This document places no restrictions on how the name tree roots are obtained as long as it’s deemed secure by the client.

Note: this document is particularly about clients that use Handshake with Transport Layer Security (TLS) and DNS Based-Authentication of Named Entities (DANE) such as web browsers. The method described here is not suitable for wallet clients.

## Motivation

For Handshake to power the internet and be a viable replacement for certificate authorities, billions of devices need
to either rely on light clients or run their own full nodes. However, there’s a limited number of public full nodes.
Clients need to be connected to full nodes at all times in order to browse the internet occupying a limited number
of inbound connections. Such a model is unlikely to be sustainable. Handshake would need a huge number of public full nodes just to secure top level domains. It may seem that traditional clients use the P2P model but in reality, they use an ineffeicent client-server architecture disguised as a P2P model since clients do not contribute resources to the network and rely completly on full nodes with a reachable public ip address.

Traditional light clients do not protect users’ privacy. First, name lookups are leaked to full nodes through
`getproof` requests. Light clients generally rely on recursive resolvers that are in theory decentralized but
in practice very unreliable for end users. Recursive resolvers today do plain text DNS lookups leaking
all DNS traffic to internet service providers. On some networks, it’s not possible to do recursion
due to interference by middleboxes[0]. Recursion is incompatible with most VPNs. Plain text DNS is easily
censorable. In fact, Handshake is unintentionally censored on some networks[1]. The internet is moving
away from plain text DNS by relying on technologies such as DNSCrypt, DoH, or Oblivious DoH to greatly
improve users’ privacy and enable technologies such as Encrypted Client Hello[2] to hide all traffic from
eavesdroppers but ECH would be moot without encrypted DNS.

To have any chance of competing with existing Web PKI, Handshake must provide a comparable solution
in terms of performance and reliability. For a client to verify a site’s identity using
DNS-Based Authentication, they need to request a proof from a full node assuming a peer is
available to serve that proof and do multiple DNS lookups to get the DNSSEC authentication chain
before they’re able to visit a site. This not only introduces undesirable latency it also makes
browsing the internet very flaky since it relies on all these network lookups to succeed
just to verify a certificate[3].

## Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",
"NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described
in RFC2119 RFC8174 when, and only when, they appear in all capitals, as shown here.

## Overview

Handshake has a unique design; it’s a referral only zone meaning it only allows a subset of DNS records
on-chain such as DS, NS and TXT records. DNS data is stored in a dedicated merkle tree called Urkel
similar to a sparse merkle tree used by Certificate Transparency. A light client defined by this standard
only needs to know the last 40 tree roots. They SHOULD be retrieved from the Handshake network and can be fetched
on demand. The tree root changes every 36 blocks (approximately 6 hours). It SHOULD be updated by clients once a day
but it MUST be updated at least once a week . This document does not restrict how the tree roots are fetched or updated but they
MUST be obtained securely.


## Theory of operation

This section explains the theory of operation from a client’s perspective.

### Obtaining a tree root:

Clients MUST store the latest 40 tree roots. Clients SHOULD initially download block headers and verify proof of work. Clients SHOULD update the tree roots at least once a day and MUST update once a week. Refer to Timing Considerations for more details.

### Initiating TLS connections:

Assuming the client already has urkel roots stored, verifying any HNS certificate is an entirely offline process
that requires zero network lookups and no help from the Handshake network.

#### Looking up a name

We first begin by finding the IP address(s). Any DNS forwarding resolver MAY be used.
The resolvers are only necessary for performing A/AAAA DNS lookups. They do not need to be trusted.
It is RECOMMENDED to use a forwarding resolver that encrypts DNS queries. Clients MAY route DNS traffic over
Tor, VPN, Oblivious DoH Proxy or any other means to protect their privacy. To avoid creating a single
point of failure, clients MAY rely on several resolvers. To promote decentralization, clients MAY use a random
resolver from an aggregator. The discovery, aggregation and anonymization of resolvers may be defined by a separate standard.

#### Verifying a certificate

The server certificate MUST contain all the data needed to verify the site's identity. This includes the Urkel proof(s), TLSA record & embedded DNSSEC authentication chain see Servers section for more details. Clients MUST assume the certificate is invalid otherwise [TODO: backward compatibility?]. Refer to the Servers section for the format and extensions ids.

For a certificate to be considered valid, validators MUST find an authentication path starting from one of the stored Urkel
roots and verifying the matching Urkel proof in the certificate. The proof MUST contain a DS RRSet or a HIP-5 delegation
to be used as a trust anchor for the TLD. The trust anchor can be used to verify the DNSSEC authentication
chain in the certificate and validate the TLSA RRSet. Clients can use the validated
TLSA RRSet to verify DANE as described in RFC6698, RFC7671 and RFC9102.


## Servers

For servers to be compliant with this standard they MUST send any proofs & DNSSEC records necessary for clients to complete the TLS Handshake by relying only on the urkel root to prove the trust chain. 

### Building a certificate chain

Example x509 certificate for https://proofofconcept:

```
+------------------------------------------------------------------+
| End-entity certificate: proofofconcept.                          |
+------------------------------------------------------------------+       
| Version: 3 (0x2)                                                 |
| Serial Number: ............                                      |  
| X509v3 extensions:                                               |
|   X509v3 Subject Alternative Name:                               |
|     DNS: proofofconcept                                          |
|   X509v3 Basic Constraints:                                      |
|     CA:FALSE                                                     |
|   Urkel: [                                                       |
|     Root: 6c3078b26c3f7858d78bd2b63530d66b413f32f0ace083e8f21... |
|     Proof: <proof>                                               |  
|     Data: proofofconcept. DS,                                    | 
|     ...                                                          |
|   ]                                                              |
|   DNSSEC Authentication chain:                                   |
|     _443._tcp.proofofconcept. TLSA                               |
|     RRSIG(_443._tcp.proofofconcept. TLSA)                        |
|     proofofconcept. DNSKEY                                       |
|     RRSIG(proofofconcept. DNSKEY)                                |
+------------------------------------------------------------------+
```


A hypothetical certificate chain with a HIP-5 delegation such as the following MAY be defined in future standards:

```
+------------------------------------------------------------------+
| End-entity certificate: example.proofofconcept.                  |
+------------------------------------------------------------------+       
| Version: 3 (0x2)                                                 |
| Serial Number: ............                                      |  
| X509v3 extensions:                                               |
|   X509v3 Subject Alternative Name:                               |
|     DNS: example.proofofconcept                                  |
|   X509v3 Basic Constraints:                                      |
|     CA:FALSE                                                     |
|   Other-chain proof: [                                           |
|     Proof:<proof>                                                |
|     Data: example.proofofconcept. DS                             |
|   ]                                                              |
|   DNSSEC Chain:                                                  |
|     _443._tcp.example.proofofconcept. TLSA                       |
|     RRSIG(_443._tcp.example.proofofconcept. TLSA)                |
|     example.www. DNSKEY                                          |
|     RRSIG(example.www. DNSKEY)                                   |
+------------------------------------------------------------------+
+------------------------------------------------------------------+
| Handshake CA: proofofconcept.                                    |
+------------------------------------------------------------------+       
| Version: 3 (0x2)                                                 |
| Serial Number: ............                                      |  
| X509v3 extensions:                                               |
|   X509v3 Name constraints:                                       |
|     proofofconcept                                               |
|   X509v3 Basic Constraints:                                      |
|     CA:TRUE                                                      |
|   Urkel: [                                                       |
|     Root: 6c3078b26c3f7858d78bd2b63530d66b413f32f0ace083e8f21... |
|     Proof: <proof>                                               |  
|     Data: 0x............._delegation. NS                         | 
|   ]                                                              |
+------------------------------------------------------------------+
```


The following is the internal TBS structure of an X509 certificate (RFC 5280).

```
TBSCertificate  ::=  SEQUENCE  {
        version         [0]  EXPLICIT Version DEFAULT v1,
        serialNumber         CertificateSerialNumber,
        signature            AlgorithmIdentifier,
        issuer               Name,
        validity             Validity,
        subject              Name,
        subjectPublicKeyInfo SubjectPublicKeyInfo,
        issuerUniqueID  [1]  IMPLICIT UniqueIdentifier OPTIONAL,
                             -- If present, version MUST be v2 or v3
        subjectUniqueID [2]  IMPLICIT UniqueIdentifier OPTIONAL,
                             -- If present, version MUST be v2 or v3
	extensions      [3]  EXPLICIT Extensions OPTIONAL
                             -- If present, version MUST be v3		
```

In the extensions field, we can define custom extensions. Each extension has an OID (Object identifier, a unique id) called `extnID` and the value of the extension `extnValue` is an ASN.1 DER encoded structure. An ASN.1 OCTET STRING should be suitable for storing arbitrary data.



### 1.0 Urkel proof extension:

```
extnId: TBD
extnValue: n<urkel root><urkel proof>,...
```

The following sections describe what to include in the `extnValue` field depending on UPDATEs to the name within the last 32 urkel intervals.

### 1.1 No UPDATEs:

Certificates MUST be generated weekly and should include the 32nd most recent tree root and proof. This is the most common case.

### 1.2 NS modified:

Exactly the same as 1.1. Servers MUST NOT include the new NS records. If its a HIP-5 delegation, refer to 1.3.

### 1.3 HIP-5 NS modified:

HIP-5 delegations are special NS records. They typically combine the security properties of NS+DS. During this transition period, servers MUST include two proofs in the certificate for the current and previous NS records.

- Proof 1: The 32nd most recent urkel root/proof from the previous UPDATEs.
- Proof 2: The urkel root/proof at the time the new UPDATE was committed.

### 1.4 DS rollover:

This case requires special care to accomodate clients with older tree roots. During this transition period, servers MUST include two proofs in the certificate for the current and previous DS records. Servers MUST sign the DNS RRSets with both DS records and include it in the embedded DNSSEC chain extension.

- Proof 1: The 32nd most recent urkel root/proof from the previous UPDATEs.
- Proof 2: The urkel root/proof at the time the new UPDATE was committed.


### 2.0 DNSSEC Authentication chain

```
extnID: TBD
extnValue: MUST use the same serialization format as described in RFC9102.
```

If the urkel extension includes DS records, include the complete DNSSEC authentication chain.



## ACME clients & servers

Servers MAY use ACME services (RFC8555) for certificate issuance that embed the appropriate x509 certificate extensions described above using a standard ACME client such as certbot without having to install an HNS client.


## Timing considerations

Certificates should include tree roots/proofs that overlaps the intervals for synced and out-of-sync (1-week behind) clients. In most cases, only a single proof is required. If there were UPDATEs within last 32 urkel intervals, two proofs may be required refer to servers section for more details.


```
------------------------------ Urkel intervals --------------------------------->
                                         [              synced client           ]
        [              out-of-sync client           ][          1-week          ]
                                                .<--------- x509 cert  ----------
                                                ^
                                                Included root/proof
                                                
```

Illustrating included proofs (urkel extension 1.2/1.3 sections) with modified DS/HIP-5 delegation 
the certificate includes two proofs. The recent proof applies to synced clients.

```
------------------------------ Urkel intervals --------------------------------->
                                         [              synced client           ]
        [              out-of-sync client           ][          1-week          ]
                                                .<-------------------------------
                                                ^
                                                old root/proof
                                                               .<----------------
                                                               ^
                                                           new root/proof
                                                
```


## Censorship resistance

While still being completely trustless, the proposed client relies on forwarding resolvers. These resolvers can censor DNS queries. You may think that a natural solution is falling back to recursion if the client detects censorship, but recursive resolvers today are easily censorable, so it's unlikely to help. Since forwarding resolvers do not need to be trusted, clients may rely on resolver aggregators & anonymized DNS relays, which are more lightweight, private, and have better censorship resistance properties. Future standards may define such techniques.


## Security considerations

Clients should dowload block headers & verify proof of work as defined in the theory of operation. It may also be possible to rely on Scalable Transparent Argument of Knowledge (STARK) to generate small proofs that clients can verify. Future standards may define how these proofs are generated & shared with clients to reduce state and reliance on full nodes. The tree root is the main trust anchor for securing HTTPS so great care must be taken to retrieve it securely. This standard does not restrict how the tree roots are obtained as long as they're deemed secure by the client.


[TODO] 

## Proof of concept implementation

[TODO]

## References

[0] https://blog.mozilla.org/security/2020/11/17/measuring-middlebox-interference-with-dns-records/

[1] https://github.com/imperviousinc/fingertip/issues/19

[2] https://www.ietf.org/archive/id/draft-ietf-tls-esni-14.html

[3] https://www.imperialviolet.org/2015/01/17/notdane.html
 