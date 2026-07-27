---
name: Track certificate compliance and utility readings in Arthur
description: Keep gas, electrical and EPC certificates current across an Arthur Online portfolio and record utility meter readings at unit and property level.
api: openapi/arthur-online-certificates-openapi.yml
also_uses:
  - openapi/arthur-online-utilities-openapi.yml
  - openapi/arthur-online-properties-openapi.yml
  - openapi/arthur-online-units-openapi.yml
  - asyncapi/arthur-online-webhooks.yml
operations:
  - listCertificates
  - viewCertificate
  - updateCertificate
  - deleteCertificate
  - addCertificateOnProperty
  - addCertificateOnUnit
  - addCertificateOnTenancy
  - listCertificatesOnProperty
  - listUtilitiesOnProperty
  - createUtilityOnProperty
  - createUtilityOnUnit
  - listUtilities
  - viewUtility
  - updateUtility
  - listReadings
  - createReading
  - viewReading
  - updateReading
generated: '2026-07-26'
method: generated
source: openapi/ derived from https://developer.arthuronline.co.uk/
---

# Compliance certificates and utility readings

UK lettings compliance is date-driven: a gas safety certificate, an electrical installation
condition report and an EPC all expire, and an expired certificate makes a tenancy non-compliant.
Arthur models this as certificate records with expiries plus webhook triggers that fire ahead of
the date.

## Before you start

- Every call: `Authorization: Bearer <token>` + `X-EntityID: <entity_id>`.
- Resolve certificate types against the Types API before writing; see
  `vocabulary/arthur-online-vocabulary.yml`.
- Dates are ISO 8601 `yyyy-MM-dd`.

## Certificates

1. **Survey what exists.** `listCertificates` across the entity, or `listCertificatesOnProperty` /
   `listCertificatesOnUnit` / `listCertificatesOnTenancy` for one record. Page with `page`/`limit`
   (max 100) and sort with `sort`/`direction`.
2. **File a new certificate.** `addCertificateOnProperty`, `addCertificateOnUnit` or
   `addCertificateOnTenancy` — attach it at the level the document actually covers. Store the PDF
   itself as an asset (`createAssetOnProperty`) and share it with the tenant.
3. **Correct or renew.** `viewCertificate` then `updateCertificate` with the new expiry.
   `deleteCertificate` only for records filed in error — deleting a certificate destroys the
   compliance audit trail.
4. **Do not poll for expiry.** Subscribe to the four Certificate triggers instead:
   `Certificate Added`, `Certificate Expires in 30 Days`, `Certificate Expires in 7 Days`,
   `Certificate Expired Today`. Details in `asyncapi/arthur-online-webhooks.yml`.

## Utilities

1. **Attach the account.** `createUtilityOnProperty` or `createUtilityOnUnit` for each supply.
   Read them back with `listUtilitiesOnProperty` / `listUtilitiesOnUnit` or entity-wide with
   `listUtilities`.
2. **Record readings.** `createReading` against the utility at check-in, check-out and at each
   meter read. `listReadings` for the history, `viewReading` / `updateReading` to correct one.
3. **React to readings.** The `UtilityReading Added` webhook trigger fires on every new reading —
   use it to drive a supplier notification rather than a nightly sweep.

## Rules

- Writes are not idempotent — a retried `createReading` files a duplicate reading and skews
  consumption. Read back with `listReadings` before retrying.
- Use `strict=true` on POST/PUT so an unknown certificate type or utility supplier name fails
  rather than being created as new reference data.
- Certificates attached at the wrong level (property vs unit) are the most common data-quality
  problem in a portfolio: a gas certificate covers a dwelling, an EPC covers the unit being let.
