# 05. Worked Examples and Implementation Patterns

## Purpose

This file turns fund concepts into practical examples for dealing, NAV timing, subscriptions, redemptions, distributions, look-through analytics, reporting, implementation and QA.

## Example 1. Amount-based subscription before cut-off

### Scenario

A client subscribes SGD 100,000 into an open-ended unit trust before the fund cut-off.

| Attribute | Value |
|---|---:|
| Subscription amount | 100,000 |
| Cut-off time | 15:00 |
| Order time | 14:30 |
| Dealing date | Same business day |
| Confirmed NAV | 10.25 |
| Subscription fee | 1.00% |

### Calculation

```text

subscription fee = 100,000 x 1.00% = 1,000
net invested amount = 100,000 - 1,000 = 99,000
confirmed units = net invested amount / NAV
confirmed units = 99,000 / 10.25 = 9,658.5366 units

```

### Platform treatment

| State | Treatment |
|---|---|
| Order entered | Cash is reserved; units are estimated only. |
| Cut-off passed | Order is accepted for the dealing date. |
| NAV received | Units and final cash impact are confirmed. |
| Settlement completes | Cash ledger and fund position are final. |

### QA assertions

| Test | Expected result |
|---|---|
| Order is before cut-off | Same dealing date is assigned if fund calendar is open. |
| NAV is unavailable | Order remains pending; units are not confirmed. |
| Subscription fee is configured | Units use net invested amount, not gross cash. |
| Rounding rules apply | Confirmed units follow fund/share-class precision. |

## Example 2. Subscription after cut-off

### Scenario

A client submits the same SGD 100,000 subscription after cut-off.

| Attribute | Value |
|---|---|
| Order time | 16:10 |
| Cut-off time | 15:00 |
| Submitted date | Monday |
| Dealing calendar | Daily, business days |
| Assigned dealing date | Tuesday |

### Correct treatment

The order should not use Monday's NAV. It should be assigned to the next valid dealing date and priced using the NAV for that dealing date.

### QA assertions

| Test | Expected result |
|---|---|
| Order is submitted after cut-off | Next valid dealing date is assigned. |
| Next day is a fund holiday | Next fund dealing day is used, not next calendar day. |
| Advisor changes order before cut-off for assigned date | Amendment workflow follows fund policy. |
| Report is generated before NAV confirmation | Pending order is shown separately from confirmed holding. |

## Example 3. Amount-based redemption with NAV movement

### Scenario

A client requests SGD 50,000 redemption from an open-ended fund. NAV is unknown at order entry.

| Attribute | Value |
|---|---:|
| Requested cash | 50,000 |
| Estimated NAV at order entry | 10.00 |
| Confirmed NAV | 9.80 |
| Redemption fee | 0.50% |

### Calculation

```text

gross redemption amount required = requested cash / (1 - redemption fee)
gross redemption amount required = 50,000 / 0.995 = 50,251.2563

units redeemed = gross redemption amount / confirmed NAV
units redeemed = 50,251.2563 / 9.80 = 5,127.6792 units

```

### QA assertions

| Test | Expected result |
|---|---|
| NAV moves between order and confirmation | Confirmed units use confirmed NAV. |
| Redemption fee applies | Units are based on gross redemption needed to deliver requested net cash. |
| Available units are insufficient | Order is blocked or reduced according to policy. |
| Final cash differs due to rounding | Difference is posted and explainable. |

## Example 4. Redemption gate and deferred balance

### Scenario

A client requests redemption of 100,000 units. The fund applies a 40% redemption gate.

| Attribute | Value |
|---|---:|
| Requested units | 100,000 |
| Gate percentage | 40% |
| NAV | 12.00 |
| Units redeemed now | 40,000 |
| Units deferred | 60,000 |

### Calculation

```text

redeemed value = 40,000 x 12.00 = 480,000
deferred units = 100,000 - 40,000 = 60,000

```

### Platform treatment

| Area | Treatment |
|---|---|
| Transaction | Book partial redemption for allowed units. |
| Position | Reduce only redeemed units; retain deferred units. |
| Cash | Do not make deferred proceeds available. |
| Reporting | Show gate, redeemed amount and deferred balance. |
| Advisory | Warn client about liquidity restriction and uncertainty. |

### QA assertions

| Test | Expected result |
|---|---|
| Gate applies | Partial redemption and deferred balance are recorded. |
| Gate is lifted later | Deferred order is re-evaluated using current fund terms. |
| Report is generated during gate | Liquidity restriction is visible. |
| User tries to sell deferred units again | Available units exclude already-deferred amount. |

## Example 5. Cash distribution versus reinvested distribution

### Scenario

A fund pays a distribution of SGD 0.20 per unit. The client holds 10,000 units.

| Attribute | Value |
|---|---:|
| Units held | 10,000 |
| Distribution per unit | 0.20 |
| Gross distribution | 2,000 |
| Reinvestment NAV | 10.00 |

### Cash distribution

```text

cash income = 10,000 x 0.20 = 2,000

```

The client receives cash and fund units do not change.

### Reinvested distribution

```text

reinvested units = gross distribution / reinvestment NAV
reinvested units = 2,000 / 10.00 = 200 units

new units = 10,000 + 200 = 10,200 units

```

### QA assertions

| Test | Expected result |
|---|---|
| Distribution option is cash | Cash income is booked; units unchanged. |
| Distribution option is reinvest | Income and reinvestment units are both traceable. |
| Class is accumulating | No artificial cash distribution is created unless formally reported. |
| Distribution includes return of capital | Income and capital classifications are separated. |

## Example 6. Share-class selection and hedged class reporting

### Scenario

The same fund has two share classes.

| Attribute | USD class | SGD-hedged class |
|---|---|---|
| ISIN | Class-specific | Class-specific |
| NAV currency | USD | SGD |
| Distribution policy | Accumulating | Distributing |
| Hedging | None | Currency hedged |
| Fee level | Institutional | Retail |

### Correct treatment

The investable instrument is the share class, not only the umbrella fund. NAV history, fee terms, distribution policy, eligibility and hedging behavior are share-class specific.

### QA assertions

| Test | Expected result |
|---|---|
| Position points only to umbrella fund | Data model QA fails for valuation and eligibility. |
| SGD-hedged class uses USD class NAV | Valuation is rejected. |
| Distribution policy differs | Reporting follows held share class. |
| Client is not eligible for institutional class | Subscription is blocked or escalated. |

## Example 7. ETF market price and NAV premium/discount

### Scenario

A client holds an ETF traded on exchange.

| Attribute | Value |
|---|---:|
| Units | 5,000 |
| Exchange market price | 21.00 |
| Published NAV per share | 20.70 |

### Calculation

```text

market value = 5,000 x 21.00 = 105,000
NAV value = 5,000 x 20.70 = 103,500
premium = (market price - NAV) / NAV
premium = (21.00 - 20.70) / 20.70 = 1.45%

```

### QA assertions

| Test | Expected result |
|---|---|
| ETF is valued for client statement | Exchange market price is used if policy says market value. |
| Analytics needs premium/discount | NAV and market price are both stored. |
| Market is halted | Valuation quality and liquidity status are flagged. |
| ETF is treated like daily unit trust | Trade lifecycle and intraday market price behavior are wrong. |

## Example 8. Look-through exposure coverage

### Scenario

A balanced fund provides holdings look-through for 80% of NAV.

| Exposure | Look-through value |
|---|---:|
| Equities | 45% |
| Bonds | 30% |
| Cash | 5% |
| Unknown / unavailable | 20% |

### Reporting principle

Look-through should show coverage quality. Do not present partial look-through as complete exposure.

### QA assertions

| Test | Expected result |
|---|---|
| Look-through coverage is 80% | Report shows coverage percentage or unknown bucket. |
| Holdings file is stale | Exposure is flagged with holdings date. |
| Fund-of-funds includes underlying funds | Double counting is avoided by methodology. |
| Look-through source is missing | Fallback to fund classification with disclosure. |

## Example 9. Private fund capital call and NAV lag

### Scenario

A private equity fund has a commitment and quarterly NAV.

| Attribute | Value |
|---|---:|
| Commitment | 1,000,000 |
| Paid-in capital before call | 300,000 |
| Capital call | 100,000 |
| Distribution received | 20,000 |
| Latest NAV | 360,000 |
| NAV date | Quarter-end, received 45 days later |

### Position view

| Measure | Value |
|---|---:|
| Paid-in after call | 400,000 |
| Unfunded commitment | 600,000 |
| Cumulative distributions | 20,000 |
| Reported NAV | 360,000 |

### QA assertions

| Test | Expected result |
|---|---|
| Capital call notice arrives | Cash obligation and paid-in capital update. |
| NAV is received late | Valuation date and received date are both stored. |
| Distribution is recallable | Reporting distinguishes recallable from final distribution. |
| Private fund is modelled as daily NAV units | Data model is inappropriate unless product terms support it. |

## Example 10. NAV restatement

### Scenario

A fund administrator restates last month's NAV from 10.00 to 9.85 after reports were generated.

### Correct workflow

| Step | Action |
|---|---|
| Detect | New NAV references previous NAV date and correction. |
| Assess | Identify impacted positions, transactions, performance and statements. |
| Recalculate | Revalue holdings and affected performance periods. |
| Decide | Apply materiality/restatement policy. |
| Publish | Supersede reports if required; keep original archive. |

### QA assertions

| Test | Expected result |
|---|---|
| Restated NAV arrives | Prior valuation is versioned, not overwritten silently. |
| Confirmed subscription used old NAV | Transaction correction workflow is evaluated. |
| Client report was published | Restatement decision is documented. |
| Performance changes | Recalculation lineage identifies NAV correction as cause. |

## Example 11. Hedge fund equalization credit

### Scenario

A client subscribes to a hedge fund after the fund has accrued performance fees. The fund applies an equalization credit so the new investor is not charged for performance generated before entry.

| Attribute | Value |
|---|---:|
| Subscription amount | 500,000 |
| Dealing NAV before equalization | 125.00 |
| Equalization credit per unit | 1.20 |
| Confirmed units | 4,000.0000 |

### Calculation

```text
gross units = subscription amount / dealing NAV
gross units = 500,000 / 125.00 = 4,000.0000

equalization credit = units x credit per unit
equalization credit = 4,000.0000 x 1.20 = 4,800

effective entry economics = subscription amount + equalization credit disclosure
```

### Correct treatment

The equalization amount is not an additional cash deposit unless the fund administrator confirms cash movement. It is an economic adjustment used for fair performance-fee allocation. Reporting should show the confirmed units, NAV, equalization credit, administrator source date and fee basis.

### QA assertions

| Test | Expected result |
|---|---|
| Equalization data is missing | Subscription is booked but equalization reporting is labelled incomplete. |
| Fund uses series accounting instead | Series/lot is modelled explicitly rather than forcing equalization credit. |
| Performance fee crystallizes later | Equalization treatment reconciles to administrator statement. |
| Client exits before crystallization | Redemption proceeds follow administrator equalization calculation. |

## Example 12. Share-class conversion

### Scenario

A client converts from an accumulating USD share class to a distributing USD share class of the same fund. The conversion should not be treated as a market sale and repurchase unless tax/accounting policy requires that treatment.

| Attribute | Old class | New class |
|---|---:|---:|
| Units before conversion | 10,000.0000 | - |
| NAV | 15.00 | 12.00 |
| Market value | 150,000 | 150,000 |
| Conversion fee | 300 | - |

### Calculation

```text
net conversion value = old units x old NAV - conversion fee
net conversion value = 10,000 x 15.00 - 300 = 149,700

new units = net conversion value / new class NAV
new units = 149,700 / 12.00 = 12,475.0000
```

### Correct treatment

The platform should close or reduce the old share class and open the new share class with lineage to the conversion event. Cost basis, performance break, fees and tax treatment depend on jurisdiction and policy, so the conversion event must preserve source terms and not silently look like ordinary trading.

### QA assertions

| Test | Expected result |
|---|---|
| New class eligibility fails | Conversion is blocked before order submission. |
| Conversion fee applies | Fee is posted separately and included in cost/performance policy. |
| Currency differs | FX treatment and settlement legs are explicit. |
| Tax policy treats conversion as disposal | Realized gain/loss workflow is triggered with source policy. |

## Example 13. Fund merger and unit exchange

### Scenario

A fund merges into another fund. Client holdings in the old fund are exchanged for units in the receiving fund using an administrator-confirmed exchange ratio.

| Attribute | Value |
|---|---:|
| Old fund units | 8,000.0000 |
| Exchange ratio | 0.7425 new units per old unit |
| Cash in lieu | 35 |

### Calculation

```text
new fund units = old fund units x exchange ratio
new fund units = 8,000.0000 x 0.7425 = 5,940.0000

cash in lieu = 35
```

### Correct treatment

The old position closes through a corporate-action-style fund merger event, not through a client-initiated redemption. The new fund inherits appropriate cost, holding-period and performance lineage according to accounting and tax policy. Reporting should explain the event, new fund identity, exchange ratio, effective date and any cash in lieu.

### QA assertions

| Test | Expected result |
|---|---|
| Exchange ratio is missing | Merger cannot create new units automatically. |
| Receiving fund is not in instrument master | Event is held in exception state until instrument setup is complete. |
| Cash in lieu is present | Cash is posted separately from new units. |
| Old fund had pending orders | Pending orders are cancelled, transferred or reviewed according to administrator notice. |

## Example 14. Notice-period redemption with holdback

### Scenario

A hedge fund requires 45 days' notice and pays 90% of redemption proceeds on settlement, with 10% held back until audit completion.

| Attribute | Value |
|---|---:|
| Redemption request value | 200,000 |
| Initial payout percentage | 90% |
| Holdback percentage | 10% |
| Notice period | 45 days |

### Calculation

```text
initial payout = redemption value x initial payout percentage
initial payout = 200,000 x 90% = 180,000

holdback receivable = redemption value x holdback percentage
holdback receivable = 200,000 x 10% = 20,000
```

### Correct treatment

During notice period, the holding remains invested unless fund terms state otherwise. After settlement, cash received, pending holdback receivable, residual units and final audit adjustment should be visible separately. The holdback is not immediately available cash.

### QA assertions

| Test | Expected result |
|---|---|
| Notice submitted after deadline | Redemption moves to next dealing window. |
| Initial payout arrives | Cash is posted and holdback remains receivable/pending. |
| Final holdback differs from estimate | Adjustment preserves original estimate and final source amount. |
| Client requests full withdrawal | Reporting explains notice period, holdback and unavailable amount. |

## Example 15. Transfer-agent rejection

### Scenario

A fund subscription is submitted before cut-off, but the transfer agent rejects it because required documentation is incomplete.

| Attribute | Value |
|---|---:|
| Submitted subscription amount | 100,000 |
| Cash reserved | 100,000 |
| Expected dealing date | 2026-07-01 |
| Rejection reason | Documentation incomplete |

### Correct workflow

| Step | Treatment |
|---|---|
| Order submitted | Reserve cash and show pending order. |
| Rejection received | Cancel or reject order with transfer-agent reason and timestamp. |
| Cash release | Release reserved cash unless policy keeps it blocked for resubmission. |
| Client/advisor action | Request missing documentation or create corrected order. |
| Reporting | Do not show fund units or exposure from rejected order. |

### QA assertions

| Test | Expected result |
|---|---|
| Transfer-agent rejection arrives after expected dealing date | Pending units are never created without confirmation. |
| Cash was reserved | Reservation is released or reclassified with audit trail. |
| Advisor resubmits order | New order links to rejected order but has its own lifecycle. |
| Rejection reason is missing | Exception queue requires manual classification before closure. |

## Example 16. Multi-level fund-of-funds look-through

### Scenario

A fund-of-funds provides look-through to two underlying funds. Each underlying fund provides only partial holdings coverage.

| Layer | Weight | Look-through coverage |
|---|---:|---:|
| Underlying Fund A | 60% | 80% |
| Underlying Fund B | 40% | 50% |

### Calculation

```text
effective coverage from Fund A = 60% x 80% = 48%
effective coverage from Fund B = 40% x 50% = 20%

total look-through coverage = 48% + 20% = 68%
unmapped exposure = 100% - 68% = 32%
```

### Correct treatment

Portfolio exposure should show both mapped look-through and unmapped residual exposure. Risk, sector, geography, issuer and ESG analytics should not present partial coverage as complete. The source date and coverage level should be visible at each level.

### QA assertions

| Test | Expected result |
|---|---|
| Underlying file is stale | Coverage for that layer is stale or excluded by policy. |
| Fund A holds Fund B | Circular or duplicate exposure is detected and controlled. |
| Residual exposure remains | Reports show unmapped percentage rather than suppressing it. |
| Underlying security identifier is missing | Exposure maps to unknown bucket with source exception. |
