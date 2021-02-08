# HIP-xxxx : Name data standard for hsd resolver plugins

```
Number:  HIP-xxxx
Title:   Name data standard for hsd resolver plugins
Type:    Informational
Status:  Draft
Authors: Mike Carson <https://impervious.com/>
Created: 2021-02-08
```

## Abstract

This HIP describes how to set the data for a Handshake name so that it resolves with the help of an [hsd](https://github.com/handshake-org/hsd) plugin.

## Motivation

There may be various reasons to resolve a name with the help of an hsd plugin.  One example is for decentralized subdomains on Ethereum.  An hsd plugin for this would recognize the data on a name in a way to resolve it with Ethereum.

## Name data standard

We propose setting the NS records for the name with an underscore suffix which will resolve to a specific plugin.  The prefix will be used as data for the plugin. For example:

```
{
  "records": [{
    "type": "NS",
    "ns": "0x36fc69f0983E536D1787cC83f481581f22CCA2A1._eth."
  }]
}
```

This would use the _eth hsd plugin as a resolver, using the prefix as the data / contract address for resolving the name.

## Registered plugins

data_suffix | plugin
--------------------------------|-----------------------------------
[ethereum contract address]._eth| [hsd-end-resolution](https://github.com/imperviousinc/hsd-ens-resolution)


