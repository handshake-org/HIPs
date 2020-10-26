# HIP-xxxx : Authentication using Handshake name

```
Number:  HIP-xxxx
Title:   Authentication using Handshake name
Type:    Informational
Status:  Draft
Authors: Fernando Falci <http://iamfernando/>
Created: 2020-10-25
```

## Abstract

This HIP describe how to authenticate a user using his/her Handshake name.

## Motivation

Many websites need to identify the user and proof he/she owns a Handshake name. Many others websites simply need to identify users, which force them to go intoa a proccess to pick an unique username that most of the time can't be consistent across different apps. By using Handshake names as username it allows both: proof ownership of name and consistent username accross unrelated apps.

## Protocol

The basics of this protocol consists in a public and private ECDSA key. The domain exposes the public key via HTTPS request.
The Service that wants to authenticate the user should provide this user with a unique challenge.
The User signs the challenge using his/her private key and returns it to the Service, among with the public key and the domain name.
The Service then validates if the challenge is correct and was correct signed. The Service also validates if the domain name indeed expose that public key.

### Provider

The above mentioned flow requires and extra piece of software: a user-trusted provider. This provider knows/holds the user private key. It can run in the client side, as a browser extension or an installed app; or it can remotelly as a regular webservice.
This provider will respond the request for signature sent by the Service.

In order to be a provider, the Provider needs to register and respond for the [custom protocol](https://html.spec.whatwg.org/#web+-scheme-prefix) `web+hns`.

This protocol will be used by the Service to send authentication challenges.

Asssuming that the community will provide different solutions and implementation options, the User will choose one Provider and register it to respond the custom protocol.

It is outside the context of this HIP to define how the Provider interacts with the User or how the public/private keys should be generated.

This also means that, once the User choose a Provider and is onboarded, the User won't be able to switch to another provider without replacing its public key. In other words: the User cannot authenticate itself with any Provider, but only with the Provider he/she used to generate the public key.

### Exposing the public key

The public key should be exposed via HTTPS request to `.well-known/authentication`. For the domain `example` the full URL should be `https://example/.well-known/authentication`.

This url should use a SSL certificate validate with [DANE](https://tools.ietf.org/html/rfc6698) or by a Certificate authority.

### Authentication request

Any Service can create an authentication request. It's a basic `GET` request to `web+hns://authentication/?challenge=<CHALLENGE>&callback=<CALLBACK>`.

| Key            | Type                 | Options                                                                                                                          |
| -------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| web+hns://     | protocol             | Fixed.                                                                                                                           |
| authentication | Reserved URL keyword | Fixed.                                                                                                                           |
| challenge      | querystring          | A 32-characters long string. Hex.                                                                                                |
| callback       | querystring          | An encoded callback URL. It will be (async) called by the provider with the challenge response. It must be a secure URL (HTTPS). |

### Authentication response

it's a Provider's decision how to identify and authenticate the user with the Provider.

Once the provider identifies the User, it can sign the challenge using the User's private key and call the Service using the provided callback URL in a `GET` request.

The following querystring should be added to the callback URL:

| Key       | Options                                     |
| --------- | ------------------------------------------- |
| domain    | The domain name that will identify the user |
| challenge | The same challenge sent by the Service      |
| signature | The signed challenge                        |

### Handling a response

When the Service is called via its callback URL, it should do the following verifications:

-   Check if the domain exposes a public key.
-   Check if the challenge is the same challenge from the request.
-   Verify if the signature is valid (using the already retrieved public key).

The Service should reject the authentication request if any of the above fails.

Note: there's no point in adding the public key in the response from the Provider to the Service since it won't be trusted.

Once all verifications are complete, the Service can use the `domain` to identify the current user.

## Consideration

This proposal does not force the domain name to be a TLD. Users could authenticate themselves with `support.example` for instance.

There's no need to be strict to a Handshake domain name. Users could authenticate themselves with `example.com` for instance.

## Reference

[DANE](https://tools.ietf.org/html/rfc6698)
