# HIP-0000 : HIP Template

```
Number:  HIP-XXXX
Title:   Adding A AAAA Records on HNS Blockchain
Type:    Informational
Status:  Draft
Authors: Skyinclude <http://skyinclude/>
Created: 2020-02-28
```

## Abstract

4 years have passed since Mainnet and the idea of HNS blockchain being a “referral only” one seems not as practical.

The [issue #125](https://github.com/handshake-org/hsd/issues/125) discussed then seemed to convince the creators not to have A, AAAA records on-chain and to only have it point to NS (name servers) which would allow then to have A records, etc.

Many in the community have asked to revisit this issue, and allow A, AAAA records to be added on-chain.

## Motivation

As we approach our fourth HandyCon this March 13-15, 2024, as always, get the pleasure to connect and network with various movers and shakers in HNS.

While in a conversation with Andrew Lee, the King of Joseon, who will be presenting at HandyCon this year (his first appearance!), he mentioned what he felt was the biggest “mistake” that Handshake protocol has made:

Not allowing A , AAAA records to be set on the TLD at the protocol level (on-chain).

Meaning, the only way to set a IP address to the TLD domain on Handshake, one has to point to / refer to a third party NS (Name server). But by allowing people to set their own A AAAA record IP address on-chain it would allow for more domains to be quickly setup and used - without needing to setup your own name server or trust someone else’s service.

## What is Rerecord? HIP018

(Also on [https://rerecord.skyinclude.com](https://rerecord.skyinclude.com) and soon on [https://rerecord](https://rerecord))

TLDR - it allows users to point to an IP address (host), on the HNS blockchain, without needing to also setup / use a name server service, at the bare TLD level.

![adding-a-aaaa-records-on-chain](https://skyinclude.com/adding-a-aaaa-records-on-chain.jpg)

**Example 1:** So you can instead of relying on Namebase dLinks + Sia Skynet dLinks integration (which failed), you can point to an IP address on your bare TLD.

**Example 2:** Instead of pointing your NS to Varo Domains with an email and password and trusting the server at Varo which hosts your domain records, you can point to an IP address directly from HNS blockchain for your TLD.

**Cons:** Slower and not as scalable - the more who would use this A record, the slower it would take to update on the HNS blockchain (from minutes to hours). Using a NS (name server) would be a 1 time set up and then can immediately update your A record, CNAME, etc at that name server solution.

**Pros:** this simply gives more choice to the TLD owner, as well as more utility to the chain.

**Proof of Concept:** One could start with an A record on the bare TLD, get traction with people interested in one’s TLD, and then launch a SLD solution with a Name Server solution later.

## Question: TSLA / HTTPS / Security

**Sajan asked:** Security - use https?

**Matt Zipkin:** Good point I guess the feature would also need to include TLSA but DANE requires a subdomain \_443.\_tcp.domain

So this already getting more complicated.

### Solution from Zipkin on TSLA

Record types are 1 byte, and only values 0-6 are defined. You could define a hundred more types if you wanted. by default all the records are named by the hns tld, but you could extend that maybe by setting the top bit in the record type to indicate it will be labeled with a subdomain. For TLSA you could assume the default is \_443.\_tcp. The goal is to conserve bytes.

## Feedback From Matt Zipkin

This would be very simple to implement. Simply define a resource version 1 (current is 0) which behaves exactly the same as version 0, but includes A and AAAA.

This is not a fork of any kind. Remember, the update covenant allows ANY DATA AT ALL without any consensus rules applied whatsoever as long as its <= 512 bytes.

I agree with JJs original motivation to be a referral only root zone but 4 years in, i think we are off track of the founders vision.

Adding this feature would boost usage, miner fees, and probably hns price as well.

Ase case is very obvious and adds functionality to the token.

## From Buffr

**Question from Matt Zipkin to Buffr:** You just need clients to understand what kind of data you are stuffing in there. Only question might be, will legacy dns resolvers choke if asked to query a TLD for an A

**Buffr Answer:** Sure some old buggy resolvers could choke on it but most mainstream resolvers should handle it fine as it's still correct DNS and you need an HNS resolver anyway so i'd say go for it. But it'd need more than just bumping the version number since hsd needs to correctly serve this data as part of the root zone with AA set ... etc.

## From Rithvik

It's true that would make it easier, I also really wanted that when I found Handshake.

There are reasons to not include this:

- Keeping the chain as a root, not a full db of all records of all zones
- Reduce bloat - A/AAAA change more frequently and we'll likely see increase in UPDATES to change these records, adding to chain bloat and making everyone's sync times slower

Also it's not just adding A/AAAA. For HTTPS, we'd also have to add TLSA. Then CNAMEs. Then someone asks for mail so MX.

## From Nathan.woodburn

Hmm. I understand having onchain records for more security. But I don't think it should be the default as it slows down DNS updates from a few minutes to 2-8 hours

**Zipkin reply to Nathan:** The tradeoff is for TLD owners who want a website on the bare TLD. They can do the slow expensive thing and update on chain records or they can do the fast scalable thing and use an NS

## From Sajan

My only concern is if adding on chain records will slow queries to SLDs in general ......if it does not ...I am all for adding utility to the token and onchain functionality

**Matt Zipkin reply:** I don't see how it could affect SLDs at all

It will definitely increase transaction activity, block size and resource requirements for full node operators. But my opinion all those things are way below capacity right now already.

These are many of the current core developers and maintainers of the HNS blockchain, and believe this enhancement would:

1. Create more immediate utility to the HNS blockchain to allow users to directly resolve and build on the TLD at the blockchain level.
2. Create more demand for the HNS coin and utilize more of the unused block space
3. While not as efficient as pointing to a name server, give quick and more costly utility to bare TLDs that want to host websites and content, in a more immutable way.

## Next Steps From Zipkin

Pull request to the HIP repo.

HSD doesnt officially support root zone export yet I don't think and decoding these extra records would be trivial.

More importantly what you need is code changes to hsd and hnsd, and then fingertip, and then bob wallet …

You'll need to have techincal specs ready.

Another youre gonna have is backwards compatability -- resource version 1 would be completely ignored by old software meaning upgraded TLDs wont resolve AT ALL

So we'd have to check how its handled by current software. you might be able to stuff A AAAA TLSA etc into resource version 0 -- but then old software will refuse to decode those records. I dont know if that makes the entire zone invalid to old software or if they just "ignore" dns records they don't understand.

[https://github.com/handshake-org/hsd/blob/2bb8f0b0db76247aebf0cb961236db5fde8d80bc/lib/dns/resource.js#L122-L124](https://github.com/handshake-org/hsd/blob/2bb8f0b0db76247aebf0cb961236db5fde8d80bc/lib/dns/resource.js#L122-L124)

Ok this implies that the best way forward would be to KEEP resource version 0

Just make sure your NS and TXT records are FIRST in your tld zone so old software can still decode those.

Then once an a record that is not NS, DS, or TXT, old software will stop decoding

## References

[https://rerecord](https://rerecord)

[https://rerecord.skyinclude.com](https://rerecord.skyinclude.com)

[https://github.com/handshake-org/hsd/blob/2bb8f0b0db76247aebf0cb961236db5fde8d80bc/lib/dns/resource.js#L122-L124](https://github.com/handshake-org/hsd/blob/2bb8f0b0db76247aebf0cb961236db5fde8d80bc/lib/dns/resource.js#L122-L124)
