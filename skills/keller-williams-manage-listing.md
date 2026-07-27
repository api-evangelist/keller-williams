---
name: Create, update and delete a KW Worldwide listing
description: Write path for the KW Worldwide Listings Search API — resolve the lookup table, create a listing with the eleven required fields, patch it by list_uuid, and delete it, with the write-safety caveats KW does not document.
api: openapi/keller-williams-listings-search-openapi.json
operations:
  - listings-read-by-table
  - listings-create
  - listings-details-update
  - listings-details-delete
  - listings-read
generated: '2026-07-26'
method: generated
source: openapi/keller-williams-listings-search-openapi.json + https://developer.kw.com/docs/listingskww/1/overview
---

# Create, update and delete a KW Worldwide listing

## Preconditions

An approved DevHub app with the write scopes selected — `listings_post`,
`listings_details_patch`, `listings_details_delete` — plus the paired
credentials (`api-key` header and an `Authorization` header). Do the whole
flow against `https://sandbox.partners.api.kw.com/v2` first.

## Step 1 — resolve ids (`listings-read-by-table`)

`GET /listings/lookup-table`

Creates are rejected unless the numeric ids match the KW vocabulary. Pull
`prop_type`, `prop_subtype`, `list_type`, `list_status`, `list_category`,
`currency`, `country` and `state_province` and map your source values to
`{id, value}` pairs before you build the body.

## Step 2 — create (`listings-create`)

`POST /listings`

Required by the `ListingsCreate` schema — all eleven, or the request is
rejected with `400 BAD_REQUEST` and a `"...\" is required"` message:

- `mls_id`
- `source_system_name`
- `mls_number`
- `current_list_price`
- `list_status_id`
- `list_category`
- `list_category_id`
- `prop_type`
- `prop_type_id`
- `prop_subtype`
- `prop_subtype_id`

The spec's own note: avoid special characters such as `%` in
`list_desc` — they break the listing.

The `201` response is `{"success": true, "data": {...}}` and carries the
`list_uuid` you will need for every subsequent call. **Persist it
immediately.**

### Write safety — read this before you retry anything

Keller Williams documents no idempotency key. There is no
`Idempotency-Key` header or parameter on this operation. A retried or
duplicated POST creates a second listing. Guard it yourself:

1. Generate your own client-side dedupe key and keep a local
   `key -> list_uuid` record before you send.
2. On a timeout or a 5xx, do **not** blind-retry. Re-query
   `GET /listings?filter[list_key][is]=<mls_id>-<mls_number>` first and only
   POST if nothing came back.

## Step 3 — update (`listings-details-update`)

`PATCH /listings/{list_uuid}`

Send only the fields you are changing — the published examples show both a
single-field body (`{"list_desc": "..."}`) and a partial multi-field body.
There is no ETag or `If-Match`, so there is no optimistic concurrency
control: last write wins. If two systems can edit the same listing,
serialise your writes.

Respect the `*_lock` booleans on the read shape (`list_desc_lock`,
`list_status_lock`, `prop_type_lock`, `photos_lock`, `open_houses_lock`,
`virtual_tours_lock`, `contract_expiry_dt_lock`, `kw_expiry_dt_lock`). A
locked field is pinned against overwrite by an upstream feed; changing it
from a partner integration is usually a mistake.

## Step 4 — delete (`listings-details-delete`)

`DELETE /listings/{list_uuid}`

Destructive and not documented as reversible. The `200` response echoes the
deleted listing. Only KW Worldwide listings are editable — attempting to
edit or delete a non-KWW listing returns:

```json
{ "success": "false", "errorCode": "FORBIDDEN", "message": "Listing not kww listing, cannot edit" }
```

Check `is_kww_listing` on the read shape before you attempt any write.

## Error handling

- `400 BAD_REQUEST` — missing required field, wrong type, disallowed property
- `401 BAD_TOKEN_AUTHENTICATION` / `BAD_API_KEY`
- `403 FORBIDDEN` — non-KWW listing, or no permission to delete this listing
- `500 INTERNAL_SERVER_ERROR`, `502 BAD_GATEWAY`

The spec declares no `404` on `/listings/{list_uuid}` for either DELETE or
PATCH, and no `429` anywhere — handle both regardless.

## Verify

Re-read with `listings-read` filtered on `list_uuid` (or `list_key`) and
confirm `kwls_status`, `list_status` and `is_deleted` are what you expect.
Listing changes are indexed asynchronously; allow a few seconds.
