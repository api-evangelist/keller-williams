---
name: Search KW Worldwide listings
description: Search Keller Williams Listing Service (KWLS) listings scoped to a KW Worldwide region, using the filter/sort/pagination grammar KW publishes, and resolve the numeric code fields against the lookup table.
api: openapi/keller-williams-listings-search-openapi.json
operations:
  - listings-read
  - listings-read-by-table
  - listings-region-details-read
generated: '2026-07-26'
method: generated
source: openapi/keller-williams-listings-search-openapi.json + https://developer.kw.com/filtering-and-sorting
---

# Search KW Worldwide listings

## Before you start

You cannot call this API anonymously. You need an approved DevHub application
(apply at https://share.hsforms.com/2JQHe7zfKRLSo_cXKjy-5nwbg45/), a signed KW
API License Agreement, and the API Key + Secret issued on the app record.

Base URLs:

- Production `https://partners.api.kw.com/v2`
- Sandbox `https://sandbox.partners.api.kw.com/v2`

Every request carries **two** headers:

```
api-key: <API_KEY>
Authorization: Basic <base64(API_KEY:API_SECRET)>
```

Use `Authorization: Bearer <ACCESS_TOKEN>` instead when acting on an
individual KW user's data (see `keller-williams-authorize-partner-app.md`).
Relevant scopes: `listings_get`, `read_all_listings`, `read_user_listings`.

## Step 1 — resolve the controlled vocabularies first (`listings-read-by-table`)

`GET /listings/lookup-table`

Listings carry paired name/id fields (`prop_type` + `prop_type_id`,
`list_status` + `list_status_id`, `list_category` + `list_category_id`,
`list_type` + `list_type_id`). Every numeric id resolves here. The response is
an Elasticsearch envelope whose `hits.hits[]` entries are discriminated by
`_id` — one per table: `prop_type`, `prop_subtype`, `list_type`,
`list_status`, `list_category`, `kwls_status`, `country`, `state_province`,
`currency`, `unit`, `flooring`, `roof_type`, `construction_material`,
`parking_feature`, `arch_style`, `appliance`, `expense_type`, `frequency`,
`oh_status`, `special_condition`, `syndicate`.

Cache this. It changes rarely and you will need it for every write.

## Step 2 — search (`listings-read`)

`GET /listings`

Filter with the bracket grammar:

```
filter[:attribute_name][:operator]=:value
```

Operators: `is`, `like`, `gt`, `gte`, `le`, `lte`, `between`, `radius`,
`coordinate`, `in`. Prefix `!` to negate (`!is`, `!like`).

```
GET /listings?filter[list_kw_uid][is]=<KWUID>
GET /listings?filter[list_dt][gte]=2021-08-04
```

Sort with `sort=`; prefix `-` for descending; comma-separate for multi-sort:

```
GET /listings?sort=list_dt,-current_list_price
```

Geo sorting requires a reference point:

```
GET /listings?sort=geo_location&location[lat]=30.263699&location[lon]=-99.785734
```

Paginate one of three ways — pick one and stay consistent:

- JSON:API style: `page[offset]` (default 1) + `page[limit]` (default 10, max 100)
- flat: `offset` + `limit` (same defaults and ceiling)
- scroll cursor for deep result sets: `scroll=1m` then `scroll_id=<id>` on each
  subsequent call

## Step 3 — read the response shape

The body is a raw Elasticsearch envelope. Listings live at
`hits.hits[]._source` (117 fields); the count is `hits.total.value`, and
`hits.total.relation` tells you whether that count is exact. `took`,
`timed_out` and `_shards` are engine metadata, not part of your domain.

Fields worth knowing on `_source`:

- identity: `list_uuid` (24-hex, the id you PATCH/DELETE against), `list_id`,
  `list_key` (`<mls_id>-<mls_number>`)
- people: `list_kw_uid`, `co_list_kw_uid`, `sell_kw_uid` (all KWUIDs)
- placement: `kww_region`, `market_center`, `mls_id`, `mls_name`, `mls_number`
- money: `current_list_price`, `original_list_price`, `close_price`,
  `lease_price`, `currency_code`
- state: `list_status`, `kwls_status`, `is_kww_listing`, `is_deleted`
- localisation: `list_desc_en` and the `*_transkey` fields carry translation
  keys for the KW Worldwide multi-locale surface
- `*_lock` booleans pin a field against overwrite by an upstream feed

## Step 4 — region scoping (`listings-region-details-read`)

`GET /listings/region/{regionId}` returns the KW Worldwide region detail.
Listings are scoped by region — if you are building a regional portal,
resolve the region before you page listings.

## Errors

All failures return the flat envelope, never RFC 9457 problem+json:

```json
{ "success": "false", "errorCode": "BAD_TOKEN_AUTHENTICATION", "message": "Token is not valid or missing" }
```

Handle at minimum:

- `400 BAD_REQUEST` — invalid filter query parameter, wrong field type
- `401 BAD_TOKEN_AUTHENTICATION` / `401 BAD_API_KEY` — credential problem
- `429 TOO_MANY_REQUESTS` ("Quota exceeded") — back off; KW publishes no
  RateLimit headers and no numeric quota, so use exponential backoff and
  contact KW for an increase
- `500 INTERNAL_SERVER_ERROR`, `502 BAD_GATEWAY` — retry with backoff

Note that `listings-read-by-table` and `listings-region-details-read` do not
declare a 401 in the spec even though the Authorization header is required —
handle it anyway.

Full registry: `errors/keller-williams-error-codes.yml`.

## Do not

- Do not present KWLS data as MLS data. The specification states the service
  "is not a substitution for or tied to any Multiple Listings Services
  database records," and Keller Williams holds no RESO certification.
- Do not expect RESO Data Dictionary field names. The vocabulary is
  proprietary KWLS.
