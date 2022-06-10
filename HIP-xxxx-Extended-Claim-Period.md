# HIP-xxxx : Extended Claim Period

```
Number:  HIP-xxxx
Title:   HIP-xxxx : Extended Claim Period
Type:    Informational
Status:  Draft
Authors: befranz <befranz@gmx.net>
Created: 2022-06-10
```

## Abstract

This HIP describes how the current 4 year claim period for reserved names on Handshake which will end in February 2024 should be extended.

The reserved names are divided into three different groups since they are handled differently:

* All current ICANN TLDs reserved on Handshake (Group 1, 3, 5, 9 - 1,516)
* All current ICANN domains eligible to claim more than 504 HNS (Group 2, 4 - 79)
* All current ICANN domains eligible to claim less than 504 HNS (Group 0 - 88,448)

The reserved name list contains 90,043 names (see References), over 4,000 are claimed by June 2022.

## Motivation

There are several reasons the current 4 year claim period and the associated claimable amount of HNS for reserved names on Handshake should be changed.

* Avoiding possible future conflicts with currently reserved ICANN TLDs
* Giving holders of legacy domains more time to claim their reserved names
* Enabling any currently active legacy domain to claim their reserved name over the extended period
* Removing financial benefits to register expired ICANN domains with reserved names on Handshake
* Motivating holders of high value legacy domains to claim their reserved names rather sooner than later

## Basic Rules

After the initial claim period ending with block 210,240 (144 blocks/day * 365 days * 4 years) the extended claim period of another 210,240 blocks will start and end with block 420,480.

The current list of reserved names remain unchanged.

## ICANN TLDs

Any current ICANN TLD reserved on Handshake will be reserved forever. During the extended claim period the claimable amount of HNS will decrease gradually over time ending with 0 HNS at the end of the extended claim period.

## ICANN domains with claimable amount greater than 504 HNS

Reserved names claimable by those domains expire after eight years. During the extended claim period the claimable amount of HNS will decrease gradually over time ending with 0 HNS at the end of the extended claim period.

## ICANN domains with claimable amount less than 504 HNS

Reserved names claimable by those domains expire after eight years. During the extended claim period only 10 HNS are claimable next to the reserved name. This makes sure that any reserved name is still claimable but the financial benefit is removed for those which were only registered to claim the associated amount of HNS.

## List of Reserved Names

In the discussions about this topic there's some consent to greatly reduce the list of reserved names for the extended claim period. But this leads to some difficult decisions which names to remove. Looking at the [Alexa Ranking](https://github.com/handshake-org/hs-names/blob/master/names/alexa.json) many popular domains - especially with ccTLDs - are not in the Top 10,000. But also for example spacex(.com) is at place 16,681. In this case I recommend to not change the list of reserved names but remove the financial benefit to claim rather useless names. Those who are interested to claim their name will also do it if just a small amount of HNS is part of the name. 10 HNS should be enough to register/move/renew the claimed name.

## References

[Reserved Names Ranking and Claimable HNS](https://github.com/befranz/HIPs/reserved-names-ranking.csv)
