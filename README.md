# Amazon Ground Station (amazon-ground-station)
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
