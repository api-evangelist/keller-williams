# Keller Williams

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

Keller Williams Realty, Inc. (KWRI) is an Austin, Texas headquartered residential real estate franchisor and the largest real estate brokerage franchise in the United States by agent count, operating more than 1,000 market centers worldwide through independently owned and operated offices. In the value chain Keller Williams sits at the brokerage and agent-platform layer, not the data-cooperative layer: it franchises brokerages, and it builds and operates KW Command, a proprietary agent operating system for lead, contact, task, listing, opportunity and marketing management, plus the KW Marketplace where third-party vendors sell integrated products to KW agents. Its API posture is real but partner-gated. Keller Williams runs a genuine Apigee-backed developer portal (DevHub) at developer.kw.com with a live gateway at partners.api.kw.com and a sandbox at sandbox.partners.api.kw.com, an OpenID Connect authorization server whose discovery document is publicly readable, and a published API License Agreement. Access is not self-serve: a developer must submit a partnership application through the "Apply to Integrate" form, be approved by KWRI, receive a DevHub account, then sign the KW API License Agreement and Terms of Use before receiving an API key and secret. Only one API — KW Worldwide Listings Search — is visible anonymously in the API catalog; the rest of the catalog (Command MC, Contact and Lead Data, Tasks, Opportunity/Deal/ Transaction Data, User/Organization/Role Information, Marketplace Subscription Management, Internal) is behind a SAML member login. On the sector's central question, Keller Williams carries no RESO posture at all: it is absent from the RESO certification directory, no RESO Web API, Data Dictionary, OData $metadata or Universal Property Identifier reference appears anywhere in its developer portal or specification, and its own listings spec states in its description that the service "is not a substitution for or tied to any Multiple Listings Services database records." This is a brokerage's own listing and CRM data behind a commercial partner agreement, not licensed MLS data behind a certified standard.

**Areas:** Real Estate · United States · Residential Real Estate · Brokerage · Franchise · Property Listings · PropTech · Agent Platform · CRM · Partner API · Marketplace · Austin Texas

## Keller Williams on APIs.io

Everything profiled for Keller Williams is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/keller-williams/](https://apis.io/providers/keller-williams/)** — or go straight to one of the APIs or properties below.

### APIs (3)

- [KW Partner Identity API (OpenID Connect)](https://apis.io/apis/keller-williams/keller-williams-partner-identity-api/) — Keller Williams uses OpenID Connect on top of OAuth 2.0 to grant a partner application access to an individual KW user's Command data. The authorization…
- [KW Marketplace Subscription & Metered Billing API](https://apis.io/apis/keller-williams/keller-williams-marketplace-subscription-api/) — The partner-facing surface behind the KW Marketplace, where vendors sell products to Keller Williams agents. Keller Williams pushes subscription lifecycle…
- [Keller Williams Search API](https://apis.io/apis/keller-williams/keller-williams-search-api/) — The Search API from Keller Williams — 5 operation(s) for search.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (1)

Multi-step workflows that chain operations into a business task.

- [Publish a KW Worldwide listing](https://apis.io/arazzos/keller-williams/keller-williams-publish-listing/)

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Keller Williams Marketplace Webhooks](https://apis.io/asyncapis/keller-williams/keller-williams-marketplace-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [keller-williams](https://apis.io/servers/keller-williams/keller-williams/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Keller Williams Agentic Access](https://apis.io/agentic-access/keller-williams/keller-williams-agentic-access/)

#### Examples (1)

Example requests and responses.

- [Keller Williams Listings Search Examples](https://apis.io/examples/keller-williams/keller-williams-listings-search-examples/)

#### Collections (1)

Postman and Open Collection request collections.

- [KW Listings Search](https://apis.io/collections/keller-williams/open-keller-williams-listings-search/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Keller Williams Listings Search Overlay](https://apis.io/overlays/keller-williams/keller-williams-listings-search-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Keller Williams Authentication](https://apis.io/security/keller-williams/keller-williams-authentication/)
- [Keller Williams Domain Security](https://apis.io/security/keller-williams/keller-williams-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Keller Williams Scopes](https://apis.io/scopes/keller-williams/keller-williams-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.kw.com/)
- [Documentation](https://developer.kw.com/getting-started)
- [Getting started](https://developer.kw.com/getting-started)
- [API reference](https://developer.kw.com/docs/listingskww/1/overview)
- [Sign up](https://share.hsforms.com/2JQHe7zfKRLSo_cXKjy-5nwbg45/)
- [Support](https://developer.kw.com/support)
- [Blog](https://outfront.kw.com/)
- [Terms of service](https://developer.kw.com/terms-and-conditions)
- [Privacy policy](https://legal.kw.com/privacy-policy)
- [GitHub](https://github.com/KWRI)
- [Status](https://status.kw.com/)

## API Evangelist rating

**Kin Score 36.0** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 74.1 |
| Contract quality | 61.8 |
| Contract governance | 4.5 |
| Operational transparency | 26.3 |
| Developer ergonomics | 19.6 |
| Access clarity | 23.7 |

**Agent readiness 34.9** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 28.7** — Securities & Market Data

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/keller-williams/](https://apis.io/providers/keller-williams/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Keller Williams: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/keller-williams/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
