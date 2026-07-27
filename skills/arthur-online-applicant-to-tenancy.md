---
name: Take an applicant from enquiry to tenancy in Arthur
description: Run the UK lettings funnel in Arthur Online - create an applicant, book a viewing, record the offer, convert it into a tenancy and invite the tenant.
api: openapi/arthur-online-applicants-openapi.yml
also_uses:
  - openapi/arthur-online-viewings-openapi.yml
  - openapi/arthur-online-tenancies-openapi.yml
  - openapi/arthur-online-tenants-openapi.yml
  - openapi/arthur-online-units-openapi.yml
operations:
  - listApplicants
  - addApplicant
  - viewApplicant
  - updateApplicantStatus
  - listCreditChecksOnApplicant
  - addViewingOnApplicant
  - addViewingOnUnit
  - listViewingsOnApplicant
  - updateViewingStatus
  - submitOfferOnViewing
  - addTenancyOnViewing
  - addTenantOnTenancy
  - inviteTenant
  - updateDepositOnTenancy
generated: '2026-07-26'
method: generated
source: openapi/ derived from https://developer.arthuronline.co.uk/
---

# Applicant to tenancy

The marquee flow for a UK letting agent: an enquiry becomes an applicant, the applicant views a
unit, an offer is entered and accepted, and the viewing becomes a tenancy with tenants and a
registered deposit.

## Before you start

- Every call: `Authorization: Bearer <token>` + `X-EntityID: <entity_id>`; JSON content type on writes.
- Set `strict=true` on every POST/PUT. Applicant `source` is a Simple type — without `strict` a
  typo creates a new permanent source value in the customer's account.
- Resolve `applicant_type`, `applicant_status`, `citizen_type`, `student_status` and `visa_type`
  against the Types API (`applicantTypes`, `applicantStatuses`, `citizenTypes`, `studentStatuses`,
  `visaTypes`) before writing. Never invent an enum member.

## Steps

1. **De-duplicate.** `listApplicants` with `_q` or `email` before creating anything. Applicants
   are people; duplicates pollute the funnel and the reporting.
2. **Create the applicant.** `addApplicant`. Keep `data.id` as `applicant_id`.
3. **Move the applicant through the funnel.** `updateApplicantStatus` (`PUT /applicant/{applicant_id}/status`
   — note the singular path segment, which is how Arthur publishes it) whenever the stage changes.
   Read the current record with `viewApplicant` first so you do not overwrite a manager's change.
4. **Check referencing.** `listCreditChecksOnApplicant` returns any credit check on file. Arthur
   exposes no operation to *initiate* a credit check — that is a UI action.
5. **Book the viewing.** Either `addViewingOnApplicant` (from the applicant) or `addViewingOnUnit`
   (from the unit). Confirm with `listViewingsOnApplicant`. Viewings hang off units, not properties.
6. **Track the appointment.** `updateViewingStatus` for confirmed / attended / cancelled. The
   Viewing webhook triggers (`viewing-add-manager`, `viewing-cancelled`, `viewing-offer`,
   `viewing-applicant-add`) are the event equivalents — subscribe rather than poll.
7. **Record the offer.** `submitOfferOnViewing` with the offer amount and frequency. Sample payloads
   in `examples/webhooks/arthur-online-viewing-offer.json` show the shape Arthur emits back.
8. **Convert to a tenancy.** `addTenancyOnViewing` creates the tenancy from the accepted viewing.
   Keep `data.id` as `tenancy_id`.
9. **Add the tenants.** `addTenantOnTenancy` for each person on the agreement (or `addTenant` then
   attach). Confirm with `listTenantsOnTenancy`.
10. **Register the deposit.** `registerDepositOnTenancy` reads the current registration;
    `updateDepositOnTenancy` writes it. UK deposit protection is statutory — do not skip this step
    or let an agent mark a tenancy current without it.
11. **Invite the tenant to the portal.** `inviteTenant` sends the tenant-portal invitation.

## Rules

- Writes are not idempotent. A retried `addTenancyOnViewing` creates a second tenancy on the same
  unit. On a timeout, read back (`listTenanciesOnUnit`) before retrying.
- The tenancy status vocabulary (prospective, approved, current, periodic, ending, past, rejected)
  is enforced by Arthur and mirrored by 21 Tenancy webhook triggers — see
  `asyncapi/arthur-online-webhooks.yml`.
- Money fields are Float, rounded to two decimals; dates are ISO 8601.
- A 400 usually means an enum value that does not exist in the matching Types endpoint, or a
  missing required header. See `errors/arthur-online-problem-types.yml`.
