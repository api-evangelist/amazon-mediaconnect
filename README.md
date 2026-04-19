# Amazon MediaConnect (amazon-mediaconnect)
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
