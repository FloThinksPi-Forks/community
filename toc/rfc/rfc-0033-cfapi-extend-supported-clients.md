---
title: RFC-0033
subtitle: Extend support for CF API Client Libraries and Tools
status: Draft
author:
- Florian Braun (FloThinksPi), SAP SE
date: 21.10.2024
abstract: |
    With the acceptance of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md), a problem arises for users of CF who rely on unsupported client libraries. To ensure the success of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md) and to transition users to supported client libraries, this RFC proposes the introduction of supported client libraries leveraging OpenAPI.
pull-request: https://github.com/cloudfoundry/community/pull/941
--- 

# Meta
[meta]: #meta
- Name: Extend support for CF API Client Libraries and Tools
- Start Date: 21.10.2024
- Author(s): Florian Braun (FloThinksPi), SAP SE
- Status: Draft
- RFC Pull Request: 

## Abstract

With the acceptance of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md), a problem arises for users of CF who rely on unsupported client libraries. To ensure the success of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md) and to transition users to supported client libraries, this RFC proposes the introduction of supported client libraries leveraging OpenAPI.

## Problem

Historically, the CloudFoundry organization has been reluctant to support client libraries for interfacing with the CloudFoundry API (CF API). According to the latest [documentation](https://docs.cloudfoundry.org/devguide/capi/client-libraries.html), the only fully supported client library is for Java. The Golang library is listed as experimental and is not suitable for production use cases. Consequently, the CloudFoundry CLI remains the only officially supported method for interacting with the CF API for all programming languages other than Java.
CF users face numerous challenges with the CF CLI, such as parsing the CF CLI`s stdout and stderr, which are not designed to be machine-readable. This leads to significant overhead in development, error-prone automation, poor error handling, and cumbersome debugging.

With the addition of the [Terraform CloudFoundry provider](https://github.com/cloudfoundry/terraform-provider-cloudfoundry) as a potentially supported mechanism for interfacing with the CF API, the need for stable and supported client libraries is more evident than ever.

The current situation forces CF users to rely on unsupported client libraries (e.g., [cf-python-client](https://github.com/cloudfoundry-community/cf-python-client), [cf-tools](https://www.npmjs.com/package/@sap/cf-tools), [cf-nodejs-client](https://github.com/prosociallearnEU/cf-nodejs-client)) or to develop their own. Both options are suboptimal and prevent CF users from fully leveraging the CloudFoundry platform. Additionally, the CloudFoundry organization has limited influence on the quality of these client libraries.

With the acceptance of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md), users of unsupported/closed source CF client libraries face a dilemma. These libraries often lack CF API V3 coverage and are poorly maintained. Users may be forced to write new clients, switch to different libraries, that may not exist yet, or fork/take over maintenance and extend their libraries when the V2 API is removed. Without CloudFoundry-supported client libraries, users are left in a difficult position. This is also backed by a [comment of Greg Copp](https://github.com/cloudfoundry/community/pull/941#issuecomment-2276852572) in the discussion around [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md).

Other PaaS providers offer various client libraries for different programming languages, enhancing platform usability and user experience. For example, [OpenSearch](https://opensearch.org/docs/latest/clients/) offers supported client libraries for eight different programming languages.

CF users thus expect a wide range of client libraries for common programming languages, similar to what other IaaS, PaaS, and SaaS providers offer.

## Motivation

1. Improve the overall CF user experience, increasing the adoption and popularity of CloudFoundry.
2. Support the adoption of [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md) by minimizing the impact on CF users who rely on unsupported client libraries. This will ultimately increase the likelihood and speed of the CF community moving forward with [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md).
3. Increase the influence of the CloudFoundry community on the client libraries used by the majority of CF users, allowing the community to address issues and requirements from both platform and client perspectives.

## Proposals

### Supporting additional Client Libraries and Tools

To enhance the usability and adoption of the CloudFoundry platform, the CloudFoundry organization MUST provide supported client libraries for the most commonly used programming languages. This will enable CF users to leverage stable and supported client libraries for their preferred programming languages, facilitating smoother automation and CI/CD pipeline integration, which are critical in daily operations.

Based on the 2024 [TIOBE Index](https://www.tiobe.com/tiobe-index/), the [PYPL Index](https://pypl.github.io/PYPL.html), and IEEE Spectrum's [ranking](https://spectrum.ieee.org/top-programming-languages), the most relevant programming languages for interfacing with CF are:

- Python
- [Java (Already Existing)](https://github.com/cloudfoundry/cf-java-client)
- JavaScript
- [Golang (Already Existing)](https://github.com/cloudfoundry/go-cfclient)
- Rust (Emerging)

Providing supported client libraries for these languages SHOULD cover the majority of CF users and MUST be pursued by the CF community. 

While other popular programming languages exist according to above statistics, they are less likely to be used for interfacing with CF by most users. These SHOULD be considdered out of scope and include(not limited to):

- C (Low level)
- C++ (Low level)
- C# (Primarily used within the Microsoft ecosystem)
- PHP (Web development)
- R (Statistical computing)
- Swift (Primarily used within the Apple ecosystem for iOS/macOS app development)
- Objective-C (Primarily used within the Apple ecosystem for iOS/macOS app development)

Additionally the CloudFoundry organization SHOULD declare also the Terraform CloudFoundry provider as a supported mechanism for interfacing with the CF API once it is mature enough. Resulting in folowing supported tools that are not considdered programming language specific client libraries:
- [CF CLI](https://github.com/cloudfoundry/cli)
- [CF Terraform Provider](https://github.com/cloudfoundry/terraform-provider-cloudfoundry)

### Documenting the CF V3 API with OpenAPI

With [OpenAPI](https://www.openapis.org/), APIs can be defined in a machine-readable format (YAML), enabling the generation of client libraries for various programming languages and the rendering of interactive HTML API documentation. The OpenAPI specification is widely adopted and supported by many tools. A [Proof of Concept (PoC)](https://github.com/FloThinksPi/cf-api-openapi-poc) is already exploring the documentation of the CF V3 API as an OpenAPI spec, providing a glimpse (currently with incorrect content) of the potential look and feel of this documentation format.

The CloudFoundry organization MUST document the CF API V3 using the OpenAPI specification. Which then can be used by CF Maintainers to generate client libraries for the most commonly used programming languages that currently lack support (Python, JavaScript, Rust). While this enables CF Maintainers themselfs, the OpenAPI documentation format furthermore empowers users with unique requirements to generate their own client libraries or starter templates, providing an exceptional user experience for specialized use cases.

> Remark: The new version [OpenAPI 4.0 (Moonwalk)](https://github.com/OAI/sig-moonwalk) is expected to be finalized in 2024. However, it MAY take some time for all the tooling to adapt to this new version. Nonetheless, it is something to keep in mind for future developments.

## Workstreams

### App Runtime Interfaces WG

**Phase 1**
- Explore OpenAPI as a documentation format for the CF API V3 as part of a PoC.
- Generate client libraries for Python, JavaScript, and Rust based on the OpenAPI spec as a PoC.

**Checkpoint 1**
- TOC review and approval of the PoC, ensuring the approach is feasible and the generated client libraries are usable.

**Phase 2**
- Document the CF API V3 using the OpenAPI specification and replace the existing documentation when feature complete.
- Generate client libraries for Python, JavaScript, and Rust based on the OpenAPI spec, filling gaps not covered by generators. Release these libraries as experimental.
- Establish processes for automated version bumps, release notes, and automated tests using a test suite for each client library.

**Checkpoint 2** (early 2026)
- TOC review and approval to move the client libraries from experimental to supported, depending on their quality and maturity and experiences in the previous phases.
- The decision MUST be made timely before the official removal of the CF V2 API to provide CF users with a clear path forward and time to adopt.

## Impact and Consequences

### Positive

- CF users will have access to supported client libraries for the most commonly used programming languages, enabling smoother automation and CI/CD pipeline integration.
- CF may see an increase in adoption and popularity due to improved usability and user experience and the use of a documentation format (OpenAPI) that is widely adopted in the industry and integrates well with other tools.
- The CF community will have more influence on the client libraries used by the majority of CF users, allowing the community to address issues/features from a client/consumer side too.
- The OpenAPI documentation format empowers users with unique requirements to generate their own client libraries or starter templates, providing an exceptional user experience for specialized use cases.
- Reduced backpressure onto [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md) due to supplying CF users with a clear path out of their dilemma.

### Negative

- The CloudFoundry organization will have to invest time and resources to document the CF API V3 using the OpenAPI specification and support additional client libraries. It is difficult to estimate if and how much of these investments will reflect as long-term costs that may increase the maintenance efforts required by the CloudFoundry community.
