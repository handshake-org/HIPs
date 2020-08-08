---
hip: 1
title: HIP Purpose and Guidelines
status: Active
type: Meta
author: Steven McKie <mckie@amentum.org>
created: 2020-08-07
updated: 2020-08-07
---

## What is an HIP?

HIP stands for Handshake Improvement Proposal. An HIP is a design document providing information to the Handshake community, or describing a new feature for Handshake or its processes or environment. The HIP should provide a concise technical specification of the feature and a rationale for the feature. The HIP author is responsible for building consensus within the community and documenting dissenting opinions.

## HIP Rationale

We intend HIPs to be the primary mechanisms for proposing new features, for collecting community technical input on an issue, and for documenting the design decisions that have gone into Handshake Because the HIPs are maintained as text files in a versioned repository, their revision history is the historical record of the feature proposal.

For Handshake client implementers, HIPs are a convenient way to track the progress of their implementation. Ideally each implementation maintainer would list the HIPs that they have implemented. This will give end users a convenient way to know the current status of a given implementation or library.

## HIP Types

There are three types of HIP:

- A **Standard Track HIP** describes any change that affects most or all Handshake implementations, such as a change to the network protocol, a change in block or transaction validity rules, proposed naming application standards/conventions, or any change or addition that affects the interoperability of applications using Handshake. Furthermore Standard HIPs can be broken down into the following categories. Standards Track HIPs consist of three parts, a design document, implementation, and finally if warranted an update to the [formal specification].
  - **Core** - improvements requiring a consensus fork, as well as changes that are not necessarily consensus critical but may be relevant to Handshake Client Developer discussions
  - **Networking** - includes improvements around P2P networking layer and tbe HNSD light client protocol, as well as proposed additional improvements to any relevant networked protocol specifications.
  - **Interface** - includes improvements around client [API/RPC] specifications and standards, and also certain language-level standards like method names. The label “interface” aligns with the [interfaces repo] and discussion should primarily occur in that repository before an HIP is submitted to the HIPs repository.
  - **HRC** - application-level standards and conventions, including contract standards such as naming and domain standards, name registries, URI schemes, library/package formats, and wallet formats.
- A **Meta HIP** describes a process surrounding Handshake or proposes a change to (or an event in) a process. Process HIPs are like Standards Track HIPs but apply to areas other than the Handshake protocol itself. They may propose an implementation, but not to Handshake's codebase; they often require community consensus; unlike Informational HIPs, they are more than recommendations, and users are typically not free to ignore them. Examples include procedures, guidelines, changes to the decision-making process, and changes to the tools or environment used in Handshake  development. Any meta-HIP is also considered a Process HIP.
- An **Informational HIP** describes an Handshake design issue, or provides general guidelines or information to the Handsahke community, but does not propose a new feature. Informational HIPs do not necessarily represent Handshake community consensus or a recommendation, so users and implementers are free to ignore Informational HIPs or follow their advice.

It is highly recommended that a single HIP contain a single key proposal or new idea. The more focused the HIP, the more successful it tends to be. A change to one client doesn't require an HIP; a change that affects multiple clients, or defines a standard for multiple apps to use, does.

An HIP must meet certain minimum criteria. It must be a clear and complete description of the proposed enhancement. The enhancement must represent a net improvement. The proposed implementation, if applicable, must be solid and must not complicate the protocol unduly.

### Special Requirements for Core HIPs

If a **Core** HIP mentions or proposes changes to the Covenent systems, it should refer to the instructions by their mnemonics and define the opcodes of those mnemonics at least once. A preferred way is the following:
```
TRANSFER (0xfe)
```

## HIP Work Flow

### Shepherding an HIP

Parties involved in the process are you, the champion or *HIP author*, the [*HIP editors*](#hip-editors), and the [*handshake Client Developers*](#client-developers).

Before you begin writing a formal HIP, you should vet your idea. Ask the Handshake community first if an idea is original to avoid wasting time on something that will be be rejected based on prior research. It is thus recommended to open a discussion thread on [the Handshake Magicians forum] to do this, but you can also use [one of the Handshake Gitter chat rooms], [the Handshake subreddit] or [the Issues section of this repository]. 

In addition to making sure your idea is original, it will be your role as the author to make your idea clear to reviewers and interested parties, as well as inviting editors, developers and community to give feedback on the aforementioned channels. You should try and gauge whether the interest in your HIP is commensurate with both the work involved in implementing it and how many parties will have to conform to it. For example, the work required for implementing a Core HIP will be much greater than for an ERC and the HIP will need sufficient interest from the Handshake client teams. Negative community feedback will be taken into consideration and may prevent your HIP from moving past the Draft stage.

### Core HIPs

For Core HIPs, given that they require client implementations to be considered **Final** (see "HIPs Process" below), you will need to either provide an implementation for clients or convince clients to implement your HIP. 

The best way to get client implementers to review your HIP is to present it on an AllCoreDevs call. You can request to do so by posting a comment linking your HIP on an [AllCoreDevs agenda GitHub Issue].  

The AllCoreDevs call serve as a way for client implementers to do three things. First, to discuss the technical merits of HIPs. Second, to gauge what other clients will be implementing. Third, to coordinate HIP implementation for network upgrades.

These calls generally result in a "rough consensus" around what HIPs should be implemented. This "rough consensus" rests on the assumptions that HIPs are not contentious enough to cause a network split and that they are technically sound.

:warning: The HIPs process and AllCoreDevs call were not designed to address contentious non-technical issues, but, due to the lack of other ways to address these, often end up entangled in them. This puts the burden on client implementers to try and gauge community sentiment, which hinders the technical coordination function of HIPs and AllCoreDevs calls. If you are shepherding an HIP, you can make the process of building community consensus easier by making sure that [the Handshake Magicians forum] thread for your HIP includes or links to as much of the community discussion as possible and that various stakeholders are well-represented.

*In short, your role as the champion is to write the HIP using the style and format described below, shepherd the discussions in the appropriate forums, and build community consensus around the idea.* 

### HIP Process 

Following is the process that a successful non-Core HIP will move along:

```
[ WIP ] -> [ DRAFT ] -> [ LAST CALL ] -> [ FINAL ]
```

Following is the process that a successful Core HIP will move along:

```
[ IDEA ] -> [ DRAFT ] -> [ LAST CALL ] -> [ ACCEPTED ] -> [ FINAL ]
```

Each status change is requested by the HIP author and reviewed by the HIP editors. Use a pull request to update the status. Please include a link to where people should continue discussing your HIP. The HIP editors will process these requests as per the conditions below.

* **Idea** -- Once the champion has asked the Handshake community whether an idea has any chance of support, they will write a draft HIP as a [pull request]. Consider including an implementation if this will aid people in studying the HIP.
  * :arrow_right: Draft -- If agreeable, HIP editor will assign the HIP a number (generally the issue or PR number related to the HIP) and merge your pull request. The HIP editor will not unreasonably deny an HIP.
  * :x: Draft -- Reasons for denying draft status include being too unfocused, too broad, duplication of effort, being technically unsound, not providing proper motivation or addressing backwards compatibility, or not in keeping with the [Handshake philosophy](https://github.com/Handshake/wiki/wiki/White-Paper#philosophy).
* **Draft** -- Once the first draft has been merged, you may submit follow-up pull requests with further changes to your draft until such point as you believe the HIP to be mature and ready to proceed to the next status. An HIP in draft status must be implemented to be considered for promotion to the next status (ignore this requirement for core HIPs).
  * :arrow_right: Last Call -- If agreeable, the HIP editor will assign Last Call status and set a review end date (`review-period-end`), normally 14 days later.
  * :x: Last Call -- A request for Last Call status will be denied if material changes are still expected to be made to the draft. We hope that HIPs only enter Last Call once, so as to avoid unnecessary noise on the RSS feed.
* **Last Call** -- This HIP will listed prominently on the https://HIPs.Handshake.org/ website (subscribe via RSS at [last-call.xml](/last-call.xml)).
  * :x: -- A Last Call which results in material changes or substantial unaddressed technical complaints will cause the HIP to revert to Draft.
  * :arrow_right: Accepted (Core HIPs only) -- A successful Last Call without material changes or unaddressed technical complaints will become Accepted.
  * :arrow_right: Final (Non-Core HIPs) -- A successful Last Call without material changes or unaddressed technical complaints will become Final.
* **Accepted (Core HIPs only)** -- This status signals that material changes are unlikely and Handshake client developers should consider this HIP for inclusion. Their process for deciding whether to encode it into their clients as part of a hard fork is not part of the HIP process.
  * :arrow_right: Draft -- The Core Devs can decide to move this HIP back to the Draft status at their discretion. E.g. a major, but correctable, flaw was found in the HIP.
  * :arrow_right: Rejected -- The Core Devs can decide to mark this HIP as Rejected at their discretion. E.g. a major, but uncorrectable, flaw was found in the HIP.
  * :arrow_right: Final -- Standards Track Core HIPs must be implemented in at least three viable Handshake clients before it can be considered Final. When the implementation is complete and adopted by the community, the status will be changed to “Final”.
* **Final** -- This HIP represents the current state-of-the-art. A Final HIP should only be updated to correct errata.

Other exceptional statuses include:

* **Active** -- Some Informational and Process HIPs may also have a status of “Active” if they are never meant to be completed. E.g. HIP 1 (this HIP).
* **Abandoned** -- This HIP is no longer pursued by the original authors or it may not be a (technically) preferred option anymore.
  * :arrow_right: Draft -- Authors or new champions wishing to pursue this HIP can ask for changing it to Draft status.
* **Rejected** -- An HIP that is fundamentally broken or a Core HIP that was rejected by the Core Devs and will not be implemented. An HIP cannot move on from this state.
* **Superseded** -- An HIP which was previously Final but is no longer considered state-of-the-art. Another HIP will be in Final status and reference the Superseded HIP. An HIP cannot move on from this state.

## What belongs in a successful HIP?

Each HIP should have the following parts:

- Preamble - RFC 822 style headers containing metadata about the HIP, including the HIP number, a short descriptive title (limited to a maximum of 44 characters), and the author details. See [below](https://github.com/Handshake/HIPs/blob/master/HIPS/HIP-1.md#HIP-header-preamble) for details.
- Abstract - A short (~200 word) description of the technical issue being addressed.
- Motivation (*optional) - The motivation is critical for HIPs that want to change the Handshake protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the HIP solves. HIP submissions without sufficient motivation may be rejected outright.
- Specification - The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Handshake platforms (HSD, RSD)
- Rationale - The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.
- Backwards Compatibility - All HIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The HIP must explain how the author proposes to deal with these incompatibilities. HIP submissions without a sufficient backwards compatibility treatise may be rejected outright.
- Test Cases - Test cases for an implementation are mandatory for HIPs that are affecting consensus changes. Other HIPs can choose to include links to test cases if applicable.
- Implementations - The implementations must be completed before any HIP is given status “Final”, but it need not be completed before the HIP is merged as draft. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of “rough consensus and running code” is still useful when it comes to resolving many discussions of API details.
- Security Considerations - All HIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. HIP submissions missing the "Security Considerations" section will be rejected. An HIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.
- Copyright Waiver - All HIPs must be in the public domain. See the bottom of this HIP for an example copyright waiver.

## HIP Formats and Templates

HIPs should be written in [markdown] format.
Image files should be included in a subdirectory of the `assets` folder for that HIP as follows: `assets/HIP-N` (where **N** is to be replaced with the HIP number). When linking to an image in the HIP, use relative links such as `../assets/HIP-1/image.png`.

## HIP Header Preamble

Each HIP must begin with an [RFC 822](https://www.ietf.org/rfc/rfc822.txt) style header preamble, preceded and followed by three hyphens (`---`). This header is also termed ["front matter" by Jekyll](https://jekyllrb.com/docs/front-matter/). The headers must appear in the following order. Headers marked with "*" are optional and are described below. All other headers are required.

` HIP:` *HIP number* (this is determined by the HIP editor)

` title:` *HIP title*

` author:` *a list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.*

` * discussions-to:` *a url pointing to the official discussion thread*

` status:` *Draft | Last Call | Accepted | Final | Active | Abandoned | Rejected | Superseded*

`* review-period-end:` *date review period ends*

` type:` *Standards Track | Informational | Meta*

` * category:` *Core | Networking | Interface | ERC* (Standards Track HIPs only)

` created:` *date created on*

` * updated:` *comma separated list of dates*

` * requires:` *HIP number(s)*

` * replaces:` *HIP number(s)*

` * superseded-by:` *HIP number(s)*

` * resolution:` *a url pointing to the resolution of this HIP*

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

#### `author` header

The `author` header optionally lists the names, email addresses or usernames of the authors/owners of the HIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the author header value must be:

> Random J. User &lt;address@dom.ain&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

#### `resolution` header

The `resolution` header is required for Standards Track HIPs only. It contains a URL that should point to an email message or other web resource where the pronouncement about the HIP is made.

#### `discussions-to` header

While an HIP is a draft, a `discussions-to` header will indicate the mailing list or URL where the HIP is being discussed. As mentioned above, examples for places to discuss your HIP include an issue in this repo or in a fork of this repo, [Reddit r/Handshake](https://www.reddit.com/r/Handshake/), and technical asynhronous channels where discussions are frequented. Overtime we will update this document to better inform an HIP author on where to best champion their efforts for acceptance/rough consensus.

No `discussions-to` header is necessary if the HIP is being discussed privately with the author.

As a single exception, `discussions-to` cannot point to GitHub pull requests.

#### `type` header

The `type` header specifies the type of HIP: Standards Track, Meta, or Informational. If the track is Standards please include the subcategory (core, networking, interface, or HRC).

#### `category` header

The `category` header specifies the HIP's category. This is required for standards-track HIPs only.

#### `created` header

The `created` header records the date that the HIP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-07.

#### `updated` header

The `updated` header records the date(s) when the HIP was updated with "substantial" changes. This header is only valid for HIPs of Draft and Active status.

#### `requires` header

HIPs may have a `requires` header, indicating the HIP numbers that this HIP depends on.

#### `superseded-by` and `replaces` headers

HIPs may also have a `superseded-by` header indicating that an HIP has been rendered obsolete by a later document; the value is the number of the HIP that replaces the current document. The newer HIP must have a `replaces` header containing the number of the HIP that it rendered obsolete.

## Auxiliary Files

HIPs may include auxiliary files such as diagrams. Such files must be named HIP-XXXX-Y.ext, where “XXXX” is the HIP number, “Y” is a serial number (starting at 1), and “ext” is replaced by the actual file extension (e.g. “png”).

## Transferring HIP Ownership

It occasionally becomes necessary to transfer ownership of HIPs to a new champion. In general, we'd like to retain the original author as a co-author of the transferred HIP, but that's really up to the original author. A good reason to transfer ownership is because the original author no longer has the time or interest in updating it or following through with the HIP process, or has fallen off the face of the 'net (i.e. is unreachable or isn't responding to email). A bad reason to transfer ownership is because you don't agree with the direction of the HIP. We try to build consensus around an HIP, but if that's not possible, you can always submit a competing HIP.

If you are interested in assuming ownership of an HIP, send a message asking to take over, addressed to both the original author and the HIP editor. If the original author doesn't respond to email in a timely manner, the HIP editor will make a unilateral decision (it's not like such decisions can't be reversed :)).

## HIP Editors

The current HIP editors are

` * Name (@GithubUser)`


## HIP Editor Responsibilities

For each new HIP that comes in, an editor does the following:

- Read the HIP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don't seem likely to get to final status.
- The title should accurately describe the content.
- Check the HIP for language (spelling, grammar, sentence structure, etc.), markup (GitHub flavored Markdown), code style

If the HIP isn't ready, the editor will send it back to the author for revision, with specific instructions.

Once the HIP is ready for the repository, the HIP editor will:

- Assign an HIP number (generally the PR number or, if preferred by the author, the Issue # if there was discussion in the Issues section of this repository about this HIP)

- Merge the corresponding pull request

- Send a message back to the HIP author with the next step.

Many HIPs are written and maintained by developers with write access to the Handshake codebase. The HIP editors monitor HIP changes, and correct any structure, grammar, spelling, or markup mistakes we see.

The editors don't pass judgment on HIPs. We merely do the administrative & editorial part.

## Style Guide

When referring to an HIP by number, it should be written in the hyphenated form `HIP-X` where `X` is the HIP's assigned number.

## History

This document was derived heavily from [Handshake's HIP-1], which was modified from the BIP process written by Amir Taaki, which in turn was derived from [Python's PEP-0001]. In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use in the Handshake Improvement Process, and should not be bothered with technical questions specific to Handshake or the HIP. Please direct all comments to the HIP editors.

August 7, 2020: HIP-1 has been improved and will be placed as a PR.


### Bibliography


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
