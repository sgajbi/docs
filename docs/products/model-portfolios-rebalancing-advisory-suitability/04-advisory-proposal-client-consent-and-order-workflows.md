# 04 - Advisory Proposal, Client Consent and Order Workflows

## 1. What is an advisory proposal?

An advisory proposal is a controlled recommendation package. It explains what action is being proposed, why, what risks are involved, how the action affects the portfolio, and what approvals are required before execution.

A proposal is not only a trade list. It should include:

- current portfolio state;
- investment rationale;
- recommended actions;
- expected post-trade allocation;
- suitability result;
- risk disclosures;
- product details;
- costs and fees;
- concentration impact;
- liquidity impact;
- currency impact;
- income impact;
- benchmark/mandate impact;
- client acknowledgements;
- approval trail;
- order status.

## 2. Proposal lifecycle

```text
Create Proposal
  -> Enrich with Portfolio Analytics
  -> Generate Recommended Actions
  -> Run Suitability and Eligibility Checks
  -> Run Pre-trade Controls
  -> Advisor Review
  -> Supervisor / PM Review if required
  -> Client Presentation
  -> Client Accept / Reject / Modify
  -> Order Creation
  -> Execution and Settlement
  -> Post-trade Monitoring
  -> Proposal Closure
```

## 3. Proposal types

| Proposal type | Description |
|---|---|
| Full portfolio rebalance | Align portfolio to target model |
| Cash deployment | Invest idle cash |
| Cash raising | Raise cash for withdrawal, fees or capital call |
| Tactical idea | Implement CIO/advisor market view |
| Product switch | Replace product with better/cheaper/preferred alternative |
| Maturity reinvestment | Reinvest matured bond/note/deposit |
| Income enhancement | Increase yield/income orientation |
| Risk reduction | Reduce volatility, concentration, duration or credit risk |
| Liquidity improvement | Increase cash/liquid assets |
| FX hedge | Reduce currency exposure |
| Mandate remediation | Correct breach or drift |
| Tax-aware action | Realize losses/gains or optimize tax lots |
| ESG transition | Replace excluded holdings with eligible holdings |

## 4. Proposal object

Minimum proposal fields:

| Field | Purpose |
|---|---|
| proposal_id | Unique identifier |
| client_id | Client/legal entity |
| portfolio_id | Target portfolio/account |
| service_model | Advisory / DPM / execution-only |
| proposal_type | Rebalance, cash deployment, switch, etc. |
| model_id/model_version | Target model used |
| benchmark_id | Benchmark used for analytics |
| created_by | Advisor/PM/system |
| created_at | Timestamp |
| valid_until | Expiry time/date |
| proposal_currency | Reporting currency |
| rationale | Investment explanation |
| status | Draft, pending, approved, rejected, ordered, completed |
| suitability_status | Pass/fail/warning/override required |
| cost_summary | Fees, spreads, taxes if estimated |
| risk_summary | Main risk changes |
| disclosure_pack_id | Linked disclosures/KID/PHS/factsheets |
| approval_workflow_id | Approval trail |
| audit_lineage_id | Full traceability |

## 5. Proposal line item

Each proposed action should be a line item.

| Field | Purpose |
|---|---|
| line_id | Unique line |
| action | BUY / SELL / SWITCH / SUBSCRIBE / REDEEM / FX / HEDGE |
| instrument_id | Target instrument |
| product_type | Equity, fund, bond, note, FX, etc. |
| quantity / nominal / amount | Proposed size |
| price_source | Indicative price/NAV/bid/offer |
| expected_cash_impact | Funding effect |
| expected_position_impact | Position change |
| rationale_code | Drift correction, tactical view, risk reduction |
| suitability_result | Pass/warning/fail |
| restriction_result | Pass/warning/fail |
| estimated_fee | Commission/spread/platform fee |
| estimated_tax | Optional/if available |
| execution_channel | OMS, fund platform, RFQ, manual |
| dependency_group | Sell-before-buy / FX-before-buy |

## 6. Client-facing explanation

A good proposal should answer:

1. What is changing?
2. Why is the change recommended?
3. What are the expected benefits?
4. What are the key risks?
5. What will the portfolio look like after the change?
6. What fees/costs may apply?
7. What products are complex or illiquid?
8. What income/cashflow impact is expected?
9. What currency exposure changes?
10. What happens if the client does nothing?

## 7. Before/after analytics

Proposal should provide before/after comparison.

| Metric | Current | Proposed | Change |
|---|---:|---:|---:|
| Equity allocation | 48% | 40% | -8% |
| Fixed income allocation | 38% | 45% | +7% |
| Cash | 5% | 6% | +1% |
| Expected yield | 3.1% | 3.5% | +0.4% |
| Portfolio volatility | 11.5% | 9.8% | -1.7% |
| Single issuer max exposure | 14% | 8% | -6% |
| Non-base FX exposure | 42% | 35% | -7% |
| Liquidity < T+7 | 55% | 65% | +10% |
| Model drift score | 18 | 4 | Improved |

## 8. Proposal rationale taxonomy

Recommended rationale codes:

| Code | Meaning |
|---|---|
| MODEL_REBALANCE | Bring portfolio closer to model |
| MANDATE_BREACH_REMEDIATION | Correct mandate/rule breach |
| TACTICAL_OVERLAY | Implement CIO/advisor tactical view |
| PRODUCT_REPLACEMENT | Replace product due to rating/cost/liquidity/performance |
| MATURITY_REINVESTMENT | Reinvest matured/redempted product proceeds |
| CASH_DEPLOYMENT | Invest idle cash |
| CASH_RAISE | Raise cash for withdrawal/fee/capital call |
| RISK_REDUCTION | Reduce volatility/concentration/duration/credit risk |
| INCOME_ENHANCEMENT | Increase income/yield objective |
| LIQUIDITY_IMPROVEMENT | Increase liquidity buffer |
| FX_HEDGE | Hedge currency exposure |
| TAX_OPTIMIZATION | Tax-aware action |
| ESG_ALIGNMENT | Align with sustainability restriction |
| CLIENT_INSTRUCTION | Client requested action |

## 9. Client consent model

Client consent should be linked to proposal version. If proposal changes materially, consent should be re-obtained.

Consent fields:

| Field | Purpose |
|---|---|
| consent_id | Unique consent record |
| proposal_id | Proposal approved |
| proposal_version | Exact version approved |
| client_party_id | Approving party |
| role | Account holder, authorized person, trustee, director |
| consent_channel | In-person, phone, digital, email, recorded line |
| consent_timestamp | Time of approval |
| consent_scope | Full proposal / selected lines |
| risk_acknowledgements | Product risks accepted |
| disclosure_documents | Docs provided |
| language | Language of explanation/documents |
| advisor_id | Advisor presenting |
| witness/supervisor_id | If required |
| expiry | Consent valid until |

## 10. Approval workflow by service model

| Service model | Required approval |
|---|---|
| Execution-only | Usually client instruction, no advice; suitability may differ by jurisdiction/product |
| Advisory | Advisor review + suitability + client consent |
| DPM | PM approval within mandate; no per-trade client consent |
| Trust/fiduciary | Trustee/authorized signatory and fiduciary checks |
| Corporate entity | Authorized signatory and entity mandate |
| Joint account | Signing rules, all/any mandate |
| Vulnerable/selected client | Additional safeguards/call-back depending on policy/regulation |

## 11. Order creation from proposal

An approved proposal line may create one or more orders.

Example: buy USD bond fund in SGD account.

| Proposal line | Generated orders |
|---|---|
| Buy USD 50,000 Global Bond Fund | FX SGD -> USD; fund subscription; cash settlement |

Example: switch fund A to fund B.

| Proposal line | Generated orders |
|---|---|
| Switch Fund A to Fund B | Fund redemption/switch-out; fund subscription/switch-in; possible FX/cash residual |

## 12. Proposal validity and staleness

A proposal should expire or require refresh when:

- market prices become stale;
- NAV date changes beyond tolerance;
- client profile changes;
- risk profile changes;
- product approval status changes;
- model version changes;
- portfolio materially changes;
- account cash/buying power changes;
- suitability rule version changes;
- regulatory/disclosure document changes;
- pending orders settle or fail;
- product subscription window closes.

## 13. Handling partial acceptance

Clients often accept only part of a proposal.

System should support:

- accept all;
- reject all;
- accept selected lines;
- modify amount;
- defer line;
- replace product;
- request advisor revision;
- split proposal into multiple orders.

Partial acceptance should trigger revalidation because the accepted subset may produce different portfolio risk and suitability outcomes.

## 14. Handling rejected proposals

Capture rejection reasons:

| Reason | Example |
|---|---|
| Client disagrees with market view | Does not want to reduce equities |
| Liquidity concern | Wants to keep more cash |
| Cost concern | Does not want transaction fees |
| Tax concern | Does not want realized gains |
| Product concern | Does not understand structured product |
| Timing concern | Wants to wait |
| Alternative product requested | Prefers ETF over mutual fund |
| Advisor cancelled | Proposal outdated |

Rejections are useful for advisor follow-up and audit.

## 15. DPM proposal/action workflow

In DPM, client may not approve each trade, but the platform still needs investment action workflow.

```text
Model / mandate signal
  -> affected portfolio population
  -> portfolio drift and restriction checks
  -> PM rebalance batch
  -> trade-list generation
  -> pre-trade compliance
  -> PM approval
  -> order staging / block trade
  -> allocation across accounts
  -> execution
  -> post-trade compliance and reporting
```

Key DPM requirements:

- fair allocation across accounts;
- trade aggregation/block trading;
- account-level restrictions;
- cash/buying-power validation;
- client-specific exclusions;
- post-trade drift and breach checks;
- audit trail from model decision to each account trade.

## 16. Audit trail

Store evidence for:

- client profile used;
- product data used;
- model version used;
- valuations used;
- risk analytics used;
- suitability rules executed;
- warnings generated;
- overrides approved;
- disclosure documents provided;
- client consent captured;
- orders generated;
- execution outcomes;
- post-trade results.

## 17. Common failure cases

| Failure | Platform impact |
|---|---|
| Proposal generated on stale portfolio | Unsuitable or incorrect recommendation |
| Product risk rating changed after proposal | Need revalidation |
| Client risk profile missing | Block recommendation |
| Client accepts after expiry | Require refresh |
| Partial fill | Need post-trade recalculation |
| Fund NAV differs from estimate | Cash residual and drift remain |
| FX rate moved | Funding amount changes |
| Corporate action between proposal and execution | Need proposal update |
| Pledged asset sale attempted | Buying power/collateral issue |
| Client rejected one leg of multi-leg proposal | Portfolio outcome no longer valid |

## 18. Summary

An advisory proposal is a controlled business object, not a static document. It should connect analytics, rationale, suitability, restrictions, disclosures, approvals, orders, execution and post-trade outcomes with full lineage.
