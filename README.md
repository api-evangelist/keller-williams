# Keller Williams (keller-williams)

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
