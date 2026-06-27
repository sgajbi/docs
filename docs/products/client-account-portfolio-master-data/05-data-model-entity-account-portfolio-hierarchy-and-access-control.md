# 05 - Data Model, Entity/Account/Portfolio Hierarchy and Access Control

## 1. Overview

This file provides a canonical data model for client/account/portfolio master data. The model is intentionally platform-oriented and can support core banking, portfolio analytics, advisory, DPM, reporting, lending, CRM, trust/family-office structures and migration use cases.

## 2. Canonical model overview

```text
Party
  Party Identifier
  Party Profile
  Party Relationship
  Household / Client Group
  Coverage Assignment

Account
  Account Role
  Account Restriction
  Account Tax Profile
  Account Service Profile

Portfolio
  Portfolio Membership
  Mandate
  Benchmark / Model Portfolio
  Portfolio Restriction

Access Control
  User / Role / Entitlement
  Client Visibility Scope
  Order Authority Scope

Events and Lineage
  Lifecycle Event
  Source Mapping
  Snapshot Version
```

## 3. Core entities

### 3.1 `party`

| Field | Notes |
|---|---|
| party_id | Canonical ID. |
| party_type | Individual, company, trust, foundation, joint, internal, external. |
| legal_name | Official name. |
| display_name | Preferred reporting/display name. |
| status | Active, inactive, deceased, dissolved, restricted, duplicate. |
| country_of_residence | Residence, not necessarily tax residence. |
| nationality / incorporation_country | Depending on party type. |
| date_of_birth / incorporation_date | Identity and KYC. |
| risk_rating | AML/KYC risk rating. |
| created_at / updated_at | Technical audit. |

### 3.2 `party_identifier`

| Field | Notes |
|---|---|
| identifier_id | Unique record. |
| party_id | Canonical party. |
| identifier_type | CIF, BR, passport, NRIC, UEN, LEI, tax ID, source ID. |
| identifier_value | Mask/encrypt sensitive identifiers. |
| source_system | Where identifier came from. |
| verified_flag | Verified/unverified. |
| effective_from / to | Historical validity. |

### 3.3 `party_relationship`

| Field | Notes |
|---|---|
| relationship_id | Unique relationship. |
| from_party_id | Actor/source. |
| to_party_id | Subject/target. |
| relationship_type | Beneficial owner, director, trustee, beneficiary, signer, RM. |
| ownership_pct | Nullable. |
| authority_level | View, instruct, approve, discretionary, all-to-sign. |
| verification_status | Verified, pending, expired, exception. |
| effective_from / to | Historical validity. |
| source_system | Source owner. |

### 3.4 `household`

| Field | Notes |
|---|---|
| household_id | Client group ID. |
| household_type | Family, relationship group, reporting group, credit group. |
| household_name | Display name. |
| lead_party_id | Principal/client lead, if applicable. |
| status | Active, inactive, archived. |
| reporting_currency | Optional default. |

### 3.5 `household_membership`

| Field | Notes |
|---|---|
| household_id | Group. |
| party_id | Member. |
| role | Principal, spouse, child, trust, company, beneficiary. |
| include_in_reporting | Yes/no. |
| include_in_rm_view | Yes/no. |
| include_in_risk | Yes/no. |
| effective_from / to | Historical validity. |

### 3.6 `account`

| Field | Notes |
|---|---|
| account_id | Canonical account. |
| source_account_id | Source system ID. |
| account_type | Cash, custody, managed, loan, collateral, margin, policy. |
| booking_entity | Bank/legal entity. |
| booking_centre | Jurisdiction/booking location. |
| base_currency | Account base currency. |
| account_status | Active, blocked, closing, closed. |
| opening_date / closing_date | Lifecycle. |
| service_model | Advisory, DPM, execution-only, EAM. |
| tax_profile_id | Tax reporting profile. |
| statement_profile_id | Statement preferences. |

### 3.7 `account_role`

| Field | Notes |
|---|---|
| account_id | Account. |
| party_id | Party. |
| account_role | Legal owner, joint holder, signer, borrower, pledgor, trustee, beneficiary. |
| ownership_pct | Nullable. |
| signing_rule | Any one, all, specific combination. |
| authority_status | Active, suspended, revoked. |
| effective_from / to | Historical validity. |

### 3.8 `portfolio`

| Field | Notes |
|---|---|
| portfolio_id | Canonical portfolio. |
| portfolio_type | Account, consolidated, DPM, advisory, sleeve, collateral, proposal. |
| owner_party_id | Primary client/entity. |
| reporting_currency | Portfolio currency. |
| inception_date | Performance start. |
| status | Active, closed, archived, simulation. |
| benchmark_id | Optional. |
| mandate_id | Optional. |
| model_portfolio_id | Optional. |

### 3.9 `portfolio_membership`

| Field | Notes |
|---|---|
| portfolio_id | Portfolio. |
| account_id | Included account. |
| inclusion_type | Full, asset-only, cash-only, loan-only, collateral-only, sleeve. |
| allocation_pct | Optional for split/sleeve. |
| exclusion_rule_id | Optional. |
| effective_from / to | Historical validity. |

### 3.10 `mandate`

| Field | Notes |
|---|---|
| mandate_id | Mandate. |
| mandate_type | Advisory, DPM, execution-only, restricted. |
| portfolio_id | Scope. |
| risk_profile | Mandate risk. |
| investment_objective | Objective. |
| authority_model | Client approval, discretionary, committee. |
| permitted_products | Rule reference. |
| restrictions | Rule reference. |
| benchmark_id | Optional. |
| model_portfolio_id | Optional. |
| effective_from / to | History. |

## 4. Access-control model

Access control must answer:

- Who can view the client/account/portfolio?
- Who can place orders?
- Who can approve orders?
- Who can generate statements?
- Who can see consolidated household data?
- Who can view sensitive KYC/tax documents?
- Which access depends on RM coverage, team, booking centre, entity or delegation?

### 4.1 Role-based + relationship-based access

Use both:

| Access type | Example |
|---|---|
| Role-based access | RM, advisor, PM, operations, compliance. |
| Attribute-based access | Booking centre, team, jurisdiction, product type. |
| Relationship-based access | Assigned RM can view client. |
| Delegated access | EAM or family office user can view specific accounts. |
| Document-level access | KYC/tax documents restricted to authorized users. |

### 4.2 Entitlement objects

| Object | Purpose |
|---|---|
| user | Internal/external user. |
| role | RM, assistant, PM, compliance, client, EAM. |
| entitlement | View, instruct, approve, administer, report. |
| scope | Party, household, account, portfolio, booking centre. |
| delegation | Temporary or permanent delegated access. |
| access_event | Grant/revoke/audit event. |

## 5. Snapshot and lineage design

Client/account data used in reports should be snapshotted or reproducible.

| Snapshot | Purpose |
|---|---|
| Client snapshot | Name, address, tax status, suitability as of report date. |
| Account snapshot | Status, owner, booking centre, restrictions. |
| Portfolio snapshot | Membership and reporting currency. |
| Mandate snapshot | Rules active during report period. |
| Household snapshot | Consolidation membership at report cut. |
| Access snapshot | Who had access at event/report time. |

## 6. API design guidance

### 6.1 Party API

```http
GET /parties/{partyId}
GET /parties/{partyId}/relationships?asOf=2026-06-30
GET /parties/{partyId}/identifiers
GET /parties/{partyId}/profiles/suitability
GET /parties/{partyId}/profiles/tax
```

### 6.2 Account API

```http
GET /accounts/{accountId}
GET /accounts/{accountId}/roles?asOf=2026-06-30
GET /accounts/{accountId}/restrictions
GET /accounts/{accountId}/portfolios
```

### 6.3 Portfolio API

```http
GET /portfolios/{portfolioId}
GET /portfolios/{portfolioId}/memberships?asOf=2026-06-30
GET /portfolios/{portfolioId}/mandate?asOf=2026-06-30
GET /portfolios/{portfolioId}/reporting-context
```

### 6.4 Household API

```http
GET /households/{householdId}
GET /households/{householdId}/members?asOf=2026-06-30
GET /households/{householdId}/consolidated-portfolios
```

## 7. Event topics

Potential events:

```text
party.created
party.updated
party.merged
party.relationship.updated
household.membership.changed
account.opened
account.status.changed
account.role.changed
portfolio.created
portfolio.membership.changed
mandate.updated
suitability.profile.updated
tax.profile.updated
coverage.assignment.changed
access.granted
access.revoked
```

## 8. Data protection considerations

Sensitive fields such as ID documents, tax identifiers, contact details, KYC documents and beneficial-owner information require masking, encryption, retention controls, audit logging and least-privilege access.

## 9. Practitioner summary

A robust model separates party, identifier, relationship, account, portfolio, mandate, household and access-control records. It also uses effective dating, source mapping and lineage so that operational decisions and historical reports can be reproduced.
