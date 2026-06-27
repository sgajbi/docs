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

## Example 68. Semi-liquid fund queue switching

### Scenario

A semi-liquid fund offers quarterly liquidity with a redemption queue. A client originally elects cash redemption, then asks to switch the queued instruction into an in-kind rollover sleeve before the dealing deadline. The platform must preserve queue priority, election history and liquidity treatment.

| Attribute | Value |
|---|---:|
| Queued redemption value | 750,000 |
| Original cash election | 100.00% |
| Revised rollover election | 60.00% |
| Revised cash election | 40.00% |
| Queue position | 18 |

### Revised election split

```text
rollover_value = 750,000 x 60.00% = 450,000
cash_redemption_value = 750,000 x 40.00% = 300,000
```

### Correct treatment

- preserve original election, revised election, queue position, cut-off and administrator acceptance;
- do not reset queue priority unless the fund terms say the switch creates a new instruction;
- create rollover sleeve exposure only after source-confirmed acceptance and identifier setup;
- keep cash redemption, rollover value, fees and tax treatment separately explainable;
- label liquidity as queued, switched, accepted or rejected rather than freely redeemable.

### QA assertions

| Test | Expected result |
|---|---|
| Switch request arrives before deadline | Election changes with history and queue treatment preserved. |
| Fund terms reset queue priority | Queue position updates with source-backed reason. |
| Rollover sleeve identifier is missing | New position creation is blocked or source-limited. |
| Client report is generated | Cash, rollover and queued status are separately visible. |

## Example 69. ETF collateral basket failure

### Scenario

An ETF creation basket includes collateral securities that fail sponsor eligibility checks. The authorized participant must replace the failed securities before the creation order can settle.

| Basket component | Proposed value | Eligible value |
|---|---:|---:|
| Government bonds | 4,000,000 | 4,000,000 |
| Corporate bonds | 2,500,000 | 1,800,000 |
| Cash component | 500,000 | 500,000 |
| Failed collateral | 700,000 | 0 |

### Eligibility shortfall

```text
proposed_basket_value = 4,000,000 + 2,500,000 + 500,000 = 7,000,000
eligible_basket_value = 4,000,000 + 1,800,000 + 500,000 = 6,300,000
collateral_shortfall = 7,000,000 - 6,300,000 = 700,000
```

### Correct treatment

- keep ETF shares uncreated until basket acceptance and settlement are confirmed;
- preserve sponsor rejection reason, failed securities, replacement basket and AP response;
- do not use rejected basket for look-through, liquidity or tax analytics;
- show primary-market creation failure separately from secondary-market ETF trading;
- track whether the failure affects client orders, AP concentration or ETF liquidity quality.

### QA assertions

| Test | Expected result |
|---|---|
| Basket component fails eligibility | Creation order remains pending or rejected. |
| Replacement basket is accepted | ETF creation proceeds from accepted basket with lineage. |
| Client report is generated before acceptance | No ETF shares are shown from rejected basket. |
| Liquidity analytics are updated | Basket failure is visible as primary-market liquidity degradation. |

## Example 70. Private-fund valuation committee override

### Scenario

A private fund administrator reports a NAV, but the internal valuation committee approves a temporary override because a material portfolio company write-down is known but not yet reflected in the administrator statement.

| Attribute | Administrator NAV | Committee override |
|---|---:|---:|
| Client capital account | 2,400,000 | 2,160,000 |
| Override percentage | n/a | -10.00% |
| Effective period | Current month | Until next statement |

### Override impact

```text
valuation_override_delta = 2,160,000 - 2,400,000 = -240,000
override_percentage = -240,000 / 2,400,000 = -10.00%
```

### Correct treatment

- preserve administrator NAV, committee price, rationale, approval, expiry and review owner;
- label valuation basis as committee override, not official administrator NAV;
- expire or reapprove the override when the next administrator statement arrives;
- separate valuation override from cashflows, capital calls and distributions;
- route client reporting and performance attribution through approved degraded-state labels.

### QA assertions

| Test | Expected result |
|---|---|
| Override is approved | Valuation uses committee basis with label and expiry. |
| Approval is missing | Administrator NAV remains active or report is blocked. |
| Next statement arrives | Override expires, reconciles or requires reapproval. |
| Performance report is generated | Valuation basis and delta are explainable. |

## Example 71. Class hedging cost allocation

### Scenario

A fund has USD base shares and a CHF-hedged class. Hedge costs are charged only to the hedged class. The platform must allocate hedging cost to the correct share class without diluting unhedged class performance.

| Share class | Class NAV | Hedging cost | Units held |
|---|---:|---:|---:|
| USD unhedged | 25,000,000 | 0 | 80,000 |
| CHF hedged | 15,000,000 | 45,000 | 60,000 |

### Hedged-class cost per unit

```text
hedging_cost_per_hedged_unit = 45,000 / 60,000 = 0.75
hedging_cost_rate = 45,000 / 15,000,000 = 0.30%
```

### Correct treatment

- allocate hedging costs only to the hedged class or series specified by administrator data;
- preserve class NAV, hedge-cost notice, FX hedge source and allocation basis;
- avoid charging unhedged class holders for hedged-class cost;
- explain hedged-class performance as fund return plus hedge cost/benefit;
- block final allocation when class-level cost data is missing or inconsistent.

### QA assertions

| Test | Expected result |
|---|---|
| Hedging cost applies to CHF class | Cost affects only hedged-class analytics. |
| Class-level cost source is missing | Allocation is blocked or source-limited. |
| Unhedged class report is generated | No hedging cost is allocated to unhedged class. |
| Administrator restates cost | Class-level performance and NAV bridge are versioned. |

## Example 72. Notice-period side-letter conflict

### Scenario

A fund has a standard 90-day redemption notice period. A side letter grants one investor 45-day notice, but the transfer agent rejects the instruction because the side-letter entitlement is not linked to the account.

| Attribute | Standard terms | Side-letter terms |
|---|---:|---:|
| Notice period | 90 days | 45 days |
| Redemption value | 1,000,000 | 1,000,000 |
| Requested notice days | 48 | 48 |
| Transfer-agent status | Rejected | Pending evidence |

### Eligibility gap

```text
standard_notice_shortfall = 90 - 48 = 42 days
side_letter_notice_surplus = 48 - 45 = 3 days
```

### Correct treatment

- preserve standard terms, side-letter terms, account entitlement mapping and transfer-agent rejection;
- accept shorter notice only when side-letter entitlement is source-backed for that account and share class;
- keep redemption queued, rejected or repaired according to transfer-agent response;
- prevent generic side-letter terms from applying to unrelated clients or accounts;
- explain liquidity terms by investor-specific entitlement where authorized.

### QA assertions

| Test | Expected result |
|---|---|
| Side-letter entitlement is missing | Redemption is rejected or routed to repair under standard terms. |
| Entitlement is linked and valid | 45-day notice treatment is allowed. |
| Transfer agent rejects despite entitlement | Dispute/repair workflow preserves both sources. |
| Client report is generated | Notice term and source basis are visible where permitted. |

## Example 73. Fund fee rebate clawback

### Scenario

A client receives a fee rebate based on an assumed service tier. Later, the fund platform determines that the client was not eligible for part of the period and claws back a portion of the rebate.

| Attribute | Value |
|---|---:|
| Original rebate paid | 18,000 |
| Ineligible days | 60 |
| Original rebate period days | 180 |
| Clawback percentage | 33.33% |
| Clawback amount | 6,000 |

### Clawback

```text
clawback_amount = original_rebate_paid x ineligible_days / original_period_days
clawback_amount = 18,000 x 60 / 180 = 6,000
net_retained_rebate = 18,000 - 6,000 = 12,000
```

### Correct treatment

- preserve original rebate, eligibility basis, corrected eligibility period and clawback notice;
- create receivable/payable or reversal according to policy instead of deleting the original rebate;
- distinguish fund fee rebate clawback from fund distribution clawback and advisor fee billing;
- update client reporting, fee analytics and tax treatment according to source classification;
- track whether clawback is paid in cash, offset against future rebates or disputed.

### QA assertions

| Test | Expected result |
|---|---|
| Eligibility correction reduces rebate | Clawback workflow opens with calculation lineage. |
| Original rebate was already paid | Original payment remains visible and linked to clawback. |
| Clawback is offset against future rebate | Offset is tracked separately from cash repayment. |
| Client report is generated | Gross rebate, clawback and net retained rebate are explainable. |

## Example 74. Fund liquidity notice blackout period

### Scenario

A semi-liquid fund normally accepts redemption notices until 30 calendar days before the dealing date. The manager announces a temporary blackout period during a portfolio sale, so new redemption notices cannot be submitted even though the next dealing date remains open in the static calendar.

| Attribute | Value |
|---|---|
| Next dealing date | 2026-09-30 |
| Standard notice deadline | 2026-08-31 |
| Blackout start | 2026-08-10 |
| Blackout end | 2026-09-05 |
| Requested redemption | 650,000 |

### Notice eligibility

```text
standard_notice_valid = request_date <= standard_notice_deadline
blackout_active = blackout_start <= request_date <= blackout_end
redemption_notice_allowed = standard_notice_valid and not blackout_active
```

### Correct treatment

- preserve the dealing calendar, manager blackout notice, affected share classes, request timestamp and administrator response;
- block or queue new notices during blackout according to fund terms;
- avoid showing static-calendar eligibility as final when a temporary notice restriction exists;
- keep existing accepted notices separate from new blocked notices;
- label liquidity as blackout-restricted, not gated or suspended unless the manager notice says so.

### QA assertions

| Test | Expected result |
|---|---|
| Request arrives during blackout | Redemption notice is blocked or queued with blackout reason. |
| Request predates blackout and was accepted | Existing accepted notice remains valid unless source notice cancels it. |
| Blackout ends before standard deadline | New notices reopen according to effective dates. |
| Client report is generated | Liquidity state distinguishes blackout from ordinary notice deadline. |

## Example 75. Subscription prefunding break

### Scenario

A fund subscription requires cash to be prefunded before the subscription cut-off. The client instruction is valid, but cash arrives after the prefunding deadline, creating a break between order intent and fund acceptance.

| Attribute | Value |
|---|---:|
| Subscription amount | 500,000 |
| Required prefunding | 500,000 |
| Cash received before deadline | 420,000 |
| Late cash receipt | 80,000 |
| Minimum accepted subscription | 500,000 |

### Prefunding shortfall

```text
prefunding_shortfall = required_prefunding - cash_received_before_deadline
prefunding_shortfall = 500,000 - 420,000 = 80,000
subscription_fundable = prefunding_shortfall <= 0
```

### Correct treatment

- preserve client order, prefunding requirement, cash timestamps, cut-off and transfer-agent response;
- keep the order pending, rejected or resized according to fund minimum and platform policy;
- do not create units from late cash unless the administrator accepts the subscription;
- release or carry reserved cash according to revised order state;
- surface funding break separately from suitability or eligibility failure.

### QA assertions

| Test | Expected result |
|---|---|
| Prefunding is short at deadline | Subscription is blocked, rejected or exceptioned. |
| Late cash arrives after cut-off | Cash is visible but does not create units automatically. |
| Administrator accepts reduced order | Units are created only for accepted amount. |
| Client report is generated | Order intent, funding break and final acceptance are traceable. |

## Example 76. ETF share split processing

### Scenario

An ETF executes a 2-for-1 share split. Client unit quantity changes, NAV per unit adjusts and total market value should remain unchanged apart from market movement and rounding.

| Attribute | Before split | After split |
|---|---:|---:|
| Units | 1,250 | 2,500 |
| NAV per unit | 80.00 | 40.00 |
| Market value | 100,000 | 100,000 |
| Split ratio | 1 | 2 |

### Split adjustment

```text
new_units = old_units x split_ratio = 1,250 x 2 = 2,500
adjusted_nav = old_nav / split_ratio = 80.00 / 2 = 40.00
market_value_check = 2,500 x 40.00 = 100,000
```

### Correct treatment

- preserve ETF sponsor notice, split ratio, ex-date, effective date, position quantity and cost-basis treatment;
- adjust units and per-unit cost/NAV without treating the event as investment performance;
- keep cash-in-lieu separate if fractional treatment applies;
- update orders, restrictions, collateral and model holdings that reference unit quantity;
- verify downstream reporting does not show artificial gain/loss from the split.

### QA assertions

| Test | Expected result |
|---|---|
| Split source is confirmed | Units and per-unit values adjust from effective date. |
| Split creates fractional residual | Cash-in-lieu or rounding treatment is source-backed. |
| Performance report is generated | Split does not create market return by itself. |
| Open orders exist | Order quantities are adjusted, cancelled or reviewed by policy. |

## Example 77. Feeder tax blocker change

### Scenario

A feeder fund changes its tax blocker structure. The investor still holds the same feeder interest, but withholding, reporting labels and look-through analytics change from the effective date.

| Attribute | Before change | After change |
|---|---|---|
| Feeder blocker | Blocker A | Blocker B |
| Withholding profile | Treaty eligible | Treaty pending |
| Effective date | Until 30 Jun | From 1 Jul |
| Client units | 10,000 | 10,000 |

### Effective-dated profile

```text
active_tax_blocker = blocker where effective_from <= report_date < effective_to
client_units_unchanged = true
```

### Correct treatment

- preserve feeder notice, blocker structure, tax opinion, effective date and affected investors/classes;
- update withholding profile, tax pack labels and look-through attributes from effective date;
- keep unit quantity and historical reports unchanged unless the fund event changes holdings;
- route missing tax documentation to source-limited reporting rather than assuming treaty eligibility;
- explain tax blocker changes separately from NAV performance.

### QA assertions

| Test | Expected result |
|---|---|
| Report date is before effective date | Prior blocker profile applies. |
| Report date is after effective date | New blocker profile and documentation state apply. |
| Tax documentation is missing | Withholding/tax reporting is source-limited or exceptioned. |
| Holdings report is generated | Units remain unchanged while tax labels change. |

## Example 78. NAV strike operational incident

### Scenario

A fund administrator publishes an incorrect NAV strike because one portfolio security was priced with a stale quote. Orders were processed before the incident was discovered.

| Attribute | Original NAV | Corrected NAV |
|---|---:|---:|
| NAV per unit | 102.40 | 101.85 |
| Subscription units issued | 4,882.8125 | 4,909.1802 |
| Subscription amount | 500,000 | 500,000 |
| NAV error | -0.55 | n/a |

### Unit correction

```text
original_units = 500,000 / 102.40 = 4,882.8125
corrected_units = 500,000 / 101.85 = 4,909.1802
unit_delta = 4,909.1802 - 4,882.8125 = 26.3677
```

### Correct treatment

- preserve original NAV, corrected NAV, administrator incident notice, affected orders and correction method;
- apply unit/cash correction only after administrator confirmation and materiality policy review;
- keep original trade, correction event and client notification linked;
- update performance, tax lots and statements according to correction policy;
- avoid silently overwriting the original NAV or order record.

### QA assertions

| Test | Expected result |
|---|---|
| Corrected NAV arrives | Unit/cash correction is calculated with source lineage. |
| Correction is below materiality threshold | Event is logged and reporting follows policy. |
| Client statement was already delivered | Correction notice or restatement workflow opens. |
| Performance report is generated | Original and corrected NAV effects are explainable. |

## Example 79. Investor-level MFN election control

### Scenario

A private fund side letter offers most-favoured-nation elections to eligible investors. One investor elects a lower management-fee term, but eligibility depends on commitment size and investor classification.

| Attribute | Investor A | MFN threshold |
|---|---:|---:|
| Commitment | 4,000,000 | 5,000,000 |
| Standard management fee | 1.50% |
| Requested MFN fee | 1.25% |
| Investor classification | Professional | Professional |

### Eligibility and fee delta

```text
mfn_commitment_eligible = commitment >= mfn_threshold
annual_fee_delta_if_allowed = commitment x (standard_fee - mfn_fee)
annual_fee_delta_if_allowed = 4,000,000 x (1.50% - 1.25%) = 10,000
```

### Correct treatment

- preserve side-letter terms, MFN notice, eligibility rules, investor classification, election deadline and administrator acceptance;
- do not apply MFN terms to ineligible investors or unrelated accounts;
- keep requested, accepted, rejected and effective fee terms separately visible;
- update fee accruals only after source-backed acceptance and effective date;
- explain investor-level economics without exposing other investors' confidential side-letter terms.

### QA assertions

| Test | Expected result |
|---|---|
| Investor fails commitment threshold | MFN election is rejected or exceptioned. |
| Administrator accepts election | Fee accrual changes from effective date with evidence. |
| Election deadline is missed | Standard fee remains active unless waiver is approved. |
| Client report is generated | Investor-level fee term is shown without leaking other investor terms. |

## Example 80. Fund redemption fee waiver

### Scenario

A fund normally charges a short-hold redemption fee. A client redeems before the minimum holding period, but the fund sponsor grants a partial waiver because the redemption was triggered by a share-class closure.

| Attribute | Value |
|---|---:|
| Gross redemption value | 1,000,000 |
| Standard redemption fee | 2.00% |
| Waiver percentage | 75.00% |
| Holding period | 45 days |
| Standard minimum holding period | 90 days |

### Waived fee

```text
standard_fee = gross_redemption_value x redemption_fee_rate
standard_fee = 1,000,000 x 2.00% = 20,000
waived_fee = standard_fee x waiver_percentage = 20,000 x 75.00% = 15,000
charged_fee = standard_fee - waived_fee = 5,000
net_redemption_cash = 1,000,000 - 5,000 = 995,000
```

### Correct treatment

- preserve redemption order, holding-period rule, standard fee, waiver approval, waiver reason and approving party;
- show standard fee, waived amount and charged fee separately instead of overwriting the fee rule;
- avoid applying the waiver to other redemptions, accounts or share classes without source evidence;
- include the charged fee in proceeds, performance and cash reporting according to platform policy;
- flag advisor-visible explanation when the waiver is client-relevant or suitability-relevant.

### QA assertions

| Test | Expected result |
|---|---|
| Waiver approval exists | Net cash reflects the reduced charged fee. |
| Waiver approval is missing | Standard redemption fee applies or exception workflow opens. |
| Same client redeems another class | Waiver is not reused unless explicitly eligible. |
| Client report is generated | Standard fee, waived fee and charged fee are explainable. |

## Example 81. Private-fund key-person event

### Scenario

A private fund declares a key-person event after a named portfolio manager leaves. Existing holdings remain valued, but new commitments and capital calls may be restricted until the advisory committee approves a remedy.

| Attribute | Value |
|---|---:|
| Client commitment | 5,000,000 |
| Funded capital | 3,200,000 |
| Unfunded commitment | 1,800,000 |
| Pending capital call | 400,000 |
| Key-person event status | Active |

### Callable exposure under event control

```text
unfunded_commitment = commitment - funded_capital
unfunded_commitment = 5,000,000 - 3,200,000 = 1,800,000
call_blocked_by_event = key_person_event_active and not advisory_committee_remedy_approved
```

### Correct treatment

- preserve key-person notice, affected fund, effective date, remedy period, committee decision and call restrictions;
- keep NAV/capital-account valuation visible while separating liquidity and commitment restrictions;
- block or exception new commitments according to the fund documents and platform policy;
- treat pending capital calls according to the notice rather than assuming all calls are cancelled;
- surface governance status to advisory, risk, mandate and reporting users without implying a realized loss.

### QA assertions

| Test | Expected result |
|---|---|
| Key-person event is active | New subscription or commitment is blocked or exceptioned. |
| Existing holding is reported | Funded value remains visible with governance flag. |
| Pending call is reviewed | Call is paid, blocked or escalated according to source notice. |
| Remedy approval arrives | Restrictions change only from the approved effective date. |

## Example 82. ETF index methodology change

### Scenario

An ETF tracks an index that changes issuer concentration rules from a 10% cap to a 5% cap. The client still holds the ETF, but look-through exposure, tracking expectations and rebalance explanation change.

| Attribute | Before change | After change |
|---|---:|---:|
| Client ETF market value | 250,000 | 250,000 |
| Issuer look-through weight | 9.00% | 5.00% target cap |
| Excess weight versus new cap | n/a | 4.00% |
| Effective date | n/a | Next rebalance |

### Look-through exposure change

```text
current_issuer_exposure = etf_market_value x current_weight
current_issuer_exposure = 250,000 x 9.00% = 22,500
target_issuer_exposure = etf_market_value x new_cap_weight
target_issuer_exposure = 250,000 x 5.00% = 12,500
expected_exposure_reduction = 22,500 - 12,500 = 10,000
```

### Correct treatment

- preserve index methodology notice, ETF sponsor notice, effective date, rebalance date and look-through file version;
- update target look-through and benchmark analytics from the effective date, not from the announcement date unless required;
- do not book a client trade because the ETF provider rebalances the underlying basket;
- explain tracking difference, sector/issuer exposure movement and benchmark changes separately from client order activity;
- re-check concentration, mandate and advisory analytics that rely on ETF look-through data.

### QA assertions

| Test | Expected result |
|---|---|
| Methodology changes before rebalance | Future target exposure is visible while current holdings remain source-backed. |
| New look-through file arrives | Issuer exposure updates with file lineage and effective date. |
| Client performance is calculated | ETF price movement drives return, not a synthetic underlying trade. |
| Mandate concentration check runs | Check uses the correct as-of look-through version. |

## Example 83. Fund audit qualification impact

### Scenario

A fund issues an annual report with a qualified audit opinion because a private asset valuation could not be independently verified. The official NAV remains published, but valuation confidence and reporting commentary must be controlled.

| Attribute | Value |
|---|---:|
| Client fund value | 600,000 |
| Fund NAV | 120,000,000 |
| Assets subject to qualification | 18,000,000 |
| Client ownership share | 0.50% |

### Qualified exposure estimate

```text
qualified_asset_percentage = qualified_assets / fund_nav
qualified_asset_percentage = 18,000,000 / 120,000,000 = 15.00%
client_qualified_exposure_estimate = client_fund_value x qualified_asset_percentage
client_qualified_exposure_estimate = 600,000 x 15.00% = 90,000
```

### Correct treatment

- preserve audit opinion, qualification reason, affected assets, reporting period, auditor date and administrator response;
- keep official NAV as the valuation source unless the administrator restates NAV or policy requires override;
- add valuation-quality, due-diligence or reporting flags without inventing a new price;
- route advisory, risk and reporting commentary through approved language;
- monitor subsequent NAV restatements, side-pocket actions or valuation committee decisions.

### QA assertions

| Test | Expected result |
|---|---|
| Qualified audit opinion is recorded | Fund retains official NAV with valuation-quality flag. |
| No NAV restatement exists | System does not adjust price or units automatically. |
| Client report is generated | Qualification note is included according to reporting policy. |
| Later restatement arrives | Restatement workflow links back to the qualification. |

## Example 84. Side-pocket secondary transfer

### Scenario

A client transfers restricted side-pocket units in a secondary transaction approved by the fund administrator. The side pocket does not become redeemable; ownership changes at an agreed transfer price.

| Attribute | Value |
|---|---:|
| Side-pocket units | 10,000 |
| Administrator NAV per unit | 50.00 |
| Secondary transfer price per unit | 17.50 |
| Transfer discount to NAV | 65.00% |

### Transfer economics

```text
administrator_nav_value = side_pocket_units x nav_per_unit
administrator_nav_value = 10,000 x 50.00 = 500,000
transfer_cash_value = side_pocket_units x transfer_price_per_unit
transfer_cash_value = 10,000 x 17.50 = 175,000
discount_to_nav = administrator_nav_value - transfer_cash_value = 325,000
```

### Correct treatment

- preserve transfer agreement, administrator consent, transfer price, settlement date, buyer/seller accounts and restriction state;
- book the transaction as a transfer or sale according to platform policy, not as a fund redemption;
- retain side-pocket restriction metadata for the buyer after transfer;
- separate administrator NAV value, transfer cash value, realized result and liquidity discount;
- check suitability, tax, mandate, lock-up and reporting effects for both parties where applicable.

### QA assertions

| Test | Expected result |
|---|---|
| Administrator consent is missing | Transfer is blocked or exceptioned. |
| Transfer settles | Seller units decrease and buyer restricted units increase. |
| Reporting value is calculated | Official NAV and transfer-price economics remain explainable. |
| Buyer later requests redemption | Restriction state still prevents normal redemption. |

## Example 85. Subscription equalization true-up

### Scenario

A private fund accepts a late subscription after the fund has accrued performance since the initial closing. The investor pays an equalization amount so existing investors are not diluted by pre-admission performance.

| Attribute | Value |
|---|---:|
| Late subscription amount | 1,000,000 |
| Pre-admission performance accrual | 3.00% |
| Equalization amount due | 30,000 |
| Total cash required | 1,030,000 |

### Equalization true-up

```text
equalization_amount = subscription_amount x pre_admission_performance_accrual
equalization_amount = 1,000,000 x 3.00% = 30,000
total_cash_required = subscription_amount + equalization_amount
total_cash_required = 1,000,000 + 30,000 = 1,030,000
```

### Correct treatment

- preserve admission date, subscription amount, equalization rate, administrator calculation and investor capital account;
- keep subscription capital and equalization amount as separate economic components;
- allocate equalization according to fund accounting policy rather than treating it as ordinary cash performance;
- ensure units, capital account, fee base and performance reporting use the administrator-confirmed admission terms;
- explain equalization in client reporting when it affects cash movement, capital account or performance.

### QA assertions

| Test | Expected result |
|---|---|
| Equalization calculation is confirmed | Total required cash includes subscription plus equalization. |
| Investor sends only subscription amount | Order is underfunded, resized or exceptioned by policy. |
| Capital account is created | Subscription and equalization components are traceable. |
| Performance report is generated | Pre-admission performance is not attributed as client-earned return. |

## Example 86. Fund expense-ratio cap expiry

### Scenario

A share class had a temporary expense-ratio cap that reduced total expenses for an introductory period. The cap expires, increasing expected ongoing charge and reducing projected net return.

| Attribute | Before expiry | After expiry |
|---|---:|---:|
| Client fund value | 500,000 | 500,000 |
| Gross expense ratio | 1.35% | 1.35% |
| Expense cap | 0.95% | n/a |
| Effective expense ratio | 0.95% | 1.35% |

### Expense impact

```text
old_annual_expense = fund_value x capped_expense_ratio
old_annual_expense = 500,000 x 0.95% = 4,750
new_annual_expense = fund_value x gross_expense_ratio
new_annual_expense = 500,000 x 1.35% = 6,750
annual_expense_increase = 6,750 - 4,750 = 2,000
```

### Correct treatment

- preserve cap terms, expiry date, share class, gross expense ratio, administrator notice and fee source;
- update projected ongoing charges from the effective date, not from the announcement date unless terms require it;
- avoid treating higher expenses as a market loss or a separate cash debit when NAV is already net of fees;
- re-check suitability, performance expectations and fee disclosures where ongoing charges materially change;
- keep historical reporting under the prior capped rate.

### QA assertions

| Test | Expected result |
|---|---|
| Cap expiry date arrives | Effective expense ratio updates for future projections. |
| Administrator notice is missing | Fee projection remains source-limited. |
| Historical report is regenerated | Old periods retain capped expense assumption. |
| Advisory review runs | Higher ongoing charge is visible in suitability and cost disclosure. |

## Example 87. Suspended dealing side-letter override

### Scenario

A fund suspends dealing for all investors, but one institutional investor claims a side-letter right to redeem during suspension. The platform must not release redemption until the administrator confirms the side-letter applies.

| Attribute | Standard terms | Claimed side-letter terms |
|---|---|---|
| Dealing state | Suspended | Override claimed |
| Redemption amount | 400,000 | 400,000 |
| Administrator status | Suspended | Pending confirmation |
| Release status | Blocked | Source-limited |

### Override rule

```text
redemption_allowed = not fund_suspended or confirmed_side_letter_override
source_limited_amount = requested_redemption_amount if override_claimed and not confirmed_side_letter_override else 0
```

### Correct treatment

- preserve suspension notice, standard fund terms, side-letter claim, investor identity, administrator confirmation and expiry;
- block redemption until the administrator confirms the side-letter entitlement and operational path;
- show claimed override as source-limited, not as approved liquidity;
- re-check DPM liquidity, client communication and reporting labels while redemption is suspended;
- audit any preferential liquidity treatment for governance and fairness review.

### QA assertions

| Test | Expected result |
|---|---|
| Fund suspension is active | Standard redemption is blocked. |
| Side-letter is claimed but unconfirmed | Redemption remains source-limited. |
| Administrator confirms override | Redemption proceeds under scoped terms. |
| Client report is generated | Suspended, claimed and approved liquidity states are distinct. |

## Example 88. Private-fund GP-led tender election

### Scenario

A private fund sponsor offers a GP-led tender: investors may sell part of their interest at a discount or roll into a continuation vehicle.

| Attribute | Value |
|---|---:|
| Client NAV | 1,200,000 |
| Tenderable percentage | 40.00% |
| Tender price as percent of NAV | 92.00% |
| Maximum tender NAV | 480,000 |
| Tender cash if elected | 441,600 |

### Tender economics

```text
maximum_tender_nav = client_nav x tenderable_percentage
maximum_tender_nav = 1,200,000 x 40.00% = 480,000
tender_cash = maximum_tender_nav x tender_price_percentage
tender_cash = 480,000 x 92.00% = 441,600
discount_to_nav = 480,000 - 441,600 = 38,400
```

### Correct treatment

- preserve GP-led tender notice, election deadline, tenderable percentage, price, continuation option and proration terms;
- separate liquidity election, transfer economics, rollover outcome and remaining commitment;
- do not assume full tender acceptance until final allocation is confirmed;
- assess suitability, liquidity need, valuation discount, tax and concentration implications before election;
- update reporting for tendered interest, cash proceeds, remaining NAV and continuation vehicle exposure.

### QA assertions

| Test | Expected result |
|---|---|
| Tender election is submitted | Election status is pending until sponsor acceptance. |
| Tender is prorated | Cash proceeds and remaining interest use final accepted percentage. |
| Investor rolls instead of tenders | Exposure migrates to continuation vehicle with lineage. |
| Client report is generated | Tender discount and remaining private-fund exposure are explainable. |

## Example 89. ETF securities-lending revenue allocation

### Scenario

An ETF earns securities-lending revenue. The sponsor allocates part of the gross lending revenue to the fund after deducting agent fees.

| Attribute | Value |
|---|---:|
| ETF market value held by client | 300,000 |
| ETF total net assets | 1,500,000,000 |
| Gross lending revenue | 2,400,000 |
| Agent fee percentage | 20.00% |
| Net revenue to fund | 1,920,000 |

### Client revenue attribution

```text
client_ownership_share = client_market_value / etf_total_net_assets
client_ownership_share = 300,000 / 1,500,000,000 = 0.02%
net_revenue_to_fund = gross_lending_revenue x (1 - agent_fee_percentage)
net_revenue_to_fund = 2,400,000 x 80.00% = 1,920,000
client_attributed_lending_revenue = net_revenue_to_fund x client_ownership_share
client_attributed_lending_revenue = 1,920,000 x 0.02% = 384
```

### Correct treatment

- preserve ETF sponsor report, gross lending revenue, agent fee, allocation period, total net assets and client market value;
- treat lending revenue as look-through fund economics, not a direct client securities-lending transaction;
- distinguish fund-level revenue attribution from cash distribution unless the ETF distributes it;
- monitor securities-lending risk disclosures, collateral quality and tracking difference where relevant;
- avoid double-counting revenue already embedded in ETF NAV return.

### QA assertions

| Test | Expected result |
|---|---|
| Sponsor publishes lending revenue | Attribution can be calculated with sponsor source and period. |
| ETF does not distribute cash | Revenue is not posted as direct client income. |
| TNA source is missing | Client attribution remains source-limited. |
| Performance report is generated | NAV return and look-through revenue explanation are not double counted. |

## Example 90. Feeder-master NAV mismatch repair

### Scenario

A feeder fund publishes NAV before the master fund restates its NAV. The feeder administrator later issues a repair notice for the mismatch.

| Attribute | Original | Corrected |
|---|---:|---:|
| Client units | 20,000 | 20,000 |
| Feeder NAV per unit | 101.25 | 100.90 |
| Client value | 2,025,000 | 2,018,000 |
| Valuation difference | n/a | -7,000 |

### Repair amount

```text
original_value = units x original_nav
original_value = 20,000 x 101.25 = 2,025,000
corrected_value = units x corrected_nav
corrected_value = 20,000 x 100.90 = 2,018,000
valuation_difference = corrected_value - original_value = -7,000
```

### Correct treatment

- preserve feeder NAV, master NAV restatement, administrator repair notice, affected dates and materiality policy;
- restate valuation and performance only for affected periods and reports;
- do not create subscription/redemption activity when only NAV is repaired;
- identify downstream impact on advisory, fees, collateral, reporting and performance;
- keep original and corrected NAV lineage for audit and client explanation.

### QA assertions

| Test | Expected result |
|---|---|
| Master NAV restatement arrives | Feeder repair workflow opens. |
| Corrected feeder NAV is published | Affected valuation and performance recalculate with lineage. |
| Client units are unchanged | No synthetic trade is created. |
| Historical report is regenerated | Original and corrected values are traceable. |

## Example 91. Fund distribution currency-election break

### Scenario

A distributing fund allows investors to elect distribution currency. A client elected USD, but the administrator pays in the default EUR currency.

| Attribute | Expected | Actual |
|---|---:|---:|
| Distribution base amount | 12,000 EUR | 12,000 EUR |
| Elected currency | USD | EUR |
| FX election rate | 1.0800 | n/a |
| Expected USD cash | 12,960 | n/a |
| Actual EUR cash | n/a | 12,000 |

### Currency-election break

```text
expected_usd_cash = distribution_eur_amount x election_fx_rate
expected_usd_cash = 12,000 x 1.0800 = 12,960
currency_election_break = actual_currency != elected_currency
```

### Correct treatment

- preserve distribution notice, currency election, election deadline, administrator confirmation, actual cash and FX rate source;
- treat wrong-currency payment as an income distribution with operational break, not a failed entitlement;
- route repair through FX conversion, administrator claim or client instruction depending on policy;
- report income amount, received currency, expected currency and repair status separately;
- avoid converting automatically if client authority or FX policy requires confirmation.

### QA assertions

| Test | Expected result |
|---|---|
| Currency election is confirmed | Expected cash uses elected currency and source-backed rate. |
| Cash arrives in default currency | Distribution is posted with currency-election break. |
| Repair FX is approved | Currency conversion or claim links to original distribution. |
| Client report is generated | Expected election and actual received currency are explainable. |

## Example 92. Share-class performance-fee equalization reopener

### Scenario

A fund administrator reopens performance-fee equalization after identifying that a share class used the wrong high-water mark for a subscription series.

| Attribute | Original | Corrected |
|---|---:|---:|
| Client units | 15,000 | 15,000 |
| Original equalization debit per unit | 0.18 | n/a |
| Corrected equalization debit per unit | n/a | 0.11 |
| Equalization delta | n/a | 1,050 credit |

### Equalization adjustment

```text
original_debit = units x original_equalization_debit_per_unit
original_debit = 15,000 x 0.18 = 2,700
corrected_debit = units x corrected_equalization_debit_per_unit
corrected_debit = 15,000 x 0.11 = 1,650
equalization_credit = original_debit - corrected_debit
equalization_credit = 2,700 - 1,650 = 1,050
```

### Correct treatment

- preserve series id, original high-water mark, corrected high-water mark, administrator notice, affected dates and equalization method;
- post only the equalization delta unless the administrator requires full reversal and repost;
- keep NAV movement, performance fee, equalization and investor capital-account effects separate;
- restate affected fee analytics, performance reporting and client explanation packs;
- prevent cross-series averaging when only one subscription series is affected.

### QA assertions

| Test | Expected result |
|---|---|
| Administrator reopens equalization | Adjustment workflow opens with series-level lineage. |
| Corrected debit is lower | Client receives equalization credit or capital-account uplift. |
| Only one series is affected | Other investor series remain unchanged. |
| Report is regenerated | Original and corrected fee economics are traceable. |

## Example 93. ETF in-kind redemption cash-substitute fee

### Scenario

An ETF in-kind redemption basket cannot deliver one security, so the sponsor applies a cash-substitute fee. The fee reduces net redemption value but does not change client ETF units until redemption is confirmed.

| Attribute | Value |
|---|---:|
| Gross basket value | 1,250,000 |
| Missing security cash substitute | 80,000 |
| Cash-substitute fee rate | 0.35% |
| Cash-substitute fee | 280 |
| Net redemption value | 1,249,720 |

### Fee calculation

```text
cash_substitute_fee = cash_substitute_amount x cash_substitute_fee_rate
cash_substitute_fee = 80,000 x 0.35% = 280
net_redemption_value = gross_basket_value - cash_substitute_fee
net_redemption_value = 1,250,000 - 280 = 1,249,720
```

### Correct treatment

- preserve ETF redemption order, basket file, missing security, cash-substitute amount, fee schedule and sponsor confirmation;
- separate in-kind delivered assets, cash substitute and cash-substitute fee;
- create delivered-asset holdings only after custodian settlement and instrument setup;
- avoid treating sponsor fee as market price movement or advisor fee;
- keep basket exception evidence for ETF liquidity, tracking difference and operational-risk review.

### QA assertions

| Test | Expected result |
|---|---|
| Basket includes cash substitute | Fee calculates from cash-substitute amount and sponsor schedule. |
| Delivered security setup is missing | Delivered holding remains pending or source-limited. |
| Redemption is rejected | No units, assets or fees are booked as confirmed. |
| Client report is generated | Basket value, cash substitute and fee are distinct. |

## Example 94. Private-fund capital-call default remedy

### Scenario

A private fund investor misses a capital-call deadline. The fund terms impose default interest and may suspend voting or distribution rights until cured.

| Attribute | Value |
|---|---:|
| Capital call due | 500,000 |
| Cash received by due date | 420,000 |
| Default shortfall | 80,000 |
| Default interest rate | 8.00% |
| Days overdue | 20 |

### Default interest

```text
default_shortfall = capital_call_due - cash_received
default_shortfall = 500,000 - 420,000 = 80,000
default_interest = default_shortfall x default_interest_rate x days_overdue / 365
default_interest = 80,000 x 8.00% x 20 / 365 = 350.68
```

### Correct treatment

- preserve capital-call notice, due date, cash receipt, shortfall, default terms, cure notice and remedy status;
- keep unpaid commitment, default interest, voting restrictions and distribution restrictions separately visible;
- do not create additional fund units or capital account value for unfunded call amounts;
- route advisory, credit, liquidity and operations escalation where client funding is insufficient;
- restore normal status only after cure cash and administrator confirmation are received.

### QA assertions

| Test | Expected result |
|---|---|
| Call is partially funded | Default shortfall and remedy workflow open. |
| Cure cash arrives late | Default interest calculates through cure date. |
| Rights are suspended | Reporting and workflow labels show remedy state. |
| Capital account report is generated | Paid-in, unfunded, default interest and restrictions are distinct. |

## Example 95. Fund administrator transition break

### Scenario

A fund changes administrator. The old administrator and new administrator report different unit balances during transition.

| Attribute | Old administrator | New administrator |
|---|---:|---:|
| Client units | 125,000.000 | 124,998.750 |
| NAV per unit | 10.42 | 10.42 |
| Unit break | n/a | -1.250 |
| Valuation break | n/a | -13.03 |

### Reconciliation break

```text
unit_break = new_admin_units - old_admin_units
unit_break = 124,998.750 - 125,000.000 = -1.250
valuation_break = unit_break x nav_per_unit
valuation_break = -1.250 x 10.42 = -13.03
```

### Correct treatment

- preserve old administrator statement, new administrator statement, transition date, cutover population and reconciliation owner;
- block final migration sign-off until unit, NAV, class and investor-id breaks are explained or approved;
- avoid synthetic redemption/subscription postings for migration-only reconciliation breaks;
- keep client reporting source-labelled during transition when official source ownership changes;
- retain cutover evidence for audit, fee billing, tax lots and performance continuity.

### QA assertions

| Test | Expected result |
|---|---|
| Unit balances differ | Transition reconciliation break opens. |
| Break is rounding-only and approved | Difference is resolved with tolerance evidence. |
| Investor id mapping is wrong | Position remains exceptioned until mapping repair. |
| Report is generated during transition | Source and reconciliation state are visible. |

## Example 96. Liquidation gate waterfall

### Scenario

A fund in liquidation applies a payment waterfall. Senior expenses and holdbacks are paid before investor distributions, and a gate limits first-round cash release.

| Attribute | Value |
|---|---:|
| Liquidation cash pool | 10,000,000 |
| Senior expenses | 600,000 |
| Litigation holdback | 1,400,000 |
| Investor distributable pool | 8,000,000 |
| Client ownership share | 2.50% |

### Investor distribution

```text
investor_distributable_pool = liquidation_cash_pool - senior_expenses - litigation_holdback
investor_distributable_pool = 10,000,000 - 600,000 - 1,400,000 = 8,000,000
client_distribution = investor_distributable_pool x client_ownership_share
client_distribution = 8,000,000 x 2.50% = 200,000
```

### Correct treatment

- preserve liquidation notice, waterfall terms, senior expenses, holdback, investor share and administrator distribution statement;
- separate liquidation proceeds, holdback receivable, expense allocation and remaining residual claim;
- cancel units only when the liquidation event legally closes the position;
- keep later holdback releases linked to the original liquidation event;
- report liquidation status, expected future distributions and uncertainty clearly.

### QA assertions

| Test | Expected result |
|---|---|
| Liquidation waterfall is published | Distribution uses source-backed priority order. |
| Holdback remains open | Receivable or residual claim remains visible. |
| Final liquidation notice arrives | Units close only when confirmed. |
| Client report is generated | Cash received, holdback and residual exposure are distinct. |

## Example 97. ESG exclusion reclassification event

### Scenario

A fund is reclassified because an exclusion screen finds exposure to an issuer that violates the client mandate. The fund remains tradable, but suitability and model-portfolio eligibility change.

| Attribute | Value |
|---|---:|
| Fund market value | 750,000 |
| Controversial issuer exposure | 3.20% |
| Mandate exclusion threshold | 0.00% |
| Exposure breaching mandate | 24,000 |
| Model buy eligibility | Suspended |

### Breach exposure

```text
excluded_exposure_value = fund_market_value x controversial_issuer_exposure
excluded_exposure_value = 750,000 x 3.20% = 24,000
mandate_breach = controversial_issuer_exposure > mandate_exclusion_threshold
```

### Correct treatment

- preserve ESG data source, exclusion rule, look-through exposure, effective date, classification decision and review owner;
- separate tradability, approved-universe status, model-portfolio eligibility and client mandate suitability;
- block new buys or model use where mandate rules require exclusion;
- keep existing holding action options explicit: hold, sell, transition, exception or client consent;
- label reports with source date, exposure confidence and remediation state.

### QA assertions

| Test | Expected result |
|---|---|
| Exclusion exposure breaches threshold | Suitability or model-eligibility exception opens. |
| ESG data is stale | Reclassification remains source-limited. |
| Client mandate allows exception | Exception captures approval, expiry and rationale. |
| Advisory proposal is generated | Fund is blocked or warning-labelled for affected mandate. |

## Example 98. Swing-pricing threshold dispute

### Scenario

A fund applies swing pricing because net redemptions allegedly exceeded the threshold. A distributor disputes the threshold calculation because one large redemption was cancelled before the dealing cut-off.

| Attribute | Value |
|---|---:|
| Fund NAV before swing | 100,000,000 |
| Swing threshold | 5.00% |
| Gross redemption orders | 6,200,000 |
| Cancelled before cut-off | 1,500,000 |
| Net eligible redemptions | 4,700,000 |

### Threshold test

```text
threshold_amount = fund_nav x swing_threshold
threshold_amount = 100,000,000 x 5.00% = 5,000,000
net_eligible_redemption_ratio = 4,700,000 / 100,000,000 = 4.70%
swing_threshold_breached = net_eligible_redemptions > threshold_amount
```

### Correct treatment

- preserve order book, cancellation timestamp, fund swing policy, administrator threshold calculation and final NAV notice;
- calculate threshold using eligible orders after valid cut-off cancellations;
- keep published NAV, disputed swung NAV and final confirmed NAV separately visible;
- block final redemption proceeds or label them provisional while the swing-pricing dispute is unresolved;
- retain swing decision lineage for client reporting, performance, fees and dilution analytics.

### QA assertions

| Test | Expected result |
|---|---|
| Valid cancellation reduces net redemptions below threshold | Swing application is disputed or reversed with evidence. |
| Administrator confirms swing | Redemption proceeds use confirmed swung NAV and dispute state closes. |
| Threshold calculation source is missing | NAV and proceeds remain source-limited. |
| Client report is generated | Swing basis and dispute state are explainable. |

## Example 99. Fund-of-funds stale underlying NAV

### Scenario

A fund-of-funds publishes a NAV while one underlying private fund has not delivered a current NAV. The administrator uses the prior NAV with a stale-source label.

| Attribute | Value |
|---|---:|
| Fund-of-funds NAV | 25,000,000 |
| Underlying stale NAV | 3,000,000 |
| Stale NAV age | 95 days |
| Stale-age policy threshold | 60 days |
| Stale coverage | 12.00% |

### Stale exposure

```text
stale_coverage = stale_underlying_nav / fund_of_funds_nav
stale_coverage = 3,000,000 / 25,000,000 = 12.00%
stale_age_breach = stale_nav_age > stale_age_policy_threshold
```

### Correct treatment

- preserve administrator NAV pack, underlying manager statement, stale age, override approval and valuation policy;
- label fund-of-funds NAV with stale underlying coverage and source date;
- avoid treating stale underlying NAV as a current confirmed valuation;
- route material stale exposure to valuation committee, reporting disclaimer or dealing control according to policy;
- update look-through, risk and performance analytics when current underlying NAV arrives.

### QA assertions

| Test | Expected result |
|---|---|
| Underlying NAV breaches stale-age policy | Fund-of-funds NAV is labelled or escalated. |
| Override is approved | Override records approver, expiry and coverage. |
| Current NAV arrives later | Fund-of-funds NAV and performance restate or update with lineage. |
| Look-through report is generated | Stale coverage and source date are visible. |

## Example 100. ETF Authorized-Participant Failed Settlement

### Scenario

An ETF creation order is accepted, but the authorized participant fails to deliver part of the basket on settlement date. The ETF shares cannot be treated as fully issued until settlement is repaired.

| Attribute | Value |
|---|---:|
| Creation units expected | 50,000 |
| Basket value expected | 1,250,000 |
| Failed basket component | 180,000 |
| Cash collateral posted | 160,000 |
| Uncovered exposure | 20,000 |

### Settlement shortfall

```text
uncovered_exposure = failed_basket_component - cash_collateral_posted
uncovered_exposure = 180,000 - 160,000 = 20,000
```

### Correct treatment

- preserve ETF creation order, AP identity, basket file, failed component, collateral, fail notice and repair state;
- keep ETF units pending or restricted until basket settlement or sponsor-approved collateral substitution is complete;
- separate failed asset delivery, collateral posted, uncovered exposure and final issuance;
- update ETF liquidity, AP concentration and operational-risk metrics;
- avoid creating client-ready ETF units without settlement evidence.

### QA assertions

| Test | Expected result |
|---|---|
| AP fails to deliver basket component | Creation remains pending, restricted or exceptioned. |
| Collateral is insufficient | Uncovered exposure is calculated and escalated. |
| Sponsor accepts cash substitute | Units issue according to approved substitution terms. |
| Report is generated | Basket fail, collateral and issued units are distinct. |

## Example 101. Private-Fund Side-Letter Fee Cap Expiry

### Scenario

A private-fund investor has a side-letter fee cap that expires mid-year. Fees after expiry should use the standard schedule unless a renewal is approved.

| Attribute | Value |
|---|---:|
| Capital account value | 4,000,000 |
| Side-letter fee cap | 0.75% |
| Standard fee rate | 1.25% |
| Days after cap expiry | 90 |
| Day-count basis | 365 |

### Incremental fee after expiry

```text
incremental_fee_rate = standard_fee_rate - side_letter_fee_cap
incremental_fee = capital_account_value x incremental_fee_rate x days_after_expiry / 365
incremental_fee = 4,000,000 x (1.25% - 0.75%) x 90 / 365 = 4,931.51
```

### Correct treatment

- preserve side letter, expiry date, renewal state, standard fee schedule, administrator fee calculation and investor notice;
- apply capped fees only through the valid side-letter period;
- separate expired cap uplift from performance fees, management fees and expense allocations;
- block or label fee reporting when side-letter renewal evidence is pending;
- keep investor-specific fee terms private and scoped to authorized recipients.

### QA assertions

| Test | Expected result |
|---|---|
| Fee cap expires | Post-expiry fees use standard schedule unless renewal is approved. |
| Renewal is pending | Fee output is provisional or exception-labelled. |
| Fee cap is investor-specific | Other investors are not affected. |
| Statement is generated | Cap period, standard period and incremental fee are explainable. |

## Example 102. Semi-Liquid Redemption Queue Proration

### Scenario

A semi-liquid fund has a quarterly redemption gate. Accepted redemptions exceed available gate capacity and must be prorated across queued investors.

| Attribute | Value |
|---|---:|
| Total valid redemption requests | 12,000,000 |
| Quarterly gate capacity | 7,500,000 |
| Client redemption request | 1,200,000 |
| Proration factor | 62.50% |

### Client accepted redemption

```text
proration_factor = gate_capacity / total_valid_redemption_requests
proration_factor = 7,500,000 / 12,000,000 = 62.50%
client_accepted_redemption = client_request x proration_factor
client_accepted_redemption = 1,200,000 x 62.50% = 750,000
client_deferred_redemption = 1,200,000 - 750,000 = 450,000
```

### Correct treatment

- preserve redemption request, queue timestamp, valid request population, gate capacity, proration file and settlement statement;
- separate accepted redemption, deferred redemption, cancelled redemption and future queue priority;
- use administrator proration evidence before posting units and cash;
- update liquidity projections, client communication and DPM funding assumptions;
- avoid treating the full requested amount as settled or available cash.

### QA assertions

| Test | Expected result |
|---|---|
| Requests exceed gate capacity | Accepted redemption is prorated from source-backed factor. |
| Client request is partially accepted | Accepted and deferred amounts remain separate. |
| Queue priority changes | Deferred redemption carries queue lineage. |
| Funding plan is generated | Only accepted settlement cash is usable by settlement date. |

## Example 103. Fund Benchmark Reclassification Event

### Scenario

A fund changes benchmark from a broad equity index to a sustainable equity index. Historical performance remains unchanged, but relative performance, tracking error and mandate classification must be versioned.

| Attribute | Prior | New |
|---|---|---|
| Benchmark | Broad Equity Index |
| New benchmark | - | Sustainable Equity Index |
| Effective date | - | 2026-07-01 |
| Portfolio value | 8,000,000 | 8,000,000 |
| Active return since effective date | n/a | 0.45% |

### Active return amount

```text
active_return_amount = portfolio_value x active_return
active_return_amount = 8,000,000 x 0.45% = 36,000
```

### Correct treatment

- preserve fund notice, benchmark change rationale, effective date, benchmark identifiers, historical benchmark lineage and mandate mapping;
- version benchmark-relative analytics from the effective date without rewriting prior reporting periods;
- separate fund benchmark reclassification from ESG eligibility, product taxonomy and client suitability decisions;
- update performance, tracking error, attribution and model-portfolio comparison basis;
- disclose benchmark transition in client reporting where required by policy.

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark changes from effective date | Relative analytics version forward from effective date. |
| Historical report is regenerated | Prior benchmark remains applied to prior periods. |
| Mandate relies on benchmark type | Suitability or model classification review opens. |
| Performance report is generated | Benchmark lineage and active-return basis are visible. |

## Example 104. Fund Proxy-Voting Restriction

### Scenario

A discretionary mandate holds a fund share class that restricts investors from directing proxy votes. The client requests vote-by-vote instruction on an underlying issuer, but the platform can only record stewardship preference, not execute a direct vote.

| Attribute | Value |
|---|---:|
| Fund market value | 900,000 |
| Look-through issuer exposure | 4.00% |
| Effective issuer exposure | 36,000 |
| Direct voting allowed | No |

### Effective exposure

```text
effective_issuer_exposure = fund_market_value x look_through_issuer_exposure
effective_issuer_exposure = 900,000 x 4.00% = 36,000
```

### Correct treatment

- preserve fund voting policy, share-class terms, stewardship policy, client preference, look-through exposure and manager voting report;
- separate economic exposure from voting authority;
- record client preference where policy permits, but do not show a direct proxy instruction if the fund does not allow it;
- route stewardship exceptions to advisory, DPM or governance review according to mandate policy;
- report manager-voted outcome and client preference separately where evidence exists.

### QA assertions

| Test | Expected result |
|---|---|
| Client tries to vote underlying issuer directly | Direct vote action is blocked or labelled unsupported. |
| Fund manager publishes voting report | Outcome is captured as manager stewardship evidence. |
| Client preference is recorded | Preference remains advisory metadata, not executed vote. |
| Client report is generated | Economic exposure and voting authority are distinct. |

## Example 105. ETF Trading Halt Fair-Value Override

### Scenario

An ETF is halted before market close while underlying securities continue trading. Pricing policy requires a fair-value override for client reporting and risk, with a source-limited tradability label.

| Attribute | Value |
|---|---:|
| Last ETF traded price | 98.40 |
| Fair-value estimate | 97.10 |
| Units held | 12,000 |
| Valuation difference | -15,600 |

### Fair-value impact

```text
valuation_difference = units_held x (fair_value_estimate - last_traded_price)
valuation_difference = 12,000 x (97.10 - 98.40) = -15,600
```

### Correct treatment

- preserve halt notice, last trade, underlying basket value, fair-value source, approval, timestamp and override expiry;
- use fair value for reporting only when policy and approval support it;
- keep tradability, valuation confidence and pricing source separately visible;
- prevent tradeability assumptions from using the fair-value estimate as executable market price;
- reverse or replace the override when ETF trading resumes and official close is available.

### QA assertions

| Test | Expected result |
|---|---|
| ETF is halted at close | Valuation uses approved fair value or is source-limited. |
| Fair-value approval is missing | Pricing is blocked, provisional or escalated. |
| ETF resumes trading | Override expires and official price replaces it from effective time. |
| Risk report is generated | Last traded price, fair value and halt state are distinct. |

## Example 106. Private-Fund NAV Side-Letter Reporting Tier

### Scenario

A private-fund investor has a side letter that permits quarterly enhanced NAV transparency. Another investor in the same fund receives only standard NAV reporting.

| Attribute | Investor A | Investor B |
|---|---:|---:|
| Capital account | 5,000,000 | 5,000,000 |
| Standard NAV reporting | Yes | Yes |
| Enhanced look-through tier | Yes | No |
| Look-through exposure shared | 2,100,000 | 0 |

### Enhanced coverage

```text
enhanced_coverage = look_through_exposure_shared / capital_account
enhanced_coverage = 2,100,000 / 5,000,000 = 42.00%
```

### Correct treatment

- preserve side letter, investor id, reporting tier, entitlement approval, look-through file and recipient permissions;
- separate fund-level NAV from investor-specific disclosure rights;
- prevent enhanced look-through fields from leaking to investors without entitlement;
- label reports by reporting tier and source date;
- keep analytics reusable internally only where policy permits aggregation without breaching confidentiality.

### QA assertions

| Test | Expected result |
|---|---|
| Investor has enhanced reporting tier | Additional look-through fields are visible only to authorized recipients. |
| Investor lacks side-letter entitlement | Standard NAV report excludes enhanced fields. |
| Side-letter expires | Enhanced reporting stops or becomes provisional until renewed. |
| Report is generated | NAV amount and disclosure tier are independently auditable. |

## Example 107. Fund Expense Accrual Cut-Off Dispute

### Scenario

A fund administrator accrues an annual audit expense after the NAV cut-off. The manager disputes whether the expense belongs in the current NAV or next valuation period.

| Attribute | Value |
|---|---:|
| Fund NAV before expense | 40,000,000 |
| Audit expense | 120,000 |
| Units outstanding | 2,000,000 |
| NAV impact per unit | 0.06 |

### NAV impact

```text
nav_impact_per_unit = audit_expense / units_outstanding
nav_impact_per_unit = 120,000 / 2,000,000 = 0.06

post_expense_nav = fund_nav_before_expense - audit_expense
post_expense_nav = 40,000,000 - 120,000 = 39,880,000
```

### Correct treatment

- preserve expense invoice, approval date, service period, NAV cut-off, administrator calculation and manager dispute;
- keep current-period NAV, disputed expense accrual and final confirmed NAV separately visible;
- avoid posting final investor NAV impact until administrator confirms period treatment;
- disclose provisional NAV or expense dispute where reporting policy requires it;
- update performance, fees and equalization after final treatment is confirmed.

### QA assertions

| Test | Expected result |
|---|---|
| Expense approval is after cut-off | Expense period is disputed or allocated to next period according to policy. |
| Administrator confirms current NAV impact | NAV and per-unit impact update with source lineage. |
| Final decision reverses accrual | NAV restates without duplicate expense booking. |
| Client report is generated | Expense dispute and provisional/final NAV state are visible. |

## Example 108. UCITS Eligible-Asset Breach Cure

### Scenario

A UCITS fund temporarily breaches an eligible-asset rule after a security is reclassified. The manager sells the ineligible asset within the cure period.

| Attribute | Value |
|---|---:|
| Fund NAV | 60,000,000 |
| Ineligible exposure | 1,800,000 |
| Breach ratio | 3.00% |
| Cure deadline | 30 days |

### Breach ratio

```text
breach_ratio = ineligible_exposure / fund_nav
breach_ratio = 1,800,000 / 60,000,000 = 3.00%
```

### Correct treatment

- preserve eligibility rule, classification source, breach notice, manager action plan, sale evidence and cure date;
- label breach state separately from fund suspension, fund liquidation or client mandate breach;
- keep buy eligibility, model eligibility and reporting warnings policy-driven during cure period;
- clear breach only when source evidence confirms exposure reduction or rule remediation;
- retain breach history for governance, audit and suitability review.

### QA assertions

| Test | Expected result |
|---|---|
| Asset becomes ineligible | Breach ratio and cure deadline are calculated. |
| Cure sale completes within deadline | Breach clears with sale and exposure evidence. |
| Cure period expires unresolved | Escalation, restriction or suspension workflow opens. |
| Advisory proposal is generated | Fund is warning-labelled or blocked according to policy. |

## Example 109. Feeder Liquidity Mismatch Stress

### Scenario

A feeder fund offers monthly liquidity, but the master fund changes liquidity to quarterly during stress. The feeder must adjust projected redemption capacity and client communication.

| Attribute | Value |
|---|---:|
| Feeder redemption requests | 10,000,000 |
| Master available liquidity | 6,000,000 |
| Feeder liquidity support | 1,000,000 |
| Unfunded redemption demand | 3,000,000 |

### Liquidity mismatch

```text
total_feeder_redemption_capacity = master_available_liquidity + feeder_liquidity_support
total_feeder_redemption_capacity = 6,000,000 + 1,000,000 = 7,000,000

unfunded_redemption_demand = feeder_redemption_requests - total_feeder_redemption_capacity
unfunded_redemption_demand = 10,000,000 - 7,000,000 = 3,000,000
```

### Correct treatment

- preserve feeder terms, master fund liquidity notice, bridge facility, redemption queue, gate policy and client communication approval;
- separate feeder-level liquidity promise from master-level liquidity source;
- update projected redemption cash, queue state, gates and DPM funding assumptions;
- label stress state without treating all feeder NAV as unavailable;
- retain mismatch history for liquidity risk, suitability and product-governance review.

### QA assertions

| Test | Expected result |
|---|---|
| Master liquidity becomes quarterly | Feeder projected capacity recalculates from master and support sources. |
| Requests exceed capacity | Unfunded demand, queue and client communication workflow open. |
| Bridge support is approved | Capacity includes support only with source-backed terms. |
| Liquidity report is generated | Feeder promise, master liquidity and unfunded demand are distinct. |

## Example 110. Fund Proxy-Voting Split Ballot

### Scenario

A fund holds shares in an issuer and receives a proxy ballot. The fund manager votes part of the eligible exposure for the proposal, part against, and abstains the remainder because different sleeves are managed under different policies.

| Attribute | Value |
|---|---:|
| Fund market value | 80,000,000 |
| Look-through issuer exposure | 2.50% |
| Vote-for allocation | 55.00% |
| Vote-against allocation | 35.00% |
| Abstain allocation | 10.00% |

### Split vote exposure

```text
issuer_exposure_value = fund_market_value x look_through_issuer_exposure
issuer_exposure_value = 80,000,000 x 2.50% = 2,000,000

vote_for_exposure = issuer_exposure_value x vote_for_allocation
vote_for_exposure = 2,000,000 x 55.00% = 1,100,000

vote_against_exposure = issuer_exposure_value x vote_against_allocation
vote_against_exposure = 2,000,000 x 35.00% = 700,000

abstain_exposure = issuer_exposure_value x abstain_allocation
abstain_exposure = 2,000,000 x 10.00% = 200,000
```

### Correct treatment

- retain ballot notice, record date, eligible position, vote-policy source, split instruction and custodian submission evidence;
- model vote allocations separately from the fund accounting position;
- ensure allocation percentages reconcile to the eligible look-through issuer exposure;
- keep abstentions visible rather than blending them into missing votes;
- avoid treating voting preference as a trade, exposure reduction or suitability override.

### QA assertions

| Test | Expected result |
|---|---|
| Split ballot is submitted | For, against and abstain exposures are calculated separately. |
| Allocations do not total 100% | Validation blocks or flags the ballot before submission. |
| Record-date holding differs from current holding | Eligible exposure uses record-date source evidence. |
| Governance report is generated | Vote policy, instruction split and submission evidence are visible. |

## Example 111. ETF Stale Intraday Indicative Value Publication

### Scenario

An ETF publishes intraday indicative value throughout the trading day, but the publication feed stalls while the exchange price continues trading. The platform must flag stale indicative value and avoid presenting the stale value as current fair value.

| Attribute | Value |
|---|---:|
| ETF units held | 40,000 |
| Last indicative value | 24.80 |
| Current exchange price | 25.15 |
| Staleness threshold | 15 minutes |
| Feed age | 47 minutes |

### Stale indicative value gap

```text
indicative_value_gap = units_held x (current_exchange_price - last_indicative_value)
indicative_value_gap = 40,000 x (25.15 - 24.80) = 14,000
```

### Correct treatment

- retain indicative value timestamp, market price, exchange status, sponsor feed status and stale threshold policy;
- flag the indicative value as stale when feed age exceeds policy;
- use the approved valuation hierarchy for reporting instead of silently using the stale indicative value;
- keep indicative value gap available for monitoring ETF market quality and advisor explanation;
- avoid overwriting exchange price, NAV, indicative value and fair-value override fields into one price.

### QA assertions

| Test | Expected result |
|---|---|
| Feed age exceeds threshold | Indicative value is stale-labelled. |
| Exchange price remains live | Price hierarchy uses approved live or fair-value source. |
| Gap exceeds tolerance | ETF monitoring exception opens. |
| Client view is generated | Stale indicative value is not shown as current fair value. |

## Example 112. Private-Fund Fee Letter Amendment Dispute

### Scenario

A private fund issues an amended fee letter that changes the management-fee rate for a subset of investors. The investor disputes whether the amendment applies to their side-letter class and effective date.

| Attribute | Value |
|---|---:|
| Capital account value | 6,500,000 |
| Prior annual fee rate | 1.00% |
| Amended annual fee rate | 1.20% |
| Disputed period | 90 days |

### Disputed fee uplift

```text
annual_fee_delta = capital_account_value x (amended_fee_rate - prior_fee_rate)
annual_fee_delta = 6,500,000 x (1.20% - 1.00%) = 13,000

disputed_fee_uplift = annual_fee_delta x disputed_days / 365
disputed_fee_uplift = 13,000 x 90 / 365 = 3,205.48
```

### Correct treatment

- retain prior fee letter, amended fee letter, side-letter class, effective-date evidence, administrator calculation and dispute owner;
- book or accrue the amended fee only under the approved dispute and accounting policy;
- keep disputed uplift separate from standard management fee accrual;
- show side-letter applicability in investor reporting and operations review;
- avoid applying amended fees across all investors when the source applies only to a subset.

### QA assertions

| Test | Expected result |
|---|---|
| Fee amendment is disputed | Uplift is calculated and marked disputed. |
| Side-letter class does not match | Amendment is blocked or routed for exception approval. |
| Administrator confirms applicability | Fee accrual uses confirmed effective date and rate. |
| Investor report is generated | Standard fee and disputed uplift are separately explainable. |

## Example 113. Expense Reimbursement Clawback

### Scenario

A fund previously reimbursed expenses under an expense-cap arrangement. Later fund documents allow the manager to claw back part of the reimbursement after performance and expense conditions are met.

| Attribute | Value |
|---|---:|
| Prior reimbursement | 320,000 |
| Clawback percentage | 30.00% |
| Units outstanding | 4,000,000 |
| Clawback approval status | Approved |

### Clawback amount and NAV impact

```text
expense_reimbursement_clawback = prior_reimbursement x clawback_percentage
expense_reimbursement_clawback = 320,000 x 30.00% = 96,000

nav_impact_per_unit = expense_reimbursement_clawback / units_outstanding
nav_impact_per_unit = 96,000 / 4,000,000 = 0.0240
```

### Correct treatment

- retain original reimbursement, expense-cap terms, clawback eligibility test, board or administrator approval and NAV impact file;
- separate clawback from new operating expenses and ordinary management fees;
- reflect NAV impact and investor communication according to fund document requirements;
- preserve performance-period linkage so the clawback is not double-counted;
- avoid treating clawback as advisor fee, distribution or redemption charge.

### QA assertions

| Test | Expected result |
|---|---|
| Clawback criteria are met | Clawback amount and NAV impact per unit are calculated. |
| Approval is missing | Booking is blocked or held as pending. |
| Prior reimbursement was already used | System prevents double clawback of the same reimbursement. |
| Statement is generated | Clawback is labelled separately from current-period expense. |

## Example 114. UCITS Passive Breach From Market Movement

### Scenario

A UCITS fund breaches an issuer concentration limit because market movement increases the value of one holding relative to fund NAV. The breach is passive and must be monitored for remediation without treating it as a deliberate purchase breach.

| Attribute | Value |
|---|---:|
| Fund NAV | 125,000,000 |
| Issuer exposure after market move | 13,375,000 |
| Concentration limit | 10.00% |
| Breach cause | Market movement |

### Passive breach excess

```text
issuer_exposure_ratio = issuer_exposure_after_market_move / fund_nav
issuer_exposure_ratio = 13,375,000 / 125,000,000 = 10.70%

passive_breach_excess = issuer_exposure_after_market_move - (fund_nav x concentration_limit)
passive_breach_excess = 13,375,000 - (125,000,000 x 10.00%) = 875,000
```

### Correct treatment

- retain compliance limit, market movement evidence, exposure calculation, breach classification and remediation plan;
- classify breach cause separately from breach amount and cure status;
- avoid blocking all fund activity unless policy requires it;
- monitor remediation timing, investor communication and product-governance impact;
- avoid treating passive breach as an intentional eligibility breach without trade evidence.

### QA assertions

| Test | Expected result |
|---|---|
| Market move causes limit breach | Breach is classified as passive with excess amount. |
| New purchase would worsen breach | Trade control blocks or escalates according to policy. |
| Market movement reverses | Breach clears only with source-backed exposure calculation. |
| Compliance report is generated | Cause, ratio, excess and remediation state are visible. |

## Example 115. Feeder Emergency Liquidity Facility Drawdown

### Scenario

A feeder fund faces redemption requests before the master fund can provide liquidity. An emergency facility is approved to bridge part of the funding gap. The facility draw must be separated from master fund liquidity, investor redemptions and fund expenses.

| Attribute | Value |
|---|---:|
| Redemption cash required | 9,000,000 |
| Master liquidity available | 5,500,000 |
| Approved facility limit | 2,000,000 |
| Facility interest rate | 6.00% |
| Draw period | 45 days |

### Facility drawdown and cost

```text
redemption_shortfall_before_facility = redemption_cash_required - master_liquidity_available
redemption_shortfall_before_facility = 9,000,000 - 5,500,000 = 3,500,000

facility_drawdown = min(redemption_shortfall_before_facility, approved_facility_limit)
facility_drawdown = min(3,500,000, 2,000,000) = 2,000,000

facility_interest_cost = facility_drawdown x facility_interest_rate x draw_days / 360
facility_interest_cost = 2,000,000 x 6.00% x 45 / 360 = 15,000
```

### Correct treatment

- retain facility agreement, approval, draw notice, master liquidity notice, redemption queue and repayment plan;
- separate emergency facility proceeds from ordinary subscription cash, distribution cash and master fund redemption proceeds;
- calculate residual funding gap after facility use;
- allocate facility cost according to fund documents and investor communication requirements;
- avoid using facility availability as permanent liquidity capacity.

### QA assertions

| Test | Expected result |
|---|---|
| Facility is approved | Drawdown is capped by approved limit. |
| Shortfall exceeds facility | Residual funding gap remains visible. |
| Interest accrues | Facility cost is calculated from draw amount, rate and days. |
| Liquidity report is generated | Master liquidity, facility draw, residual gap and repayment plan are distinct. |

## Example 116. Fund Vote Instruction Deadline Miss

### Scenario

A fund vote requires investor instructions by a transfer-agent deadline. The advisor submits the vote after the custodian cut-off, so the instruction cannot be transmitted. The platform must retain late-instruction evidence and avoid showing the vote as accepted.

| Attribute | Value |
|---|---:|
| Eligible units | 48,000 |
| Instruction units submitted | 48,000 |
| Custodian cut-off | Day 10 16:00 |
| Advisor submission | Day 10 17:25 |
| Late units | 48,000 |

### Late voting exposure

```text
late_vote_units = instruction_units_submitted where submission_time > custodian_cutoff
late_vote_units = 48,000
```

### Correct treatment

- retain voting notice, eligible units, custodian cut-off, advisor submission timestamp, transfer-agent status and late reason;
- keep vote instruction state separate from vote eligibility and fund holding state;
- report late instruction as missed or rejected, not accepted;
- preserve escalation outcome if transfer agent makes an exception;
- avoid changing investor consent, proxy status or voting outcome without source acceptance.

### QA assertions

| Test | Expected result |
|---|---|
| Instruction arrives after cut-off | Vote status becomes late or rejected. |
| Transfer agent grants exception | Accepted status requires source evidence. |
| Eligible units differ from submitted units | Late and accepted units are calculated separately. |
| Investor report is generated | Eligible units, submitted units and accepted vote status are distinct. |

## Example 117. ETF Fair-Value Basket Proxy Gap

### Scenario

An ETF trades during a market disruption while several basket securities have no reliable price. A fair-value proxy is approved for most securities, but some basket exposure remains unpriced.

| Attribute | Value |
|---|---:|
| Total basket market value | 18,000,000 |
| Securities with approved proxy | 15,750,000 |
| Securities without proxy | 2,250,000 |
| Unpriced basket ratio | 12.50% |

### Basket proxy gap

```text
unpriced_basket_value = total_basket_market_value - securities_with_approved_proxy
unpriced_basket_value = 18,000,000 - 15,750,000 = 2,250,000

unpriced_basket_ratio = unpriced_basket_value / total_basket_market_value
unpriced_basket_ratio = 2,250,000 / 18,000,000 = 12.50%
```

### Correct treatment

- retain ETF basket file, unavailable prices, proxy methodology, valuation committee approval and unresolved security list;
- label NAV or indicative value as fair-value estimated when proxy coverage is partial;
- prevent unpriced basket exposure from being hidden inside a normal market price;
- show uncertainty in risk, reporting and client explanations where material;
- expire proxy approval according to governance policy.

### QA assertions

| Test | Expected result |
|---|---|
| Basket securities lack prices | Unpriced basket value and ratio are calculated. |
| Proxy is approved for part of basket | Estimated and unresolved exposure are separated. |
| Proxy expires | Valuation state becomes stale or blocked until renewed. |
| Report is generated | Fair-value proxy basis and unresolved basket gap are visible. |

## Example 118. Private-Fund Most-Favoured-Nation Fee Election

### Scenario

A private fund offers an MFN election that lets an eligible investor adopt a lower management-fee term granted to another investor. The investor elects the lower fee, but only from the effective date in the side-letter process.

| Attribute | Value |
|---|---:|
| Commitment | 5,000,000 |
| Original annual fee rate | 1.50% |
| Elected MFN fee rate | 1.25% |
| Days effective in period | 90 |

### MFN fee saving

```text
period_fee_saving = commitment x (original_fee_rate - elected_mfn_fee_rate) x days_effective / 360
period_fee_saving = 5,000,000 x (1.50% - 1.25%) x 90 / 360 = 3,125
```

### Correct treatment

- retain MFN offer, eligibility evidence, election notice, accepted side-letter term, effective date and administrator confirmation;
- apply fee election from effective date, not retroactively unless terms allow it;
- keep MFN fee saving separate from rebate, distribution or capital activity;
- explain elected fee term in investor reporting where fees are transparent;
- prevent non-eligible investors from inheriting the elected term.

### QA assertions

| Test | Expected result |
|---|---|
| Investor is eligible and elects | Fee rate changes from effective date. |
| Election is late | Prior period fee remains at original rate unless accepted source says otherwise. |
| Investor lacks eligibility | MFN election is blocked. |
| Fee report is generated | Original rate, elected rate, effective days and saving reconcile. |

## Example 119. Expense-Cap Board Renewal Delay

### Scenario

A fund expense cap is due for annual renewal. The board approval arrives after the old cap expires, creating a gap period where expenses cannot automatically use the renewed cap.

| Attribute | Value |
|---|---:|
| Expense cap rate | 0.85% |
| Fund NAV | 220,000,000 |
| Renewal gap days | 12 |
| Gross expense rate | 0.96% |

### Gap-period uncapped expense exposure

```text
gap_expense_cap_exposure = fund_nav x (gross_expense_rate - expense_cap_rate) x renewal_gap_days / 365
gap_expense_cap_exposure = 220,000,000 x (0.96% - 0.85%) x 12 / 365 = 7,956.16
```

### Correct treatment

- retain prior cap agreement, expiry date, board agenda, delayed approval, effective date and administrator treatment;
- avoid applying renewed cap to the gap period without explicit retroactive approval;
- show pending cap renewal separately from active cap;
- reconcile expense accruals if board approval is later made retroactive;
- disclose or report material expense treatment changes according to fund policy.

### QA assertions

| Test | Expected result |
|---|---|
| Cap expires before renewal | Gap period is flagged. |
| Renewal is retroactive | Gap accrual is recalculated with approval evidence. |
| Renewal is not retroactive | Gross or uncapped treatment applies to gap according to policy. |
| Expense report is generated | Cap expiry, renewal date and gap exposure are visible. |

## Example 120. UCITS Breach Cure Sequencing

### Scenario

A UCITS fund has multiple compliance breaches. The manager proposes curing the smaller breach first, but policy requires curing the active investment breach before a passive market-movement breach.

| Attribute | Value |
|---|---:|
| Active breach excess | 1,100,000 |
| Passive breach excess | 875,000 |
| Available trade capacity | 1,250,000 |
| Required first cure | Active breach |

### Cure capacity after first breach

```text
remaining_capacity_after_active_cure = available_trade_capacity - active_breach_excess
remaining_capacity_after_active_cure = 1,250,000 - 1,100,000 = 150,000
```

### Correct treatment

- retain breach register, cause classification, remediation policy, proposed trades, capacity constraints and compliance approval;
- sequence active and passive breach remediation according to policy, not only breach size;
- show partial cure capacity and residual passive breach after active cure;
- prevent remediation trades that worsen another breach without approval;
- keep compliance sign-off with the final cure order.

### QA assertions

| Test | Expected result |
|---|---|
| Active and passive breaches coexist | Cure sequence follows policy priority. |
| Capacity cannot cure both | Residual breach remains with owner and plan. |
| Proposed cure worsens another limit | Trade is blocked or escalated. |
| Compliance report is generated | Breach cause, sequence, capacity and residual exposure are visible. |

## Example 121. Feeder Facility Repayment Waterfall

### Scenario

A feeder fund used an emergency liquidity facility. Later master fund redemption proceeds arrive and must repay facility principal, accrued interest and remaining investor redemption amounts in the agreed waterfall order.

| Attribute | Value |
|---|---:|
| Master redemption proceeds | 2,250,000 |
| Facility principal outstanding | 2,000,000 |
| Accrued facility interest | 15,000 |
| Residual proceeds after facility repayment | 235,000 |

### Repayment waterfall

```text
facility_repayment_due = facility_principal_outstanding + accrued_facility_interest
facility_repayment_due = 2,000,000 + 15,000 = 2,015,000

residual_after_facility_repayment = master_redemption_proceeds - facility_repayment_due
residual_after_facility_repayment = 2,250,000 - 2,015,000 = 235,000
```

### Correct treatment

- retain master redemption notice, facility agreement, repayment waterfall, interest calculation, payment evidence and investor redemption queue;
- repay facility principal and interest before releasing residual liquidity when documents require that order;
- keep facility repayment separate from investor redemption cash and fund expenses;
- calculate any residual investor funding gap after waterfall application;
- report emergency facility use, repayment and remaining liquidity stress distinctly.

### QA assertions

| Test | Expected result |
|---|---|
| Master proceeds arrive | Repayment waterfall applies before residual liquidity is released. |
| Proceeds are insufficient | Facility principal, interest and investor residual are prioritized by terms. |
| Interest calculation changes | Repayment due recalculates from source rate and days. |
| Liquidity report is generated | Proceeds, repayment, residual cash and remaining investor gap reconcile. |
