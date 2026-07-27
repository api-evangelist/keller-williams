---
name: Authorize a KW partner application with OpenID Connect
description: Take a Keller Williams partner application through the DevHub OpenID Connect authorization-code flow — /authorize, consent, /token exchange, refresh and /revoke — and keep the two-day access token alive.
api: authentication/keller-williams-openid-configuration.json
operations:
  - GET https://partners.api.kw.com/idp/authorize
  - POST https://partners.api.kw.com/idp/token
  - POST https://partners.api.kw.com/v2/oauth2/partner/token
  - POST https://partners.api.kw.com/idp/revoke
generated: '2026-07-26'
method: generated
source: https://developer.kw.com/getting-started + https://partners.api.kw.com/idp/.well-known/openid-configuration
grounding: >-
  No OpenAPI exists for the KW identity surface. Every endpoint, parameter
  and response below is quoted from the DevHub Getting Started guide and
  the live OpenID Connect discovery document, both fetched 2026-07-26.
---

# Authorize a KW partner application with OpenID Connect

Keller Williams grants access to an individual KW user's Command data with
OpenID Connect on top of OAuth 2.0. The authorization server is
`https://partners.api.kw.com/idp` and its discovery document is public.

## Preconditions

- An approved DevHub app with **at least one product** selected.
- A valid callback URL registered on the app. It must be `https://`, and
  **only one redirect URI per application is supported**.
- The API Key (which is your `client_id`) and Secret from the app record.
- A way for the user to trigger the flow — a "Sign in with KW" button.

## Step 1 — `/authorize`

```
GET https://partners.api.kw.com/idp/authorize
  ?client_id=<API_KEY>
  &redirect_uri=<your registered callback>
  &response_type=code
  &scope=openid%20profile%20<your chosen scopes>
```

`response_type` must be exactly `code`. `openid` and `profile` are required
defaults on every call. Separate additional scopes with `%20`. PKCE is
supported (`code_challenge_methods_supported: ["S256"]`) — use it.

The scopes you may request are the ones selected on the DevHub app. The
eight documented products are:

| Product | Scope |
|---|---|
| Read Contact Data | `read_contact` |
| Create or Update Contact Data | `write_contact` |
| Read Command Contact Settings | `read_contact_setting` |
| Create Contact Custom Fields | `write_custom_field` |
| Read KW User Information | `read_user` |
| Read KW Organizational Information | `read_organization` |
| Read Task Information | `read_task` |
| Create or Edit Tasks | `write_task` |

The authorization server advertises 40 scopes in total (see
`scopes/keller-williams-scopes.yml`); the other 32 are granted per
partnership. Note the divergence: the docs name `read_user`, but the
discovery document advertises `read_all_user` and `read_all_users_lw`
instead — confirm with KW which one your app was provisioned with.

## Step 2 — login and consent

The user is redirected to the Command login screen, signs in with their KW
credentials, and is shown a consent screen listing exactly the scopes in
your `scope` parameter. Request the minimum — every extra scope is visible
to the agent at consent time.

## Step 3 — the callback

KW redirects to your registered URI with the authorization code:

```
https://example.com?code=<AUTHORIZATION_CODE>&iss=<issuer>
```

## Step 4 — `/token` exchange

```bash
curl -X POST https://partners.api.kw.com/idp/token \
  -H "Authorization: Basic <base64(API_KEY:SECRET)>" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code" \
  -d "redirect_uri=<same callback>" \
  -d "code=<AUTHORIZATION_CODE>"
```

Client authentication may be `client_secret_basic`, `client_secret_post` or
`none`. Response:

```json
{ "access_token": "...", "refresh_token": "...", "token_type": "Bearer", "expires_in": "..." }
```

## Step 5 — call the API

Both headers, always:

```
Authorization: Bearer <ACCESS_TOKEN>
api-key: <API_KEY>
```

HTTPS is mandatory; plain HTTP fails.

## Step 6 — refresh before day two

Access tokens live **2 days**; refresh tokens live **365 days** and are
**single-use** — each refresh returns a new refresh token and invalidates
the old one. Persist the new one atomically or you will lock the user out.

```bash
curl -X POST https://partners.api.kw.com/v2/oauth2/partner/token \
  -H "Authorization: Basic <base64(API_KEY:SECRET)>" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=refresh_token" \
  -d "account_identifier=<user account identifier>" \
  -d "refresh_token=<REFRESH_TOKEN>"
```

The response includes `access_token`, `refresh_token`,
`refresh_token_expires_in`, `refresh_token_status`, `refresh_count`,
`old_access_token_life_time`, `expires_in`, `issued_at` and `status`.

If a refresh token expires you cannot self-issue a new one — contact KW.

## Step 7 — logout / `/revoke`

Give the user a way to disconnect. Logging out retracts the permissions
they granted:

```bash
curl --location 'https://partners.api.kw.com/idp/revoke' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Basic <base64(API_KEY:SECRET)>' \
  --data '{"token":"<ACCESS_TOKEN>"}'
```

Returns `200 OK` with an empty body.

## Gotchas

- The discovery document lives at
  `https://partners.api.kw.com/idp/.well-known/openid-configuration`, **not**
  at the issuer root. A standards-compliant client that resolves the issuer
  (`https://partners.api.kw.com`) to `/.well-known/openid-configuration` gets
  a 404. Configure the URL explicitly.
- The advertised `jwks_uri` (`/idp/keys`) returns 404 to an anonymous
  client, so you cannot verify ID-token signatures until KW exposes it to
  your partner credentials. Ask KW for the keys rather than skipping
  verification.
- Marketplace integrations do **not** use this redirect flow for
  per-user tokens — those arrive inside the `SUBSCRIPTION_ORDER` and
  `USER_ASSIGNMENT` webhook payloads. See
  `keller-williams-handle-marketplace-subscriptions.md`.
