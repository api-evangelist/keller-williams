# Keller Williams (keller-williams)

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

Keller Williams Realty, Inc. (KWRI) is an Austin, Texas headquartered residential real estate franchisor and the largest real estate brokerage franchise in the United States by agent count, operating more than 1,000 market centers worldwide through independently owned and operated offices. It builds and operates KW Command, a proprietary agent operating system, and the KW Marketplace where third-party vendors sell integrated products to KW agents. Its API posture is real but partner-gated: a genuine Apigee-backed developer portal (DevHub) at developer.kw.com, a live gateway at partners.api.kw.com, an OpenID Connect authorization server with a publicly readable discovery document, and a published API License Agreement — but access requires an approved partnership application. Keller Williams carries no RESO posture: it is absent from the RESO certification directory, and its own listings specification states the service "is not a substitution for or tied to any Multiple Listings Services database records."

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/keller-williams/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/keller-williams/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United States
- Residential Real Estate
- Brokerage
- Franchise
- Property Listings
- PropTech
- Agent Platform
- CRM
- Partner APIs
- Marketplace
- Austin Texas

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### KW Worldwide Listings Search API

The only Keller Williams API whose specification is published anonymously in the DevHub API catalog. Returns Keller Williams Listing Service (KWLS) listings scoped to a KW Worldwide region, with listing create, update and delete, a controlled-vocabulary lookup table, and an organization-people lookup. The published OpenAPI 3.0.1 document declares 5 paths and 8 operations.

- **Human URL:** [https://developer.kw.com/docs/listingskww/1/overview](https://developer.kw.com/docs/listingskww/1/overview)
- **Base URL:** `https://partners.api.kw.com/v2`

#### Tags

- Property Listings
- Search
- KW Worldwide
- KWLS

#### Properties

- [OpenAPI](openapi/keller-williams-listings-search-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://developer.kw.com/docs/listingskww/1/overview)
- [API Reference](https://developer.kw.com/docs/listingskww/1/routes/listings/get)
- [Documentation](https://developer.kw.com/filtering-and-sorting)
- [Sandbox](https://sandbox.partners.api.kw.com/v2)

### KW Partner Identity API (OpenID Connect)

OpenID Connect on top of OAuth 2.0, granting a partner application access to an individual KW user's Command data. The authorization server at `partners.api.kw.com/idp` serves a publicly readable discovery document advertising authorize, token, introspect, userinfo, revoke, end_session and device_authorization endpoints, RS256 ID tokens, PKCE S256, and 40 supported scopes.

- **Human URL:** [https://developer.kw.com/getting-started](https://developer.kw.com/getting-started)
- **Base URL:** `https://partners.api.kw.com/idp`

#### Tags

- Authentication
- OpenID Connect
- OAuth 2.0
- Scopes

#### Properties

- [OpenID Connect Discovery](authentication/keller-williams-openid-configuration.json)
- [Documentation](https://developer.kw.com/getting-started)
- [Documentation](https://developer.kw.com/docs-authentication)
- [Documentation](https://developer.kw.com/docs-refresh-token)

### KW Marketplace Subscription & Metered Billing API

The partner-facing surface behind the KW Marketplace. Keller Williams pushes subscription lifecycle events to a partner's secured HTTPS endpoint, and partners report consumption back through a pre-configured metered billing endpoint. Sample payloads and an error-code reference are published publicly; no OpenAPI document is offered for this surface.

- **Human URL:** [https://developer.kw.com/marketplace-documentation](https://developer.kw.com/marketplace-documentation)
- **Base URL:** `https://partners.api.kw.com/v1`

#### Tags

- Marketplace
- Subscriptions
- Billing
- Events

#### Properties

- [Documentation](https://developer.kw.com/marketplace-documentation)
- [Documentation](https://developer.kw.com/metered-billing)
- [Documentation](https://developer.kw.com/subscription-lifecycle-walkthrough)
- [Documentation](https://developer.kw.com/docs-synchronous-subscription)
- [Documentation](https://developer.kw.com/docs-asynchronous-subscription)
- [Documentation](https://developer.kw.com/docs-error-codes-reference)

## Access

| Question | Answer |
| --- | --- |
| Developer portal | https://developer.kw.com/ (HTTP 200, Apigee integrated portal, site id `devhub-apigee-prod-kwri`) |
| Access gate | Application + approval, then a signed API License Agreement |
| What you must sign or join | Submit the "Apply to Integrate" / "Become A Partner" HubSpot application, receive KWRI approval and a DevHub account, accept the KW API License Agreement (effective 2023-08-02) and the KWRI API Terms of Use |
| Auth model | OAuth 2.0 / OpenID Connect (authorization code, PKCE S256) plus API key + secret; Basic auth for non-user-specific resources; SAML sign-in for the DevHub portal itself |
| RESO posture | No RESO reference anywhere; not listed among the 578 organizations in the RESO certification directory |
| Open data | None |

## Maintainers

- Kin Lane — kin@apievangelist.com
