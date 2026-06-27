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
