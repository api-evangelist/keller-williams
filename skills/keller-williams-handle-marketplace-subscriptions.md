---
name: Handle KW Marketplace subscription events
description: Receive and correctly answer the seven Keller Williams Marketplace subscription webhooks, run the synchronous or asynchronous provisioning flow, and report metered usage back to KW.
api: asyncapi/keller-williams-marketplace-webhooks.yml
operations:
  - "webhook: SUBSCRIPTION_ORDER"
  - "webhook: SUBSCRIPTION_CHANGE"
  - "webhook: SUBSCRIPTION_CANCEL"
  - "webhook: SUBSCRIPTION_NOTICE"
  - "webhook: USER_ASSIGNMENT"
  - "webhook: USER_UNASSIGNMENT"
  - POST https://partners.api.kw.com/v1/subscription/async/confirm
  - POST https://partners.api.kw.com/v1/subscription/billing/usage
generated: '2026-07-26'
method: generated
source: https://developer.kw.com/marketplace-documentation
grounding: >-
  Keller Williams publishes no OpenAPI or AsyncAPI for the Marketplace
  surface. Every event name, payload field, response code and error code
  below is quoted from the DevHub Marketplace documentation set fetched
  2026-07-26.
---

# Handle KW Marketplace subscription events

You are the vendor (ISV). KW pushes subscription lifecycle events to a
secured HTTPS endpoint you register with the KW team; you answer inline,
and you report usage back.

## Setup

- Register **two** endpoints with KW: one for sandbox
  (`https://sandbox.partners.api.kw.com`), one for production
  (`https://partners.api.kw.com`).
- Your endpoint must authenticate the caller with HTTP Basic **or** OAuth
  2.0 — the KW Gateway supports only those two.
- Create two Marketplace products per service; name the development one
  with "(dev)" or "(test)". A published product can never be unpublished,
  only made invisible, and every change needs KW approval.
- KW issues the API Key and Secret once the gateway entry exists.

## The envelope

```json
{
  "app-name": "<APP_NAME>",
  "access-token": "<TOKEN>",
  "refresh-token": "<REFRESH_TOKEN>",
  "payload": { "event": {
    "type": "SUBSCRIPTION_ORDER",
    "marketplace": { "baseUrl": "https://marketplace.kw.com", "partner": "KELLERWILLIAMS" },
    "flag": "DEVELOPMENT",
    "creator": { "...": "the KW user who triggered it" },
    "payload": {
      "account": { "accountIdentifier": "...", "status": "ACTIVE" },
      "configuration": { "entry": { "key": "KWID", "value": "<KWID>" } },
      "order": { "editionCode": "Tier 1", "pricingDuration": "MONTHLY", "freeTrial": { "active": "false" } }
    }
  }}
}
```

Key rules:

- `event.flag` is your test-vs-live discriminator (`DEVELOPMENT` in the
  published samples).
- The **KWUID** in `configuration.entry` is the user identity. A missing or
  invalid one is rejected by the gateway with `BAD_KWUID`.
- `access-token` and `refresh-token` are present **only** on
  `SUBSCRIPTION_ORDER` and `USER_ASSIGNMENT`. This is how per-user tokens
  are minted for Marketplace integrations — there is no redirect flow.
  Store them against the KWUID the moment you receive them.
- The teams variant of `SUBSCRIPTION_ORDER` adds `roleId` and `orgId` to
  `configuration.entry`, a `company.externalId`, and an
  `order.customAttributes.customAttribute` named `external_company_role`.

## Step 1 — answer the order event

**Synchronous** (what most partners need — everything turns on at once,
billing starts immediately):

```
200 {"success": "true", "accountIdentifier": "unique-value-for-subscription-order"}
```

The `accountIdentifier` must be unique per subscription order and must map
back to a user on your side. KW uses it on every later event.

**Asynchronous** (you need onboarding, identity confirmation, or profile
completion first — no billing of any kind starts until you confirm):

```
202 {"success": "true"}
```

The subscription enters `PENDING`. Async must be enabled for you by KW;
otherwise you get `500 INTERNAL_SERVER_ERROR`.

**Rejection** on either mode:

```
409 {"success": "false", "errorCode": "USER_ALREADY_EXISTS", "message": "..."}
```

Valid `errorCode` values: `USER_ALREADY_EXISTS`, `USER_NOT_FOUND`,
`ACCOUNT_NOT_FOUND`, `MAX_USERS_REACHED`, `UNAUTHORIZED`,
`OPERATION_CANCELLED`, `CONFIGURATION_ERROR`, `INVALID_RESPONSE`,
`PENDING`, `FORBIDDEN`, `BINDING_NOT_FOUND`, `TRANSPORT_ERROR`,
`UNKNOWN_ERROR`.

## Step 2 — confirm or reject an async subscription

```
POST https://<ENVIRONMENT>/v1/subscription/async/confirm
Authorization: Bearer <TOKEN>
API-Key: <API_KEY>
```

Confirm: `{"success": "true", "accountIdentifier": "..."}` → `201 {"success": true, "kwuid": "1"}`

Reject: `{"success": "false", "errorCode": "USER_ALREADY_EXISTS", "message": "..."}` → `200 {"success": true, "kwuid": "1"}`

`400 BAD_REQUEST` means the event for that agent was not found or has
expired — do not leave a PENDING subscription hanging.

## Step 3 — handle change, cancel and notice

- `SUBSCRIPTION_CHANGE` — edition switch, seat change, metered-item change.
  Answer `200 {"success": "true"}`. Re-read `order.editionCode` and adjust
  entitlements.
- `SUBSCRIPTION_CANCEL` — answer `200 {"success": "true"}` and stop
  service; charges cease and the subscription leaves the account.
- `SUBSCRIPTION_NOTICE` — carries `notice.type` of `DEACTIVATED`,
  `REACTIVATED`, `CLOSED` or `UPCOMING_INVOICE`, plus the new
  `account.status` (`ACTIVE`, `SUSPENDED`, `FREE_TRIAL`,
  `FREE_TRIAL_EXPIRED`, `CANCELLED`).

State machine you must implement (from the lifecycle walkthrough):

- Trial expires with no upgrade → `SUBSCRIPTION_NOTICE(DEACTIVATED)` →
  `FREE_TRIAL_EXPIRED`; with a grace period, later
  `SUBSCRIPTION_NOTICE(CLOSED)` → `CANCELLED`.
- Trial cancelled before expiry → `SUBSCRIPTION_CANCEL` only, **no**
  notice event.
- Manual upgrade during trial → `SUBSCRIPTION_CHANGE` then
  `SUBSCRIPTION_NOTICE(REACTIVATED)`.
- Automatic upgrade at trial end → `SUBSCRIPTION_NOTICE(REACTIVATED)`
  only, **no** change event.
- Missed payment (default: three) → `SUBSCRIPTION_NOTICE(DEACTIVATED)` →
  `SUSPENDED`, suspension reason `PAYMENT_OVERDUE`. Paid →
  `SUBSCRIPTION_NOTICE(REACTIVATED)` → `ACTIVE`. Unpaid indefinitely →
  stays `SUSPENDED`.

Do not assume a change event always precedes a reactivation, or that a
cancel is always preceded by a notice — the walkthrough is explicit that
both vary by path.

## Step 4 — seats

`USER_ASSIGNMENT` adds a seat and delivers that user's own access and
refresh tokens; `USER_UNASSIGNMENT` removes the seat. Both carry
`payload.user` with `uuid`, `email` and names. Revoke your side's access on
unassignment.

## Step 5 — report metered usage

```
POST https://<ENVIRONMENT>/v1/subscription/billing/usage
Authorization: Bearer <TOKEN>
API-Key: <API_KEY>

{
  "account": { "accountIdentifier": "..." },
  "items": [ { "quantity": 1000, "unit": "MEGABYTE" }, { "quantity": 1, "unit": "USER" } ]
}
```

`200 {"result": {"message": "Bill usage API call succeeded.", "success": true}}`

The unit type, price and invoice description are pre-configured with KW —
you may only send units KW has set up. There are ~130 accepted unit types
including `USER`, `ACTIVE_USERS`, `API_CALLS`, `CONTACT`, `EMAIL`,
`TRANSACTION_FEE`, `PROPERTY`, `DOCUMENT`, `LICENSE`, `SESSION`.

Failures: `400 BAD_REQUEST` ("Invalid JSON payload"), `500 UNHANDLED_ERROR`.

## Reliability notes

KW documents no webhook signature, no retry policy, no delivery ordering
guarantee and no de-duplication key. Defend yourself:

- Treat `accountIdentifier` + `event.type` + KWUID as your idempotency key
  and make handlers replay-safe.
- Log every raw payload; KW support asks for KWUIDs, OrgIDs and the exact
  error message (PartnerTechSupport@KW.com, 24-48 business hours).
- If your endpoint fails, KW surfaces `INVALID_PROVISION_ENDPOINT` — check
  your own logs for the response your endpoint returned to the gateway.
