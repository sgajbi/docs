# 08 - Platform Implementation, Controls, Reconciliation and Test Scenarios

## 1. Reference architecture

```text
Data Sources
  -> Holdings / Cash / Transactions / Prices / Product Master / Client Profile / Mandate / Models
      -> Portfolio Snapshot Service
          -> Analytics Engine
              -> Drift Engine
                  -> Proposal Engine
                      -> Suitability and Rule Engine
                          -> Approval and Consent Workflow
                              -> Order Gateway / OMS / Fund Platform / RFQ / FX
                                  -> Execution and Settlement Updates
                                      -> Post-trade Analytics and Reporting
```

## 2. Key services

| Service | Responsibility |
|---|---|
| Model Portfolio Service | Store models, versions, targets, ranges and product universes |
| Mandate Service | Store client mandates, restrictions and benchmark mappings |
| Client Profile Service | KYC/risk profile/knowledge/preferences |
| Portfolio Snapshot Service | Create reproducible holdings/cash/exposure snapshot |
| Analytics Service | Allocation, risk, performance, income, liquidity |
| Drift Engine | Compare current portfolio to target/model/ranges |
| Rebalancing Engine | Generate candidate actions and optimize trade list |
| Suitability Engine | Evaluate client-product-portfolio fit |
| Rule Engine | Execute product, mandate, pre-trade and regulatory rules |
| Proposal Service | Manage proposal versions, line items and rationale |
| Approval Workflow | Advisor, supervisor, PM, compliance, client consent |
| Order Adapter | Create orders in OMS/fund platform/RFQ/FX |
| Post-trade Monitor | Validate execution, settlement and post-trade compliance |
| Reporting Service | Client, advisor, PM and compliance reporting |
| Audit/Lineage Service | Store inputs, outputs, versions and evidence |

## 3. API capabilities

Suggested APIs:

| API | Purpose |
|---|---|
| `POST /portfolio-snapshots` | Create snapshot |
| `GET /model-portfolios/{id}/versions/{version}` | Retrieve model version |
| `POST /drift-analysis` | Calculate drift versus model/mandate |
| `POST /rebalancing/proposals` | Generate proposal |
| `POST /suitability/assessments` | Evaluate proposal suitability |
| `POST /proposals/{id}/submit-review` | Submit for approval |
| `POST /proposals/{id}/client-consent` | Capture client consent |
| `POST /proposals/{id}/orders` | Generate orders |
| `GET /proposals/{id}/lineage` | Retrieve full lineage |
| `POST /post-trade-review` | Evaluate actual outcome |

## 4. Idempotency and versioning

All mutation APIs should support idempotency keys.

Examples:

- creating proposal;
- submitting for approval;
- capturing consent;
- generating orders;
- approving override;
- posting post-trade review.

Versioning rules:

- client profile version must be stored;
- product data version must be stored;
- model version must be stored;
- rule set version must be stored;
- portfolio snapshot must be immutable;
- proposal changes create a new proposal version;
- client consent links to exact proposal version.

## 5. Reconciliation controls

| Reconciliation | Purpose |
|---|---|
| Position reconciliation | Holdings used in proposal equal source/custodian state |
| Cash reconciliation | Available cash/buying power correct |
| Price/NAV reconciliation | Valuations aligned to source and as-of date |
| Model target reconciliation | Targets sum to 100% or valid range logic |
| Rule evaluation reconciliation | All required rules executed |
| Proposal/order reconciliation | Approved lines created orders correctly |
| Execution reconciliation | Orders executed and booked correctly |
| Post-trade reconciliation | Portfolio effect matches expected within tolerance |
| Consent reconciliation | Orders not placed without valid approval/consent |
| Disclosure reconciliation | Required documents provided before consent |

## 6. Data quality controls

Block or warn when:

- holdings are missing;
- cash is stale;
- prices are stale;
- FX rates are stale;
- product classification missing;
- client risk profile expired;
- product risk rating missing;
- model version retired;
- rule set version changed;
- pending trades not considered;
- unsettled cash ignored;
- look-through data missing for material fund/structured exposure;
- portfolio valuation does not reconcile.

## 7. Proposal expiry controls

Proposal should expire on:

- price/NAV staleness;
- client profile change;
- product status change;
- model update;
- mandate update;
- portfolio change above threshold;
- cash/buying power change;
- market event;
- regulatory document change;
- manual cancellation.

## 8. Pre-trade integration controls

Before sending to OMS/order platform:

1. check proposal approved;
2. check consent valid if advisory;
3. check PM approval if DPM;
4. re-run key pre-trade checks;
5. check product still open/available;
6. check cash/buying power;
7. check order size/lot/minimum;
8. check trading window;
9. check restricted list;
10. generate dependent orders in right sequence;
11. store order references.

## 9. Post-trade controls

After execution:

- update positions and cash;
- mark proposal lines filled/partial/cancelled;
- calculate actual allocation;
- compare expected versus actual;
- identify residual cash/drift;
- re-run mandate checks;
- record post-trade suitability/compliance status;
- generate advisor/client confirmation;
- schedule follow-up if needed.

## 10. Batch DPM controls

DPM rebalance batches need additional controls:

| Control | Purpose |
|---|---|
| Account eligibility | Only include eligible accounts |
| Account exclusions | Apply client restrictions |
| Cash availability | Avoid overdrafts |
| Fair allocation | Allocate block trades fairly |
| Partial fill handling | Pro-rata or policy-based allocation |
| Cross-account concentration | Monitor issuer/security concentration |
| PM approval | Portfolio manager sign-off |
| Compliance review | Pre/post-trade checks |
| Model version lock | Batch uses consistent model version |
| Exception queue | Accounts that cannot be traded |

## 11. Common test scenarios

### Scenario 1: Simple advisory rebalance

- Client balanced profile.
- Portfolio equity overweight by 8%.
- Proposal sells equity fund and buys bond fund.
- Suitability passes.
- Client accepts.
- Orders execute fully.
- Post-trade drift improves.

Expected: proposal completed, lineage stored, reporting updated.

### Scenario 2: Product risk too high

- Conservative client.
- Advisor proposes high-risk structured note.
- Product risk exceeds client profile.

Expected: suitability fail or override required depending on policy.

### Scenario 3: Partial client acceptance

- Proposal has five lines.
- Client accepts only three.
- Accepted subset causes cash overweight and residual drift.

Expected: re-run analytics and suitability before order creation.

### Scenario 4: Stale NAV

- Proposal uses fund NAV older than tolerance.

Expected: warning or block depending on rule severity.

### Scenario 5: Pledged asset sale

- Proposed sell bond is pledged to Lombard credit line.

Expected: collateral impact check; block or credit approval required.

### Scenario 6: DPM batch with restricted security

- Model trade buys ETF.
- One client has ESG restriction blocking that ETF.

Expected: account excluded or substitute generated; exception recorded.

### Scenario 7: Cash deployment with FX

- Client has SGD cash, proposal buys USD bond.

Expected: FX order generated, settlement sequence handled, FX exposure shown.

### Scenario 8: Private-market commitment

- Upcoming capital call requires USD 200,000.
- Rebalance proposal would deploy cash.

Expected: liquidity rule blocks or preserves required cash.

### Scenario 9: Model version changed

- Proposal generated under V1.
- Model V2 approved before client accepts.

Expected: proposal expires or advisor confirms use of V1 under policy.

### Scenario 10: Partial fill

- Equity sell fills partially.
- Dependent buy cannot fully execute.

Expected: dependent order adjusted or held; post-trade review flags residual drift.

## 12. Audit tests

For any completed recommendation, QA should verify:

- exact client profile version;
- exact product master version;
- exact model version;
- exact portfolio snapshot;
- all required rules evaluated;
- all warnings shown;
- overrides approved;
- consent captured before order;
- required disclosures delivered;
- orders match approved proposal;
- execution captured;
- post-trade result calculated;
- report reflects actual state.

## 13. Operational exception queues

Recommended queues:

| Queue | Purpose |
|---|---|
| Data quality exceptions | Missing/stale data |
| Suitability failures | Blocked recommendations |
| Override approvals | Pending supervisor/compliance review |
| Client pending | Awaiting client consent |
| Order failures | Rejected/cancelled/failed orders |
| Settlement exceptions | Failed/late settlement |
| Post-trade breaches | New compliance issues |
| Model drift alerts | Portfolios needing rebalance |
| Disclosure gaps | Missing documents/acknowledgements |

## 14. Observability metrics

| Metric | Meaning |
|---|---|
| proposals_created | Volume of proposals |
| proposal_acceptance_rate | Client adoption |
| suitability_fail_rate | Quality/product fit issue |
| override_rate | Control pressure indicator |
| order_conversion_rate | Approved-to-ordered ratio |
| execution_success_rate | Order success |
| post_trade_breach_count | Control effectiveness |
| average_drift_before_after | Rebalance effectiveness |
| stale_data_block_count | Data quality issue |
| consent_missing_block_count | Workflow control issue |

## 15. Implementation principles

- Make snapshots immutable.
- Make proposals versioned.
- Make rules versioned.
- Store all evaluations.
- Separate lifecycle events from accounting transactions.
- Separate target model from client-specific proposal.
- Separate advisory consent from DPM authority.
- Treat product eligibility and suitability as first-class controls.
- Keep calculation engines deterministic and reproducible.
- Build exception management from day one.

## 16. Summary

The technology challenge is not only calculating target trades. It is building a controlled, auditable, reproducible advisory and mandate platform that connects data quality, analytics, suitability, approvals, execution and post-trade reporting.
