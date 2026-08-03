# Amazon Ground Station (amazon-ground-station)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AWS Ground Station is a fully managed service that lets you control satellite communications, process satellite data, and scale your satellite operations without having to worry about building or managing your own ground station infrastructure.

**URL:** [https://aws.amazon.com/ground-station/](https://aws.amazon.com/ground-station/)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Data Processing, IoT, Satellite Communications, Space Technology

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### AWS Ground Station API
The AWS Ground Station API provides programmatic access to manage satellite contacts, mission profiles, configs, ground stations, and dataflow endpoint groups for satellite communications and data processing.

**Human URL:** [https://aws.amazon.com/ground-station/](https://aws.amazon.com/ground-station/)

#### Tags:

 - Data Processing, Satellite Communications, Space Technology

#### Properties

- [Documentation](https://docs.aws.amazon.com/ground-station/latest/APIReference/Welcome.html)
- [OpenAPI](openapi/amazon-ground-station-openapi.yaml)
- [GettingStarted](https://aws.amazon.com/ground-station/getting-started/)
- [Pricing](https://aws.amazon.com/ground-station/pricing/)
- [FAQ](https://aws.amazon.com/ground-station/faqs/)
- [APIReference](https://docs.aws.amazon.com/ground-station/latest/APIReference/Welcome.html)
- [JSONSchema](json-schema/ground-station-contact-schema.json)
- [JSONLD](json-ld/amazon-ground-station-context.jsonld)

## Common Properties

- [Portal](https://aws.amazon.com/ground-station/)
- [Documentation](https://docs.aws.amazon.com/ground-station/)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/publicsector/tag/aws-ground-station/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/groundstation/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)

## Features

| Name | Description |
|------|-------------|
| Managed Ground Station Infrastructure | AWS manages a global network of antennas so you do not need to build or operate your own. |
| Satellite Contact Scheduling | Schedule satellite contacts through a simple API. |
| Global Antenna Network | Access antennas at strategic worldwide locations for maximum satellite coverage. |
| Data Downlink and Processing | Receive satellite data directly into AWS cloud services. |
| Mission Profile Configuration | Configure mission profiles specifying dataflow and processing parameters. |
| Integration with AWS Services | Stream satellite data into S3, Kinesis, EC2, and other AWS services. |

## Use Cases

| Name | Description |
|------|-------------|
| Earth Observation | Collect and process satellite imagery for environmental monitoring. |
| Weather Forecasting | Receive data from weather satellites for meteorological analysis. |
| Maritime Tracking | Track ship positions using satellite AIS data. |
| Communications Relay | Use geostationary satellites for communications relay. |
| Scientific Research | Support space-based scientific missions with managed data collection. |

## Integrations

| Name | Description |
|------|-------------|
| Amazon S3 | Store downlinked satellite data directly in S3. |
| Amazon Kinesis | Stream real-time satellite data for immediate processing. |
| Amazon EC2 | Process satellite data on EC2 instances co-located with endpoints. |
| Amazon SageMaker | Apply machine learning to satellite imagery and telemetry data. |

## Artifacts

### OpenAPI

- [Amazon Ground Station OpenAPI](openapi/amazon-ground-station-openapi.yaml)

### JSON Schema

203 schema files in [json-schema/](json-schema/)

### JSON Structure

203 structure files in [json-structure/](json-structure/)

### JSON-LD

- [Amazon Ground Station Context](json-ld/amazon-ground-station-context.jsonld)

### Examples

203 example files in [examples/](examples/)

## Capabilities

### Shared Per-API Definitions

- [Amazon Ground Station](capabilities/shared/amazon-ground-station.yaml) — 10 operations for satellite contact and mission management

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Amazon Ground Station Satellite Operations](capabilities/amazon-ground-station-satellite-operations.yaml) | Amazon Ground Station | 10 | Satellite Operator, Mission Control Engineer |

## Vocabulary

- [Amazon Ground Station Vocabulary](vocabulary/amazon-ground-station-vocabulary.yaml) — Unified taxonomy mapping 6 resources, 7 actions, 1 workflow, and 2 personas

## Rules

- [Amazon Ground Station Spectral Rules](rules/amazon-ground-station-spectral-rules.yml) — 8 rules enforcing Amazon Ground Station API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
