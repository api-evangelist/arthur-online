---
name: List a property and onboard its units in Arthur
description: Create a property in Arthur Online, add its lettable units, attach compliance certificates and publish shared information, using the Arthur API v2.
api: openapi/arthur-online-properties-openapi.yml
also_uses:
  - openapi/arthur-online-units-openapi.yml
  - openapi/arthur-online-certificates-openapi.yml
  - openapi/arthur-online-types-openapi.yml
operations:
  - listEntities
  - listProperties
  - addProperty
  - viewProperty
  - listUnitsOnProperty
  - addUnit
  - addCertificateOnProperty
  - addGeneralInfoForProperty
  - listAssetsOnProperty
  - createAssetOnProperty
generated: '2026-07-26'
method: generated
source: openapi/ derived from https://developer.arthuronline.co.uk/
---

# Onboard a property and its units

Use this when a manager brings a new building into Arthur and it needs to be lettable: the
property record, the units beneath it, the compliance certificates, and the information
shared with owners, tenants and contractors.

## Before you start

- Base URL `https://api.arthuronline.co.uk/v2`. Every call needs `Authorization: Bearer <token>`
  and `X-EntityID: <entity_id>`; POST and PUT also need `Content-Type: application/json`.
- Call `listEntities` first and confirm which entity you are operating in. Ids are only
  meaningful inside their entity, and a wrong entity id returns 404, not 403.
- Add `strict=true` to every POST and PUT. Without it Arthur silently creates any unrecognised
  Simple-type value (for example a new `source`) as permanent reference data in the customer's account.
- Arthur has no idempotency key. If a write times out, **do not retry** — read back with
  `listProperties` (filter on address) and confirm before writing again.

## Steps

1. **Confirm the entity.** `listEntities` → pick the entity, and use its id in `X-EntityID` for
   everything that follows.
2. **Check the property does not already exist.** `listProperties` with the address filters
   (`full_address`, `postcode`, `city`) and `limit=100`. Page with `page` if `pagination.pageCount > 1`.
3. **Resolve reference values.** Any enumerated field (property type, area type, contract type)
   must come from the Types API — see `vocabulary/arthur-online-vocabulary.yml` for the 39
   endpoints. Never guess an enum value.
4. **Create the property.** `addProperty` with the supported fields for the operation. Keep the
   returned `data.id` — that is `property_id` everywhere below.
5. **Verify.** `viewProperty` with the new id and confirm the record reads back as expected.
6. **Add units.** For each lettable space, `addUnit` referencing the property. Then confirm with
   `listUnitsOnProperty`. Arthur models tenancies and viewings against **units**, not properties,
   so a property with no units cannot be let.
7. **Attach compliance certificates.** `addCertificateOnProperty` for gas safety, electrical and
   EPC records, with their expiry dates. Expiry drives the Certificate webhook triggers at 30 days,
   7 days and on the day — subscribe to those rather than polling.
8. **Publish shared information.** `addGeneralInfoForProperty` with `title`, `description`,
   `faq_type` and the `share_owner` / `share_tenant` / `share_contractor` flags for anything
   occupants need (bin days, alarm codes, cleaning schedule).
9. **Attach documents.** `createAssetOnProperty` with `file` (base64), `mime_type` and `file_name`,
   plus the share flags. Read back with `listAssetsOnProperty`.

## Rules

- Custom fields read as an array of `{name, api_name, value}` but write as an object keyed by
  `api_name`: `{"custom_fields": {"custom_field_1": "value"}}`.
- Dates are ISO 8601 (`yyyy-MM-dd`); date-times are `yyyy-MM-ddTHH:mm:ssZ`.
- Success bodies carry `{status, data, pagination}`. Errors carry `{status, error, message}` —
  see `errors/arthur-online-problem-types.yml`. A 401 with `expired_token` means refresh at
  `https://auth.arthuronline.co.uk/oauth/token` and retry the **read**, not a half-completed write.
- Budget: 5,000 requests per hour across the whole integration.
