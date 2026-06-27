# 08 - Platform Implementation, Source Ownership, Reconciliation and Test Scenarios

## 1. Overview

Client/account/portfolio master data should be implemented as a governed domain with clear source ownership, reconciliation, lineage, APIs, event handling, quality controls and QA scenarios.

## 2. Source ownership matrix

| Data domain | Typical source owner | Notes |
|---|---|---|
| Party identity | Core banking / onboarding / CRM | Golden source may require MDM. |
| CIF/source IDs | Source systems | Retain all identifiers. |
| KYC/AML risk | KYC platform / compliance system | Highly controlled. |
| Beneficial ownership | KYC / entity management | Requires documentation and verification. |
| Tax profile | Tax documentation / CRS/FATCA system | Must be report-grade. |
| Suitability profile | Advisory/suitability system | Effective-dated and reviewed. |
| Account | Core banking / custody / loan system | Legal/accounting record. |
| Portfolio | Portfolio platform / reporting system | Analytics/reporting construct. |
| Household | CRM / RM workspace / family-office system | Needs governance due privacy risk. |
| Mandate | Advisory/DPM system | Rules and authority. |
| Booking centre | Account/core banking | Account-level contract attribute. |
| RM assignment | CRM / coverage system | Effective-dated. |
| Access roles | IAM/entitlement platform | Must use source relationship and role scopes. |

## 3. Integration patterns

| Pattern | Use case |
|---|---|
| Event-driven | Client update, account opened, mandate changed, RM reassigned. |
| Batch snapshot | Daily account/client golden copy, reporting cut. |
| API lookup | Real-time suitability, advisor dashboard, access control. |
| Change data capture | Source-system master data changes. |
| Reference data publish | Product eligibility and booking-centre rules. |
| Reconciliation file | Daily comparison to source systems. |
| Manual workflow | Exception handling and override approval. |

## 4. Golden-copy design

A golden copy should not hide source conflicts. It should preserve:

- selected value
- source system
- confidence score
- priority rule
- effective date
- timestamp
- approver where manual override
- previous values
- unresolved conflicts

Example:

| Field | Value | Source | Confidence | Status |
|---|---|---|---:|---|
| client_name | ABC Holdings Pte Ltd | Core Banking | 0.98 | Golden |
| tax_residence | SG | Tax System | 0.95 | Golden |
| RM | Sandeep | CRM | 0.90 | Golden |
| address | Conflict | Core vs CRM | 0.50 | Exception |

## 5. Reconciliation controls

### 5.1 Daily reconciliation

| Reconciliation | Example |
|---|---|
| Party count | Source clients vs golden clients. |
| Account count | Core active accounts vs master active accounts. |
| Account status | Blocked/closed statuses match. |
| Portfolio membership | Source/account portfolio links match. |
| Booking centre | Account booking centre consistent. |
| RM assignment | CRM vs advisory platform. |
| Suitability status | Advisory system vs rule engine. |
| Tax profile | Tax system vs reporting system. |
| Household membership | CRM vs reporting group. |
| Access entitlements | IAM vs coverage assignments. |

### 5.2 Exception states

| State | Meaning |
|---|---|
| Matched | Values reconcile. |
| Tolerated mismatch | Known timing or approved difference. |
| Break | Needs investigation. |
| Pending source | Source missing/delayed. |
| Manual override | Approved override active. |
| Stale | Not refreshed within SLA. |
| Blocker | Prevents report/order/control. |

## 6. Data quality rules

| Rule | Severity |
|---|---|
| Active account must have legal owner | Critical |
| Active account must have booking centre | Critical |
| Active advisory/DPM account must have suitability profile | Critical |
| CRS-reportable account must have tax profile or documented exception | Critical |
| Trust/company must have controlling-person/BO records or exception | Critical |
| Portfolio must have effective-dated membership | High |
| DPM portfolio must have mandate and benchmark/model where required | High |
| Client report recipient must be authorized | Critical |
| Account closure cannot proceed with unsettled trades unless exception | High |
| Household membership must have inclusion flags | Medium |
| RM assignment must be effective-dated | Medium |

## 7. Platform architecture sketch

```text
Source systems
  Core Banking / Custody / CRM / KYC / Tax / Suitability / IAM
      -> ingestion adapters
      -> canonical master-data service
      -> rule engines and golden-copy resolution
      -> APIs and events
      -> consumers:
          advisory
          DPM
          reporting
          performance
          risk
          lending
          order management
          client digital
```

## 8. API quality requirements

| Requirement | Reason |
|---|---|
| `asOf` support | Historical reporting and audit. |
| Source lineage | Explain and reconcile values. |
| Effective-dated relationships | Accurate past state. |
| Partial/degraded state flags | Avoid false certainty. |
| Idempotent updates | Safe event replay. |
| Versioned schemas | Migration resilience. |
| Pagination/filtering | Large household/account lists. |
| Access control at API boundary | Prevent data leakage. |
| Audit logs | Compliance and investigation. |

## 9. Test scenarios

### 9.1 Basic hierarchy

| Scenario | Expected result |
|---|---|
| Individual client opens account and portfolio | Party, account, portfolio linked correctly. |
| Account status changes to blocked | Orders prevented; reports show status. |
| RM reassignment effective tomorrow | Today's dashboard shows old RM; tomorrow shows new RM. |
| Portfolio currency changed | Future reports use new currency; past reports reproducible. |

### 9.2 Joint and household

| Scenario | Expected result |
|---|---|
| Joint account with any-one-to-sign rule | Either authorized holder can approve instruction. |
| Joint account with all-to-sign rule | Single approval insufficient. |
| Household includes father's account and trust account | Consolidated view shown only to authorized users. |
| Member removed from household effective date | Past reports unchanged; future reports exclude member. |

### 9.3 Trust/entity

| Scenario | Expected result |
|---|---|
| Company account missing beneficial owner | Critical data-quality exception. |
| Trust account with trustee, settlor and beneficiaries | Roles represented separately. |
| Director loses authority | Future orders blocked for that party. |
| Beneficiary data restricted | Only authorized roles can view. |

### 9.4 Tax and suitability

| Scenario | Expected result |
|---|---|
| Tax residency changes | CRS/FATCA reporting derived from effective date. |
| Missing TIN | Reporting exception and remediation workflow. |
| Risk profile downgraded | High-risk product recommendations blocked. |
| Complex product knowledge expired | Structured product recommendation requires renewal. |

### 9.5 Booking centre and cross-border

| Scenario | Expected result |
|---|---|
| Client resident in one country, account booked in another | Both dimensions retained; rules evaluate both. |
| Booking centre transferred | Product/disclosure/tax/reporting rules re-evaluated. |
| RM outside permitted location attempts recommendation | Cross-border control blocks or routes to approval. |
| Product not approved for booking centre | Recommendation blocked. |

### 9.6 Lending and collateral

| Scenario | Expected result |
|---|---|
| Borrower differs from pledgor | Both roles represented. |
| Cross-pledge A to B | Collateral view shows legal owner and supported borrower. |
| Pledged account included in household report | Report labels restricted/pledged assets. |
| Collateral account blocked | Assets not freely available for trading. |

### 9.7 Migration

| Scenario | Expected result |
|---|---|
| Same client from two source CIFs | Canonical party with two source identifiers. |
| Duplicate merge reversed | Historical lineage preserved. |
| Account mapping missing for migrated transaction | Break generated and report blocked/degraded. |
| Legacy household mapping conflicts with CRM | Exception raised, not silently overwritten. |

## 10. Production support checklist

| Symptom | Likely cause |
|---|---|
| Portfolio missing in report | Portfolio membership missing/stale. |
| Wrong RM shown | Coverage assignment effective date/source issue. |
| Product not available | Booking-centre or suitability rule mismatch. |
| Household total wrong | Inclusion/exclusion flags or duplicate account. |
| Wrong tax report | Tax profile, TIN or controlling-person issue. |
| Order blocked unexpectedly | Account restriction, KYC/suitability or access rule. |
| User cannot see client | IAM/coverage/delegation mismatch. |
| Past statement changed | Lack of snapshot/effective-date handling. |

## 11. Practitioner summary

Treat client/account/portfolio master data as a first-class platform domain. Define source ownership, lineage, effective dating, reconciliation, degraded states and QA scenarios before building advisor dashboards or reports on top of it.
