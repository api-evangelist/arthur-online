---
name: Authorize an Arthur integration and keep it in sync
description: Complete the Arthur Online OAuth 2.0 authorization code flow, discover the entity, subscribe to webhooks, and run a safe incremental sync against the Arthur API v2.
api: openapi/arthur-online-entities-openapi.yml
also_uses:
  - authentication/arthur-online-authentication.yml
  - conventions/arthur-online-conventions.yml
  - asyncapi/arthur-online-webhooks.yml
  - rate-limits/arthur-online-rate-limits.yml
operations:
  - listEntities
  - viewEntity
  - listProperties
  - listUnits
  - listTenancies
  - listTasks
  - listTransactions
generated: '2026-07-26'
method: generated
source: >-
  https://developer.arthuronline.co.uk/ (Getting Started > Create Application, Authentication,
  Refresh Token) plus openapi/ derived from the same collection.
---

# Authorize and sync

The first thing any Arthur integration has to get right: getting a token at all, knowing which
entity it addresses, and staying inside a 5,000 request per hour budget.

## Getting access (out of band)

Arthur credentials are not self-serve. Before any code runs:

1. Be inside a paying Arthur account.
2. Ask Arthur support for API access — raise a ticket at https://support.arthuronline.co.uk/support/home.
3. In Arthur, go to Settings > Your Account > OAuth Applications > Add Application and supply a
   name, description, website and an **HTTPS** callback URL. Arthur issues a client id and secret.

Note the commercial terms: broken endpoints are fixed free of charge, but new endpoints are quoted
at GBP 150 + VAT per hour (see `lifecycle/arthur-online-lifecycle.yml`).

## The OAuth flow

1. **Authorize.** Send the user to
   `https://auth.arthuronline.co.uk/oauth/authorize?client_id=<id>&redirect_uri=<uri>`.
   Arthur redirects back with `code`. The code expires in **15 minutes**.
2. **Exchange.** `POST https://auth.arthuronline.co.uk/oauth/token` as
   `application/x-www-form-urlencoded` with `grant_type=authorization_code`, `code`, `client_id`,
   `client_secret` and `redirect_uri`. The access token is valid **14 days**.
3. **Refresh.** `POST` the same endpoint with `grant_type=refresh_token`, `refresh_token`,
   `client_id`, `client_secret`. Refresh tokens are valid **21 days** — an integration idle for
   three weeks has to be re-authorized by the user, so refresh proactively.
4. **Handle expiry.** A `401` with `{"error": "expired_token", "message": "This token has expired."}`
   means refresh and retry. Only retry the read; never blind-retry a write.

There are no scopes. A token carries whatever the authorizing Arthur user can see, so the real
access control is the Manager account you ask the customer to create — Enterprise accounts can set
per-Manager permissions to narrow what the integration can reach.

## Discover the entity

Every call needs `X-EntityID`. Call `listEntities` after authorization, store the entity id
alongside the token, and confirm with `viewEntity`. Ids in Arthur are only meaningful inside their
entity — the same numeric id in another entity is a different record, and a mismatch returns 404.

## Sync strategy

- **Events first.** Subscribe on the Arthur webhook page to the triggers you care about (125
  available across 30 models). There is no API to manage subscriptions — this is a UI step in the
  customer's account.
- **Verify, do not trust.** Webhook deliveries are form-encoded, unsigned, and identify themselves
  only by a `CakePHP` user agent. Treat a delivery as a hint and re-read the record through the API
  before acting on it.
- **Backfill by page.** `listProperties`, `listUnits`, `listTenancies`, `listTasks`,
  `listTransactions` with `limit=100`, walking `pagination.pageCount`. Where an endpoint accepts
  `modified`, `modifiedFrom`/`modifiedTo` or `date_from`/`date_to`, use them for the incremental pass.
- **Budget.** 5,000 requests/hour, platform-wide. A 100-per-page full sweep of a 10,000-unit
  portfolio is ~100 calls; a per-record polling loop is not affordable. Ask Arthur support to raise
  the limit before designing around a higher one.
- **No idempotency.** There is no request key and no replay protection. Make writes conditional on a
  read, and log every write with the returned id so a crashed run can reconcile instead of repeat.
