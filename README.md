# Arthur Online (arthur-online)

Arthur Online is a London-headquartered UK property management software platform, founded in 2008 and acquired by Aareon in January 2021, that gives letting agents, self-managing landlords, block and social housing managers, and student accommodation operators a cloud system of record for properties, units, tenancies, tenants, applicants, viewings, maintenance work orders, certificates, utilities and rental financials, with companion mobile apps for managers, tenants, contractors and owners. It sits in the middle of the UK residential lettings value chain: it is the agency-side operational system that pushes stock out to the Rightmove and Zoopla portals and pulls accounting into Xero and QuickBooks, rather than a listings marketplace or a data cooperative. Its API posture is unusually open for the sector but is not open data: the full Arthur API v2 reference is published without a login as a public Postman collection at developer.arthuronline.co.uk covering 324 documented requests across 16 resource areas, yet every call is tenant-scoped OAuth 2.0 Authorization Code plus an X-EntityID header, credentials are issued only from inside a paying Arthur account after contacting Arthur support, and no data is readable anonymously. There is no RESO posture at all — the United Kingdom has no MLS, no NAR, and no RESO Data Dictionary or Web API certification regime, so listings interoperability here is portal-to-CRM feeds rather than a certified machine-readable standard.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/arthur-online/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/arthur-online/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- Property Management
- PropTech
- Rentals
- Lettings
- Tenancy
- Maintenance
- Property Listings
- Social Housing
- Student Housing
- Block Management

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### Arthur Properties API

Property records and everything hung off a property in Arthur - assets, certificates, conversations, general information, notes, units, tenancies, tasks, work orders, utilities, transactions and tags. 20 documented paths across GET, POST, PUT and DELETE.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Properties
- Real Estate
- Property Records

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Units API

Individual lettable units beneath a property, plus the unit-level assets, certificates, conversations, notes, viewings, tenancies, tasks, work orders, utilities, transactions and tags. 22 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Units
- Real Estate
- Inventory

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Tenancies API

Tenancy agreements over a unit, including tenants on the tenancy, deposit registration, recurring charges, certificates, tasks, work orders, conversations, transactions, notes and tags. 21 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Tenancies
- Leases
- Deposits

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Tenants API

Tenant people records - list, view, create and update tenants, and invite a tenant into the Arthur tenant portal. 3 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Tenants
- People
- Occupancy

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Applicants API

Prospective renters moving through the lettings funnel, including applicant status, credit checks, managers, assets, conversations, notes, tasks and tags. 15 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Applicants
- Leasing
- Credit Checks

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Viewings API

Property and unit viewing appointments, the applicants attached to each viewing, assigned managers, conversations, notes and tags. 15 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Viewings
- Leasing
- Scheduling

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Maintenance API

The largest surface in the API - tasks, subtasks, work orders, quotes and the contractor workflow around them. 45 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Maintenance
- Work Orders
- Contractors

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Financials API

Rental financials - invoices, transactions, transaction payoff and recurring charges. Read and update only; no create operations are documented. 6 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Financials
- Payments
- Accounting

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Assets API

Files and documents stored in Arthur and shared with owners, tenants and contractors. 2 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Assets
- Documents
- Files

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Utilities API

Utility accounts attached to properties and units, and the meter readings recorded against them. 4 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Utilities
- Meter Readings

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Certificates API

Compliance certificates - gas safety, electrical, EPC and similar - with expiry tracking that also drives webhook events at 30 days, 7 days and on the day. 2 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Certificates
- Compliance
- Safety

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Entities API

Arthur entities - the account boundary that every API call must name in the mandatory X-EntityID request header. 2 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Entities
- Accounts
- Multi-tenancy

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Conversations API

Threaded conversations and messages between managers, tenants, owners and contractors, with attached assets. 4 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Conversations
- Messaging
- Communications

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Tags API

Cross-cutting tags applied to properties, units, tenancies, applicants, viewings and notes, with tag and untag operations on each resource. 2 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Tags
- Classification

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Notes API

Free-text notes recorded against any Arthur resource, with their own tagging operations. 5 documented paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Notes
- Annotations

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

### Arthur Types API

Read-only reference vocabularies backing every enumerated field in Arthur - access detail types, applicant statuses and types, area types, asset types, certificate types, citizen types, contract types and 32 more. 39 documented GET paths.

- **Human URL:** [https://developer.arthuronline.co.uk/](https://developer.arthuronline.co.uk/)
- **Base URL:** `https://api.arthuronline.co.uk/v2`

#### Tags

- Types
- Reference Data
- Vocabulary

#### Properties

- [Documentation](https://developer.arthuronline.co.uk/)
- [API Reference](https://developer.arthuronline.co.uk/)
- [Postman Collection](collections/arthur-online.postman_collection.json) — [Postman Collection 2.0](https://schema.getpostman.com/json/collection/v2.0.0/collection.json)
- [Authentication](https://auth.arthuronline.co.uk/oauth/token)
- [Webhooks](https://developer.arthuronline.co.uk/)

## Common Properties

- [Website](https://www.arthuronline.co.uk/)
- [Portal](https://developer.arthuronline.co.uk/)
- [Documentation](https://developer.arthuronline.co.uk/)
- [APIReference](https://developer.arthuronline.co.uk/)
- [PostmanCollection](collections/arthur-online.postman_collection.json)
- [Signup](https://www.arthuronline.co.uk/connect/arthur-api)
- [Support](https://support.arthuronline.co.uk/)
- [Blog](https://www.arthuronline.co.uk/blog/)
- [Integrations](https://www.arthuronline.co.uk/connect/)
- [PrivacyPolicy](https://www.arthuronline.co.uk/privacy-policy/)
- [TermsOfService](https://www.arthuronline.co.uk/terms-and-conditions/)

## Maintainers

- Kin Lane — kin@apievangelist.com
