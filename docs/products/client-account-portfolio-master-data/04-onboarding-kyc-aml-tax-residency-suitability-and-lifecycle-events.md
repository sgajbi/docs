# 04 - Onboarding, KYC/AML, Tax Residency, Suitability and Lifecycle Events

## 1. Overview

Client/account master data is dynamic. A client is onboarded, reviewed, reclassified, transferred, restricted, deceased, merged, split, closed or reactivated. Each lifecycle event can affect product eligibility, access, tax reporting, statements, mandate rules and portfolio analytics.

This file explains the lifecycle and key control domains.

## 2. Client onboarding lifecycle

```text
Lead / prospect
  -> preliminary eligibility
  -> KYC data capture
  -> document collection
  -> AML/sanctions/PEP screening
  -> tax self-certification
  -> suitability/risk profiling
  -> account contract setup
  -> booking-centre assignment
  -> service model and RM assignment
  -> account/portfolio activation
```

## 3. Onboarding data domains

| Domain | Examples |
|---|---|
| Identity | Name, date of birth/incorporation, nationality, address, ID documents. |
| Legal capacity | Individual, company, trust, foundation, joint account, minor, estate. |
| KYC profile | Occupation, business activity, source of wealth, source of funds. |
| AML risk | PEP, sanctions, adverse media, high-risk jurisdiction, complex structure. |
| Tax profile | Tax residency, TIN, FATCA/CRS classification, US indicia. |
| Suitability | Risk tolerance, knowledge/experience, objectives, loss capacity, horizon. |
| Service model | Advisory, DPM, execution-only, EAM, family office. |
| Account setup | Booking centre, base currency, statement preferences, restrictions. |
| Authority | Authorized signers, POA, trustees, directors, investment committee. |
| Consent | Data privacy, marketing, e-delivery, consolidated reporting, cross-border. |

## 4. KYC / AML / CDD concepts

Customer due diligence should identify and verify the customer, understand the purpose and nature of the relationship, identify beneficial owners/controllers where applicable, risk-rate the relationship and apply ongoing monitoring.

### AML/KYC attributes

| Attribute | Purpose |
|---|---|
| customer_risk_rating | Low, medium, high, prohibited. |
| pep_status | PEP, RCA, non-PEP, unknown. |
| sanctions_status | Clear, potential hit, confirmed hit, blocked. |
| adverse_media_status | Clear, under review, material issue. |
| source_of_wealth | How wealth was accumulated. |
| source_of_funds | Origin of funds entering account. |
| expected_activity | Expected transaction and product pattern. |
| high_risk_country_exposure | Residence, nationality, source of funds, business links. |
| beneficial_ownership_complete | Yes/no/exception. |
| kyc_review_due_date | Periodic refresh date. |
| exception_approval | Manual approval and expiry. |

## 5. Tax residency and CRS/FATCA data

Tax residency should be captured as a structured, effective-dated profile.

### Tax residency fields

| Field | Meaning |
|---|---|
| tax_residency_country | Jurisdiction of tax residence. |
| tin | Taxpayer identification number or functional equivalent. |
| tin_unavailable_reason | Reason for missing TIN. |
| self_certification_date | Date client certified tax status. |
| fatca_status | US person, non-US, recalcitrant, exempt, entity category. |
| crs_status | Reportable/non-reportable category. |
| controlling_persons | For passive entities/trusts. |
| documentation_expiry | Expiry/review date. |
| reportable_jurisdictions | Derived from tax profile and rules. |

### Why tax profile affects platform design

| Impact | Example |
|---|---|
| Account reporting | CRS/FATCA reporting values. |
| Product eligibility | US persons may be restricted from certain funds/products. |
| Statement data | Tax vouchers, withheld tax summaries. |
| Withholding | Product/country/payment-specific withholding. |
| Entity classification | Passive entity controlling-person reporting. |
| Data quality | Missing TIN can block reporting or create exceptions. |

## 6. Suitability profile

Suitability profiles should be modelled separately from product holdings. They are client/account/mandate-level decision inputs.

### Suitability attributes

| Attribute | Examples |
|---|---|
| risk_tolerance | Low, medium, high. |
| investment_objective | Income, balanced, growth, speculation. |
| time_horizon | Short, medium, long. |
| loss_capacity | Limited, moderate, high. |
| knowledge_experience | Equities, bonds, funds, derivatives, structured products. |
| liquidity_need | High, medium, low. |
| concentration_tolerance | Maximum issuer/asset/product limits. |
| leverage_appetite | No leverage, limited, permitted. |
| ESG/preferences | Exclusions, sustainability preferences. |
| complex_product_eligibility | Allowed/disallowed/pending assessment. |
| review_date | Next profile review. |

### Suitability event examples

| Event | Impact |
|---|---|
| Risk profile downgraded | Block high-risk product recommendations. |
| Product knowledge expired | Require re-assessment before complex products. |
| Liquidity need increased | Highlight illiquid assets and private markets. |
| Client becomes vulnerable | Strengthen consent, explanation and review workflow. |
| Mandate changed | Rebalance and breach-monitoring rules may change. |

## 7. Account and client lifecycle events

| Event | Description | Downstream impact |
|---|---|---|
| Client created | New canonical party. | KYC, CRM, access, onboarding. |
| Account opened | New legal account. | Cash/securities/transactions can post. |
| Portfolio created | New reporting/investment container. | Performance inception and mandate setup. |
| Mandate activated | Investment authority active. | Suitability/rebalancing/controls enabled. |
| RM reassigned | Coverage changes. | Dashboard, statements, alerts, approvals. |
| KYC refresh due | Periodic review required. | Account may become restricted if overdue. |
| Tax status changed | New tax residency/TIN/FATCA/CRS state. | Reporting and product restrictions updated. |
| Address changed | Contact and tax/cross-border implications. | Document generation and notifications. |
| Client deceased | Estate workflow. | Trading restrictions and reporting changes. |
| Account blocked | Manual or compliance block. | Orders and withdrawals restricted. |
| Account closed | No new activity. | Historical reporting retained. |
| Client merged | Duplicate resolution. | Consolidation and source mapping updated. |
| Client split | Prior merge reversal or source correction. | Historical lineage critical. |
| Booking centre transfer | Account migrates jurisdiction/entity. | Product eligibility, statements, tax and legal terms change. |

## 8. Effective dating and event sourcing

Use effective-dated records for business state and event logs for audit.

```text
Current state answers: what is true today?
Effective history answers: what was true on reporting date?
Event log answers: who changed what, when, why and from which source?
```

Recommended event fields:

| Field | Meaning |
|---|---|
| event_id | Unique event. |
| event_type | KYC_REFRESH, ACCOUNT_BLOCK, RM_CHANGE, TAX_STATUS_CHANGE. |
| subject_type | Party, account, portfolio, mandate, household. |
| subject_id | Relevant ID. |
| effective_date | Business-effective date. |
| event_timestamp | System/event timestamp. |
| source_system | Source of event. |
| previous_value / new_value | Change details. |
| approval_reference | Compliance/ops approval. |
| reason_code | Why changed. |
| lineage_id | Traceability. |

## 9. Data quality gates

| Gate | Example rule |
|---|---|
| Identity completeness | Legal name, DOB/incorporation date, ID number, country present. |
| Account activation | KYC approved, tax self-cert captured, contracts signed. |
| Trading enablement | Product permissions and suitability complete. |
| DPM activation | Mandate signed, model linked, restrictions configured. |
| Complex product eligibility | Knowledge/experience and disclosure completed. |
| CRS/FATCA reporting | Tax residency/TIN status valid. |
| Statement delivery | Address/email/e-delivery consent valid. |
| Access control | Authorized viewer/instructor roles active. |
| Closure | Open trades, pending income, collateral, loan, fees resolved. |

## 10. Practitioner summary

Onboarding and lifecycle management should not be treated as CRM-only processes. They are data events that drive product eligibility, trading, reporting, tax, mandate, analytics, access control and operational risk. Every significant client/account state must be effective-dated, source-owned and auditable.
