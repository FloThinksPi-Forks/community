---
subtitle: Extend support for CF API Client Libraries and Tools
title: RFC-0033
---
# Meta

- Name: Documenting the CF API with OpenAPI
- Start Date: 2025-06-27
- Author(s): @flothinkspi
- Status: Draft
- RFC Pull Request: [community#1112](https://github.com/cloudfoundry/community/pull/1112)

## Summary

With the acceptance of
[RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md),
a problem arises for users of CF who rely on unsupported client
libraries. To ensure the success of
[RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md)
and to enable users to better transition to the V3 API, this RFC proposes
to document the CF API V3 using the OpenAPI specification.

## Problem

## Motivation

1. Improve the overall CF user experience, increasing the adoption and
    popularity of CloudFoundry.

2. Support the adoption of
    [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md)
    by minimizing the impact on CF users who rely on unsupported client
    libraries. This will ultimately increase the likelihood and speed of
    the CF community moving forward with
    [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md).

## Proposal

### Documenting the CF V3 API with OpenAPI

With [OpenAPI](https://www.openapis.org/), APIs can be defined in a
machine-readable format (YAML), enabling the generation of client
libraries for various programming languages and the rendering of
interactive HTML API documentation. The OpenAPI specification is widely
adopted and supported by many tools. A [Proof of Concept
(PoC)](https://github.com/FloThinksPi/cf-api-openapi-poc) and also is already
exploring the documentation of the CF V3 API as an OpenAPI spec,
providing a glimpse (currently with incorrect content) of the potential
look and feel of this documentation format.

The CloudFoundry organization SHOULD document the CF API V3 using the
OpenAPI specification. Which then can be used by CF Maintainers to
generate client libraries for the most used programming languages that
currently lack support (Python, JavaScript, Rust). While this enables CF
Maintainers themselves, the OpenAPI documentation format furthermore
empowers users with unique requirements to generate their own client
libraries or starter templates, providing an exceptional user experience
for specialized use cases.

> Remark: The new version [OpenAPI 4.0
> (Moonwalk)](https://github.com/OAI/sig-moonwalk) is expected to be
> finalized in 2024. However, it MAY take some time for all the tooling
> to adapt to this new version. Nonetheless, it is something to keep in
> mind for future developments.

## Workstreams

### App Runtime Interfaces WG

**Phase 1** - Explore OpenAPI as a documentation format for the CF API
V3 as part of a PoC. - Generate client libraries for Python, JavaScript,
and Rust based on the OpenAPI spec as a PoC.

**Checkpoint 1** - TOC review and approval of the PoC, ensuring the
approach is feasible and the generated client libraries are usable.

**Phase 2** - Document the CF API V3 using the OpenAPI specification and
replace the existing documentation when feature complete. - Generate
client libraries for Python, JavaScript, and Rust based on the OpenAPI
spec, filling gaps not covered by generators. Release these libraries as
experimental. - Establish processes for automated version bumps, release
notes, and automated tests using a test suite for each client library.

**Checkpoint 2** (early 2026) - TOC review and approval to move the
client libraries from experimental to supported, depending on their
quality and maturity and experiences in the previous phases. - The
decision SHOULD be made timely before the official removal of the CF V2
API to provide CF users with a clear path forward and time to adopt.

## Impact and Consequences

### Positive

- CF users will have access to supported client libraries for the most
  commonly used programming languages, enabling smoother automation and
  CI/CD pipeline integration.

- CF may see an increase in adoption and popularity due to improved
  usability and user experience and the use of a documentation format
  (OpenAPI) that is widely adopted in the industry and integrates well
  with other tools.

- The CF community will have more influence on the client libraries used
  by the majority of CF users, allowing the community to address
  issues/features from a client/consumer side too.

- The OpenAPI documentation format empowers users with unique
  requirements to generate their own client libraries or starter
  templates, providing an exceptional user experience for specialized
  use cases.

- Reduced backpressure onto
  [RFC-32](https://github.com/cloudfoundry/community/blob/d7b48620d0da3bcadeea18aaf64f6fa36c329e7f/toc/rfc/rfc-0032-cfapiv2-eol.md)
  due to supplying CF users with a clear path out of their dilemma.

### Negative

- The CloudFoundry organization will have to invest time and resources
  to document the CF API V3 using the OpenAPI specification and support
  additional client libraries. It is difficult to estimate if and how
  much of these investments will reflect as long-term costs that may
  increase the maintenance efforts required by the CloudFoundry
  community.
