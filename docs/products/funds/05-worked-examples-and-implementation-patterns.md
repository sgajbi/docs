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

## Example 17. Equalization After Partial Redemption

### Scenario

A hedge fund investor redeems part of a holding before the next performance-fee crystallization date. The administrator confirms the redemption proceeds and an equalization adjustment on the redeemed units.

| Attribute | Value |
|---|---:|
| Opening units | 10,000.0000 |
| Units redeemed | 4,000.0000 |
| Dealing NAV before equalization | 128.00 |
| Equalization debit per redeemed unit | 0.75 |

### Calculation

```text
gross redemption value = 4,000.0000 x 128.00 = 512,000
equalization debit = 4,000.0000 x 0.75 = 3,000
net redemption value = 512,000 - 3,000 = 509,000
remaining units = 10,000.0000 - 4,000.0000 = 6,000.0000
```

### Correct treatment

Equalization should follow the administrator statement and apply only to the redeemed units or series covered by the event. It should not be spread across unrelated lots or presented as a generic redemption fee unless the administrator classifies it that way.

### QA assertions

| Test | Expected result |
|---|---|
| Partial redemption is confirmed | Redeemed units, remaining units and equalization amount reconcile to the administrator. |
| Equalization data is missing | Proceeds are labelled incomplete or pending according to reporting policy. |
| Multiple series exist | Equalization is calculated by series or lot, not averaged across all holdings. |
| Cash settles later | Receivable remains linked to the redemption and equalization event. |

## Example 18. Performance-Fee Crystallization

### Scenario

A hedge fund applies a 20% performance fee above a high-water mark at crystallization.

| Attribute | Value |
|---|---:|
| Units | 5,000.0000 |
| High-water mark NAV | 100.00 |
| Pre-fee NAV | 112.00 |
| Performance fee rate | 20% |

### Calculation

```text
gain above high-water mark per unit = 112.00 - 100.00 = 12.00
performance fee per unit = 12.00 x 20% = 2.40
post-fee NAV = 112.00 - 2.40 = 109.60
total fee economics = 5,000.0000 x 2.40 = 12,000
```

### Correct treatment

Performance-fee crystallization is usually reflected in NAV or administrator fee allocation, not necessarily as a separate cash debit from the client account. Reporting should show the fee basis where available, including high-water mark, hurdle, equalization or series treatment.

### QA assertions

| Test | Expected result |
|---|---|
| Pre-fee NAV is below high-water mark | No performance fee crystallizes unless fund terms state otherwise. |
| Hurdle rate applies | Fee base is reduced by the hurdle before fee calculation. |
| Fee is embedded in NAV | Platform does not create a duplicate cash fee. |
| Administrator statement differs | Source-confirmed value overrides estimates with audit lineage. |

## Example 19. In-Specie Fund Redemption

### Scenario

A fund redemption is settled partly by transferring securities rather than paying only cash.

| Attribute | Value |
|---|---:|
| Redemption value | 1,000,000 |
| Cash component | 250,000 |
| Delivered bond market value | 450,000 |
| Delivered equity basket value | 300,000 |

### Reconciliation

```text
delivered asset value = 450,000 + 300,000 = 750,000
total settlement value = 250,000 + 750,000 = 1,000,000
```

### Correct treatment

The fund position should reduce through the redemption event, while delivered securities become new direct holdings only after custodian confirmation. Cost basis, acquisition date, liquidity, suitability and mandate treatment depend on policy and source terms.

### QA assertions

| Test | Expected result |
|---|---|
| Delivered instruments are not in instrument master | New direct holdings remain in exception state until setup is complete. |
| Custodian confirms only cash | Securities are not created from expected delivery alone. |
| Mandate disallows delivered asset | Exception or forced-sale workflow is triggered according to mandate policy. |
| Settlement values do not reconcile | Redemption remains open with a settlement break. |

## Example 20. Custody Re-Registration Of Fund Units

### Scenario

A client transfers fund units from one custodian account to another without changing beneficial ownership.

| Attribute | Value |
|---|---:|
| Units transferred | 20,000.0000 |
| Source custodian balance before transfer | 20,000.0000 |
| Target custodian balance before transfer | 0.0000 |
| NAV on transfer date | 15.50 |

### Position movement

```text
transfer value for reporting reference = 20,000.0000 x 15.50 = 310,000
source custodian ending units = 20,000.0000 - 20,000.0000 = 0.0000
target custodian ending units = 0.0000 + 20,000.0000 = 20,000.0000
```

### Correct treatment

Custody re-registration is not an investment sale, redemption or subscription when beneficial ownership is unchanged. The event should preserve cost basis, performance history, holding period and advisory context while changing custody location and reconciliation source.

### QA assertions

| Test | Expected result |
|---|---|
| Beneficial owner is unchanged | No realized gain/loss or client cashflow is created. |
| Transfer is partial | Source and target balances reconcile to the transferred units. |
| Target account is not eligible | Transfer remains pending or blocked with reason. |
| Custodian statements disagree | Re-registration break remains open until both sides reconcile. |

## Example 21. Fund Suspension Reopening

### Scenario

A fund suspended dealing during market stress and later announces a reopening with limited initial liquidity.

| Attribute | Value |
|---|---|
| Suspension date | 2026-03-15 |
| Reopening date | 2026-06-30 |
| Reopening dealing frequency | Monthly |
| Initial redemption cap | 25% of submitted units |

### Correct workflow

| Step | Treatment |
|---|---|
| Suspension | Block new subscriptions/redemptions unless fund notice allows exceptions. |
| Pending orders | Mark orders as suspended, cancelled or carried forward according to administrator notice. |
| Reopening | Restore dealing calendar with new terms and source notice. |
| Liquidity cap | Apply cap or gate rules to submitted redemptions. |
| Reporting | Show suspension history and current liquidity limitation. |

### QA assertions

| Test | Expected result |
|---|---|
| Order is entered during suspension | Order is blocked or accepted only into an allowed pending state. |
| Reopening terms differ from old terms | New calendar and cap rules are effective-dated. |
| Report is generated after reopening | Liquidity status reflects reopened but constrained dealing. |
| Old pending order is carried forward | Order keeps original lineage and new dealing treatment. |

## Example 22. Stale Look-Through Override Expiry

### Scenario

A fund holdings file is normally monthly but has not arrived. Risk approves a temporary stale look-through override for reporting while waiting for the administrator file.

| Attribute | Value |
|---|---|
| Last holdings date | 2026-04-30 |
| Normal freshness threshold | 45 days |
| Override approved until | 2026-06-30 |
| Reporting date | 2026-07-05 |

### Correct treatment

The look-through file may be used only while the override is valid. After expiry, exposure should move to stale, partial, or classification-only reporting according to policy.

```text
override valid on 2026-06-20 = true
override valid on 2026-07-05 = false
```

### QA assertions

| Test | Expected result |
|---|---|
| Override is active | Report labels look-through as stale but approved through the expiry date. |
| Override expires | Look-through analytics degrade or block according to policy. |
| New holdings file arrives | Override closes and source date updates. |
| Override lacks approver or reason | It cannot support client-ready reporting. |

## Example 23. Cross-Border Fund Eligibility Change

### Scenario

A fund share class becomes unavailable for new subscriptions in a booking centre after a distribution-permission change. Existing holdings may be retained, but new purchases and switches into the class are blocked.

| Attribute | Value |
|---|---|
| Share class | Global Balanced Fund Class A |
| Booking centre | Singapore |
| Eligibility change | New subscriptions blocked |
| Existing holdings | Hold and redeem allowed |
| Effective date | 2026-08-01 |

### Correct treatment

| Area | Treatment |
|---|---|
| Product master | Store effective-dated eligibility by share class, booking centre, client type and channel. |
| Orders | Block subscriptions and switches into the class after the effective date. |
| Existing positions | Allow hold, redemption or switch out according to notice terms. |
| Advisory | Explain restriction without implying the fund has become unsuitable for every existing holder. |
| Reporting | Label restricted-for-new-business status separately from valuation or liquidity state. |

### QA assertions

| Test | Expected result |
|---|---|
| New subscription after effective date | Order is blocked with eligibility reason. |
| Redemption of existing holding | Order remains allowed if terms permit. |
| Client type is exempt | Eligibility rule evaluates client type and booking centre. |
| Rule changes mid-order | Pending order is reviewed against source policy and timestamp. |

## Example 24. Fund Tax-Status Change

### Scenario

A fund changes tax classification for distributions after a regulatory or administrator update. Future distributions must use the new classification, while prior income reports may need review.

| Attribute | Before | After |
|---|---|---|
| Distribution classification | Income | Return of capital component introduced |
| Effective date | Before 2026-07-01 | From 2026-07-01 |
| Withholding basis | Standard income rate | Split by component |

### Correct treatment

| Area | Treatment |
|---|---|
| Source terms | Store effective-dated tax status and distribution component mapping. |
| Income posting | Split income, capital return, withholding and reclaim fields where source-backed. |
| Cost basis | Adjust basis when return-of-capital treatment applies under policy. |
| Reporting | Preserve prior-period classification unless a restatement is required. |
| Controls | Flag missing component breakdown as support-limited for tax reporting. |

### QA assertions

| Test | Expected result |
|---|---|
| Distribution after effective date | Uses updated component classification. |
| Component breakdown is missing | Report labels tax classification incomplete. |
| Prior report is affected | Restatement or correction workflow records decision. |
| Different jurisdictions apply different treatment | Booking-centre and tax-residency rules are separated. |

## Example 25. Trailer-Fee Rebate

### Scenario

A client is entitled to a rebate of trailer fees on a retrocession-free service model. The rebate is calculated from eligible fund holdings and rebate rate.

| Attribute | Value |
|---|---:|
| Average eligible holding value | 800,000 |
| Annual trailer fee rate | 0.30% |
| Rebate period days | 90 |
| Day-count basis | 365 |

### Calculation

```text
rebate = average eligible value x trailer rate x days / basis
rebate = 800,000 x 0.30% x 90 / 365
rebate = 591.78
```

### Correct treatment

| Area | Treatment |
|---|---|
| Eligibility | Confirm share class, client service model, regulatory rule and rebate policy. |
| Cash | Book rebate cash or receivable only when confirmed by source or policy. |
| Performance | Classify as fee rebate, not fund income or market return. |
| Reporting | Show period, rate, eligible holdings and source status. |
| Controls | Prevent duplicate rebate when already embedded in clean share class pricing. |

### QA assertions

| Test | Expected result |
|---|---|
| Client service model changes mid-period | Rebate calculation splits by effective date. |
| Share class is clean-fee class | No duplicate trailer rebate is created. |
| Source amount differs from estimate | Estimate is adjusted to confirmed amount with lineage. |
| Holding has stale valuation | Rebate is labelled estimated or blocked by policy. |

## Example 26. Anti-Dilution Levy Dispute

### Scenario

A fund redemption applies an anti-dilution levy. The client disputes the levy because the fund notice later reports a lower levy rate for the dealing date.

| Attribute | Original | Corrected |
|---|---:|---:|
| Redemption value before levy | 250,000 | 250,000 |
| Levy rate | 1.00% | 0.60% |
| Levy amount | 2,500 | 1,500 |
| Cash correction | - | 1,000 |

### Correct treatment

| Area | Treatment |
|---|---|
| Original redemption | Preserve original levy, NAV, dealing date and source. |
| Dispute | Open exception linked to fund notice, transfer-agent confirmation and client impact. |
| Correction | Post receivable/cash adjustment when corrected levy is source-confirmed. |
| Reporting | Explain corrected redemption proceeds and restatement decision. |
| QA | Ensure levy is not confused with advisor fee or tax withholding. |

### QA assertions

| Test | Expected result |
|---|---|
| Corrected levy is lower | Cash adjustment or receivable is created with lineage. |
| Fund rejects dispute | Original levy remains and dispute closure reason is stored. |
| Client report was issued | Restatement policy is evaluated. |
| Levy source is missing | Redemption is labelled incomplete for final fee breakdown. |

## Example 27. Hard-To-Value Side-Pocket Exit

### Scenario

A hedge fund side pocket holds an illiquid asset. The administrator later realizes the asset and distributes partial proceeds to investors.

| Attribute | Value |
|---|---:|
| Side-pocket units | 2,000 |
| Last reported side-pocket NAV | 18.00 |
| Realized exit proceeds per unit | 12.50 |
| Cash proceeds | 25,000 |

### Correct treatment

| Area | Treatment |
|---|---|
| Valuation | Preserve stale/estimated valuation history until administrator confirms realization. |
| Cash | Book realized proceeds as side-pocket distribution, not ordinary fund redemption. |
| Position | Reduce or close side-pocket units according to administrator notice. |
| Performance | Separate valuation write-down from realized distribution where methodology supports it. |
| Reporting | Show restricted side-pocket history, realization amount and residual position. |

### QA assertions

| Test | Expected result |
|---|---|
| Side pocket partially realizes | Units and value reduce only for realized portion. |
| Proceeds differ from last NAV | Performance explains valuation change and cash distribution. |
| Residual asset remains | Restricted side-pocket position stays open. |
| Administrator file is missing | No final cash proceeds are inferred. |

## Example 28. Fund Closure Distribution

### Scenario

A fund enters liquidation and pays a final closure distribution after selling remaining assets.

| Attribute | Value |
|---|---:|
| Units held | 15,000 |
| Final liquidation NAV per unit | 7.20 |
| Closure distribution | 108,000 |
| Residual units after closure | 0 |

### Correct treatment

| Area | Treatment |
|---|---|
| Lifecycle | Mark fund as closing, liquidation pending, final distribution paid and closed. |
| Cash | Book final distribution and any residual adjustment separately from ordinary distribution if source classifies it differently. |
| Position | Close units only after administrator or custodian confirms final event. |
| Reporting | Explain liquidation status and avoid showing a stale market value after closure. |
| Controls | Block new orders once closure notice is effective. |

### QA assertions

| Test | Expected result |
|---|---|
| Closure notice arrives | New subscriptions are blocked and status updates. |
| Final cash is paid | Units close and cash posts with closure lineage. |
| Residual payment follows later | Additional liquidation distribution links to closed fund event. |
| NAV remains in price feed after closure | Report uses closure state rather than stale ongoing valuation. |

## Example 29. Master-Feeder Restructure

### Scenario

A feeder fund restructures into a new feeder that invests into the same master fund with different fee and liquidity terms.

| Attribute | Old feeder | New feeder |
|---|---|---|
| Units | 10,000 | Calculated by exchange ratio |
| Exchange ratio | - | 0.875 new units per old unit |
| Fee terms | Legacy | Clean-fee class |
| Liquidity | Quarterly | Monthly after notice |

### Calculation

```text
new feeder units = old feeder units x exchange ratio
new feeder units = 10,000 x 0.875
new feeder units = 8,750
```

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Close old feeder and open new feeder through restructure event, not ordinary redemption/subscription. |
| Look-through | Preserve master-fund exposure while updating feeder wrapper, fees and liquidity terms. |
| Cost basis | Carry or reset basis according to tax/accounting policy and source terms. |
| Eligibility | Re-check new feeder eligibility, disclosures and mandate permissions. |
| Reporting | Explain wrapper change, exchange ratio, effective date and ongoing master exposure. |

### QA assertions

| Test | Expected result |
|---|---|
| Exchange ratio is missing | New feeder units are not created automatically. |
| New feeder fails eligibility | Exception workflow opens before position conversion. |
| Master exposure is unchanged | Look-through avoids double-counting old and new feeder exposure. |
| Fee terms change | Reporting and rebate logic use new effective-dated terms. |

## Example 30. Omnibus transfer-agent allocation

### Scenario

A platform submits one omnibus subscription order to the transfer agent, then allocates confirmed units across multiple client accounts. The transfer agent confirms the aggregate order, not each client allocation.

| Attribute | Value |
|---|---:|
| Omnibus subscription amount | 1,000,000 |
| Confirmed NAV | 25.00 |
| Confirmed omnibus units | 40,000.0000 |
| Client A intended amount | 400,000 |
| Client B intended amount | 350,000 |
| Client C intended amount | 250,000 |

### Allocation

```text
client allocation ratio = client intended amount / omnibus amount
Client A units = 40,000 x 40.00% = 16,000
Client B units = 40,000 x 35.00% = 14,000
Client C units = 40,000 x 25.00% = 10,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Order lifecycle | Preserve omnibus order id, transfer-agent confirmation and client allocation instructions. |
| Units | Allocate confirmed units, not estimated units, using governed rounding policy. |
| Cash | Reconcile each client reservation to allocated units, NAV, fees and residual rounding cash. |
| Reporting | Show client-level position while retaining omnibus source lineage. |
| Operations | Open allocation break if client allocations do not reconcile to omnibus confirmation. |

### QA assertions

| Test | Expected result |
|---|---|
| Omnibus confirmation arrives | Client allocations are created from confirmed aggregate units. |
| Rounding leaves residual units or cash | Residual is allocated or posted according to policy with evidence. |
| One client becomes ineligible before allocation | Allocation is blocked or rebalanced according to order policy. |
| Transfer-agent confirmation differs from estimate | Client units and cash use confirmed source values. |
| Reconciliation is run | Sum of client units equals omnibus units within precision tolerance. |

## Example 31. ETF creation/redemption basket and authorized participant flow

### Scenario

An ETF trades on exchange, but large institutional flows may use a creation/redemption process through an authorized participant. The basket can contain securities plus a cash balancing amount.

| Attribute | Value |
|---|---:|
| Creation unit size | 50,000 ETF shares |
| ETF market price | 20.10 |
| Published NAV | 20.00 |
| Basket securities value | 998,000 |
| Cash balancing amount | 2,000 |
| Creation fee | 500 |

### Basket value

```text
creation unit NAV value = creation unit size x NAV
creation unit NAV value = 50,000 x 20.00 = 1,000,000

total basket consideration = basket securities value + cash balancing amount + creation fee
total basket consideration = 998,000 + 2,000 + 500 = 1,000,500
```

### Correct treatment

| Area | Treatment |
|---|---|
| Retail order | Normal client ETF trades use exchange trade lifecycle. |
| Creation/redemption | Basket workflow applies only when participant, size and product terms support it. |
| Valuation | ETF market price, NAV and basket value are separate source concepts. |
| Liquidity | Premium/discount and creation-unit liquidity should be visible where relevant. |
| Reporting | Do not confuse ETF shares held by the client with underlying basket securities unless delivered. |

### QA assertions

| Test | Expected result |
|---|---|
| Client holds ETF shares | Position remains ETF shares, not underlying basket holdings. |
| Creation basket is used | Securities and cash balancing amount reconcile to ETF units created. |
| Basket security is missing in instrument master | Creation workflow remains in exception state. |
| Market price diverges from NAV | Premium/discount analytics show the difference. |
| ETF is halted | Liquidity and valuation quality states update. |

## Example 32. Liquidity bucket reclassification after fund notice

### Scenario

A fund changes dealing terms from weekly liquidity to quarterly liquidity with 60 days' notice. Portfolio liquidity reports must reclassify the holding from short-term to less liquid.

| Attribute | Before | After |
|---|---|---|
| Dealing frequency | Weekly | Quarterly |
| Notice period | 5 business days | 60 calendar days |
| Redemption gate | None | Up to 25% |
| Effective date | Before notice | From fund notice date |

### Correct treatment

| Area | Treatment |
|---|---|
| Product master | Store effective-dated liquidity terms by share class. |
| Liquidity analytics | Reclassify the holding based on new notice and dealing terms. |
| Advisory | Explain that market value did not change, but accessible liquidity worsened. |
| DPM | Rebalance engines should not assume the holding can fund near-term trades. |
| Reporting | Show liquidity bucket, notice period, gate terms and source date. |

### QA assertions

| Test | Expected result |
|---|---|
| Fund notice changes liquidity terms | Bucket changes from old terms to new effective-dated terms. |
| Report date is before notice | Historical report uses old liquidity bucket. |
| Redemption is submitted under new terms | Notice period and gate are applied. |
| Liquidity terms source is missing | Liquidity report is source-limited rather than overconfident. |
| Model rebalance needs cash in 10 days | Holding is excluded from near-term funding source. |

## Example 33. Swing-factor governance and NAV adjustment

### Scenario

A fund applies swing pricing because net redemptions exceed the swing threshold. The administrator publishes both unswung and swung NAV for the dealing date.

| Attribute | Value |
|---|---:|
| Unswung NAV | 10.0000 |
| Swing factor | -0.75% |
| Swung NAV | 9.9250 |
| Redeemed units | 20,000 |

### Redemption value

```text
swung NAV = unswung NAV x (1 + swing factor)
swung NAV = 10.0000 x (1 - 0.75%) = 9.9250

gross redemption value = redeemed units x swung NAV
gross redemption value = 20,000 x 9.9250 = 198,500
```

### Correct treatment

| Area | Treatment |
|---|---|
| NAV source | Store swung NAV, unswung NAV, swing factor and administrator source. |
| Cash | Use confirmed dealing NAV for settlement, not estimated unswung NAV. |
| Performance | Explain swing adjustment separately from market movement where source supports it. |
| Advisory | Explain dilution protection and why proceeds differ from indicative NAV. |
| Controls | Prevent local recalculation from overriding administrator-confirmed NAV. |

### QA assertions

| Test | Expected result |
|---|---|
| Swing factor applies | Redemption proceeds use swung NAV. |
| Only unswung NAV is available | Order remains estimated or source-limited. |
| Swing factor is corrected | Cash/proceeds correction workflow preserves original NAV and corrected source. |
| Report shows NAV movement | Swing-pricing note is available for client explanation. |
| Subscription and redemption swing differently | Direction and dealing-side rules are source-backed. |

## Example 34. Fund-of-funds fee layering

### Scenario

A client invests in a fund-of-funds. The top-level fund charges a management fee and underlying funds also charge fees. Reporting should avoid showing only the top-level fee when look-through fee data is available.

| Layer | Weight | Annual fee |
|---|---:|---:|
| Top-level fund | 100% | 0.75% |
| Underlying Fund A | 60% | 0.90% |
| Underlying Fund B | 40% | 1.20% |

### Effective fee estimate

```text
weighted underlying fee = 60% x 0.90% + 40% x 1.20%
weighted underlying fee = 0.54% + 0.48% = 1.02%

estimated layered fee = top-level fee + weighted underlying fee
estimated layered fee = 0.75% + 1.02% = 1.77%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Disclosure | Label fee layering as estimate unless source provides official ongoing charges. |
| Performance | Do not double-book fees already embedded in NAV. |
| Advisory | Explain all-in cost, uncertainty and data coverage. |
| Reporting | Show top-level fee, look-through fee coverage and unknown residual where applicable. |
| QA | Confirm fee source date, share class and effective period. |

### QA assertions

| Test | Expected result |
|---|---|
| Underlying fee data is partial | Report shows fee coverage percentage. |
| Underlying fund changes weight | Effective layered fee recalculates by source date. |
| Fee is embedded in NAV | No separate cash fee is posted. |
| Share-class fee differs | Calculation uses held share class, not umbrella-level average. |
| Official ongoing-charge figure exists | Report prioritizes governed official figure where policy requires. |

## Example 35. Side-letter liquidity term override

### Scenario

A family-office client has a negotiated side letter that permits quarterly redemption with 30 days' notice, while the standard fund term is annual redemption with 90 days' notice.

| Attribute | Standard term | Side-letter term |
|---|---|---|
| Redemption frequency | Annual | Quarterly |
| Notice period | 90 days | 30 days |
| Investor scope | All investors | Specific client/entity |
| Source | Offering document | Signed side letter |

### Correct treatment

| Area | Treatment |
|---|---|
| Eligibility | Apply side-letter terms only to the entitled client/entity and share class. |
| Liquidity analytics | Show both standard term and client-specific override where relevant. |
| Orders | Redemption order uses the client's governed side-letter terms. |
| Confidentiality | Side-letter terms are sensitive and should not leak to unrelated users or reports. |
| Audit | Preserve document reference, effective date, approver and expiry/review date. |

### QA assertions

| Test | Expected result |
|---|---|
| Side-letter client redeems | Order uses side-letter notice and frequency. |
| Another client redeems same fund | Standard fund terms apply. |
| Side-letter expires | Liquidity terms revert or move to review state. |
| Report is generated for unrelated user | Confidential side-letter detail is not exposed. |
| Side-letter source document is missing | Override cannot support client-ready liquidity treatment. |

## Example 36. Fund class-action proceeds

### Scenario

A fund receives class-action settlement proceeds linked to securities previously held by the fund. The administrator allocates proceeds to investors based on eligible units during the claim period.

| Attribute | Value |
|---|---:|
| Eligible units during claim period | 12,000 |
| Proceeds per eligible unit | 0.08 |
| Gross proceeds | 960 |
| Withholding or admin cost | 40 |
| Net cash | 920 |

### Correct treatment

| Area | Treatment |
|---|---|
| Cashflow | Book as administrator-confirmed class-action proceeds or special distribution according to source classification. |
| Eligibility | Use historical eligible units, not current units if holdings changed. |
| Performance | Link proceeds to fund event; do not treat as external contribution. |
| Reporting | Explain claim period, source classification and net cash. |
| Tax | Use jurisdiction/source classification where available; otherwise label incomplete. |

### QA assertions

| Test | Expected result |
|---|---|
| Client no longer holds fund | Proceeds may still post if historical eligibility is source-confirmed. |
| Eligible units differ from current units | Calculation uses claim-period units. |
| Source classification is missing | Reporting labels cashflow classification incomplete. |
| Proceeds are received net of cost | Gross, cost and net are separately reportable where sourced. |
| Duplicate administrator file arrives | Duplicate detection prevents double posting. |

## Example 37. Fund-platform migration event

### Scenario

A fund platform migrates holdings from one transfer-agent account structure to another. The economic holdings should remain unchanged, but identifiers, accounts, order channels and reconciliation sources change.

| Attribute | Before migration | After migration |
|---|---|---|
| Fund account id | TA-OLD-1001 | TA-NEW-8821 |
| Units | 25,000.0000 | 25,000.0000 |
| Share class | Same ISIN | Same ISIN |
| Order channel | Legacy platform | New platform |
| Migration date | 2026-09-30 | 2026-10-01 |

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Preserve units, cost basis, holding history and performance lineage. |
| Identifiers | Map old and new transfer-agent account identifiers with effective dates. |
| Orders | Freeze or route orders during cutover according to migration plan. |
| Reconciliation | Compare pre- and post-migration unit balances by fund, account and share class. |
| Reporting | Avoid showing migration as a sale, redemption, subscription or contribution. |
| Operations | Keep cutover evidence, exception list and sign-off. |

### QA assertions

| Test | Expected result |
|---|---|
| Units match before and after migration | No client cashflow or realized result is created. |
| Share-class mapping fails | Position enters exception state until instrument/account mapping is resolved. |
| Order is submitted during freeze | Order is blocked, queued or routed according to cutover rule. |
| Performance report crosses migration date | Return continuity is preserved. |
| Transfer-agent balance differs post-cutover | Migration reconciliation break remains open with owner and aging. |

## Example 38. Fund liquidity stress waterfall

### Scenario

A discretionary portfolio needs SGD 500,000 cash within 10 business days. It holds several fund positions with different liquidity terms and estimated execution reliability.

| Holding | Market value | Liquidity term | Stress haircut | Stress-usable amount |
|---|---:|---|---:|---:|
| Daily UCITS fund | 220,000 | T+3 dealing | 2% | 215,600 |
| Weekly bond fund | 180,000 | next weekly dealing | 5% | 171,000 |
| Quarterly alternatives fund | 300,000 | 60-day notice | 20% | 0 for 10-day need |
| Money market fund | 160,000 | T+1 dealing | 0% | 160,000 |

### Stress liquidity

```text
ten_day_stress_liquidity = 215,600 + 171,000 + 160,000 = 546,600
surplus = 546,600 - 500,000 = 46,600
```

### Correct treatment

- use horizon-specific liquidity, not total market value;
- apply gates, notice periods, dealing frequency, settlement lag and stress haircuts;
- preserve whether liquidity depends on estimated sale, confirmed redemption or already settled cash;
- exclude client-specific restricted or side-letter-limited holdings unless permitted.

### QA assertions

| Test | Expected result |
|---|---|
| Liquidity need is 10 business days | Quarterly fund is excluded from immediate funding. |
| Weekly dealing date has passed | Weekly fund moves to next eligible window. |
| Fund is gated | Stress-usable amount is capped by confirmed gate. |
| Report is generated | Liquidity waterfall shows horizon, haircut and source state. |

## Example 39. Private fund recallable distribution

### Scenario

A private fund distributes cash but designates part of it as recallable. The client receives cash now, but unfunded commitment can increase again.

| Attribute | Value |
|---|---:|
| Opening commitment | 1,000,000 |
| Paid-in before distribution | 700,000 |
| Distribution received | 120,000 |
| Recallable portion | 80,000 |
| Non-recallable portion | 40,000 |

### Commitment impact

```text
cash_received = 120,000
paid_in_after_distribution = 700,000 - 120,000 = 580,000
recallable_commitment_increase = 80,000
effective_unfunded_commitment = 300,000 + 80,000 = 380,000
```

### Correct treatment

- separate cash received from recallable commitment exposure;
- do not treat recallable distribution as permanent liquidity surplus;
- update capital-call forecasting, commitment reporting and suitability/liquidity analytics;
- preserve administrator notice, recallable amount and expiry or conditions where provided.

### QA assertions

| Test | Expected result |
|---|---|
| Distribution is marked recallable | Unfunded commitment or recallable exposure increases. |
| Recallable split is missing | Distribution classification is incomplete. |
| Future capital call uses recallable amount | Call is matched to prior recallable distribution where sourced. |
| Client report shows cash only | QA fails because contingent recall exposure is hidden. |

## Example 40. ETF market-maker withdrawal

### Scenario

An ETF's primary market maker withdraws during volatile markets. Exchange trading continues, but spreads widen and creation/redemption liquidity becomes uncertain.

| Attribute | Before | Stress state |
|---|---:|---:|
| ETF NAV | 100.00 | 100.00 |
| Market bid | 99.90 | 96.80 |
| Market ask | 100.10 | 101.90 |
| Primary market maker | Active | Withdrawn |

### Liquidity analytics

```text
bid_discount_to_nav = (96.80 - 100.00) / 100.00 = -3.20%
bid_ask_spread = 101.90 - 96.80 = 5.10
```

### Correct treatment

- keep ETF position valued according to policy, but surface liquidity stress separately;
- distinguish NAV, exchange bid/ask and creation/redemption basket liquidity;
- block or escalate large orders if execution quality cannot be supported;
- preserve market-maker status source and timestamp.

### QA assertions

| Test | Expected result |
|---|---|
| Market maker withdraws | ETF liquidity state degrades. |
| Bid trades at large discount | Premium/discount analytics show stress. |
| Client tries large sell | Order is routed for liquidity review or staged execution. |
| NAV remains stable | Report still explains market-price liquidity stress. |

## Example 41. Cross-border share-class conversion restriction

### Scenario

A client wants to convert from a distributing share class to an accumulating share class, but the accumulating class is not eligible in the client's booking centre.

| Attribute | Current class | Target class |
|---|---|---|
| Distribution policy | Distributing | Accumulating |
| Currency | USD | USD |
| Booking-centre eligibility | Eligible | Not eligible |
| Conversion fee | 0.25% | 0.25% |

### Correct treatment

- treat conversion as blocked until target share-class eligibility is source-confirmed;
- do not simulate conversion as redemption/subscription unless tax, cost-basis and dealing policy allow it;
- preserve eligibility source, booking-centre rule and advisor/client decision;
- keep current share class and income treatment until conversion is accepted.

### QA assertions

| Test | Expected result |
|---|---|
| Target class is ineligible | Conversion is blocked before order release. |
| Advisor requests workaround | Alternative share class or redemption/subscription path requires suitability and tax review. |
| Eligibility changes later | Conversion becomes available only from effective date. |
| Report shows projected income | Current distributing class remains basis until accepted conversion. |

## Example 42. Administrator fee restatement

### Scenario

A fund administrator restates prior NAV because management fee accrual was understated for five dealing days.

| Attribute | Original | Restated |
|---|---:|---:|
| Client units | 15,000 | 15,000 |
| Original NAV | 20.0000 | - |
| Restated NAV | - | 19.9600 |
| NAV delta | - | -0.0400 |

### Valuation impact

```text
original_value = 15,000 x 20.0000 = 300,000
restated_value = 15,000 x 19.9600 = 299,400
valuation_delta = -600
```

### Correct treatment

- preserve original and restated NAV with administrator notice and reason;
- restate valuation, performance and reporting only for affected dates and holdings;
- do not book a cash fee unless administrator confirms a cash movement;
- label client reports or corrections according to publication policy.

### QA assertions

| Test | Expected result |
|---|---|
| Restated NAV arrives | Historical valuation is versioned with reason. |
| Report was already published | Correction workflow opens if materiality threshold is breached. |
| Fee accrual changed but no cash moved | No standalone cash fee is posted. |
| Performance is recalculated | Original and restated return are reconcilable. |

## Example 43. Tax-lot continuity through fund-platform migration

### Scenario

A fund holding migrates to a new platform account. Units are unchanged, but tax-lot history must remain available for future redemption reporting.

| Lot | Units | Cost per unit | Acquisition date | Migration state |
|---|---:|---:|---|---|
| Lot A | 6,000 | 9.50 | 2024-02-15 | Migrated |
| Lot B | 4,000 | 10.20 | 2025-06-10 | Migrated |
| Total | 10,000 | - | - | Reconciled |

### Correct treatment

- migrate units and lot history without creating a redemption/subscription;
- preserve acquisition dates, cost basis, source account and target account;
- reconcile total units, lot units and transfer-agent balance;
- block future tax-lot disposal reporting if lot migration is incomplete.

### QA assertions

| Test | Expected result |
|---|---|
| Total units migrate but lots do not | Position is usable for valuation but tax-lot reporting is degraded. |
| Future redemption occurs | Lot selection uses migrated acquisition/cost data. |
| Target account id changes | Lineage maps old and new platform identifiers. |
| Migration creates cashflow | QA fails because beneficial ownership did not change. |

## Example 44. Fund liquidity facility drawdown

### Scenario

An open-ended fund uses a short-term liquidity facility to meet redemptions before portfolio assets settle. The fund NAV remains published, but leverage and liquidity risk need to be disclosed to portfolio analytics and advisory controls.

| Attribute | Value |
|---|---:|
| Fund NAV before facility | 100,000,000 |
| Liquidity facility drawdown | 5,000,000 |
| Facility rate | 6.00% |
| Expected days outstanding | 10 |
| Fund assets sold but unsettled | 5,200,000 |

### Facility cost estimate

```text
facility_interest_cost = 5,000,000 x 6.00% x 10 / 360 = 8,333.33
facility_drawdown_ratio = 5,000,000 / 100,000,000 = 5.00%
```

### Correct treatment

- keep the client holding as fund units; do not create a client-level loan unless the client directly borrows;
- track facility drawdown as fund-level leverage/liquidity disclosure where sourced;
- reflect expected cost through NAV when administrator confirms it, not as a direct client cash charge;
- escalate advisory, DPM and liquidity analytics if facility use indicates redemption stress.

### QA assertions

| Test | Expected result |
|---|---|
| Facility drawdown is reported | Fund liquidity/leverage state updates with source date. |
| NAV is published before facility cost is reflected | Analytics label cost estimate separately from confirmed NAV. |
| Facility ratio breaches policy threshold | Advisory/DPM review opens. |
| Client report is generated | Fund-level borrowing is explained as fund risk, not client borrowing. |

## Example 45. Redemption-in-kind basket under stress

### Scenario

A fund meets a large redemption by delivering securities in kind rather than cash. The client receives a basket with valuation, liquidity and operational settlement implications.

| Attribute | Value |
|---|---:|
| Redemption units | 20,000 |
| Dealing NAV | 50.00 |
| Gross redemption value | 1,000,000 |
| Cash portion | 250,000 |
| In-kind basket fair value | 750,000 |

### Redemption split

```text
gross_redemption_value = 20,000 x 50.00 = 1,000,000
in_kind_ratio = 750,000 / 1,000,000 = 75.00%
cash_ratio = 250,000 / 1,000,000 = 25.00%
```

### Correct treatment

- reduce fund units based on accepted redemption;
- create received securities only after identifiers, quantities, custody eligibility and settlement confirmation are sourced;
- keep valuation, liquidity and suitability checks for the delivered basket separate from fund redemption cash;
- report cash and in-kind proceeds distinctly, with residual unsettled components visible.

### QA assertions

| Test | Expected result |
|---|---|
| In-kind basket identifiers are missing | Securities are not finalized and redemption remains partially pending. |
| Basket contains restricted assets | Custody, eligibility and suitability exceptions open. |
| Cash portion settles before securities | Cash and in-kind legs have separate settlement states. |
| Report shows redemption as cash only | QA fails because in-kind proceeds are hidden. |

## Example 46. Stale private-fund capital-account statement

### Scenario

A private fund has not issued a current capital-account statement. The portfolio report must decide whether to use the latest available NAV, mark it stale, or block final reporting depending on policy.

| Attribute | Value |
|---|---:|
| Last capital-account NAV | 880,000 |
| Statement date | 2026-03-31 |
| Report date | 2026-06-30 |
| Staleness threshold | 60 days |
| Days stale | 91 |

### Staleness test

```text
days_since_statement = 91
stale_by = 91 - 60 = 31 days
```

### Correct treatment

- preserve last administrator statement value and date;
- mark valuation stale when it breaches policy threshold;
- avoid fabricating interim NAV unless an approved estimate process exists;
- reflect stale valuation in report disclosures, performance quality flags and risk analytics coverage.

### QA assertions

| Test | Expected result |
|---|---|
| Statement age exceeds threshold | Valuation is labelled stale or reporting is blocked per policy. |
| Estimated NAV is entered | Estimate requires source, approver, method and expiry. |
| Capital call occurs after stale statement | Commitment/cashflow updates separately from stale NAV. |
| Client report is generated | Report shows valuation date and stale-state disclosure. |

## Example 47. Fund manager replacement

### Scenario

A fund replaces its portfolio manager after underperformance. The legal fund remains the same, but risk, mandate due diligence, watchlist status and recommendation rules may change.

| Attribute | Before | After |
|---|---|---|
| Fund ISIN | Same | Same |
| Manager | Manager A | Manager B |
| Watchlist status | Normal | Under review |
| Existing holding | Allowed | Hold allowed, new buys blocked |

### Correct treatment

- do not treat manager replacement as a fund merger, sale or instrument change;
- preserve effective date, board notice, manager identity, strategy change and due-diligence state;
- allow existing holdings, new purchases and DPM use according to approved-universe policy;
- refresh suitability, target market, risk rating and model-portfolio eligibility where required.

### QA assertions

| Test | Expected result |
|---|---|
| Manager changes but fund identifier remains same | Position continuity is preserved. |
| New buys are blocked during review | Order entry enforces watchlist status. |
| DPM model includes the fund | Model review opens if approved-universe status changes. |
| Client report is produced | Manager change is disclosed only where reporting policy requires. |

## Example 48. Side-pocket write-off

### Scenario

A fund side pocket contains an illiquid position that is written down to zero by the administrator. The main fund position remains active.

| Attribute | Value |
|---|---:|
| Side-pocket units | 5,000 |
| Prior side-pocket NAV | 12.00 |
| New side-pocket NAV | 0.00 |
| Main fund units | 30,000 |

### Write-off impact

```text
prior_side_pocket_value = 5,000 x 12.00 = 60,000
new_side_pocket_value = 5,000 x 0.00 = 0
valuation_write_down = -60,000
```

### Correct treatment

- keep main fund and side-pocket positions separate if the administrator reports them separately;
- process write-off as valuation change unless administrator confirms cancellation, distribution or taxable event;
- preserve administrator notice, effective date, affected sleeve and reason;
- report write-off impact separately from main fund market movement where possible.

### QA assertions

| Test | Expected result |
|---|---|
| Side-pocket NAV goes to zero | Side-pocket value is written down with source lineage. |
| Main fund NAV remains active | Main fund position is not closed. |
| Administrator later cancels side-pocket units | Cancellation event is separate from valuation write-down. |
| Performance report is run | Write-off contribution is explainable and not hidden in ordinary fund return. |

## Example 49. ETF index rebalance execution

### Scenario

An ETF tracks an index that rebalances quarterly. The ETF portfolio changes constituents, which affects look-through exposure, turnover, transaction costs and tax reporting, while the client still holds the same ETF units.

| Attribute | Before rebalance | After rebalance |
|---|---:|---:|
| Client ETF units | 12,000 | 12,000 |
| ETF NAV | 25.00 | 24.92 |
| Look-through coverage | 98% | 98% |
| Estimated rebalance cost impact | - | 0.08 per unit |

### NAV cost impact

```text
estimated_rebalance_cost = 12,000 x 0.08 = 960
post_rebalance_value = 12,000 x 24.92 = 299,040
```

### Correct treatment

- keep client accounting units unchanged unless the client trades;
- update ETF look-through constituents from the effective basket/index file;
- explain NAV impact, turnover and tracking difference without booking client-level constituent trades;
- refresh risk, sector, country, duration or factor exposures from the new basket.

### QA assertions

| Test | Expected result |
|---|---|
| Index rebalance file arrives | Look-through exposure updates from effective date. |
| Client did not trade ETF | Client units remain unchanged. |
| NAV falls due to rebalance cost | Performance reflects NAV movement, not client trade cost. |
| Basket file is late | Look-through analytics are labelled stale/source-limited. |

## Example 50. Fund distribution waterfall

### Scenario

A fund pays a distribution made up of income, realized gains and return of capital. The client receives cash after withholding tax. Reporting must show the economic components rather than treating the entire amount as ordinary income.

| Component | Amount per unit |
|---|---:|
| Interest and dividend income | 0.1800 |
| Realized capital gain | 0.0700 |
| Return of capital | 0.0500 |
| Total distribution | 0.3000 |

Client units are 20,000 and withholding applies only to the income component at 15%.

### Distribution calculation

```text
gross_distribution = 20,000 x 0.3000 = 6,000
income_component = 20,000 x 0.1800 = 3,600
capital_gain_component = 20,000 x 0.0700 = 1,400
return_of_capital_component = 20,000 x 0.0500 = 1,000
withholding = 3,600 x 15% = 540
net_cash = 6,000 - 540 = 5,460
```

### Correct treatment

- preserve fund administrator tax breakdown and effective date;
- report income, capital gain and return-of-capital components separately where source-backed;
- reduce cost basis for return of capital only where policy and jurisdiction require it;
- do not classify the full distribution as yield when part of it is capital return.

### QA assertions

| Test | Expected result |
|---|---|
| Distribution breakdown is available | Report shows income, gain and capital-return components. |
| Tax breakdown arrives late | Initial report is labelled estimated or corrected when final breakdown arrives. |
| Return of capital exists | Cost-basis treatment follows configured jurisdiction and source evidence. |
| Withholding applies only to income | Withholding is not applied to every component unless source policy says so. |

## Example 51. Series accounting equalization for hedge fund subscription

### Scenario

A hedge fund uses series accounting. A client subscribes mid-performance period into a new series so performance-fee equalization is isolated from earlier investors.

| Attribute | Value |
|---|---:|
| Subscription amount | 500,000 |
| Series NAV at subscription | 100.00 |
| Units issued | 5,000 |
| Series high-water mark | 100.00 |
| Later series NAV | 112.00 |
| Performance fee rate | 20% |

### Fee accrual

```text
gross_gain_per_unit = 112.00 - 100.00 = 12.00
performance_fee_per_unit = 12.00 x 20% = 2.40
performance_fee_accrual = 5,000 x 2.40 = 12,000
```

### Correct treatment

- assign units to the correct series at subscription;
- calculate high-water mark and performance fee by series, not only by fund;
- merge or convert series only when administrator confirms the event;
- preserve administrator series id, subscription date, NAV and fee method.

### QA assertions

| Test | Expected result |
|---|---|
| Client subscribes mid-period | New units receive the correct series id. |
| Performance fee accrues | Fee uses series high-water mark and series NAV. |
| Series merge notice arrives | Units convert only from source-confirmed ratio and date. |
| Report aggregates fund holding | Series-level fee economics remain traceable underneath the summary. |

## Example 52. Retirement-plan eligibility restriction

### Scenario

A fund is available to ordinary discretionary accounts but not to a retirement-plan account because the share class fails an eligibility rule.

| Attribute | Value |
|---|---|
| Fund share class | Institutional accumulation class |
| Account type | Retirement plan |
| Eligibility status | Not eligible |
| Existing holding | None |
| Proposed subscription | 100,000 |

### Correct treatment

- evaluate fund eligibility by account type, jurisdiction, tax wrapper, investor type and share class;
- block new subscriptions when eligibility fails;
- do not use a generic fund approval to override wrapper-specific restrictions;
- preserve eligibility source, reason code, review date and allowed alternatives.

### QA assertions

| Test | Expected result |
|---|---|
| Retirement account orders ineligible class | Order is blocked before placement. |
| Same fund has eligible share class | Alternative class can be suggested if approved. |
| Existing holding becomes ineligible later | Holding, sell, switch and buy rules follow policy state. |
| Eligibility source is stale | Order entry blocks or requires review. |

## Example 53. Fund expense cap reimbursement

### Scenario

A fund has an expense cap. Actual expenses exceed the cap, so the manager reimburses the fund. Client reporting should show the NAV effect without booking a direct client cash payment unless the administrator distributes cash.

| Attribute | Value |
|---|---:|
| Average fund assets | 200,000,000 |
| Actual expense ratio | 1.25% |
| Expense cap | 1.00% |
| Client ownership share | 0.40% |
| Days | 365 |

### Reimbursement estimate

```text
excess_expense_rate = 1.25% - 1.00% = 0.25%
fund_reimbursement = 200,000,000 x 0.25% = 500,000
client_nav_benefit_estimate = 500,000 x 0.40% = 2,000
```

### Correct treatment

- treat reimbursement as fund-level NAV support unless client-level cash distribution is confirmed;
- preserve expense-cap agreement, actual expense source, reimbursement notice and NAV impact;
- distinguish expense-cap economics from trailer rebates or client fee discounts;
- report as estimate until administrator confirms final NAV and reimbursement amount.

### QA assertions

| Test | Expected result |
|---|---|
| Expense cap is breached | Reimbursement estimate is calculated at fund level. |
| Administrator confirms NAV | Client value reflects NAV, not a separate cash receivable unless confirmed. |
| Client fee report is generated | Expense-cap reimbursement is not confused with advisory fee rebate. |
| Cap agreement expires | Future calculations stop or move to exception state. |

## Example 54. ETF heartbeat trade and low-basis basket

### Scenario

An ETF uses an in-kind creation/redemption basket that removes low-basis securities from the fund. The client still holds ETF units, but tax and tracking analytics need to understand the fund-level event.

| Attribute | Value |
|---|---:|
| Client ETF units | 15,000 |
| ETF NAV before event | 40.00 |
| ETF NAV after event | 40.02 |
| Low-basis securities removed by ETF | 12,000,000 |
| Client direct taxable sale | 0 |

### Client value

```text
client_value_before = 15,000 x 40.00 = 600,000
client_value_after = 15,000 x 40.02 = 600,300
nav_change = 300
```

### Correct treatment

- keep client accounting position as ETF units;
- do not book client-level sales of the ETF basket constituents;
- track heartbeat event as ETF-level tax/portfolio-management context where source-backed;
- refresh look-through holdings and tax-distribution expectations after the basket event.

### QA assertions

| Test | Expected result |
|---|---|
| ETF heartbeat basket is reported | Look-through and tax context update at ETF level. |
| Client did not trade ETF units | No client realized gain/loss is booked. |
| Basket file is missing | Event is source-limited and not used for detailed look-through. |
| Tax estimate is shown | Estimate is labelled fund-level and not client tax advice. |

## Example 55. Fund liquidation tax reporting

### Scenario

A fund liquidates and pays a final distribution. The event closes the fund position, but tax reporting must separate final proceeds, cost basis, withholding and any post-liquidation receivable.

| Attribute | Value |
|---|---:|
| Units held | 12,000 |
| Liquidation NAV | 18.50 |
| Gross liquidation proceeds | 222,000 |
| Cost basis | 195,000 |
| Withholding | 3,000 |
| Estimated post-liquidation receivable | 4,000 |

### Liquidation result

```text
realized_gain_before_withholding = 222,000 - 195,000 = 27,000
net_cash_received = 222,000 - 3,000 = 219,000
estimated_receivable = 4,000
```

### Correct treatment

- close fund units only when liquidation is final and transfer agent confirms cancellation;
- record realized gain/loss from proceeds and cost basis according to configured tax-lot policy;
- keep post-liquidation receivable separate from final cash until confirmed;
- preserve liquidation notice, NAV, cancellation date, withholding and tax breakdown.

### QA assertions

| Test | Expected result |
|---|---|
| Liquidation proceeds settle | Fund units close and cash posts with liquidation lineage. |
| Post-liquidation receivable is estimated | Receivable is labelled estimated and excluded from available cash until confirmed. |
| Tax-lot history is missing | Realized gain/loss is provisional or blocked according to policy. |
| Withholding is present | Net cash, gross proceeds and withholding are separately reportable. |

## Example 56. Share-class closure migration

### Scenario

A fund manager closes an old share class and migrates eligible investors into a successor class. The client should not be treated as selling and repurchasing unless the source event is actually a taxable switch or redemption/subscription pair.

| Attribute | Old class | New class |
|---|---:|---:|
| Units | 10,000 | 9,850 |
| NAV | 12.30 | 12.4873 |
| Market value | 123,000 | 123,000 |
| Ongoing charge | 1.20% | 0.95% |

### Conversion ratio

```text
conversion_ratio = old_market_value / new_nav / old_units
conversion_ratio = 123,000 / 12.4873 / 10,000 = 0.9850
new_units = 10,000 x 0.9850 = 9,850
```

### Correct treatment

- preserve closure notice, successor share class, conversion ratio, effective date and tax treatment;
- create the new share-class position from confirmed conversion terms;
- carry cost basis and acquisition history only when the event is source-confirmed as non-disposal or tax-continuity;
- update fees, eligibility, reporting label and product documents from the new class.

### QA assertions

| Test | Expected result |
|---|---|
| Closure conversion is confirmed | Old class closes and new class opens using source conversion ratio. |
| Tax treatment is missing | Cost-basis continuity is blocked or labelled provisional. |
| New class has lower fee | Reporting and advisory materials use the new ongoing charge. |
| Investor is ineligible for successor class | Migration is exceptioned instead of forced. |

## Example 57. Cross-border trailer-fee clean-share conversion

### Scenario

A jurisdiction bans retrocession for a client segment. The platform converts a trailer-paying share class into a clean share class and must separate fund economics, advisory fee changes and client disclosure.

| Attribute | Trailer class | Clean class |
|---|---:|---:|
| Units before conversion | 8,000 | n/a |
| Old NAV | 15.00 | n/a |
| New NAV | n/a | 14.85 |
| Trailer rate | 0.50% | 0.00% |
| Advisory fee change | n/a | +0.30% |

### Converted units

```text
old_value = 8,000 x 15.00 = 120,000
new_units = 120,000 / 14.85 = 8,080.8081
```

### Correct treatment

- preserve regulatory trigger, client segment, old class, clean class, conversion ratio and disclosure evidence;
- stop trailer-fee accrual from the conversion effective date;
- create advisory or platform fee changes only from approved fee schedule and client disclosure;
- do not present the clean class as cheaper without considering external advisory-fee changes.

### QA assertions

| Test | Expected result |
|---|---|
| Clean-share conversion is confirmed | Old class closes and clean class opens from source ratio. |
| Trailer-fee stop date is reached | Trailer accrual stops from effective date. |
| Advisory fee schedule is missing | All-in cost comparison is labelled incomplete. |
| Cross-border eligibility differs | Conversion is blocked or routed to exception workflow. |

## Example 58. Private-fund clawback escrow

### Scenario

A private equity fund returns capital, but part of the distribution is held in escrow for potential clawback. Portfolio reporting must separate received cash from contingent receivable and possible clawback obligation.

| Attribute | Value |
|---|---:|
| Gross distribution | 250,000 |
| Cash released now | 210,000 |
| Escrow holdback | 40,000 |
| Potential clawback period | 18 months |

### Cash and escrow split

```text
cash_received = 210,000
escrow_receivable = 40,000
cash_release_percentage = 210,000 / 250,000 = 84.00%
```

### Correct treatment

- post only released cash as settled cash;
- track escrow holdback as contingent or restricted receivable according to policy;
- preserve distribution notice, escrow agreement, clawback period, release conditions and expiry date;
- avoid counting escrow as available cash, income yield or realized profit until confirmed.

### QA assertions

| Test | Expected result |
|---|---|
| Escrow holdback exists | Settled cash and contingent receivable are separated. |
| Escrow release is confirmed | Receivable converts to cash from settlement date. |
| Clawback is triggered | Obligation is recorded with fund notice and approval lineage. |
| Performance report is generated | Cash distribution, escrow and contingent liability are explainable. |

## Example 59. ETF custom basket rejection

### Scenario

An authorized participant submits a custom ETF creation basket, but the ETF sponsor rejects it because one security breaches concentration and liquidity rules. The client may still trade ETF units, but primary-market basket analytics must not assume the rejected basket is valid.

| Attribute | Value |
|---|---:|
| Proposed basket value | 5,000,000 |
| Rejected security value | 750,000 |
| Sponsor limit per security | 10.00% |
| Proposed rejected-security weight | 15.00% |

### Breach amount

```text
allowed_security_value = 5,000,000 x 10.00% = 500,000
excess_security_value = 750,000 - 500,000 = 250,000
```

### Correct treatment

- keep secondary-market ETF holdings separate from rejected primary-market basket workflow;
- preserve submitted basket, sponsor rejection reason, limit rule, timestamp and revised basket state;
- do not update look-through or creation/redemption economics from a rejected basket;
- route basket exception to operations or ETF execution desk before relying on it for liquidity assumptions.

### QA assertions

| Test | Expected result |
|---|---|
| Custom basket is rejected | Basket is not used for confirmed look-through or settlement analytics. |
| Revised basket is accepted | Analytics update only from accepted basket version. |
| Client trades ETF on exchange | Trade lifecycle is independent from AP basket rejection. |
| Liquidity report is generated | Report labels rejected basket state and does not overstate primary-market liquidity. |

## Example 60. Feeder-fund currency-hedged class break

### Scenario

A feeder fund offers currency-hedged share classes. A hedge execution break causes the hedged class NAV to diverge from expected hedged performance. Reporting must distinguish feeder NAV, hedge break, currency exposure and administrator correction.

| Attribute | Value |
|---|---:|
| Client units | 6,000 |
| Expected hedged NAV | 104.20 |
| Reported NAV | 103.70 |
| Difference per unit | 0.50 |

### NAV impact

```text
hedge_break_impact = 6,000 x (104.20 - 103.70) = 3,000
reported_value = 6,000 x 103.70 = 622,200
expected_hedged_value = 6,000 x 104.20 = 625,200
```

### Correct treatment

- use administrator-confirmed NAV for official valuation while preserving hedge-break explanation;
- track hedge execution break, affected class, correction status and administrator notice;
- do not overwrite official NAV with expected hedged NAV unless restatement is confirmed;
- label performance attribution as source-limited when hedge-break data is incomplete.

### QA assertions

| Test | Expected result |
|---|---|
| Hedge break notice is received | NAV impact and class state are tracked separately. |
| Administrator restates NAV | Historical valuation and performance are versioned with restatement lineage. |
| No restatement is confirmed | Official valuation remains reported NAV. |
| Client report is generated | Report explains currency-hedged class break without inventing a cash event. |

## Example 61. Fund board suspension vote

### Scenario

A fund board votes to suspend subscriptions and redemptions after a liquidity event. Existing holdings remain valued under the stated policy, but dealing, liquidity projections and advisory recommendations must reflect the suspension.

| Attribute | Value |
|---|---|
| Board vote date | 2026-06-15 |
| Suspension effective date | 2026-06-16 |
| Affected activity | Subscriptions and redemptions |
| Last official NAV | 18.75 |
| Next review date | 2026-07-15 |

### Correct treatment

- preserve board resolution, effective date, affected dealing activities, NAV policy and review date;
- block or queue new dealing instructions according to suspension terms;
- continue valuation only under the disclosed NAV policy and stale/degraded-state rules;
- show liquidity as suspended rather than low or estimated.

### QA assertions

| Test | Expected result |
|---|---|
| Suspension is effective | New subscription/redemption orders are blocked or queued by policy. |
| Existing holding is reported | Position remains visible with suspended liquidity state. |
| NAV is stale after suspension | Valuation is labelled stale or source-limited according to policy. |
| Board lifts suspension | Dealing state changes only from source-confirmed effective date. |

## Example 62. Private-fund continuation vehicle election

### Scenario

A private equity fund reaches end of life and offers investors an election: sell their interest for cash or roll into a continuation vehicle. The client elects to roll 70% and receive cash for 30%. The platform must preserve the original investment history while creating the new vehicle position only from confirmed election and transfer terms.

| Attribute | Value |
|---|---:|
| Existing fund NAV | 1,500,000 |
| Roll election | 70.00% |
| Cash exit election | 30.00% |
| Continuation vehicle NAV basis | 1,050,000 |
| Cash proceeds before costs | 450,000 |
| Transaction costs | 7,500 |

### Election split

```text
roll_value = 1,500,000 x 70.00% = 1,050,000
cash_exit_value = 1,500,000 x 30.00% = 450,000
net_cash_proceeds = 450,000 - 7,500 = 442,500
```

### Correct treatment

- close or reduce the old fund position only after confirmed election processing;
- create the continuation vehicle as a new position with its own identifier, terms, liquidity and valuation policy;
- preserve cost-basis, commitment, recallable distribution and tax treatment according to source guidance;
- separate rollover value, cash exit proceeds, costs and any unrealized gain/loss treatment;
- route suitability and product-governance checks for the continuation vehicle rather than assuming automatic eligibility.

### QA assertions

| Test | Expected result |
|---|---|
| Election is unconfirmed | Old position remains unchanged or source-limited. |
| Partial rollover is confirmed | Old fund reduces, new vehicle opens, and net cash is posted separately. |
| Continuation vehicle identifier is missing | New position creation is blocked until instrument setup completes. |
| Client report is generated | Rollover, cash exit, costs and residual exposure are separately explainable. |

## Example 63. ETF authorized-participant concentration failure

### Scenario

An ETF relies heavily on one authorized participant for creation/redemption activity. That AP withdraws during market stress, widening ETF premium/discount and reducing primary-market liquidity. The client still holds ETF shares, but liquidity analytics must reflect AP concentration risk.

| Attribute | Normal state | Stress state |
|---|---:|---:|
| Active AP count | 5 | 2 |
| Largest AP creation share | 35.00% | 72.00% |
| ETF NAV | 100.00 | 100.00 |
| Market bid | 99.80 | 96.50 |

### Liquidity discount

```text
bid_discount_to_nav = (96.50 - 100.00) / 100.00 = -3.50%
ap_concentration_change = 72.00% - 35.00% = 37.00%
```

### Correct treatment

- keep ETF legal holding and exchange price valuation separate from creation/redemption liquidity analytics;
- preserve AP status, market-maker status, ETF sponsor notices, bid/ask, NAV and basket availability;
- degrade liquidity assumptions when AP concentration or market-maker withdrawal breaches policy thresholds;
- explain NAV premium/discount separately from underlying fund NAV movement;
- avoid using normal creation/redemption settlement assumptions during AP stress.

### QA assertions

| Test | Expected result |
|---|---|
| AP count falls below threshold | ETF liquidity state degrades or triggers review. |
| Bid discount widens materially | Liquidity analytics show discount to NAV and market stress. |
| Client sells on exchange | Sale uses executable market price, not NAV. |
| Look-through report is generated | Legal ETF holding, underlying exposure and liquidity quality remain distinct. |

## Example 64. Fund tax-character restatement after year end

### Scenario

A fund initially reports a year-end distribution as ordinary income. After year end, the fund administrator restates part of the distribution as return of capital. Client tax packs, income analytics and cost basis need correction lineage.

| Attribute | Original | Restated |
|---|---:|---:|
| Gross distribution | 120,000 | 120,000 |
| Ordinary income component | 120,000 | 80,000 |
| Return of capital component | 0 | 40,000 |
| Cost-basis reduction | 0 | 40,000 |

### Restatement impact

```text
ordinary_income_delta = 80,000 - 120,000 = -40,000
return_of_capital_delta = 40,000 - 0 = 40,000
```

### Correct treatment

- preserve original tax character, restated tax character, administrator notice and effective tax year;
- update client tax pack and income analytics through correction workflow rather than overwriting prior output silently;
- reduce cost basis only when return-of-capital treatment is source-confirmed and applicable under reporting policy;
- distinguish accounting distribution cash from tax-character restatement;
- record whether amended client reporting or next-cycle correction is required.

### QA assertions

| Test | Expected result |
|---|---|
| Restatement notice arrives after tax pack | Correction workflow opens and impacted reports are identified. |
| Cash amount is unchanged | Cash ledger does not repost the distribution. |
| Return of capital increases | Cost-basis adjustment is source-backed and traceable. |
| Client report is regenerated | Original and corrected tax character remain auditable. |

## Example 65. UCITS concentration breach remediation

### Scenario

A UCITS fund breaches a concentration limit because one issuer's value rises sharply. The breach is passive, but the fund must remediate within policy. Client reporting should explain the breach as fund-level compliance risk, not direct client concentration in a legally owned security.

| Attribute | Limit | Actual |
|---|---:|---:|
| Single issuer limit | 10.00% |
| Issuer exposure after move | 12.40% |
| Fund NAV | 500,000,000 |
| Excess exposure value | 12,000,000 |

### Breach amount

```text
allowed_exposure = 500,000,000 x 10.00% = 50,000,000
actual_exposure = 500,000,000 x 12.40% = 62,000,000
excess_exposure = 62,000,000 - 50,000,000 = 12,000,000
```

### Correct treatment

- preserve fund-level compliance notice, passive/active breach classification, remediation deadline and regulator/fund-board communication where available;
- show client legal holding as fund units while look-through analytics reflect issuer concentration;
- avoid forcing client sale unless product governance, mandate or advisory policy requires it;
- update risk, suitability and approved-universe status if breach persists or changes product risk;
- track remediation state and effective date.

### QA assertions

| Test | Expected result |
|---|---|
| Passive breach notice is received | Fund compliance state updates without creating client transaction. |
| Breach persists past deadline | Product-governance or advisory review is triggered. |
| Look-through file is stale | Concentration breach reporting is labelled stale or source-limited. |
| Client report is generated | Fund units and look-through issuer concentration are clearly separated. |

## Example 66. Private-credit fund PIK income reporting

### Scenario

A private-credit fund receives payment-in-kind interest from a borrower. The fund NAV increases, but no cash is distributed to the client. Reporting must avoid presenting PIK economics as client cash income unless the fund distributes cash.

| Attribute | Value |
|---|---:|
| Client fund units | 100,000 |
| Prior NAV per unit | 10.00 |
| PIK-driven NAV uplift per unit | 0.08 |
| New NAV per unit | 10.08 |
| Client value uplift | 8,000 |

### NAV uplift

```text
client_value_uplift = 100,000 x 0.08 = 8,000
new_market_value = 100,000 x 10.08 = 1,008,000
```

### Correct treatment

- treat PIK as fund-level income affecting NAV or capital account unless administrator confirms client distribution;
- preserve borrower PIK notice if available, fund administrator statement, NAV bridge and income classification;
- separate NAV appreciation, distributable income, cash distribution and tax-reporting character;
- label analytics as estimate or source-limited when the fund does not provide enough income breakdown;
- avoid booking client cash income without cash distribution or tax reporting source.

### QA assertions

| Test | Expected result |
|---|---|
| PIK income is fund-level only | Client cash balance does not increase. |
| NAV increases from PIK | Market value updates through NAV and attribution explains source if available. |
| Fund later distributes cash | Distribution is booked as separate cash event. |
| Income breakdown is missing | Reporting labels PIK attribution as source-limited. |

## Example 67. Fund distribution clawback notice

### Scenario

A private fund issues a clawback notice after a prior distribution. The notice requires part of the earlier cash distribution to be returned. The platform must separate original distribution, clawback payable, cash repayment and performance correction.

| Attribute | Value |
|---|---:|
| Prior distribution cash | 300,000 |
| Clawback percentage | 12.00% |
| Clawback payable | 36,000 |
| Payment due date | 2026-09-30 |

### Clawback payable

```text
clawback_payable = prior_distribution_cash x clawback_percentage
clawback_payable = 300,000 x 12.00% = 36,000
net_retained_distribution = 300,000 - 36,000 = 264,000
```

### Correct treatment

- preserve clawback notice, prior distribution id, calculation basis, due date and approval state;
- record payable or contingent obligation before cash repayment where policy requires;
- do not delete or overwrite the original distribution event;
- update performance, cashflow and tax reporting according to correction policy and source guidance;
- track whether the clawback is funded by cash, offset against future distributions or disputed.

### QA assertions

| Test | Expected result |
|---|---|
| Clawback notice is received | Payable or contingent obligation is created with source lineage. |
| Cash repayment is made | Cash outflow is linked to the clawback notice and prior distribution. |
| Clawback is disputed | Obligation remains pending or disputed rather than removed. |
| Client report is generated | Original distribution, clawback payable and net retained amount are explainable. |
