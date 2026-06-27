# 06 - Data Model: Portfolio Targets, Rules, Events and Lineage

## 1. Design objective

The data model should support the full journey from model portfolio to executed trade and reporting outcome.

A strong model should answer:

- Which model applied?
- Which version applied?
- What was the current portfolio at proposal time?
- Which restrictions were evaluated?
- Which rules passed or failed?
- Who approved the proposal?
- Which orders were generated?
- What was executed?
- What was the post-trade effect?
- Can we reproduce the recommendation later?

## 2. Core domain objects

```text
ClientProfile
Account / Portfolio
Mandate
ModelPortfolio
ModelVersion
TargetAllocation
PortfolioSnapshot
DriftAnalysis
Proposal
ProposalLine
SuitabilityAssessment
RuleEvaluation
Override
Consent
OrderInstruction
Execution
PostTradeReview
AuditLineage
```

## 3. Model portfolio entity

| Field | Purpose |
|---|---|
| model_id | Unique model identifier |
| model_name | Name |
| model_type | Asset allocation, product model, sleeve model, income model, etc. |
| owner_team | CIO, PM, advisory, investment committee |
| base_currency | Reference currency |
| benchmark_id | Linked benchmark |
| risk_profile_band | Suitable risk band |
| service_model | Advisory/DPM/both |
| status | Draft, approved, retired |
| created_at/updated_at | Lifecycle |

## 4. Model version entity

| Field | Purpose |
|---|---|
| model_version_id | Unique version |
| model_id | Parent model |
| version_number | V1/V2/etc. |
| effective_from | Start date |
| effective_to | End date |
| approved_by | Approver/investment committee |
| approval_date | Approval timestamp |
| rationale | Model change explanation |
| source_documents | IC memo, factsheet, outlook |
| status | Draft/approved/retired |

## 5. Target allocation entity

| Field | Purpose |
|---|---|
| target_id | Unique record |
| model_version_id | Model version |
| allocation_level | Asset class, region, sector, currency, product type, instrument |
| classification_code | E.g. EQUITY_US, FI_IG, CASH_USD |
| target_weight | Preferred allocation |
| min_weight | Mandate lower bound |
| max_weight | Mandate upper bound |
| warning_band | Monitoring threshold |
| action_band | Rebalance threshold |
| priority | Optimization priority |
| liquidity_bucket | Optional |
| benchmark_component_id | Optional |

## 6. Portfolio snapshot entity

Snapshot is critical for reproducibility.

| Field | Purpose |
|---|---|
| snapshot_id | Unique snapshot |
| portfolio_id | Client portfolio |
| as_of_date_time | Valuation timestamp |
| valuation_source_set | Price/NAV source set |
| cash_balance_version | Cash source/version |
| position_version | Holdings source/version |
| pending_trade_inclusion | Included/excluded flag |
| reporting_currency | Snapshot currency |
| total_market_value | Portfolio value |
| data_quality_status | Complete/stale/missing |

## 7. Portfolio exposure snapshot

| Field | Purpose |
|---|---|
| snapshot_id | Parent snapshot |
| exposure_type | Asset class, issuer, sector, region, currency, duration, rating |
| exposure_code | Classification |
| market_value | Value |
| weight | Percentage |
| lookthrough_flag | Direct/look-through |
| source | Direct holding / fund look-through / structured note look-through |
| confidence | High/medium/low |

## 8. Drift analysis entity

| Field | Purpose |
|---|---|
| drift_analysis_id | Unique result |
| snapshot_id | Portfolio state |
| model_version_id | Target model |
| calculation_time | Timestamp |
| drift_score | Overall drift score |
| breach_count | Number of mandate breaches |
| warning_count | Number of warnings |
| status | In target / monitor / rebalance candidate / breach |

Drift line:

| Field | Purpose |
|---|---|
| drift_line_id | Unique line |
| exposure_code | Classification |
| current_weight | Current allocation |
| target_weight | Target allocation |
| min_weight/max_weight | Allowed range |
| absolute_drift | Current - target |
| relative_drift | Drift / target |
| status | Pass/warning/action/breach |
| recommended_action | Buy/sell/hold |

## 9. Rule catalogue

Rules should be data-driven.

| Field | Purpose |
|---|---|
| rule_id | Unique rule |
| rule_type | Suitability, mandate, product eligibility, pre-trade |
| rule_name | Name |
| severity | Info/warning/block |
| jurisdiction | Applicable jurisdiction |
| service_model | Advisory/DPM/execution-only |
| product_scope | Product categories |
| client_scope | Client types/profile |
| expression | Rule logic or reference to rule engine |
| effective_from/effective_to | Versioning |
| owner | Compliance/product/investment/credit |
| evidence_required | Disclosure, consent, approval |

## 10. Rule evaluation entity

Every rule execution should be stored.

| Field | Purpose |
|---|---|
| evaluation_id | Unique result |
| proposal_id | Proposal |
| proposal_line_id | Optional line-level result |
| rule_id | Rule evaluated |
| rule_version | Version used |
| input_hash | Hash of inputs |
| result | Pass/warning/fail/not applicable |
| message | Human explanation |
| severity | Info/warning/block |
| evaluated_at | Timestamp |
| override_id | If overridden |

## 11. Proposal entity

See file 04 for detailed fields. Key lineage fields:

| Field | Purpose |
|---|---|
| source_snapshot_id | Portfolio state used |
| source_model_version_id | Model used |
| source_rule_set_version | Suitability/rule version |
| generated_by_engine_version | Calculation engine version |
| proposal_version | Proposal version |
| previous_proposal_version | Link for revisions |

## 12. Proposal line entity

| Field | Purpose |
|---|---|
| proposal_line_id | Unique line |
| proposal_id | Parent proposal |
| action_type | Buy/sell/switch/FX/subscribe/redeem |
| instrument_id | Instrument |
| target_amount | Proposed amount |
| target_quantity | Proposed quantity |
| target_nominal | Proposed nominal |
| order_type | Market, limit, fund subscription, RFQ |
| rationale_code | Why action is proposed |
| dependency_group | Order dependencies |
| suitability_status | Line status |
| expected_post_trade_weight | Impact estimate |

## 13. Suitability assessment entity

| Field | Purpose |
|---|---|
| assessment_id | Unique assessment |
| proposal_id | Proposal |
| client_profile_version | Client data used |
| product_data_version | Product data used |
| portfolio_snapshot_id | Portfolio data used |
| assessment_time | Timestamp |
| overall_result | Pass/warning/override/fail |
| reason_summary | Main reason |
| disclosure_requirements | Required docs/acknowledgements |

## 14. Override entity

See file 05. Add lineage:

| Field | Purpose |
|---|---|
| override_scope | Rule/proposal/proposal line/order |
| expiry_condition | Date, event, post-trade review |
| monitoring_rule | Follow-up monitoring |
| attachment_id | Evidence |

## 15. Consent entity

| Field | Purpose |
|---|---|
| consent_id | Unique consent |
| proposal_id/proposal_version | Exact proposal approved |
| consenting_party_id | Client/authorized person |
| authorization_basis | Account holder/trustee/director/POA |
| channel | Digital/phone/in-person/email |
| consent_time | Timestamp |
| consent_scope | All/selected lines |
| risk_acknowledgements | Risks accepted |
| documents_provided | Disclosure docs |
| recording_reference | If phone/call-back |

## 16. Order linkage

| Field | Purpose |
|---|---|
| order_id | OMS/order id |
| proposal_line_id | Source line |
| trade_group_id | Multi-leg group |
| order_status | New/placed/part-filled/filled/cancelled/rejected |
| execution_venue | Exchange/RFQ/fund platform/OTC |
| estimated_amount | Proposal estimate |
| executed_amount | Actual executed amount |
| settlement_status | Pending/settled/failed |

## 17. Event model

Important events:

| Event | Meaning |
|---|---|
| MODEL_APPROVED | Model version approved |
| MODEL_RETIRED | Model version retired |
| PORTFOLIO_SNAPSHOT_CREATED | Snapshot captured |
| DRIFT_CALCULATED | Drift analysis generated |
| PROPOSAL_CREATED | Proposal generated |
| PROPOSAL_REVISED | Proposal version changed |
| SUITABILITY_COMPLETED | Checks completed |
| OVERRIDE_APPROVED | Rule override approved |
| CLIENT_CONSENT_CAPTURED | Client approved |
| ORDER_CREATED | Order instruction created |
| ORDER_EXECUTED | Trade executed |
| SETTLEMENT_COMPLETED | Settlement complete |
| POST_TRADE_REVIEW_COMPLETED | Outcome validated |
| BREACH_DETECTED | Rule/mandate breach detected |
| BREACH_REMEDIATED | Breach fixed |

## 18. Lineage and reproducibility

For every recommendation, store:

```text
who + what + when + why + based on which data + using which rule version + approved by whom + executed how + final result
```

Minimum lineage keys:

- model version;
- benchmark version;
- client profile version;
- product master version;
- price/NAV/FX source version;
- holdings/cash snapshot;
- rule catalogue version;
- engine version;
- proposal version;
- consent version;
- order/execution references.

## 19. Data quality flags

| Flag | Meaning |
|---|---|
| STALE_PRICE | Price older than tolerance |
| STALE_NAV | Fund NAV delayed |
| MISSING_LOOKTHROUGH | Fund/structured product exposure incomplete |
| UNCLASSIFIED_HOLDING | No asset class/sector/issuer mapping |
| PENDING_TRADE | Existing trades not settled |
| CASH_UNRECONCILED | Cash data mismatch |
| PRODUCT_APPROVAL_UNKNOWN | Product governance data missing |
| CLIENT_PROFILE_EXPIRED | KYC/risk profile stale |
| RULE_VERSION_CHANGED | Suitability rules changed since proposal |

## 20. Summary

A robust data model is event-sourced and lineage-aware. It should allow the firm to reproduce a proposal, explain a recommendation, prove suitability checks, link approvals to exact proposal versions, and reconcile execution outcomes to portfolio analytics.
