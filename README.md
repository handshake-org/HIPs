# Handshake Improvement Proposals (HIPs)



Handshake Improvement Proposals (HIPs) describe standards for the Handshake platform, including core protocol specifications, client APIs, and covenant standards.

A browsable version of all current and draft HIPs can be found on [Future Site](HSD-Dev.org).

# Contributing

 1. Review [HIP-1](HIPS/hip-1.md).
 2. Fork the repository by clicking "Fork" in the top right.
 3. Add your HIP to your fork of the repository. There is a [template HIP here](hip-template.md).
 4. Submit a Pull Request to Handshake [HIPs repository](https://github.com/handshake-org/HIPs/).

Your first PR should be a first draft of the final HIP. It must meet the formatting criteria enforced by the build (largely, correct metadata in the header). An editor will manually review the first PR for a new HIP and assign it a number before merging it. Make sure you include a `discussions-to` header with the URL to a discussion forum or open GitHub issue where people can discuss the HIP as a whole.

If your HIP requires images, the image files should be included in a subdirectory of the `assets` folder for that HIP as follows: `assets/hip-N` (where **N** is to be replaced with the HIP number). When linking to an image in the HIP, use relative links such as `../assets/hip-1/image.png`.

Once your first PR is merged, we have a bot that helps out by automatically merging PRs to draft HIPs. For this to work, it has to be able to tell that you own the draft being edited. Make sure that the 'author' line of your HIP contains either your GitHub username or your email address inside <triangular brackets>. If you use your email address, that address must be the one publicly shown on [your GitHub profile](https://github.com/settings/profile).

When you believe your HIP is mature and ready to progress past the draft phase, you should do one of two things:

 - **For a Standards Track HIP of type Core**, ask to have your issue added to [the agenda of an upcoming Handshake Developer meeting](Link to Meeting Schedule), where it can be discussed for inclusion in a future hard fork. If implementers agree to include it, the HIP editors will update the state of your HIP to 'Accepted'.
 - **For all other HIPs**, open a PR changing the state of your HIP to 'Final'. An editor will review your draft and ask if anyone objects to its being finalised. If the editor decides there is no rough consensus - for instance, because contributors point out significant issues with the HIP - they may close the PR and request that you fix the issues in the draft before trying again.

# HIP Status Terms

* **Draft** - an HIP that is undergoing rapid iteration and changes.
* **Last Call** - an HIP that is done with its initial iteration and ready for review by a wide audience.
* **Accepted** - a core HIP that has been in Last Call for at least 2 weeks and any technical changes that were requested have been addressed by the author. The process for Handshake Client Developers to decide whether to encode an HIP into their clients as part of a hard fork is not part of the HIP process. If such a decision is made, the HIP will move to final.
* **Final (Non-Core)** - an HIP that has been in Last Call for at least 2 weeks and any technical changes that were requested have been addressed by the author.
* **Final (Core)** - an HIP that the Handshake Client Developers have decided to implement and release in a future hard fork or has already been released in a hard fork. 

# Preferred Citation Format

The canonical URL for HIP that has achieved draft status at any point is at the Handshake-Org HIPs. For example, the canonical URL for HIP-1 is (Link to HIP-001)

# Validation

**Not Relant to Handshake HIP Process At This Time**

HIPs must pass some validation tests.  The HIP repository ensures this by running tests using [html-proofer](https://rubygems.org/gems/html-proofer) and [hip_validator](Link to Validator).

It is possible to run the HIP validator locally:
```sh
gem install hip_validator
hip_validator <INPUT_FILES>
```

# Automerger

**Not Relevant to Handshake HIP Process At This Time**

The HIP repository contains an "auto merge" feature to ease the workload for EIP editors.  If a change is made via a PR to a draft HIP, then the authors of the HIP can GitHub approve the change to have it auto-merged by the [hip-automerger](https://github.com/hip-automerger/automerger) bot.

