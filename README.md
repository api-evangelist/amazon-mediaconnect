# Amazon MediaConnect (amazon-mediaconnect)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AWS Elemental MediaConnect is a high-quality transport service for live video that provides the reliability, security, and visibility customers expect from traditional satellite and fiber services. It enables broadcasters to build live video workflows in the cloud with reliable transport of broadcast-quality content using protocols including Zixi, RIST, SRT, RTP, and RTP with FEC.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amazon-mediaconnect/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Broadcasting, Live Video, Media, Media Transport

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### AWS Elemental MediaConnect API
The AWS Elemental MediaConnect API provides programmatic access to create and manage flows, sources, outputs, entitlements, VPC interfaces, bridges, gateways, and media streams for reliable live video transport in the cloud.

**Human URL:** [https://aws.amazon.com/mediaconnect/](https://aws.amazon.com/mediaconnect/)

#### Tags:

 - Broadcasting, Live Video, Media Transport, Flows, Bridges, Gateways

#### Properties

- [Documentation](https://docs.aws.amazon.com/mediaconnect/latest/api/welcome.html)
- [OpenAPI](openapi/amazon-mediaconnect-openapi-original.yml)
- [GettingStarted](https://aws.amazon.com/mediaconnect/getting-started/)
- [Pricing](https://aws.amazon.com/mediaconnect/pricing/)
- [FAQ](https://aws.amazon.com/mediaconnect/faqs/)

## Common Properties

- [Portal](https://aws.amazon.com/mediaconnect/)
- [Documentation](https://docs.aws.amazon.com/mediaconnect/)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/mediaconnect/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)

## Features

| Name | Description |
|------|-------------|
| Video Transport Protocols | Supports Zixi, RIST, SRT, RTP, and RTP with FEC protocols for reliable live video delivery over IP networks. |
| Gateway Capability | Transmit compressed video between on-premises multicast environments and cloud infrastructure via the MediaConnect Gateway. |
| Uncompressed Video Support | Handle uncompressed and visually-lossless video through AWS CDI and JPEG XS encoding with low-latency delivery. |
| End-to-End Encryption | Built-in AES encryption with AWS Secrets Manager integration for encryption key management. |
| Entitlements | Grant partner and customer accounts controlled access to your video streams via entitlements. |
| Flow Management | Programmatically create and manage flows, sources, outputs, and VPC interfaces. |
| Workflow Monitor | Visualize relationships between resources in live video workflows across connected AWS services. |

## Use Cases

| Name | Description |
|------|-------------|
| 24/7 TV Channel Operation | Transport continuous broadcast streams reliably for round-the-clock television channels. |
| Live Event Streaming | Manage event-based video distribution for sports, concerts, news, and other live events. |
| Content Sharing | Share live video feeds with partners and customers through controlled entitlements. |
| Disaster Recovery | Provide redundant video pathways for business continuity in broadcast workflows. |

## Integrations

| Name | Description |
|------|-------------|
| AWS Elemental MediaLive | Send video flows to MediaLive for transcoding and processing. |
| Amazon CloudWatch | Monitor MediaConnect performance metrics and set alarms. |
| Amazon EventBridge | Trigger event-driven workflows based on MediaConnect source health changes. |
| Amazon CloudFront | Deliver processed video content at scale using CloudFront. |
| AWS Secrets Manager | Securely manage encryption keys for content protection. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Amazon MediaConnect OpenAPI](openapi/amazon-mediaconnect-openapi-original.yml)

### JSON Schema

- 243 schema files in [json-schema/](json-schema/)

### JSON Structure

- 243 structure files in [json-structure/](json-structure/)

### JSON-LD

- [Amazon MediaConnect API Context](json-ld/amazon-mediaconnect-api-context.jsonld)

### Examples

- 243 example files in [examples/](examples/)

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [MediaConnect](capabilities/shared/mediaconnect.yaml) — 50 operations for live video transport

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Amazon MediaConnect Live Video Transport](capabilities/amazon-mediaconnect-live-video-transport.yaml) | MediaConnect | 8 | Broadcast Engineer |

## Vocabulary

- [Amazon MediaConnect Vocabulary](vocabulary/amazon-mediaconnect-vocabulary.yaml) — Unified taxonomy mapping resources, actions, workflows, and personas across operational (OpenAPI) and capability (Naftiko) dimensions

## Rules

- [Amazon MediaConnect Spectral Rules](rules/amazon-mediaconnect-spectral-rules.yml) — 20 rules across 8 categories enforcing Amazon MediaConnect API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
