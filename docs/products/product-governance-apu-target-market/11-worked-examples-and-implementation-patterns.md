# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns the governance framework into concrete product, advisory, analytics, reporting and implementation examples. Use it when a product-governance concept needs to become a rule, data model, control, test case or stakeholder explanation.

## Example 1. Structured note target-market and disclosure gate

### Scenario

A relationship manager proposes a USD autocallable note linked to three equities.

| Attribute | Value |
|---|---|
| Client segment | Professional investor |
| Client risk profile | High |
| Knowledge and experience | Structured products: experienced |
| Booking centre | Singapore |
| Product status | Active in APU |
| Product complexity | Complex |
| Product risk rating | High |
| Liquidity classification | Limited secondary liquidity |
| Required documents | Term sheet, key information document, issuer risk disclosure, scenario disclosure |
| Negative target market | Capital preservation objective, low-risk clients, clients needing daily liquidity |

### Rule evaluation

| Rule | Result | Reason |
|---|---|---|
| APU active status | Pass | Product is approved and active for new buys. |
| Client segment | Pass | Product is allowed for professional investors. |
| Risk-profile match | Pass | Client and product are both high risk. |
| Knowledge and experience | Pass | Client has relevant structured-product experience. |
| Liquidity need | Warning | Client has no immediate liquidity need, but product has limited secondary liquidity. |
| Disclosure delivery | Block until complete | Required documents must be delivered and acknowledged before order submission. |

### Platform behavior

The proposal workflow can show the note as eligible only after the disclosure package is complete. The system should store:

1. product governance rule version,
2. product risk rating and complexity at decision time,
3. client classification and risk profile at decision time,
4. disclosure document versions,
5. pass, warning and block reason codes,
6. final advisor rationale and client acknowledgement.

### QA assertions

| Test | Expected result |
|---|---|
| Missing key information document | Proposal can be saved, order cannot be submitted. |
| Client downgraded to medium risk before order | Recommendation becomes blocked or requires new review. |
| Product moved to hold-only | New buy is blocked; sell/exit workflow remains available. |
| Disclosure document version changes | New acknowledgement is required before new order. |

## Example 2. Bond downgrade and mandate eligibility change

### Scenario

A discretionary mandate holds a callable subordinated bank bond. The issuer is downgraded and the product risk rating changes from medium to high.

| Attribute | Before downgrade | After downgrade |
|---|---|---|
| Issuer rating | A- | BBB- |
| Product risk rating | Medium | High |
| Mandate allowed product risk | Low to medium | Low to medium |
| APU status | Active | Active, watchlist |
| Client action | None | Portfolio manager review required |

### Correct governance treatment

The downgrade should not be treated as only a market-data update. It is a product-governance event because it changes mandate eligibility, concentration monitoring, product risk disclosure and portfolio review behavior.

### Implementation pattern

```text

Rating change event
  -> product risk reclassification
  -> APU version update
  -> mandate eligibility recomputation
  -> portfolio breach or watchlist event
  -> advisor/PM review task
  -> client-reporting annotation if material

```

### Reporting impact

The report should not simply display the new price. It should be able to explain:

1. the bond is now outside the mandate's normal risk band,
2. the position may be permitted to hold while a review is open,
3. new purchases are blocked unless mandate rules allow an exception,
4. the review decision and action plan are recorded.

### QA assertions

| Test | Expected result |
|---|---|
| Bond rating falls below mandate threshold | Mandate exception is raised. |
| Same product is in a self-directed account | Advisory warning is raised, not DPM breach. |
| Advisor attempts additional buy | Pre-trade rule blocks or escalates based on policy. |
| Historical report is generated for date before downgrade | Old rating and old rule version are used. |

## Example 3. Fund gate and product lifecycle status

### Scenario

An open-ended fund announces a temporary redemption gate. Existing holders can remain invested, but redemptions are limited and new subscriptions are suspended.

| Attribute | Treatment |
|---|---|
| APU status | Hold-only or suspended for new subscriptions |
| Redemption status | Allowed with gated amount or delayed settlement |
| Liquidity classification | Revised to restricted |
| Reporting label | Liquidity restriction / redemption gate |
| Advisory action | Client review for liquidity needs |

### Common implementation mistake

A platform may mark the fund inactive and block all activity. That is too blunt. The correct treatment distinguishes:

1. new subscription,
2. additional buy,
3. sell/redemption,
4. switch,
5. transfer,
6. existing position reporting,
7. mandate breach handling.

### Rule shape

| Action | Result |
|---|---|
| New buy | Block |
| Redemption up to allowed gate | Allow with warning and expected delay |
| Full redemption above gate | Block or split into allowed and queued portions |
| Portfolio valuation | Continue using latest available NAV with stale-price controls |
| Client report | Show liquidity restriction and valuation-date note |

### QA assertions

| Test | Expected result |
|---|---|
| New subscription is attempted | Blocked with product lifecycle reason. |
| Existing holder requests partial redemption within allowed limit | Allowed with gated-liquidity disclosure. |
| NAV is stale during gate period | Valuation warning appears in reports. |
| Gate is lifted | Product lifecycle status and liquidity classification are versioned, not overwritten without audit. |

## Example 4. Cross-border restriction and audit replay

### Scenario

A product is approved for Singapore booked professional investors, but not offerable to a client resident in another jurisdiction.

### Decision model

| Dimension | Required evidence |
|---|---|
| Client residence | Effective-dated client jurisdiction record |
| Booking centre | Account and booking entity |
| Advisor location | Advisor jurisdiction and servicing model |
| Product offerability | Product-jurisdiction approval matrix |
| Channel | Advisory, DPM, execution-only or digital |
| Exception policy | Whether cross-border override is legally permitted |

### Runtime decision

The system should return a clear decision object:

```text

decision: BLOCK
primary_reason: PRODUCT_NOT_OFFERABLE_TO_CLIENT_JURISDICTION
rule_version: APU-RULE-2026-06-27-001
product_status: ACTIVE
client_residence: restricted jurisdiction
booking_centre: Singapore
allowed_actions: hold, sell
blocked_actions: buy, switch-in, advisory recommendation

```

### Audit replay requirement

If a complaint or audit review happens later, replay must use the data as it existed on the original decision date. The platform needs effective dating for:

1. product approval,
2. jurisdiction matrix,
3. client residence,
4. client classification,
5. advisor servicing model,
6. disclosure status,
7. exception approvals.

## Example 5. Product governance data contract

### Minimum governance profile

| Field | Why it matters |
|---|---|
| product_id | Stable join key across product master, APU, OMS and reporting. |
| approval_status | Drives availability and lifecycle treatment. |
| approval_effective_from / to | Supports historical decisions and audit replay. |
| target_market_codes | Allows machine-readable eligibility. |
| negative_target_market_codes | Blocks unsuitable client segments by design. |
| risk_rating | Feeds suitability, mandate controls and reporting. |
| complexity_classification | Drives appropriateness and disclosure rules. |
| liquidity_classification | Feeds advisory explanation, portfolio construction and reporting. |
| allowed_jurisdictions | Controls cross-border offerability. |
| allowed_channels | Separates advisory, DPM, execution-only and digital channels. |
| required_disclosures | Controls proposal and order submission. |
| review_due_date | Triggers ongoing due-diligence workflow. |
| rule_version | Makes runtime decisions explainable and replayable. |

### Design principle

Product governance data should be versioned and consumed centrally. If eligibility logic is copied into advisory UI, order management, reporting and DPM engines separately, the firm eventually gets inconsistent decisions and weak audit evidence.

## Example 6. Stakeholder questions and expected artifacts

| Stakeholder | Question | Artifact that should answer it |
|---|---|---|
| Product due diligence | Why is this product approved? | Due-diligence memo, approval decision, review schedule. |
| Advisory | Can I recommend this product to this client? | Eligibility decision with reason codes and disclosure status. |
| DPM | Can this product be held in this mandate? | Mandate eligibility matrix and breach workflow. |
| Operations | Can the product be settled, custodied and serviced? | Operational readiness checklist and source-owner map. |
| Reporting | How should restrictions appear to the client? | Report label standard and exception annotations. |
| QA | What must regression cover? | Rule matrix, decision snapshots and historical replay tests. |

## Example 7. Numeric target-market rule scoring

### Scenario

A client asks about a USD high-yield bond fund. The product is approved for accredited and professional investors, but only when risk profile, liquidity need, knowledge and concentration limits are compatible.

| Attribute | Client | Product rule |
|---|---:|---:|
| Risk profile score | 4 of 5 | Minimum 3; maximum mismatch allowed 1 level |
| Investment horizon | 48 months | Minimum 36 months |
| Liquidity need | Monthly or longer | Daily liquidity need is negative target market |
| Credit-product knowledge | Experienced | Basic knowledge blocks |
| Existing high-yield exposure | 8% of portfolio | Maximum after trade 15% |
| Proposed allocation | 5% of portfolio | Maximum after trade 15% |

### Score calculation

| Rule | Calculation | Result |
|---|---|---|
| Risk profile | client score 4 >= product minimum 3 | Pass |
| Horizon | 48 months >= 36 months | Pass |
| Liquidity | monthly liquidity need is compatible | Pass |
| Knowledge | experienced >= required credit-product knowledge | Pass |
| Concentration | existing 8% + proposed 5% = 13% <= 15% | Pass |
| Negative target market | no daily liquidity need and no capital-guarantee objective | Pass |

### Decision output

```text
decision: ALLOW
primary_reason: TARGET_MARKET_COMPATIBLE
rule_version: TARGET-MARKET-HY-FUND-V3
post_trade_high_yield_exposure_pct: 13.00
required_disclosures: prospectus, product-risk-disclosure, liquidity-risk-disclosure
warnings: CREDIT_SPREAD_VOLATILITY, LIQUIDITY_MAY_DETERIORATE
```

### Implementation notes

1. Store the numeric inputs and rule version used at decision time.
2. Keep positive target-market checks separate from negative target-market conflicts.
3. Treat concentration as a post-trade calculation, not a static client attribute.
4. Return warnings separately from blocks so the advisor can explain material risks.
5. Do not reduce suitability to one opaque score; preserve the component results.

### QA assertions

| Test | Expected result |
|---|---|
| Existing high-yield exposure is 12% and proposed allocation is 5% | Block or escalate because post-trade exposure is 17%. |
| Client liquidity need changes to daily | Block due to negative target-market conflict. |
| Credit-product knowledge is missing | Block until knowledge/experience evidence is available. |
| Historical decision is replayed after rule version changes | Replay uses the original rule version and original client/product attributes. |

## Example 8. Target-market matrix evaluation for channel and mandate

### Scenario

The same investment-grade bond is available through advisory and DPM channels, but not through online self-directed trading for selected client segments.

| Dimension | Client A | Client B | Client C |
|---|---|---|---|
| Client segment | Accredited investor | Retail client | Professional investor |
| Channel | Advisory | Online self-directed | DPM mandate |
| Product risk | Medium | Medium | Medium |
| Product complexity | Non-complex | Non-complex | Non-complex |
| Required disclosure | Bond factsheet | Bond factsheet | Mandate disclosure pack |
| Mandate eligibility | Not applicable | Not applicable | Allowed by mandate |

### Matrix result

| Rule | Client A | Client B | Client C |
|---|---|---|---|
| Client segment allowed | Pass | Conditional | Pass |
| Channel allowed | Pass | Block | Pass |
| Risk profile compatible | Pass | Pass | Pass |
| Disclosure available | Pass | Pass | Pass |
| Mandate rule compatible | Not applicable | Not applicable | Pass |
| Final decision | Allow | Block | Allow |

### Why this matters

A product can be suitable in one distribution model and blocked in another. The decision must preserve:

1. client classification,
2. channel,
3. advisory model,
4. mandate context,
5. product approval status,
6. disclosure basis,
7. reason code for each pass, warning or block.

### QA assertions

| Test | Expected result |
|---|---|
| Retail client switches from online to advisor-assisted channel | Decision is recomputed; online block does not automatically apply to advisory. |
| DPM mandate excludes below-A rated bonds | Bond is blocked for DPM even if advisory channel would allow it. |
| Disclosure document is unavailable for one booking centre | Decision blocks in that booking centre but may allow elsewhere. |
| Matrix row is changed | New decisions use the new matrix version; historical replay uses the old version. |

## Example 9. Disclosure version expiry and re-acknowledgement

### Scenario

A structured deposit is approved and active. A new risk disclosure version is issued after a change in payoff explanation. Existing proposal drafts were created under the old disclosure version.

| Attribute | Old value | New value |
|---|---|---|
| Disclosure id | DCI-RISK-v2 | DCI-RISK-v3 |
| Effective date | 2026-01-15 | 2026-06-01 |
| Product status | Active | Active |
| Old acknowledgement | Present | Not valid for new orders after effective date |
| Affected action | New order submission | Requires new acknowledgement |

### Correct behavior

| Workflow | Treatment |
|---|---|
| Draft proposal created before disclosure change | Keep draft, mark disclosure stale. |
| Advisor edits proposal after new disclosure effective date | Require new disclosure delivery. |
| Client acknowledged old disclosure but order not submitted | Block submission until new acknowledgement. |
| Historical report for prior completed order | Show disclosure version used at decision time. |

### Decision output

```text
decision: BLOCK
primary_reason: DISCLOSURE_VERSION_EXPIRED
required_disclosure_id: DCI-RISK-v3
current_acknowledgement: DCI-RISK-v2
allowed_actions: save_draft, deliver_disclosure, cancel_proposal
blocked_actions: submit_order
```

### QA assertions

| Test | Expected result |
|---|---|
| New disclosure version becomes effective before order submission | Submit is blocked until new acknowledgement is captured. |
| Completed order is replayed for audit | Original disclosure version remains visible. |
| Disclosure version changes but product remains active | Product is not suspended; only affected actions are blocked. |
| Advisor attempts to override without permitted exception policy | Override is blocked and audited. |

## Example 10. Product suspension impact across multiple booking centres

### Scenario

A fund is suspended for new subscriptions in Booking Centre A because required local disclosure is expired. The same fund remains available in Booking Centre B where the disclosure is current.

| Attribute | Booking Centre A | Booking Centre B |
|---|---|---|
| Product APU status | Suspended for new buys | Active |
| Reason | Local disclosure expired | Disclosure current |
| Existing holders | Hold and redeem allowed | Hold, buy and redeem allowed |
| DPM use | No new allocation | Allowed if mandate compatible |
| Reporting | Show local restriction on affected accounts | No restriction label required |

### Action matrix

| Action | Booking Centre A | Booking Centre B |
|---|---|---|
| New subscription | Block | Allow |
| Additional subscription | Block | Allow |
| Redemption | Allow with normal fund rules | Allow with normal fund rules |
| Transfer-in | Block unless exception-approved | Allow |
| Existing holding valuation | Continue with NAV/stale controls | Continue with NAV/stale controls |
| Client statement | Show restricted-for-new-buy label | No local restriction label |

### Audit replay across booking centres

Audit replay must prove:

1. which booking-centre rule applied,
2. which disclosure version was current,
3. which action was requested,
4. which action was allowed or blocked,
5. whether the client was an existing holder,
6. whether a DPM mandate or advisory channel was used,
7. which rule version produced the decision.

### QA assertions

| Test | Expected result |
|---|---|
| Client in Booking Centre A attempts a new subscription | Blocked with local disclosure-expiry reason. |
| Same client redeems an existing position | Allowed unless another fund lifecycle restriction applies. |
| Client in Booking Centre B attempts a new subscription | Allowed if target-market, suitability and disclosure checks pass. |
| Historical replay runs after Booking Centre A disclosure is renewed | Old suspended decision remains reproducible for the original date. |
