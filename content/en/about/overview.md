---
type: docs
category: About Sigstore
description: Documentation for Sigstore
home: true
title: Overview
weight: 10
---

![Sigstore](sigstore-logo_horizontal-color.svg)

**Sigstore is an open source project for improving software supply chain security. The Sigstore framework and tooling empowers software developers and consumers to securely sign and verify software artifacts such as release files, container images, binaries, software bills of materials (SBOMs), and more. Signatures are generated with ephemeral signing keys so there's no need to manage keys. Signing events are recorded in a tamper-resistant public log so software developers can audit signing events.**

The project is backed by the Open Source Security Foundation (OpenSSF) under the Linux Foundation, with contributions from Google, Red Hat, Chainguard, GitHub and Purdue University. It is 100% open source and free to use for all developers and software providers. The Sigstore community develops and maintains tools to simplify code signing and verification, and also operates a public-good, non-profit service to improve the open source software supply chain.

## Why cryptographic signing?

In a landscape of growing software supply chain vulnerability, unsigned software is at risk for several attack vectors, such as:

- **Typosquatting packages with similar names**
- **Compromised site where package is hosted**
- **Tampering after being published**

Digital signatures are a way to verify the authenticity of a software artifact. Software consumers can trace software back to the source to know who created the artifact and that it has not been altered or tampered with after it was signed.

## Why Sigstore?

Traditional artifact signing relies on exchanging cryptographic keypairs for signature verification. The software creator keeps one key secret (the private “signing” key) and publishes the other (the public “verification” key). When a software consumer wants to verify an artifact’s signature, the verification keys are exchanged to prove that the holder of the private key created the signature.

This traditional approach has several weaknesses:

- **Identity**: How do you know the person signing the artifact is who they say they are?
- **Key management**: How do you keep the private key secure so it can’t be lost or stolen? How do you make the public key easily accessible for users, but also protect it from tampering by a malicious attacker?
- **Key revocation**: If the keypair is compromised, how do you distribute new keys in a way that convinces users of your legitimacy and that you’re not an attacker?

Sigstore addresses these problems by helping users move away from a key-based signing approach to an identity-based one. When using Sigstore’s full capabilities, your artifact is:

- **Signed**: By using a Sigstore client (Cosign).
- **Associated**: With an identity through our certificate authority (Fulcio).
- **Witnessed**: By recording the signing information in a permanent transparency log (Rekor).

The signer ideally forgoes using long-lived keypairs. With “keyless” or “ephemeral key” signing, users verify the artifact using the transparency log for signature verification rather than keys. Sigstore improves on traditional methods of signing to be more convenient and secure:

- **Convenience**: Users can take advantage of convenient tooling, easy container signing, and can even bypass the difficult problem of key management and rotation.
- **Automation**: Users can also automate signing using our [CI tooling]({{< relref "quickstart/quickstart-ci">}}).
- **Security**: With Sigstore, the artifact is not just signed; it’s signed with an ephemeral key, associated with a known identity, and publicly auditable.

## How Sigstore works

A Sigstore client, such as Cosign, creates a public/private key pair and makes a certificate signing request to our code-signing certificate authority (Fulcio) with the public key. A verifiable OpenID Connect (OIDC) identity token is also provided in the request. The OIDC token contains a user's email address or service account, or workflow information in the case of CI signing. The certificate authority verifies this token and issues a short-lived certificate bound to the provided identity and public key.

You don’t have to manage signing keys, and Sigstore services never obtain your private key. The public key that a Sigstore client creates gets bound to the issued certificate, and the private key is discarded after a single signing.

After the client signs the artifact, the artifact's digest, signature and certificate are persisted in a transparency log: an immutable, append-only ledger known as Rekor. With this log, signing events can be publicly audited. Identity owners can monitor the log to verify that their identity is being properly used, and someone who downloads an artifact can confirm that the certificate was valid at the time of signing.

For verifying an artifact, a Sigstore client will verify the signature on the artifact using the public key from the certificate, verify the identity in the certificate matches an expected identity, verify the certificate's signature using Sigstore's root of trust, and verify proof of inclusion in Rekor. Together, verification of this information tells the user that the artifact comes from its expected source and has not been tampered with after its creation.

For more information on the modules that make up Sigstore, review [Tooling]({{< relref "about/tooling">}}).

## How to use Sigstore

To use Sigstore, you must first install the client. Review the [Installation]({{< relref "cosign/system_config/installation">}}) instructions. You can then pick the subject matter you wish to learn about from the menu items on the left. For a quick introduction, you can try using one of the links below:

- To get a quick view of how to use the program see [Quick Start]({{< relref "quickstart/quickstart-cosign">}})
- To learn how to work with blobs, see [sign a blob]({{< relref "cosign/signing/signing_with_blobs">}})
- To learn how to work with containers, see [sign a container]({{< relref "cosign/signing/signing_with_containers">}})
- To use Gitsign, see [Sign Git commits with Gitsign]({{< relref "cosign/signing/gitsign">}})
- To learn about verification, see [the importance of verification]({{< relref "about/the-importance-of-verification">}}) and [verify entries with Cosign]({{< relref "cosign/verifying/verify">}})
- To learn about incorporating Sigstore into your CI System, see [our CI Quickstart]({{< relref "quickstart/quickstart-ci">}})

## Contributing

Up to date documentation, best practices, and detailed scenarios for Sigstore live here. These pages are maintained by the community and intended to help anyone get set up with any of the technologies, to find what you’re looking for fast. It’s also where we keep all the relevant pages for the Sigstore trust root, from signing ceremonies to security practices.

Ready to jump in? Check the [contributing guidelines]({{< relref "about/contributing">}}).

## Learn more

- [Sigstore YouTube Channel](https://www.youtube.com/@projectsigstore)
- [Sigstore Blog](https://blog.sigstore.dev/)
