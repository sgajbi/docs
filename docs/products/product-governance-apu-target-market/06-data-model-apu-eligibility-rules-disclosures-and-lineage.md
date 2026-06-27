# 06. Data Model: APU, Eligibility Rules, Disclosures and Lineage

## 1. Design objective

The product governance data model should make product approval and eligibility decisions auditable, explainable and reusable across advisory, order management, reporting, suitability, mandate monitoring and product review processes.

Avoid modelling product governance as free text in product master.

## 2. Conceptual model

```text
Product / Instrument
    +-- Product Governance Profile
    +-- Product Risk Profile
    +-- Target Market Profile
    +-- Distribution Eligibility Rules
    +-- Disclosure Requirements
    +-- Jurisdiction / Booking Centre Rules
    +-- Mandate Eligibility Rules
    +-- Lifecycle Review Events
    +-- Approval Decisions
    +-- Rule Versions / Audit Lineage
```

## 3. Core entities

| Entity | Purpose |
|---|---|
| product | security/product identity |
| product_governance_profile | approval status and governance attributes |
| product_risk_profile | risk, complexity, liquidity, leverage classifications |
| target_market_profile | positive and negative target market |
| product_eligibility_rule | machine-readable rules for distribution |
| disclosure_requirement | required documents and acknowledgements |
| jurisdiction_rule | country/booking-centre/residency restrictions |
| mandate_eligibility_rule | DPM/model portfolio eligibility |
| product_review_event | periodic/event-driven review records |
| approval_decision | committee decisions and evidence |
| rule_version | versioning for eligibility logic |
| exception_approval | manual override records |
| decision_audit | runtime eligibility decisions |

## 4. Product governance profile

Recommended fields:

| Field | Meaning |
|---|---|
| product_id | internal product/instrument ID |
| governance_status | approved, restricted, suspended, hold-only |
| status_effective_from | effective date/time |
| status_effective_to | optional expiry |
| approval_scope | global, regional, booking-centre-specific |
| approval_owner | product committee / shelf owner |
| last_approval_date | date of approval |
| next_review_date | next due diligence review |
| review_frequency | annual, semi-annual, event-driven |
| product_owner | accountable business owner |
| data_owner | reference data owner |
| control_owner | governance control owner |
| source_document_set_id | termsheet/prospectus/PHS/KID links |
| status_reason_code | why status exists |

## 5. Product risk profile

| Field | Example |
|---|---|
| risk_rating | LOW / MEDIUM / HIGH / VERY_HIGH |
| risk_rating_score | numeric score |
| complexity_level | NON_COMPLEX / COMPLEX / HIGHLY_COMPLEX |
| liquidity_category | DAILY / MONTHLY / LIMITED / ILLIQUID |
| leverage_flag | true/false |
| embedded_derivative_flag | true/false |
| capital_protection_type | NONE / CONDITIONAL / GUARANTEED / AT_MATURITY |
| max_loss_profile | LIMITED_TO_INVESTMENT / MORE_THAN_INVESTMENT |
| valuation_transparency | EXCHANGE_PRICE / VENDOR / ISSUER / MODEL |
| issuer_credit_required | true/false |
| product_risk_reason_codes | array of drivers |
| review_triggers | array of event triggers |

## 6. Target market profile

| Field | Example |
|---|---|
| positive_client_segments | accredited, professional, retail |
| negative_client_segments | conservative retail, vulnerable clients |
| minimum_risk_profile | balanced/growth/aggressive |
| objectives_supported | income, growth, hedging, diversification |
| objectives_not_supported | capital guarantee, short-term liquidity |
| min_horizon_months | 36 |
| liquidity_need_allowed | monthly_or_longer |
| knowledge_required | structured_products, derivatives |
| loss_capacity_required | partial_loss/full_loss |
| currency_risk_allowed | yes/no |
| target_market_narrative | human-readable explanation |

## 7. Eligibility rule model

Rules should be explicit and testable.

Example rule fields:

| Field | Meaning |
|---|---|
| rule_id | unique ID |
| product_id | product or product group |
| rule_scope | product/client/channel/mandate/booking centre |
| condition_expression | machine-readable condition |
| action | allow/block/warn/escalate/require_disclosure |
| severity | hard stop / soft warning / approval required |
| reason_code | standardized reason |
| effective_from | start date |
| effective_to | end date |
| rule_version | version ID |
| owner | rule owner |

Example:

```json
{
  "ruleId": "RULE-APU-001",
  "productType": "STRUCTURED_NOTE",
  "condition": "client.segment == 'RETAIL' && product.complexityLevel == 'HIGHLY_COMPLEX'",
  "action": "BLOCK",
  "reasonCode": "RETAIL_CLIENT_NOT_ALLOWED_FOR_HIGHLY_COMPLEX_PRODUCT",
  "effectiveFrom": "2026-01-01",
  "ruleVersion": "APU-2026-01"
}
```

## 8. Disclosure requirement model

| Field | Meaning |
|---|---|
| disclosure_id | unique disclosure requirement |
| product_id / product_type | applies to product or type |
| document_type | prospectus, KID, PHS, termsheet, risk disclosure |
| document_version | version ID |
| language | EN, ZH, etc. |
| required_for_segment | retail/accredited/professional |
| required_for_channel | advisory/online/execution-only |
| acknowledgement_required | yes/no |
| delivery_timing | pre-trade / post-trade / periodic |
| expiry_date | document validity |
| evidence_required | click, signature, call recording, RM attestation |

## 9. Runtime eligibility decision audit

Every product eligibility decision should be recorded.

| Field | Meaning |
|---|---|
| decision_id | unique decision ID |
| decision_time | timestamp |
| product_id | product checked |
| client_id | client checked |
| account_id | account/portfolio |
| channel | advisory, online, DPM |
| booking_centre | SG/HK/etc. |
| request_type | proposal, order, rebalance, switch |
| decision | allow, block, warn, escalate |
| reason_codes | standardized reasons |
| rules_evaluated | list of rule IDs |
| rule_version | active rule version |
| data_snapshot_id | product/client/account snapshot |
| override_id | if overridden |
| user_id/system_id | who requested/processed |

## 10. Data lineage

Data lineage should connect:

```text
External source documents
    -> product master ingestion
    -> due-diligence assessment
    -> committee approval
    -> APU rule publication
    -> advisory/order decision
    -> client reporting and audit record
```

For every product attribute used in suitability or eligibility, capture:

- source system,
- source document,
- extraction method,
- data owner,
- validation status,
- effective date,
- last update date,
- approval status,
- downstream consumers.

## 11. Versioning

Versioning must exist at multiple levels:

| Versioned object | Why |
|---|---|
| product terms | terms can change |
| risk rating | risk changes over time |
| target market | distribution restrictions change |
| disclosure document | client must receive correct version |
| suitability rule | rules change over time |
| client classification | client status changes |
| mandate rules | portfolio mandates change |
| APU status | approved/restricted/suspended over time |

## 12. API design patterns

### 12.1 Product governance profile API

```http
GET /products/{productId}/governance-profile?asOfDate=2026-06-27
```

Returns approved status, risk rating, target market, disclosures and rule version.

### 12.2 Eligibility check API

```http
POST /product-eligibility/check
```

Request:

```json
{
  "productId": "XS1234567890",
  "clientId": "C123",
  "accountId": "A456",
  "channel": "ADVISORY",
  "bookingCentre": "SG",
  "requestType": "BUY_ORDER",
  "orderAmount": "100000",
  "asOfDateTime": "2026-06-27T10:00:00+08:00"
}
```

Response:

```json
{
  "decision": "ESCALATE",
  "reasonCodes": ["COMPLEX_PRODUCT", "CONCENTRATION_LIMIT_NEAR_BREACH"],
  "requiredActions": ["DELIVER_TERMSHEET", "CLIENT_ACKNOWLEDGEMENT", "SUPERVISOR_APPROVAL"],
  "ruleVersion": "APU-2026-06",
  "decisionId": "DEC-98765"
}
```

## 13. Event-driven architecture

Product governance should publish events:

| Event | Consumers |
|---|---|
| ProductApproved | advisory, OMS, reporting, suitability |
| ProductRestricted | OMS, proposal, monitoring |
| ProductSuspended | OMS, advisory desktop |
| ProductRiskRatingChanged | suitability, reporting, mandate monitoring |
| TargetMarketChanged | advisory, proposal, post-sale monitoring |
| DisclosureDocumentExpired | order blocking, advisor alert |
| ProductReviewOverdue | product governance dashboard |
| ProductRemovedFromAPU | advisor and impacted holdings workflow |

## 14. Data-quality controls

Hard-stop critical fields:

- product_id,
- product_type,
- approval_status,
- effective_from,
- booking centre eligibility,
- client segment eligibility,
- risk rating,
- complexity level,
- liquidity category,
- target market,
- disclosure requirement,
- valuation/pricing source,
- approval owner,
- next review date.

If any critical field is missing, the product should not be activated for new buys.

## 15. Canonical rule outcome codes

Recommended outcome codes:

| Code | Meaning |
|---|---|
| PRODUCT_NOT_APPROVED | product not in APU |
| PRODUCT_SUSPENDED | product suspended |
| HOLD_ONLY | buys blocked, holdings allowed |
| CLIENT_SEGMENT_NOT_ALLOWED | client type not eligible |
| JURISDICTION_NOT_ALLOWED | cross-border/location restriction |
| CHANNEL_NOT_ALLOWED | channel restricted |
| MANDATE_NOT_ALLOWED | portfolio mandate blocks product |
| RISK_PROFILE_TOO_LOW | client risk profile below product requirement |
| KNOWLEDGE_REQUIRED | client lacks required product knowledge |
| DISCLOSURE_NOT_DELIVERED | required document missing |
| DISCLOSURE_EXPIRED | document out of date |
| CONCENTRATION_BREACH | post-trade exposure exceeds limit |
| LIQUIDITY_MISMATCH | product liquidity incompatible with need |
| OVERRIDE_REQUIRED | supervisory approval needed |

## 16. Architecture principle

Keep product-governance decisions deterministic and explainable. Avoid burying critical eligibility logic inside UI code, Excel sheets, free-text notes or local order-entry rules.

A high-quality architecture centralizes the decision logic but exposes it through APIs and events to all consuming platforms.
