# 10. Worked Examples and Implementation Patterns

## Purpose

This file turns cash, deposit, money-market and FX concepts into practical examples for advisory workflows, portfolio analytics, reporting, implementation and QA.

## Example 1. Ledger cash versus available cash

### Scenario

A client account has USD cash, unsettled equity trades and an open fund subscription.

| Item | Amount | Treatment |
|---|---:|---|
| Ledger cash | 500,000 | Current accounting cash balance |
| Pending equity buy settlement | -120,000 | Cash reserved for settlement |
| Pending bond sale proceeds | 80,000 | Expected but not settled cash |
| Open fund subscription order | -50,000 | Cash blocked for pending order |
| Minimum liquidity buffer | -25,000 | Policy or mandate reserve |

### Calculation

```text

projected cash = ledger cash - pending buys + pending sells - order blocks - liquidity buffer
projected cash = 500,000 - 120,000 + 80,000 - 50,000 - 25,000
projected cash = 385,000

```

### Correct interpretation

The client does not have USD 500,000 available for new orders. The platform should distinguish:

| Balance | Amount | Use |
|---|---:|---|
| Ledger cash | 500,000 | Accounting and statement balance. |
| Projected cash | 460,000 | Includes pending settlement movements. |
| Available cash | 385,000 | Applies order blocks and liquidity buffer. |

### QA assertions

| Test | Expected result |
|---|---|
| New order asks for USD 450,000 cash funding | Blocked or escalated because available cash is USD 385,000. |
| Pending bond sale fails settlement | Available cash is recomputed without the USD 80,000 expected proceeds. |
| Fund order is cancelled | USD 50,000 order block is released. |
| Liquidity buffer is mandate-specific | Same ledger cash may produce different available cash by mandate. |

## Example 2. Term deposit accrual, maturity and early breakage

### Scenario

A client places a 90-day USD term deposit.

| Attribute | Value |
|---|---:|
| Principal | 1,000,000 |
| Annual rate | 4.20% |
| Tenor | 90 days |
| Day-count basis | Actual/360 |
| Early break day | Day 45 |
| Breakage rate applied | 2.00% annualized for actual holding period |

### Full maturity interest

```text

interest = principal x annual rate x days / day-count denominator
interest = 1,000,000 x 4.20% x 90 / 360
interest = 10,500

maturity proceeds = principal + interest
maturity proceeds = 1,010,500

```

### Early breakage interest

```text

breakage interest = 1,000,000 x 2.00% x 45 / 360
breakage interest = 2,500

early redemption proceeds = 1,002,500

```

### Reporting treatment

| Report area | Full maturity | Early break |
|---|---|---|
| Position | Deposit shown until maturity date. | Deposit closed on break date. |
| Income | Interest accrues over tenor. | Accrued interest is revised to breakage amount. |
| Performance | Interest is investment income. | Lost expected interest is not a market loss unless policy explicitly shows opportunity impact. |
| Liquidity | Locked until maturity. | Available after break settlement. |

### QA assertions

| Test | Expected result |
|---|---|
| Deposit is broken before maturity | Interest uses breakage terms, not original maturity terms. |
| Accrued income was already reported | Accrual is adjusted or reversed consistently. |
| Deposit currency differs from portfolio base currency | Principal and income are translated separately for reporting. |
| Break settlement is next business day | Available cash updates on value date, not request date. |

## Example 3. Money market fund liquidity gate

### Scenario

A client holds a money market fund that imposes a temporary redemption gate during market stress.

| Attribute | Value |
|---|---|
| Fund type | Floating NAV money market fund |
| Holding | 2,000,000 units |
| Latest NAV | 1.0025 |
| Requested redemption | 1,000,000 units |
| Gate limit | 25% of holding |
| Allowed redemption | 500,000 units |

### Valuation

```text

market value = units x NAV
market value = 2,000,000 x 1.0025
market value = 2,005,000

allowed redemption value = 500,000 x 1.0025
allowed redemption value = 501,250

queued redemption units = 1,000,000 - 500,000
queued redemption units = 500,000

```

### Platform behavior

| Workflow | Correct treatment |
|---|---|
| Redemption order | Split into allowed and queued portions or reject amount above gate. |
| Available cash | Does not include queued redemption proceeds. |
| Liquidity analytics | Holding is flagged as liquidity restricted. |
| Client report | Shows fund value, gate note and pending redemption status. |
| Advisory | Warns that money market funds are not identical to bank cash. |

### QA assertions

| Test | Expected result |
|---|---|
| Client requests full redemption | Only gate-allowed portion proceeds if policy permits split. |
| NAV becomes stale during gate | Report shows stale valuation status. |
| Gate is lifted | Queued order is re-evaluated against latest fund terms. |
| Fund is stable NAV rather than floating NAV | Valuation and reporting labels follow fund type. |

## Example 4. FX spot conversion and settlement exposure

### Scenario

A client converts USD to SGD for a securities purchase.

| Attribute | Value |
|---|---:|
| USD sold | 100,000 |
| FX rate | 1.3500 SGD per USD |
| SGD bought | 135,000 |
| Trade date | T |
| Value date | T+2 |

### Transaction model

```text

USD cash leg = -100,000
SGD cash leg = +135,000

```

### Position and cash treatment

| Date basis | USD cash | SGD cash | Explanation |
|---|---:|---:|---|
| Trade date | Reduced or reserved | Increased as pending | Economic exposure has changed. |
| Settlement date | Settled outflow | Settled inflow | Cash is legally available after value date. |
| Available cash before settlement | Reduced | Usually not fully available | Depends on funding, credit and settlement policy. |

### QA assertions

| Test | Expected result |
|---|---|
| SGD securities buy is booked before FX settles | Buying power must consider pending FX proceeds and settlement timing. |
| USD leg settles but SGD leg fails | Settlement exception is raised; cash availability is restricted. |
| Value date is a currency holiday | Next valid joint currency value date is used. |
| Portfolio base currency is SGD | FX trade should not create artificial investment gain from the conversion itself. |

## Example 5. FX forward hedge valuation and exposure

### Scenario

A USD-based client holds EUR assets and enters a EUR forward hedge.

| Attribute | Value |
|---|---:|
| EUR asset exposure | 1,000,000 |
| Forward action | Sell EUR / buy USD |
| Forward notional | 700,000 EUR |
| Hedge ratio | 70% |
| Contract forward rate | 1.0900 USD per EUR |
| Current forward valuation rate | 1.0700 USD per EUR |

### Mark-to-market estimate

For a simplified reporting estimate:

```text

forward value = notional x (contract rate - current forward valuation rate)
forward value = 700,000 x (1.0900 - 1.0700)
forward value = 14,000 USD gain

```

Actual valuation may require discounting, curve inputs, credit valuation adjustments and independent price verification depending on policy.

### Exposure treatment

| Exposure dimension | Treatment |
|---|---|
| Legal holding | Forward is a derivative contract, not a cash balance. |
| Currency exposure | EUR exposure is reduced by hedge notional for economic exposure reporting. |
| Counterparty exposure | Positive mark-to-market creates counterparty exposure. |
| Liquidity | Contract may require close-out rather than normal sale. |
| Performance | Hedge P&L should be separated from underlying asset return. |

### QA assertions

| Test | Expected result |
|---|---|
| Forward notional exceeds EUR asset exposure | Over-hedge warning appears. |
| Forward matures before asset sale | Hedge expiry creates unhedged exposure unless rolled. |
| Valuation rate missing | Report shows source-limited derivative valuation state. |
| Counterparty is downgraded | Counterparty exposure and risk controls update. |

## Example 6. NDF cash settlement

### Scenario

A client uses a non-deliverable forward to hedge an emerging-market currency exposure. The contract settles in USD.

| Attribute | Value |
|---|---:|
| NDF notional | 10,000,000 local currency |
| Contract rate | 5.0000 local per USD |
| Fixing rate | 5.1000 local per USD |
| Settlement currency | USD |

### Simplified settlement calculation

One common settlement approach calculates the USD cash difference using:

```text

settlement amount = notional x (1 / contract rate - 1 / fixing rate)
settlement amount = 10,000,000 x (1 / 5.0000 - 1 / 5.1000)
settlement amount = 10,000,000 x (0.200000 - 0.196078)
settlement amount = 39,220 USD

```

The sign depends on whether the client bought or sold the non-deliverable currency and the contract convention.

### Platform treatment

| Object | Treatment |
|---|---|
| Contract | Derivative with notional in restricted currency. |
| Fixing | Official source input with timestamp and source lineage. |
| Settlement | Cash movement only in settlement currency. |
| Exposure | Economic exposure to local currency until fixing or maturity. |
| Reporting | Show notional, settlement currency, fixing rate and realised P&L. |

### QA assertions

| Test | Expected result |
|---|---|
| Fixing source is missing | Contract cannot settle automatically. |
| Fixing is corrected after settlement | Adjustment or reversal workflow is triggered. |
| Wrong sign convention is configured | Regression test catches P&L sign error. |
| Local currency is treated as deliverable cash | Data-model validation fails. |

## Example 7. Liquidity ladder across cash-like instruments

### Scenario

A portfolio holds operating cash, a notice deposit, a T-bill and a money market fund.

| Holding | Value | Liquidity bucket |
|---|---:|---|
| Current account cash | 300,000 | Same day |
| Money market fund | 500,000 | T+1 under normal conditions |
| 32-day notice deposit | 750,000 | 32 days after notice |
| T-bill maturing in 75 days | 1,000,000 | 75 days or secondary sale |

### Reporting view

| Bucket | Amount |
|---|---:|
| Same day | 300,000 |
| Next day | 500,000 |
| 2 to 35 days | 750,000 |
| 36 to 90 days | 1,000,000 |

### Advisory interpretation

The portfolio may look cash-heavy by market value, but only a portion is same-day liquid. Liquidity reporting should separate cash-like valuation from actual cash availability.

### QA assertions

| Test | Expected result |
|---|---|
| Money market fund gate occurs | T+1 bucket is reduced or flagged as restricted. |
| Notice is submitted for deposit | Projected cashflow appears on notice maturity date. |
| T-bill is sold before maturity | Trade lifecycle and settlement determine actual cash date. |
| Client has a near-term withdrawal need | Advisory workflow uses liquidity ladder, not total cash-like value. |

## Example 8. Multi-currency buying power with FX funding

### Scenario

A client wants to buy a SGD security, but most free liquidity is in USD.

| Attribute | Value |
|---|---:|
| SGD ledger cash | 20,000 |
| SGD reserved for fees and pending orders | -5,000 |
| USD ledger cash | 100,000 |
| USD reserved cash | -10,000 |
| FX rate | 1.3500 SGD per USD |
| FX haircut / buffer | 2.00% |
| Proposed SGD purchase | 120,000 |

### Calculation

```text
available SGD cash = 20,000 - 5,000
available SGD cash = 15,000

available USD cash = 100,000 - 10,000
available USD cash = 90,000

SGD equivalent before buffer = 90,000 x 1.3500
SGD equivalent before buffer = 121,500

SGD equivalent after FX buffer = 121,500 x (1 - 2.00%)
SGD equivalent after FX buffer = 119,070

total SGD buying power = 15,000 + 119,070
total SGD buying power = 134,070
```

### Correct interpretation

The order appears fundable after FX conversion and buffer, but it is not the same as settled SGD cash. The platform should show:

| View | Amount | Treatment |
|---|---:|---|
| Settled SGD available cash | 15,000 | Immediately available in purchase currency. |
| FX-funded buying power | 119,070 | Available only if FX conversion is permitted and executable. |
| Total indicative buying power | 134,070 | Should carry FX dependency and buffer basis. |
| Proposed purchase | 120,000 | Passes indicative funding, subject to FX execution and settlement policy. |

### Advisory and DPM treatment

| Area | Treatment |
|---|---|
| Advisory | Explain that the purchase depends on converting USD into SGD and may be affected by rate movement, spread, execution cut-off and settlement timing. |
| DPM | Rebalance engines should reserve the USD funding source and create a linked FX requirement, not just consume aggregate portfolio cash. |
| Reporting | Show purchase-currency cash, source-currency cash, FX funding dependency, buffer basis and pending conversion state. |
| Operations | If FX does not execute, the purchase order should remain blocked, amended or escalated according to funding policy. |

### QA assertions

| Test | Expected result |
|---|---|
| FX rate moves beyond buffer before execution | Buying power is recalculated and the purchase may become blocked. |
| USD cash is pledged or restricted | Restricted USD is excluded from FX-funded buying power. |
| FX cut-off has passed | Buying power is labelled next-value-date or unavailable according to policy. |
| Purchase currency differs from portfolio base currency | Funding uses purchase currency and value date, not only portfolio-base reporting value. |
| Bridge funding is not supported | Pending FX proceeds cannot be treated as settled purchase cash before allowed policy date. |

## Example 9. Failed FX settlement and cash restriction

### Scenario

A client enters an FX spot trade to fund a securities purchase. One currency leg settles, but the other does not arrive on value date.

| Attribute | Value |
|---|---:|
| FX action | Sell USD / buy SGD |
| USD sold | 100,000 |
| Contract rate | 1.3500 SGD per USD |
| Expected SGD bought | 135,000 |
| Value date | T+2 |
| Linked securities purchase | SGD 130,000 |

### Expected transaction state

| Date | USD leg | SGD leg | Platform treatment |
|---|---:|---:|---|
| Trade date | Pending outflow | Pending inflow | Reserve USD and project SGD. |
| Value date morning | -100,000 settled | Pending | Restrict projected SGD until matched. |
| Value date end | -100,000 settled | Failed | Raise FX settlement exception and block linked settlement use. |
| Resolution date | Confirmed adjustment | +135,000 or corrected amount | Release or amend restricted cash after source confirmation. |

### Failed-settlement treatment

The platform should distinguish:

| State | Meaning |
|---|---|
| Pending FX receivable | Expected cashflow before value date or before confirmation. |
| Failed FX receivable | Contractual receivable did not settle as expected. |
| Restricted projected cash | Cash shown for transparency but not available for new funding. |
| Corrected settlement | Later source-confirmed settlement, adjustment or cancellation. |

### Reporting and operations

| Area | Correct treatment |
|---|---|
| Cash report | Show failed or pending SGD receivable separately from settled SGD cash. |
| Buying power | Exclude failed SGD receivable unless explicit bridge funding is approved. |
| Trade settlement | Linked securities settlement should block, use approved overdraft/credit, or escalate. |
| Reconciliation | Match custodian cash movement, FX confirmation and internal trade state. |
| Client explanation | Explain settlement delay without showing projected cash as available cash. |

### QA assertions

| Test | Expected result |
|---|---|
| One FX leg settles and the other fails | Exception workflow opens and unmatched cash is restricted. |
| Linked securities purchase needs failed proceeds | Purchase is blocked, bridged or escalated according to policy. |
| Counterparty later corrects amount | Adjustment preserves original failed state and correction lineage. |
| FX trade is cancelled after one leg settles | Reversal workflow is required; no silent deletion of cash movement. |
| Report is generated during failure window | Report labels the receivable as failed/pending and excludes it from available cash. |

## Example 10. Cash sweep into money market fund

### Scenario

An account has excess USD cash above its operating buffer. The service model sweeps excess cash into a money market fund.

| Attribute | Value |
|---|---:|
| USD ledger cash before sweep | 250,000 |
| Minimum operating cash buffer | 50,000 |
| Sweep threshold | 100,000 |
| Target residual cash after sweep | 60,000 |
| Money market fund NAV | 1.0008 |

### Sweep calculation

```text
excess cash above target residual = 250,000 - 60,000
excess cash above target residual = 190,000

sweep order amount = 190,000
estimated MMF units = 190,000 / 1.0008
estimated MMF units = 189,848.12
```

Final units should use confirmed dealing NAV, fees and fund settlement terms.

### Lifecycle treatment

| Step | Treatment |
|---|---|
| Sweep trigger | Identify eligible cash after reservations, restrictions, upcoming withdrawals and pending settlements. |
| Order creation | Create MMF subscription order with cut-off, dealing date and settlement date. |
| Cash reservation | Reserve swept amount until order is accepted, rejected or cancelled. |
| NAV confirmation | Convert cash amount to confirmed units using dealing NAV. |
| Settlement | Reduce cash and create/adjust MMF position according to settlement confirmation. |
| Unwind | Redemption follows fund order lifecycle, not instant cash unless product terms support same-day liquidity. |

### Reporting and analytics

| Area | Correct treatment |
|---|---|
| Cash reporting | Show residual operating cash and pending sweep separately. |
| Liquidity | Money market fund is cash-like but not identical to bank cash; liquidity bucket depends on fund terms. |
| Performance | Sweep fund return is investment return; movement from cash to MMF is internal activity. |
| Advisory | Explain yield pickup, NAV/liquidity risk, gate/suspension risk and settlement timing. |
| DPM | Do not let automatic sweep consume cash reserved for model trades, fees, withdrawals or collateral calls. |

### QA assertions

| Test | Expected result |
|---|---|
| Pending withdrawal exists | Sweep excludes cash required for the withdrawal. |
| MMF order misses cut-off | Sweep uses next eligible dealing date and keeps reserved cash visible. |
| NAV changes between estimate and confirmation | Units use confirmed NAV, while estimate remains audit context. |
| Fund applies liquidity gate | Sweep or redemption follows gate behavior and liquidity labels update. |
| Cash buffer differs by mandate | Sweep threshold and residual cash are mandate/service-model specific. |

## Example 11. Approved overdraft funding for a securities purchase

### Scenario

A client wants to buy a security before sale proceeds settle. The platform supports an approved overdraft only when the account, client segment, product, currency and mandate permit temporary funding.

| Attribute | Value |
|---|---:|
| Settled USD cash | 20,000 |
| Pending sale proceeds settling T+2 | 80,000 |
| Security purchase cash required T+1 | 65,000 |
| Approved overdraft limit | 50,000 |
| Overdraft buffer reserved by policy | 5,000 |

### Funding calculation

```text
cash shortfall on purchase date = purchase cash required - settled cash
cash shortfall on purchase date = 65,000 - 20,000 = 45,000

usable overdraft = approved limit - required buffer
usable overdraft = 50,000 - 5,000 = 45,000

post-trade available cash = settled cash - purchase cash required + usable overdraft
post-trade available cash = 20,000 - 65,000 + 45,000 = 0
```

### Correct treatment

The pending sale proceeds remain projected cash until settlement. The purchase can proceed only because an explicit overdraft facility covers the temporary shortfall. Reporting should show settled cash, projected cash, overdraft utilization and facility headroom separately.

### QA assertions

| Test | Expected result |
|---|---|
| Purchase shortfall exceeds usable overdraft | Order is blocked, resized or escalated. |
| Product is not eligible for overdraft funding | Buying power excludes the facility. |
| Sale proceeds fail to settle | Overdraft remains drawn and exception workflow starts. |
| Client report is produced during funding window | Report labels the negative cash as overdraft utilization, not normal cash availability. |
| Mandate disallows leverage | Purchase is blocked even if a facility exists. |

## Example 12. Negative interest on large cash balances

### Scenario

A booking centre applies negative interest to large EUR cash balances above a threshold. The charge must be accrued as an expense and not hidden inside investment performance.

| Attribute | Value |
|---|---:|
| Average EUR cash balance | 1,500,000 |
| Exempt threshold | 250,000 |
| Negative annual rate | -0.50% |
| Accrual days | 30 |
| Day-count basis | 360 |

### Accrual calculation

```text
chargeable balance = average cash balance - exempt threshold
chargeable balance = 1,500,000 - 250,000 = 1,250,000

negative interest charge = chargeable balance x abs(rate) x days / basis
negative interest charge = 1,250,000 x 0.50% x 30 / 360 = 520.83
```

### Reporting and advisory treatment

| Area | Correct treatment |
|---|---|
| Cash statement | Show negative interest as a cash expense or bank charge with period and rate basis. |
| Performance | Treat as expense/cash drag according to methodology, not as market price loss. |
| Advisory | Consider deposits, T-bills, money market funds, FX conversion or mandate constraints before moving cash. |
| DPM | Preserve liquidity buffers for fees, withdrawals, collateral and model trades before reducing cash. |
| Tax/reporting | Apply local classification and withholding/reporting rules where relevant. |

### QA assertions

| Test | Expected result |
|---|---|
| Balance is below threshold | No negative-interest accrual is created. |
| Rate changes mid-period | Accrual splits by effective date. |
| Currency is unsupported for negative interest | Calculation is blocked or labelled unsupported. |
| Charge is reversed by bank | Reversal preserves original charge lineage. |
| Client asks why cash fell | Statement explains balance, threshold, rate, period and charge. |

## Example 13. Cross-currency settlement holiday

### Scenario

A USD client buys a JPY security. The order requires FX funding, but the USD and JPY calendars do not share the same valid settlement date.

| Attribute | Value |
|---|---|
| Security trade date | Monday |
| Security settlement cycle | T+2 |
| Intended security settlement | Wednesday |
| USD holiday | Wednesday |
| JPY business day | Wednesday |
| Next valid joint USD/JPY FX value date | Thursday |

### Platform behavior

| Step | Correct treatment |
|---|---|
| Trade capture | Store security settlement date and required funding currency. |
| FX planning | Calculate the earliest valid joint currency value date. |
| Funding check | Identify one-day mismatch between security settlement and FX value date. |
| Decision | Block, pre-fund, use approved credit, or amend settlement instructions according to policy. |
| Reporting | Show the settlement mismatch and funding dependency before treating cash as available. |

### QA assertions

| Test | Expected result |
|---|---|
| Currency holiday creates mismatch | Buying power is labelled settlement-dependent. |
| Prefunding is supported | Required USD or JPY cash is reserved before security settlement. |
| Credit bridge is not approved | Trade is blocked or escalated before settlement date. |
| Calendar is missing | FX funding date is unavailable and order cannot be auto-approved. |
| Holiday is updated by source | Settlement plan recalculates with audit trail. |

## Example 14. Sweep unwind during market stress

### Scenario

A client needs USD 120,000 for a withdrawal. Cash is partly invested in a money market sweep fund, but the fund imposes a temporary 40% redemption gate.

| Attribute | Value |
|---|---:|
| Settled USD cash | 50,000 |
| MMF sweep position value | 150,000 |
| Withdrawal request | 120,000 |
| Redemption gate | 40% of requested redemption can settle |
| Same-day redeemable amount requested | 70,000 |

### Liquidity calculation

```text
redeemable MMF cash = requested redemption x gate percentage
redeemable MMF cash = 70,000 x 40% = 28,000

same-day cash available for withdrawal = settled cash + redeemable MMF cash
same-day cash available for withdrawal = 50,000 + 28,000 = 78,000

withdrawal shortfall = requested withdrawal - same-day cash available
withdrawal shortfall = 120,000 - 78,000 = 42,000
```

### Correct treatment

The sweep fund cannot be treated as bank cash during stress. The platform should show available cash, gated redemption amount, pending redemption, shortfall and possible funding alternatives separately.

### QA assertions

| Test | Expected result |
|---|---|
| Fund gate applies | Available cash includes only confirmed redeemable amount. |
| Withdrawal exceeds available liquidity | Workflow offers partial payment, alternative funding or escalation. |
| Gate is lifted later | Pending redemption can proceed with new source confirmation. |
| Report is generated during gate | Liquidity label states gated fund exposure and shortfall. |
| DPM model trade competes for cash | Withdrawal priority, mandate policy and client instruction determine reservation order. |

## Example 15. Credit-line-funded purchase and collateral reservation

### Scenario

A client buys a bond using a secured credit line. The order is permitted only if collateral value, haircuts, concentration caps and currency policy leave enough borrowing headroom after the purchase.

| Attribute | Value |
|---|---:|
| Facility limit | 500,000 |
| Current loan exposure | 300,000 |
| Eligible collateral market value | 900,000 |
| Collateral haircut | 45% |
| Bond purchase funding need | 120,000 |
| Minimum headroom after purchase | 20,000 |

### Borrowing capacity calculation

```text
collateral lending value = collateral market value x (1 - haircut)
collateral lending value = 900,000 x 55% = 495,000

facility headroom = min(facility limit, collateral lending value) - current exposure
facility headroom = min(500,000, 495,000) - 300,000 = 195,000

required headroom including buffer = purchase funding need + minimum headroom
required headroom including buffer = 120,000 + 20,000 = 140,000

remaining headroom after purchase = 195,000 - 120,000 = 75,000
```

### Correct treatment

The purchase can proceed only if the credit facility is approved for the client, account, product, currency and collateral set. The purchase should create loan exposure and collateral reservation, not negative free cash that looks available.

### QA assertions

| Test | Expected result |
|---|---|
| Collateral price is stale | Facility availability is stale or blocked. |
| Purchase breaches concentration cap | Order is blocked even when facility headroom is positive. |
| FX haircut is required | Lending value includes FX haircut before approving funding. |
| Facility is shared across accounts | Reservation reduces group-level headroom. |
| Bond settles but drawdown fails | Exception workflow links trade, loan drawdown, cash and collateral states. |

## Example 16. Overdraft Pricing Tiers And Utilization

### Scenario

An account has an approved overdraft facility with tiered pricing. A short-dated funding gap creates temporary overdraft utilization until sale proceeds settle.

| Attribute | Value |
|---|---:|
| Overdraft utilization | 180,000 |
| Tier 1 limit | 100,000 |
| Tier 1 annual rate | 6.00% |
| Tier 2 utilization | 80,000 |
| Tier 2 annual rate | 8.50% |
| Utilization days | 5 |
| Day-count basis | 360 |

### Pricing calculation

```text
tier 1 charge = 100,000 x 6.00% x 5 / 360 = 83.33
tier 2 charge = 80,000 x 8.50% x 5 / 360 = 94.44
total overdraft charge = 83.33 + 94.44 = 177.77
```

### Correct treatment

| Area | Treatment |
|---|---|
| Cash balance | Show negative cash or overdraft utilization separately from free cash. |
| Facility | Track limit, utilization, buffer, pricing tier, expiry and renewal state. |
| Interest expense | Accrue overdraft interest by tier, currency, account and effective date. |
| Performance | Treat overdraft cost as financing expense according to methodology. |
| Advisory | Explain leverage, cost, settlement dependency and mandate permission. |

### QA assertions

| Test | Expected result |
|---|---|
| Utilization crosses a tier threshold | Interest accrual splits by tier. |
| Facility expires during utilization | New orders using overdraft are blocked and existing usage is escalated. |
| Mandate disallows leverage | Buying power excludes overdraft despite available facility. |
| Sale proceeds fail to settle | Utilization remains open and escalation starts. |
| Rate changes mid-period | Accrual splits by effective date and source evidence. |

## Example 17. Intraday Liquidity Restriction

### Scenario

The account has enough end-of-day projected cash, but a same-day payment queue and market settlement cycle restrict intraday availability.

| Item | Amount | Timing |
|---|---:|---|
| Settled cash at start of day | 500,000 | 09:00 |
| Same-day outgoing payment | -300,000 | 10:30 cut-off |
| Securities settlement debit | -250,000 | 14:00 |
| Expected income receipt | 150,000 | End of day |
| Intraday minimum buffer | -25,000 | All day |

### Liquidity view

```text
intraday available after payment and settlement = 500,000 - 300,000 - 250,000 - 25,000
intraday available after payment and settlement = -75,000

end-of-day projected cash = -75,000 + 150,000
end-of-day projected cash = 75,000
```

### Correct treatment

End-of-day projected cash is positive, but the account has an intraday shortfall before the income receipt arrives. A platform should not approve new same-day cash usage merely because the end-of-day projection is positive.

| Workflow | Treatment |
|---|---|
| Payment approval | Reserve cash before cut-off and check same-day sequencing. |
| Securities settlement | Consider intraday cash need by settlement window, not only date. |
| Income receipt | Treat as projected until source confirms receipt. |
| Escalation | Open funding, overdraft, payment-delay or settlement-priority workflow. |

### QA assertions

| Test | Expected result |
|---|---|
| End-of-day cash is positive but intraday cash is negative | Same-day new orders are blocked or escalated. |
| Incoming cash is delayed | Intraday shortfall remains and payment priority is reassessed. |
| Payment cut-off is missed | Payment moves to next valid window with client/service impact status. |
| Buffer is mandate-specific | Intraday availability changes by account or service model. |

## Example 18. Payment Cut-Off Failure

### Scenario

A client withdrawal is approved, but the payment instruction misses the currency cut-off. Cash is still in the account, yet the client-visible payment date changes.

| Attribute | Value |
|---|---|
| Payment currency | EUR |
| Approved payment amount | 75,000 |
| Internal approval time | 15:40 |
| Currency payment cut-off | 15:30 |
| Original requested value date | Today |
| Next eligible value date | Next business day |

### Correct workflow

| Step | Treatment |
|---|---|
| Approval | Preserve approval time, approver, amount, currency and client instruction. |
| Cut-off validation | Compare approval and release time against payment calendar and channel cut-off. |
| Cash reservation | Keep cash reserved until payment is released, cancelled or amended. |
| Client communication | Show revised value date and reason code. |
| Reconciliation | Match payment release, bank acknowledgement and cash ledger movement. |

### QA assertions

| Test | Expected result |
|---|---|
| Payment misses cut-off | Value date rolls to next eligible business day. |
| Cash is still visible in ledger | Reserved cash is excluded from available cash. |
| Client cancels before next release | Reservation is released with cancellation lineage. |
| Holiday follows missed cut-off | Next value date respects payment calendar. |
| Payment is urgent | Escalation path records approval and any manual override evidence. |

## Example 19. Liquidity Stress Escalation

### Scenario

During market stress, a client needs USD 400,000 within two days. The account has multiple cash-like sources, but some have gates, cut-offs or settlement risk.

| Source | Value | Stress availability |
|---|---:|---:|
| Settled cash | 120,000 | 120,000 |
| Money market fund | 200,000 | 80,000 under gate |
| T-bill sale estimate | 150,000 | 140,000 after haircut and settlement risk |
| Deposit break estimate | 100,000 | 95,000 after breakage |

### Stress view

```text
stress available liquidity = 120,000 + 80,000 + 140,000 + 95,000
stress available liquidity = 435,000

stress surplus = 435,000 - 400,000
stress surplus = 35,000
```

### Correct treatment

The surplus is not the same as free cash. It depends on gates, sale execution, deposit break approval, settlement timing and client mandate permissions.

| Escalation item | Required evidence |
|---|---|
| Liquidity source ranking | Same-day cash, redeemable MMF, saleable securities, breakable deposits and credit options. |
| Haircuts | Stress haircut and source for each non-cash liquidity item. |
| Time horizon | Availability by day, cut-off and settlement status. |
| Approval | Advisor, DPM, operations, credit or treasury approval depending on action. |
| Client explanation | Explain uncertainty, sequencing and possible shortfall. |

### QA assertions

| Test | Expected result |
|---|---|
| MMF gate tightens | Stress liquidity recomputes and may breach target. |
| T-bill sale fails | Alternative funding path or escalation opens. |
| Deposit break requires approval | Liquidity remains conditional until approved. |
| Client report is generated | Stress labels distinguish cash from conditional liquidity. |
| Multiple clients draw liquidity at once | Concentration and operational capacity limits are considered. |

## Example 20. Cash Pooling Across Accounts

### Scenario

A family group uses cash pooling for liquidity monitoring. One account has surplus cash while another has a funding need, but legal ownership and mandate rules constrain movement.

| Account | Legal owner | Currency | Available cash | Funding need |
|---|---|---|---:|---:|
| Account A | Parent | USD | 300,000 | 0 |
| Account B | Child trust | USD | 20,000 | 150,000 |
| Account C | Operating company | USD | 90,000 | 0 |

### Pooling treatment

| View | Treatment |
|---|---|
| Group liquidity view | Show aggregate liquidity for monitoring and planning. |
| Legal cash availability | Do not move cash across owners without authority, instruction and tax/legal checks. |
| Mandate impact | DPM and advisory rules apply at account and mandate level, not only group level. |
| Reporting | Identify pooled view as analytical unless a legal sweep or transfer is executed. |
| Audit | Preserve instruction, authority, purpose, approval and transfer evidence. |

### QA assertions

| Test | Expected result |
|---|---|
| Account B uses Account A surplus without transfer authority | Funding is blocked. |
| Group report shows aggregate liquidity | Report labels it as consolidated analytical liquidity. |
| Transfer is approved | Cash movement is posted as transfer, not investment income or performance gain. |
| Currency differs across accounts | FX funding and value-date rules apply before transfer availability. |
| Trust account restrictions apply | Pooling excludes restricted cash or labels it unavailable. |

## Example 21. Nostro Reconciliation For Cash Ledger

### Scenario

The internal USD cash ledger and bank nostro statement differ at close of business.

| Component | Amount |
|---|---:|
| Internal ledger cash | 1,250,000 |
| Nostro statement cash | 1,244,500 |
| Difference | 5,500 |

The difference is explained by a late fee posting and a same-day payment not yet acknowledged by the bank.

| Break component | Amount |
|---|---:|
| Missing custody fee | -500 |
| Pending payment acknowledgement | -5,000 |
| Total explained break | -5,500 |

### Correct reconciliation

| Step | Treatment |
|---|---|
| Detect | Compare internal cash ledger to bank/custodian statement by account, currency, date and source. |
| Explain | Split break into missing fee, pending payment, timing item, duplicate, unknown or source correction. |
| Restrict | Exclude unresolved positive cash breaks from available cash. |
| Resolve | Book missing fee, match payment acknowledgement or escalate unresolved item. |
| Close | Close only with statement line, internal posting and approver evidence. |

### QA assertions

| Test | Expected result |
|---|---|
| Break is fully explained | Reconciliation closes with component evidence. |
| Break includes unknown component | Unknown amount remains open and aged. |
| Positive break would increase buying power | Available cash excludes it until resolved. |
| Manual adjustment is used | Adjustment requires reason, approver and source evidence. |
| Same break repeats | Root-cause workflow opens for feed, mapping or timing issue. |

## Example 22. Source-calendar governance for cash and FX dates

### Scenario

A booking platform must calculate value dates for USD payments, SGD securities settlement and USD/SGD FX funding. Calendar sources disagree after an unscheduled public holiday is declared.

| Calendar input | Source A | Source B | Required treatment |
|---|---|---|---|
| USD business day | Open | Open | No conflict. |
| SGD business day | Closed | Open | Conflict blocks automatic value-date approval. |
| Payment cut-off | 16:00 | 15:30 | Use governed source or escalate conflict. |
| FX value date | T+2 | T+3 | Recalculate after calendar owner confirms. |

### Correct treatment

The platform should treat calendars as governed source data, not static code constants. Calendar conflicts affect payments, FX value dates, settlement funding, deposit maturities, notice periods, sweep dealing dates and liquidity ladders.

| Workflow | Impact |
|---|---|
| FX funding | Earliest valid joint currency value date may change. |
| Payment release | Cut-off and next value date must use governed payment calendar. |
| Securities settlement | Funding date mismatch may require prefunding, credit bridge or escalation. |
| Reporting | Projected cashflows should show calendar-source status when dates are uncertain. |

### QA assertions

| Test | Expected result |
|---|---|
| Two calendar sources disagree | Automatic date approval is blocked or marked source-conflicted. |
| Calendar owner publishes correction | Affected projected cashflows recalculate with audit trail. |
| Existing settled transaction is replayed | Historical calendar version used at booking time remains reproducible. |
| Calendar feed is unavailable | New date-sensitive workflow enters source-limited state. |

## Example 23. Payment repair queue and name-screening hold

### Scenario

A client withdrawal instruction passes cash availability checks, but payment validation identifies a beneficiary-name mismatch and a screening alert.

| Attribute | Value |
|---|---|
| Payment amount | USD 75,000 |
| Available cash before payment | USD 120,000 |
| Beneficiary name | Minor mismatch against stored beneficiary |
| Screening status | Review required |
| Payment state | Held for repair and screening |

### Cash treatment

| Balance view | Amount | Treatment |
|---|---:|---|
| Ledger cash | 120,000 | Cash still on account until payment is released. |
| Reserved cash | -75,000 | Payment amount reserved while repair/screening is open. |
| Available cash | 45,000 | Excludes held payment amount. |
| Projected cash after release | 45,000 | Applies only when payment is approved and released. |

### Correct workflow

1. Reserve the payment amount after client approval.
2. Move instruction to repair queue with reason code.
3. Keep screening and repair decisions separate.
4. Prevent reuse of reserved cash for new orders.
5. Release, amend or cancel payment only through authorized workflow.
6. Preserve client instruction, repair changes, approver and release evidence.

### QA assertions

| Test | Expected result |
|---|---|
| Payment enters repair queue | Cash is reserved and excluded from available cash. |
| Screening hold is cleared but repair remains open | Payment is not released until both conditions clear. |
| Payment is cancelled | Reservation is released with cancellation lineage. |
| Advisor tries to use reserved cash for an order | Order is blocked or resized. |
| Statement is generated during hold | Report labels cash as reserved for pending payment. |

## Example 24. Multi-bank cash concentration and intraday credit cap

### Scenario

A client holds cash across two banks. A treasury process concentrates surplus into the preferred operating bank, but the receiving bank applies an intraday credit cap.

| Bank | Currency | Cash balance | Concentration action |
|---|---|---:|---|
| Bank A | USD | 800,000 | Send 500,000 to Bank B |
| Bank B | USD | 100,000 | Receive 500,000 |
| Bank B intraday credit cap | USD | 400,000 | Excess requires split or next window |

### Correct treatment

| Area | Treatment |
|---|---|
| Liquidity view | Show source bank, target bank, transfer status and concentration window. |
| Intraday availability | Do not count excess transfer above receiving cap until confirmed. |
| Buying power | Use settled and confirmed cash by bank/currency where policy requires. |
| Operations | Split transfer, defer excess or escalate to treasury. |
| Reporting | Distinguish analytical group liquidity from settled operating cash. |

### QA assertions

| Test | Expected result |
|---|---|
| Transfer exceeds receiving bank intraday cap | Excess is split, deferred or escalated. |
| Bank A sends but Bank B does not confirm receipt | In-transit cash is restricted. |
| Concentration is reversed | Reversal preserves original transfer and reason. |
| New trade depends on concentrated cash | Funding is conditional until receiving bank confirmation. |
| Cap changes intraday | Concentration plan recalculates with audit evidence. |

## Example 25. CLS/PVP settlement control for FX

### Scenario

A USD/EUR FX trade is eligible for payment-versus-payment settlement through a settlement utility. The control should reduce principal settlement risk by ensuring both currency legs settle together.

| Attribute | Value |
|---|---|
| Trade | Sell USD / buy EUR |
| USD leg | -1,000,000 |
| EUR leg | +920,000 |
| Settlement model | PVP eligible |
| Settlement status | Matched pending settlement |

### Correct treatment

| State | Platform behavior |
|---|---|
| Matched before cut-off | Reserve sold currency and show bought currency as pending PVP receipt. |
| Unmatched before cut-off | Escalate; do not treat bought currency as available. |
| PVP settled | Post both legs as settled with settlement evidence. |
| PVP fails | Keep both legs restricted and open settlement exception. |
| Bilateral fallback used | Record fallback reason and settlement-risk approval. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade is PVP eligible but unmatched | Bought currency is not available for new orders. |
| One leg appears without PVP confirmation | Exception workflow blocks automatic availability. |
| Bilateral fallback is approved | Audit records approver, reason and residual settlement risk. |
| Settlement confirmation arrives | Both legs update atomically or through controlled paired posting. |
| Client report is produced pre-settlement | Report labels pending PVP settlement and excludes pending receipt from settled cash. |

## Example 26. Treasury cash placement workflow

### Scenario

Treasury places excess client cash into a short-term deposit ladder while preserving liquidity buffers, concentration limits and client service-model constraints.

| Attribute | Value |
|---|---:|
| Excess USD cash | 2,000,000 |
| Minimum operating buffer | 300,000 |
| Maximum single-bank placement | 750,000 |
| Maximum tenor | 30 days |
| Near-term withdrawal forecast | 400,000 |

### Placement plan

```text
cash available for placement = excess cash - operating buffer - withdrawal forecast
cash available for placement = 2,000,000 - 300,000 - 400,000
cash available for placement = 1,300,000
```

| Placement | Amount | Tenor | Control |
|---|---:|---|---|
| Bank A deposit | 650,000 | 14 days | Within single-bank cap. |
| Bank B deposit | 650,000 | 30 days | Within tenor and bank cap. |
| Residual cash | 700,000 | Same day | Covers buffer and forecast. |

### Correct treatment

| Area | Treatment |
|---|---|
| Liquidity | Deposit ladder is not same-day cash; maturity dates drive liquidity view. |
| Concentration | Bank/counterparty exposure must include existing deposits and cash balances. |
| Advisory | Explain yield, lock-up, early break terms, counterparty exposure and liquidity trade-off. |
| DPM | Do not place cash reserved for model trades, fees, collateral calls or client withdrawals. |
| Reporting | Show placement amount, tenor, rate, counterparty, maturity and liquidity bucket. |

### QA assertions

| Test | Expected result |
|---|---|
| Placement breaches single-bank cap | Plan is blocked or resized. |
| Withdrawal forecast increases | Placement amount is reduced before execution. |
| Deposit is broken early | Breakage workflow adjusts interest and liquidity reporting. |
| Counterparty limit changes | Future placements recompute available headroom. |
| Statement is generated | Deposit ladder appears separately from operating cash. |

## Example 27. CLS fallback dispute after failed PVP settlement

### Scenario

A USD/JPY FX trade is expected to settle through payment-versus-payment processing. The trade misses the matching cut-off and treasury approves bilateral fallback, but the counterparty later disputes the JPY amount.

| Attribute | Value |
|---|---:|
| Trade | Sell USD / buy JPY |
| USD sold | 2,000,000 |
| Contract rate | 150.2500 JPY per USD |
| Expected JPY receipt | 300,500,000 |
| Settlement model | CLS/PVP intended, bilateral fallback approved |
| Counterparty disputed amount | 300,350,000 |
| Difference | 150,000 JPY |

### Correct treatment

The fallback approval does not make the bought currency risk-free. The platform should preserve the original PVP intent, fallback reason, residual settlement-risk approval and disputed settlement amount separately.

| Object | Treatment |
|---|---|
| FX trade | Keep original economic terms and value date. |
| Fallback approval | Store approver, reason, timestamp and residual settlement-risk category. |
| USD leg | Post only when external cash confirmation is available. |
| JPY leg | Restrict disputed amount until counterparty confirmation or adjustment. |
| Linked funding | Do not release disputed JPY for securities settlement without approved bridge funding. |
| Reporting | Show bilateral fallback and disputed receivable state where material. |

### QA assertions

| Test | Expected result |
|---|---|
| PVP matching fails before cut-off | Trade moves to escalation state, not normal settlement. |
| Bilateral fallback is approved | Approval evidence and residual settlement-risk reason are retained. |
| Counterparty disputes bought-currency amount | Disputed difference is restricted and aged. |
| Linked JPY purchase needs the disputed amount | Purchase is blocked, bridged or resized according to funding policy. |
| Final settlement differs from contract amount | Adjustment preserves original contract, disputed state and resolution evidence. |

## Example 28. Payment recall after client withdrawal release

### Scenario

A USD payment has been released to a beneficiary, then the client requests a recall because the beneficiary account number was wrong. The cash has left the account, but the receiving bank has not confirmed return of funds.

| Attribute | Value |
|---|---:|
| Payment amount | 125,000 |
| Payment state | Released |
| Recall request time | Same day after release |
| Beneficiary bank response | Pending |
| Expected recall fee | 75 |

### Cash and reporting treatment

| Balance or event | Treatment |
|---|---|
| Original payment | Remains a released cash outflow with bank reference. |
| Recall request | Opens an operational workflow; it is not a cash reversal. |
| Expected returned funds | Projected only, not available cash. |
| Recall fee | Accrue or disclose when fee source confirms. |
| Client statement | Shows original payment and any later returned funds as distinct events. |

### Correct workflow

1. Preserve original payment instruction and release evidence.
2. Create a recall case linked to payment reference, beneficiary bank and client request.
3. Keep cash unavailable until returned funds are confirmed by the bank.
4. Post returned funds separately from original payment.
5. Retain client communication and fee evidence.

### QA assertions

| Test | Expected result |
|---|---|
| Recall is requested after release | No automatic cash reversal is posted. |
| Receiving bank rejects recall | Original payment remains final and recall case closes as unsuccessful. |
| Funds are returned net of fee | Returned cash and fee are posted as separate, explainable entries. |
| Client tries to reuse expected returned cash | Buying power excludes projected recall proceeds. |
| Report is generated during recall | Report shows released payment and pending recall state, not available cash. |

## Example 29. Cross-bank liquidity stress across multiple client accounts

### Scenario

Several related client accounts hold USD cash across three banks. A same-day liquidity need arises during a market event, but transfer capacity and bank-specific cut-offs constrain available liquidity.

| Bank | Confirmed cash | Same-day transfer cap | Cut-off status | Stress-usable today |
|---|---:|---:|---|---:|
| Bank A | 700,000 | 500,000 | Open | 500,000 |
| Bank B | 450,000 | 250,000 | Cut-off passed | 0 |
| Bank C | 300,000 | 300,000 | Open | 300,000 |
| Total | 1,450,000 | 1,050,000 | Mixed | 800,000 |

### Correct interpretation

The group has USD 1.45 million of confirmed cash, but only USD 800,000 is same-day stress-usable without exceptional processing. Liquidity views must show bank, account, legal-owner and transfer-window constraints.

| Area | Treatment |
|---|---|
| Liquidity analytics | Separate total confirmed cash from same-day transferable cash. |
| Advisory | Explain concentration, transfer windows, bank cut-offs and operational uncertainty. |
| DPM | Funding decisions should not assume all group cash can be moved across legal owners. |
| Operations | Create transfer plan with source bank, destination bank, amount, cut-off and confirmation status. |
| Reporting | Label stress liquidity by availability horizon and transfer constraint. |

### QA assertions

| Test | Expected result |
|---|---|
| Bank cut-off has passed | Same-day stress-usable amount excludes that bank unless override is approved. |
| Transfer cap is lower than cash balance | Liquidity view uses cap-limited transfer amount. |
| Accounts have different legal owners | Group view is analytical unless authority and transfer workflow exist. |
| Destination bank delays confirmation | In-transit cash remains restricted. |
| Multiple clients need liquidity at once | Stress workflow checks operational capacity and bank concentration. |

## Example 30. Treasury counterparty downgrade during placement cycle

### Scenario

Treasury has approved a short-term deposit placement, but the counterparty bank is downgraded before execution. The client still wants yield pickup, but counterparty and concentration limits must be recomputed.

| Attribute | Value |
|---|---:|
| Planned placement | 600,000 |
| Original bank rating | A |
| New bank rating | BBB |
| Rating-based single-bank limit before downgrade | 750,000 |
| Rating-based single-bank limit after downgrade | 250,000 |
| Existing exposure to bank | 150,000 |

### Revised headroom

```text
revised bank headroom = new single-bank limit - existing exposure
revised bank headroom = 250,000 - 150,000
revised bank headroom = 100,000

excess planned placement = planned placement - revised bank headroom
excess planned placement = 600,000 - 100,000
excess planned placement = 500,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Placement workflow | Block, resize or reroute the placement based on the new rating limit. |
| Counterparty exposure | Include deposits, cash balances, repo exposure and unsettled placements where policy requires. |
| Advisory | Explain that yield may be reduced because counterparty quality changed. |
| Reporting | Show placement status as blocked or amended due to counterparty limit. |
| Audit | Preserve old rating, new rating, source timestamp, limit recalculation and approval decision. |

### QA assertions

| Test | Expected result |
|---|---|
| Rating downgrade arrives before execution | Placement eligibility is recomputed before release. |
| Existing exposure consumes most headroom | Placement is resized to available counterparty headroom. |
| Advisor requests override | Override requires policy-approved reason and evidence. |
| Rating feed is stale | Placement enters source-limited state. |
| Placement had already executed | Exposure breach workflow opens if new limit is breached post-trade. |

## Example 31. Same-day liquidity allocation across client groups

### Scenario

A treasury desk has limited same-day settlement capacity during a market disruption. Multiple client groups request urgent cash movement, and allocation must respect contractual priority, legal ownership, service model and risk.

| Client group | Requested same-day liquidity | Priority basis | Approved same-day allocation |
|---|---:|---|---:|
| Group A | 400,000 | Contractual withdrawal already approved | 400,000 |
| Group B | 350,000 | Margin-call cure deadline | 300,000 |
| Group C | 250,000 | Non-urgent rebalance funding | 0 |
| Group D | 150,000 | Estate distribution with legal deadline | 150,000 |
| Available same-day capacity | 850,000 | Operational cap | 850,000 |

### Allocation logic

| Rule | Treatment |
|---|---|
| Legal authority | Allocate only within legal owner and account authority. |
| Contractual obligation | Approved client withdrawals and margin deadlines outrank discretionary rebalances. |
| Mandate and suitability | Do not create leverage, overdraft or asset sale without allowed mandate. |
| Communication | Communicate partial allocation, revised timing and reason code. |
| Evidence | Keep allocation decision, approvers, timestamps and rejected alternatives. |

### QA assertions

| Test | Expected result |
|---|---|
| Same-day capacity is insufficient | Allocation follows documented priority rules. |
| Lower-priority rebalance competes with client withdrawal | Rebalance funding is deferred. |
| Group cash cannot legally support another group | Allocation engine blocks cross-owner use. |
| Allocation is partial | Workflow records remaining shortfall and next action. |
| Client report or advisor view is generated | Shows available, allocated, deferred and restricted liquidity separately. |

## Example 32. Payment sanctions false-positive remediation

### Scenario

A client payment is held because name screening produces a potential sanctions match. Compliance later clears it as a false positive. Cash must remain reserved during review and release only after clearance evidence is recorded.

| Attribute | Value |
|---|---:|
| Payment amount | 90,000 |
| Currency | GBP |
| Screening result | Potential match |
| Review outcome | False positive cleared |
| Original value date | Today |
| Revised value date after clearance | Next business day |

### Correct treatment

| Workflow area | Treatment |
|---|---|
| Cash availability | Reserve payment amount while screening is open. |
| Screening state | Keep potential match, reviewer, decision, timestamp and rationale. |
| Payment release | Release only after clearance and payment cut-off/value-date recalculation. |
| Client communication | Provide operational status without exposing restricted screening details. |
| Reporting | Show reserved payment cash and revised value date where appropriate. |

### QA assertions

| Test | Expected result |
|---|---|
| Potential match is raised | Payment is held and cash is reserved. |
| Compliance clears false positive | Payment may proceed only after clearance evidence is recorded. |
| Cut-off passes during review | Value date recalculates to the next eligible date. |
| Client asks why payment is delayed | Client-facing reason is operationally safe and does not leak restricted screening details. |
| Payment is cancelled during review | Reservation is released with cancellation and screening lineage retained. |

## Example 33. Deposit ladder repricing during rate shock

### Scenario

A client has a short-term deposit ladder. Market rates rise sharply, and the advisor wants to compare holding existing deposits to maturity versus breaking and reinvesting.

| Deposit | Principal | Existing rate | Days to maturity | Break penalty | New available rate |
|---|---:|---:|---:|---:|---:|
| A | 250,000 | 3.20% | 30 | 400 | 4.80% |
| B | 250,000 | 3.35% | 60 | 700 | 4.85% |
| C | 250,000 | 3.50% | 90 | 950 | 4.90% |

### Simplified comparison

```text
incremental_interest_a = 250,000 x (4.80% - 3.20%) x 30 / 360 = 333.33
net_break_benefit_a = 333.33 - 400 = -66.67
```

The same comparison should be run per maturity bucket, with actual bank breakage terms, day-count convention and reinvestment tenor.

### Correct treatment

| Area | Treatment |
|---|---|
| Advisory | Explain yield pickup, break penalty, reinvestment tenor and liquidity need. |
| DPM | Reinvestment must respect mandate, cash buffer and concentration limits. |
| Reporting | Show existing ladder, projected maturity cash and any breakage cost separately. |
| Operations | Break and reinvest only after bank quote, client/mandate approval and cut-off validation. |
| Audit | Preserve old deposit terms, new quote, penalty, approval and execution state. |

### QA assertions

| Test | Expected result |
|---|---|
| New rate quote is stale | Break/reinvest recommendation is blocked or labelled indicative. |
| Penalty exceeds incremental interest | Net benefit is negative and clearly shown. |
| Deposit is pledged collateral | Break workflow requires collateral release or substitution. |
| Reinvestment concentration breaches limit | Placement is resized or rerouted. |

## Example 34. Multi-currency payment recall with FX reversal risk

### Scenario

A client sends EUR 200,000 funded by a same-day USD/EUR FX conversion. The payment is recalled after release, but the FX conversion has already settled.

| Attribute | Value |
|---|---:|
| EUR payment | 200,000 |
| USD/EUR conversion rate | 0.92 |
| USD sold | 217,391.30 |
| Returned EUR after bank charges | 199,850 |
| Recall fee | 75 EUR |

### Cash treatment

```text
returned_eur_net = 199,850
unreturned_eur_difference = 200,000 - 199,850 = 150
```

The returned EUR does not automatically restore the original USD balance. A new FX trade may be required if the client wants USD back, creating FX risk and potentially a realized gain or loss.

### QA assertions

| Test | Expected result |
|---|---|
| Payment recall succeeds after FX settlement | Returned EUR posts separately; original USD sale is not reversed silently. |
| Returned amount is net of charges | Bank charges and residual difference are visible. |
| Client requests USD restoration | New FX quote/order is required. |
| FX reversal rate differs | Realized FX impact is calculated and reported separately. |

## Example 35. Client cash segmentation by booking centre

### Scenario

A client group has cash in two booking centres. The relationship view shows total cash, but regulatory, tax, booking and operational rules restrict movement between centres.

| Booking centre | Currency | Ledger cash | Transfer allowed today | Reason |
|---|---|---:|---:|---|
| Singapore | USD | 600,000 | 600,000 | Same legal owner and open cut-off |
| Switzerland | USD | 450,000 | 0 | Cross-border documentation pending |
| Total | USD | 1,050,000 | 600,000 | Mixed availability |

### Correct interpretation

Total group cash is USD 1,050,000, but same-day usable cash for a Singapore-booked trade is USD 600,000 unless cross-border transfer documentation and approvals are complete.

| Area | Treatment |
|---|---|
| Client 360 | Show total cash, booking-centre cash and transfer-restricted cash separately. |
| Advisory | Do not imply cross-centre cash is freely available. |
| DPM | Funding engine must use mandate, booking centre and legal-owner constraints. |
| Reporting | Consolidated reports may show group cash but should label restrictions when material. |
| Operations | Cross-centre transfer requires authority, documentation, cut-off and FX checks. |

### QA assertions

| Test | Expected result |
|---|---|
| Cross-border documentation is pending | Transferable cash excludes restricted booking centre. |
| Group report is generated | Total and restricted cash are both explainable. |
| Trade is booked in Singapore | Funding uses Singapore-available cash unless transfer is approved. |
| Restriction clears | Transferable amount updates from effective time with audit lineage. |

## Example 36. Intraday overdraft escalation

### Scenario

An account has positive projected end-of-day cash but an intraday settlement debit arrives before incoming funds. The account needs temporary overdraft approval.

| Attribute | Value |
|---|---:|
| Opening settled cash | 80,000 |
| Securities settlement debit at 10:00 | 250,000 |
| Incoming FX settlement at 15:00 | 220,000 |
| Minimum intraday buffer | 25,000 |

### Intraday gap

```text
intraday_required_cash = 250,000 + 25,000 = 275,000
intraday_shortfall_before_inflow = 275,000 - 80,000 = 195,000
projected_end_of_day_cash = 80,000 - 250,000 + 220,000 = 50,000
```

The positive projected end-of-day balance does not eliminate the 10:00 funding shortfall.

### QA assertions

| Test | Expected result |
|---|---|
| Settlement debit arrives before incoming funds | Intraday shortfall is raised. |
| End-of-day projection is positive | Intraday overdraft workflow still requires approval or funding action. |
| Overdraft limit is insufficient | Settlement is blocked, reprioritized or escalated. |
| Incoming FX settlement fails | Overdraft exposure remains open and aged. |

## Example 37. Treasury sweep exception reporting

### Scenario

An automated sweep should move excess cash into an approved money market sweep vehicle, but exceptions prevent full sweep execution.

| Attribute | Value |
|---|---:|
| Eligible excess cash | 900,000 |
| Minimum operating cash buffer | 100,000 |
| Sweep target amount | 800,000 |
| Amount successfully swept | 550,000 |
| Exception amount | 250,000 |

### Exception classification

| Exception reason | Amount | Treatment |
|---|---:|---|
| Cut-off missed | 150,000 | Retry next eligible dealing window. |
| Product eligibility missing | 75,000 | Hold as cash until eligibility is resolved. |
| Screening hold | 25,000 | Keep reserved/restricted until cleared. |

### QA assertions

| Test | Expected result |
|---|---|
| Sweep partially executes | Swept amount, unswept cash and exception reasons are all visible. |
| Cut-off missed | Retry date and expected value date are calculated. |
| Eligibility is missing | Cash is not swept into an unsupported product. |
| Screening hold exists | Restricted cash remains excluded from sweepable amount. |
| Advisor view is generated | Shows cash drag reason instead of treating unswept cash as unexplained idle cash. |

## Example 38. Trapped cash after market disruption

### Scenario

A client has cash at a correspondent bank in a market where payment rails are temporarily unavailable. The balance is confirmed by bank statement, but it cannot be transferred, swept or used for same-day settlement.

| Attribute | Value |
|---|---:|
| Confirmed ledger cash | 420,000 |
| Currency | USD |
| Market | Restricted settlement market |
| Transfer rail status | Suspended |
| Same-day usable amount | 0 |
| Expected review date | 2026-07-03 |

### Liquidity treatment

```text
reported_cash = 420,000
same_day_usable_cash = 0
trapped_cash = 420,000 - 0 = 420,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Valuation | Keep the confirmed cash balance as cash if the bank statement supports it. |
| Liquidity | Exclude trapped cash from available cash, buying power and settlement funding. |
| Advisory | Explain that the balance exists but is operationally unavailable. |
| DPM | Do not fund rebalances, withdrawals or margin cures from trapped cash. |
| Reporting | Show restricted or trapped cash when material to liquidity explanation. |
| Operations | Track rail status, correspondent bank update, expected review date and escalation owner. |

### QA assertions

| Test | Expected result |
|---|---|
| Bank balance is confirmed but payment rails are suspended | Cash is reported but excluded from same-day available cash. |
| Advisor proposes a funded withdrawal using trapped cash | Workflow blocks or escalates the instruction. |
| Transfer rail reopens | Usable amount updates only from effective timestamp with source evidence. |
| Client report is produced | Report separates total cash from restricted/trapped cash. |

## Example 39. Blocked-market currency conversion control

### Scenario

A client holds an onshore currency that cannot be freely converted offshore. The client relationship view shows the local-currency cash equivalent in reporting currency, but conversion and repatriation require market-specific approval.

| Attribute | Value |
|---|---:|
| Onshore cash | 2,000,000 |
| Onshore FX rate to USD | 0.1380 |
| Reporting equivalent | 276,000 USD |
| Approved convertible amount today | 500,000 local currency |
| Approval expiry | End of day |

### Convertible value

```text
approved_convertible_usd = 500,000 x 0.1380 = 69,000
blocked_equivalent_usd = (2,000,000 - 500,000) x 0.1380 = 207,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Reporting | Show translated value, but label conversion and repatriation limits. |
| FX order entry | Limit conversion order to approved amount and valid approval window. |
| Liquidity | Treat only approved convertible amount as near-term usable offshore liquidity. |
| Performance | Translate local-currency cash for performance, but do not imply transferability. |
| Operations | Preserve approval id, source, expiry, rate source and failed-attempt lineage. |

### QA assertions

| Test | Expected result |
|---|---|
| Client has blocked-market cash | Reporting equivalent is not treated as freely transferable USD. |
| FX order exceeds approved amount | Order is blocked or resized. |
| Approval expires before execution | Conversion returns to blocked state. |
| Rate updates but approval does not | Reporting equivalent updates; approved notional remains governed by approval evidence. |

## Example 40. Pooled-client cash interest allocation

### Scenario

A booking centre receives interest on a pooled client cash account. The bank credits interest at pool level, but allocation to clients must follow the documented average-balance method after excluding restricted or non-interest-bearing balances.

| Client | Average eligible balance | Restricted balance | Allocation weight |
|---|---:|---:|---:|
| A | 300,000 | 50,000 | 30.00% |
| B | 500,000 | 0 | 50.00% |
| C | 200,000 | 100,000 | 20.00% |
| Total eligible | 1,000,000 | 150,000 | 100.00% |

Pool interest credited is 4,200.

### Allocation

```text
client_a_interest = 4,200 x 300,000 / 1,000,000 = 1,260
client_b_interest = 4,200 x 500,000 / 1,000,000 = 2,100
client_c_interest = 4,200 x 200,000 / 1,000,000 = 840
```

### Correct treatment

| Area | Treatment |
|---|---|
| Allocation basis | Use documented eligible average balance, not closing balance. |
| Restrictions | Exclude restricted or non-interest-bearing balances where policy requires. |
| Ledger | Post client-level interest with pool-level reconciliation reference. |
| Reporting | Show interest income separately from balance movement. |
| QA | Reconcile allocated interest back to pool credit within rounding tolerance. |

### QA assertions

| Test | Expected result |
|---|---|
| Pool interest is received | Client allocations sum to pool credit within tolerance. |
| Restricted balance is present | Restricted amount is excluded when policy marks it non-interest-bearing. |
| Average-balance source is missing | Allocation enters exception state. |
| Rounding creates residual | Residual is posted according to documented rounding policy. |

## Example 41. Cash compensation after operational incident

### Scenario

A payment was delayed because of an internal processing error. The client should be compensated for lost interest based on the delayed amount, delay period and approved compensation rate.

| Attribute | Value |
|---|---:|
| Delayed payment amount | 180,000 |
| Delay | 4 days |
| Compensation rate | 3.60% |
| Day-count basis | 360 |

### Compensation

```text
cash_compensation = 180,000 x 3.60% x 4 / 360 = 72.00
```

### Correct treatment

| Area | Treatment |
|---|---|
| Root cause | Link compensation to the operational incident and approval. |
| Calculation | Use approved rate, date range, day-count basis and affected notional. |
| Ledger | Post compensation as operational compensation, not investment income. |
| Reporting | Label payment clearly so it is not confused with deposit interest or market return. |
| Controls | Require approval and prevent duplicate compensation for the same incident. |

### QA assertions

| Test | Expected result |
|---|---|
| Incident is approved for compensation | Compensation amount is calculated and posted with incident reference. |
| Date range changes | Compensation recalculates from authoritative delay dates. |
| Payment was delayed by correspondent bank, not internal error | Compensation requires policy decision and source classification. |
| Duplicate claim arrives | Duplicate detection blocks second payment unless approved as adjustment. |

## Example 42. Payment fraud recovery and provisional credit

### Scenario

A fraudulent payment leaves the client account. The bank grants a provisional credit while recovery investigation is open. Later, part of the funds are recovered and the final loss is allocated according to policy.

| Attribute | Value |
|---|---:|
| Fraudulent payment | 250,000 |
| Provisional credit | 250,000 |
| Recovered amount | 180,000 |
| Final unrecovered amount | 70,000 |

### Recovery state

```text
provisional_credit = 250,000
recovered_amount = 180,000
unrecovered_amount = 250,000 - 180,000 = 70,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Availability | Provisional credit may be restricted until final decision. |
| Ledger | Track original fraud debit, provisional credit, recovery receipt and final adjustment separately. |
| Reporting | Avoid presenting provisional credit as final recovered cash. |
| Operations | Preserve fraud case id, investigation state, recoveries, approval and client communication. |
| Risk | Escalate repeated events, compromised instruction channel or control failure. |

### QA assertions

| Test | Expected result |
|---|---|
| Provisional credit is posted | Balance is restored but marked provisional or restricted if policy requires. |
| Partial recovery arrives | Recovery receipt reduces open exposure without duplicating client credit. |
| Final decision allocates unrecovered loss | Final adjustment closes provisional state with evidence. |
| Client report is generated during investigation | Report labels provisional state and avoids overstating final recovery. |

## Example 43. Central-bank holiday emergency funding

### Scenario

An unexpected central-bank holiday closes domestic payment rails. A client has a securities settlement obligation due today, but domestic cash cannot move. Treasury considers an emergency bridge from an approved offshore liquidity source.

| Attribute | Value |
|---|---:|
| Settlement obligation | 650,000 |
| Domestic cash balance | 700,000 |
| Domestic payment rail | Closed |
| Approved offshore emergency line | 500,000 |
| Required emergency top-up | 150,000 |

### Funding gap

```text
same_day_domestic_usable_cash = 0
available_emergency_line = 500,000
remaining_shortfall = 650,000 - 500,000 = 150,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Liquidity | Domestic balance remains reported but unavailable for same-day settlement. |
| Funding | Emergency line can fund only within approval, currency, legal-owner and facility terms. |
| Settlement | Any remaining shortfall must be escalated to settlement, custody and advisor teams. |
| Client communication | Explain operational market closure and revised funding path. |
| Evidence | Preserve holiday source, emergency approval, facility drawdown, residual shortfall and resolution. |

### QA assertions

| Test | Expected result |
|---|---|
| Central-bank holiday closes rails | Domestic cash is excluded from same-day settlement funding. |
| Emergency line is smaller than obligation | Remaining shortfall is visible and escalated. |
| Emergency line is not approved for client/legal owner | Funding cannot be used. |
| Holiday is lifted | Cash availability updates from effective rail reopening time with source evidence. |

## Example 44. Client cash haircut policy for liquidity and buying power

### Scenario

A client has cash across currencies and operational states. The relationship summary shows total cash, but order funding and liquidity analytics must apply policy haircuts for currency convertibility, transfer timing and restrictions.

| Cash bucket | Reporting currency equivalent | Policy haircut | Usable amount |
|---|---:|---:|---:|
| USD settled cash | 600,000 | 0.00% | 600,000 |
| HKD same-day convertible cash | 300,000 | 5.00% | 285,000 |
| Restricted event proceeds | 100,000 | 100.00% | 0 |
| Total | 1,000,000 | | 885,000 |

### Haircut treatment

```text
usable_cash = 600,000 x (1 - 0%)
            + 300,000 x (1 - 5%)
            + 100,000 x (1 - 100%)
            = 885,000

liquidity_haircut = 1,000,000 - 885,000 = 115,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Client reporting | Show total cash separately from policy-usable cash when the difference is material. |
| Buying power | Use haircut-adjusted cash for pre-trade funding, not headline cash. |
| Advisory | Explain whether haircut comes from FX, settlement, documentation, restriction or policy buffer. |
| DPM | Apply mandate liquidity tests to the approved liquidity definition. |
| Operations | Preserve haircut policy version, effective date, currency, booking centre and restriction reason. |

### QA assertions

| Test | Expected result |
|---|---|
| Restricted proceeds are present | Restricted cash remains valued but contributes zero usable buying power. |
| Currency haircut changes | Usable cash recalculates from policy effective date. |
| Advisor uses total cash for order funding | Pre-trade check blocks or warns when usable cash is insufficient. |
| Client report is generated | Report labels cash, usable liquidity and restrictions without merging them into one number. |

## Example 45. Liquidity transfer pricing for treasury cash usage

### Scenario

Treasury uses client overnight cash as part of an approved liquidity placement process. Client crediting, internal liquidity transfer pricing and treasury spread must be separated so performance and client reporting do not confuse internal economics with client yield.

| Attribute | Value |
|---|---:|
| Average eligible balance | 2,500,000 |
| Client credited rate | 3.60% |
| Internal liquidity transfer price | 4.10% |
| Placement yield | 4.35% |
| Days | 30 |
| Day-count basis | 360 |

### Economics

```text
client_interest = 2,500,000 x 3.60% x 30 / 360 = 7,500.00
internal_transfer_value = 2,500,000 x 4.10% x 30 / 360 = 8,541.67
treasury_placement_income = 2,500,000 x 4.35% x 30 / 360 = 9,062.50
treasury_spread = 9,062.50 - 8,541.67 = 520.83
```

### Correct treatment

| Area | Treatment |
|---|---|
| Client accounting | Post client credited interest according to account terms and disclosure. |
| Internal management | Track liquidity transfer price and placement spread as treasury economics. |
| Reporting | Do not show internal transfer price as client yield. |
| Controls | Require approved eligible-balance policy and treasury placement authority. |
| QA | Reconcile client credited interest, internal transfer value and placement income separately. |

### QA assertions

| Test | Expected result |
|---|---|
| Client statement is generated | Statement shows client credited interest only. |
| Treasury profitability report is generated | Report includes transfer price and spread without changing client interest. |
| Eligible-balance policy is missing | Transfer-pricing calculation is exceptioned. |
| Rate changes mid-period | Interest and transfer value split by effective dates. |

## Example 46. Multi-entity treasury sweep without cross-subsidy

### Scenario

A family office has multiple legal entities. Treasury wants to sweep surplus cash into an overnight instrument, but cash cannot be pooled across entities unless legal authority and product eligibility are present.

| Entity | Cash | Minimum reserve | Product eligible | Cross-entity authority | Sweepable amount |
|---|---:|---:|---|---|---:|
| Holding company | 850,000 | 250,000 | Yes | Yes | 600,000 |
| Trust account | 400,000 | 300,000 | Yes | No | 100,000 |
| Foundation | 220,000 | 250,000 | Yes | No | 0 |
| Operating company | 500,000 | 150,000 | No | Yes | 0 |

### Sweep calculation

```text
entity_sweepable = max(0, cash - minimum_reserve) where product_eligible = true
total_sweepable = 600,000 + 100,000 + 0 + 0 = 700,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Legal ownership | Calculate sweepability by legal entity before any family-office consolidation. |
| Product eligibility | Exclude entities not eligible for the sweep product even when cash is available. |
| Cross-subsidy | Do not fund one entity reserve shortfall from another entity without explicit authority. |
| Reporting | Show consolidated cash and entity-level sweep decisions separately. |
| Controls | Preserve authority evidence, reserve policy, product eligibility and approval state. |

### QA assertions

| Test | Expected result |
|---|---|
| Entity has cash but no product eligibility | Sweepable amount is zero. |
| Entity has reserve shortfall | Other entities are not automatically used to cover it. |
| Family-office report is generated | Consolidated view reconciles to entity-level sweep decisions. |
| Cross-entity authority changes | Sweep logic updates only from effective-dated authority evidence. |

## Example 47. Intraday payment prioritization under limited liquidity

### Scenario

A client has several same-day payment instructions and limited intraday liquidity. The platform must prioritize obligations according to policy instead of processing instructions in arrival order.

| Payment | Amount | Priority | Cut-off | Status |
|---|---:|---:|---|---|
| Securities settlement funding | 800,000 | 1 | 11:00 | Ready |
| Tax authority payment | 250,000 | 2 | 13:00 | Ready |
| Property deposit | 150,000 | 3 | 15:00 | Ready |
| Discretionary investment transfer | 300,000 | 4 | 16:00 | Ready |

Opening same-day usable cash is 1,100,000 and policy buffer is 100,000.

### Prioritization

```text
payable_capacity = 1,100,000 - 100,000 = 1,000,000
priority_1_paid = 800,000
remaining_capacity = 200,000
priority_2_partial_shortfall = 250,000 - 200,000 = 50,000
lower_priority_blocked = 150,000 + 300,000 = 450,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Payment execution | Fund highest-priority obligations first within cut-off and approval constraints. |
| Partial funding | Do not partially pay an instruction unless rail and business policy permit it. |
| Advisor workflow | Surface blocked or delayed lower-priority payments before client commitment. |
| Reporting | Explain failed or deferred payments by capacity, priority and cut-off reason. |
| Operations | Preserve queue order, priority policy, cut-off clocks and manual override approvals. |

### QA assertions

| Test | Expected result |
|---|---|
| Capacity is insufficient | Lower-priority payments are blocked or queued, not silently released. |
| Partial payment is not allowed | Tax payment remains pending with shortfall instead of partial execution. |
| Manual override is entered | Override requires authority, reason and audit evidence. |
| New intraday inflow arrives | Queue recalculates from effective availability timestamp. |

## Example 48. Client money segregation breach detection

### Scenario

A pooled client-money account must hold segregated cash equal to eligible client balances. Reconciliation identifies a shortfall caused by a late internal transfer.

| Attribute | Value |
|---|---:|
| Required segregated client money | 4,250,000 |
| External segregated account balance | 4,180,000 |
| Shortfall | 70,000 |
| Root cause | Late internal transfer |
| Remediation transfer approved | 70,000 |

### Breach amount

```text
segregation_shortfall = required_client_money - external_segregated_balance
segregation_shortfall = 4,250,000 - 4,180,000 = 70,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Control state | Mark as breach or potential breach according to policy and jurisdiction. |
| Availability | Do not treat the missing 70,000 as freely available house cash. |
| Remediation | Top up segregated account and preserve approval, timestamp and source account. |
| Reporting | Produce control evidence for compliance, operations and management review. |
| Root cause | Track late transfer cause and corrective action to prevent recurrence. |

### QA assertions

| Test | Expected result |
|---|---|
| Segregated account is short | Breach amount is calculated and escalated. |
| Top-up transfer is approved | Remediation closes shortfall from effective settlement timestamp. |
| Required balance source changes | Control recalculates from authoritative client-money balance source. |
| Report is generated | Report shows breach amount, duration, remediation and root cause. |

## Example 49. Cross-border payment repair analytics

### Scenario

Operations reviews a monthly population of cross-border payments to identify repair drivers. The goal is to reduce repeat defects in payment instructions and improve first-pass release.

| Repair reason | Count | Average delay |
|---|---:|---:|
| Missing beneficiary address | 8 | 1.5 days |
| Invalid BIC or clearing code | 5 | 1.0 days |
| Missing purpose code | 3 | 2.0 days |
| Sanctions review false positive | 2 | 0.5 days |
| Total repaired payments | 18 | |

Total cross-border payments for the month are 120.

### Repair rate

```text
payment_repair_rate = repaired_payments / total_payments
payment_repair_rate = 18 / 120 = 15.00%

beneficiary_address_share = 8 / 18 = 44.44%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Operations | Track repair reason, owner, elapsed time, client impact and final release state. |
| Client experience | Prioritize recurring defects that delay urgent client payments. |
| Data quality | Feed repeated missing fields into standing-instruction and onboarding remediation. |
| Controls | Separate sanctions review from formatting repair and payment-data defects. |
| Reporting | Show repair trend, top defect drivers and aging, not only open repair count. |

### QA assertions

| Test | Expected result |
|---|---|
| Monthly repair dashboard is generated | Repair rate, reason mix and aging are visible. |
| Payment has multiple repair reasons | Primary and contributing reasons are captured without double counting total repaired payments. |
| Standing instruction is corrected | Future payments use updated instruction and repair rate should improve. |
| Sanctions false positive occurs | Case is classified separately from data-quality repair. |

## Example 50. Money-market fund liquidity fee activation

### Scenario

A money-market fund applies a liquidity fee after same-day redemption pressure exceeds the fund's configured threshold. The platform must calculate expected proceeds from confirmed fund terms, preserve the fee as a separate economic component and avoid presenting gross redemption value as available liquidity.

| Attribute | Value |
|---|---:|
| Redeemed units | 1,000,000 |
| NAV per unit | 1.0000 |
| Liquidity fee rate | 0.75% |
| Gross redemption value | 1,000,000.00 |

### Proceeds calculation

```text
liquidity_fee = 1,000,000.00 x 0.75% = 7,500.00
net_redemption_proceeds = 1,000,000.00 - 7,500.00 = 992,500.00
```

### Correct treatment

| Area | Treatment |
|---|---|
| Position lifecycle | Reduce money-market fund units from the confirmed redemption date and settlement state. |
| Cash projection | Project net proceeds only; do not treat the fee amount as available cash. |
| Performance | Classify the fee separately from market performance and ordinary income. |
| Client reporting | Label the fee as liquidity fee or redemption fee according to fund documentation. |
| Operations | Preserve fund notice, fee activation time, threshold rule, NAV, settlement date and transfer-agent confirmation. |

### QA assertions

| Test | Expected result |
|---|---|
| Liquidity fee is active | Net proceeds reflect fee deduction. |
| Fee activation notice is missing | Redemption remains source-limited or exceptioned. |
| Client liquidity view is generated | Available liquidity uses net proceeds, not gross redemption value. |
| Fund later reverses the fee | Cash projection and fee transaction are reversed with audit lineage. |

## Example 51. Repo collateral substitution failure

### Scenario

A client-funded repo position allows collateral substitution, but the proposed replacement securities fail eligibility because the haircut-adjusted value is insufficient. Treasury and collateral operations must keep the existing collateral in place until substitution is approved and settled.

| Attribute | Value |
|---|---:|
| Repo cash amount | 2,000,000 |
| Required collateral coverage | 102.00% |
| Required collateral value | 2,040,000 |
| Replacement market value | 2,100,000 |
| Replacement haircut | 5.00% |

### Eligibility calculation

```text
haircut_adjusted_replacement_value = 2,100,000 x (1 - 5.00%) = 1,995,000
collateral_shortfall = 2,040,000 - 1,995,000 = 45,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Repo lifecycle | Keep original collateral attached until substitution is approved and settled. |
| Collateral analytics | Show proposed replacement value, haircut-adjusted value and shortfall separately. |
| Availability | Do not release original collateral when replacement collateral is short. |
| Operations | Preserve substitution request, eligibility policy, haircut source, approval state and settlement state. |
| Reporting | Explain failed substitution as collateral eligibility failure, not repo default. |

### QA assertions

| Test | Expected result |
|---|---|
| Replacement collateral is insufficient | Substitution is rejected or held pending top-up. |
| Original collateral release is requested | Release is blocked until replacement settlement is confirmed. |
| Haircut policy changes | Eligibility recalculates from effective-dated haircut policy. |
| Client report is generated | Report distinguishes existing collateral, proposed replacement and failed substitution. |

## Example 52. Intraday nostro forecast variance

### Scenario

Treasury forecasts intraday cash availability using expected nostro inflows and outflows. A delayed correspondent-bank credit creates a forecast variance that affects same-day payment release capacity.

| Item | Forecast | Actual |
|---|---:|---:|
| Opening nostro balance | 1,250,000 | 1,250,000 |
| Expected incoming credits | 900,000 | 600,000 |
| Expected outgoing payments | 1,500,000 | 1,500,000 |
| Operating buffer | 150,000 | 150,000 |

### Capacity impact

```text
forecast_release_capacity = 1,250,000 + 900,000 - 150,000 = 2,000,000
actual_release_capacity = 1,250,000 + 600,000 - 150,000 = 1,700,000
capacity_variance = 1,700,000 - 2,000,000 = -300,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Payment release | Recalculate release capacity when expected credits miss the cut-off. |
| Forecasting | Separate forecast variance from realized nostro balance and unsettled items. |
| Operations | Track correspondent bank, value date, cut-off, delayed credit reason and expected resolution. |
| Risk | Escalate material negative variance before releasing lower-priority payments. |
| Reporting | Show forecast, actual and variance rather than only the ending balance. |

### QA assertions

| Test | Expected result |
|---|---|
| Expected inflow is delayed | Same-day release capacity decreases by delayed amount. |
| Lower-priority payment is queued | Queue explains capacity variance and cut-off impact. |
| Credit arrives after cut-off | Payment capacity updates from actual receipt timestamp. |
| Treasury dashboard is generated | Forecast, actual, variance and reason code are visible. |

## Example 53. Correspondent-bank fee dispute

### Scenario

A cross-border payment settles, but the correspondent bank deducts a fee that exceeds the agreed schedule. The platform must separate client cash movement, external bank fee, disputed amount and potential reimbursement.

| Attribute | Value |
|---|---:|
| Payment principal | 250,000 |
| Contracted correspondent fee | 35 |
| Deducted correspondent fee | 95 |
| Excess fee under dispute | 60 |

### Dispute amount

```text
excess_fee = deducted_fee - contracted_fee
excess_fee = 95 - 35 = 60
```

### Correct treatment

| Area | Treatment |
|---|---|
| Cash accounting | Post actual cash settlement and fee deduction from bank confirmation. |
| Fee control | Track contracted fee, deducted fee and disputed excess separately. |
| Client experience | Do not promise reimbursement until dispute outcome is confirmed. |
| Operations | Preserve payment instruction, correspondent fee schedule, SWIFT/bank confirmation, claim id and resolution state. |
| Reporting | Label disputed fee state clearly in operational reports and client-service workflow. |

### QA assertions

| Test | Expected result |
|---|---|
| Deducted fee exceeds schedule | Excess amount is flagged as fee dispute. |
| Dispute is approved for reimbursement | Reimbursement is posted separately from original payment. |
| Fee schedule is missing | Fee validation is exceptioned rather than assumed. |
| Client statement is generated | Actual deducted fee and later reimbursement remain traceable. |

## Example 54. Account-level reserve optimization

### Scenario

An advisor wants to invest excess operating cash, but each account must retain a reserve for expected fees, tax payments and upcoming settlement obligations. Optimization should maximize investable cash without breaching account-level reserves.

| Account | Cash | Required reserve | Upcoming settlement | Investable cash |
|---|---:|---:|---:|---:|
| Account A | 900,000 | 150,000 | 200,000 | 550,000 |
| Account B | 300,000 | 125,000 | 250,000 | 0 |
| Account C | 500,000 | 100,000 | 50,000 | 350,000 |

### Investable amount

```text
account_investable = max(0, cash - required_reserve - upcoming_settlement)
total_investable = 550,000 + 0 + 350,000 = 900,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Portfolio modelling | Calculate reserve by account before any consolidated family or relationship view. |
| Advisory workflow | Surface non-investable cash caused by reserves or settlement obligations. |
| DPM | Preserve reserve policy and mandate-level cash floor before trade generation. |
| Reporting | Show cash, reserve, upcoming obligations and investable cash as distinct figures. |
| Controls | Require effective-dated reserve rules and settlement obligation sources. |

### QA assertions

| Test | Expected result |
|---|---|
| Account has cash but reserve shortfall | Investable cash floors at zero. |
| Settlement obligation changes | Investable amount recalculates from updated obligation. |
| Consolidated view is generated | Consolidated investable cash reconciles to account-level calculation. |
| Advisor attempts full-cash investment | Pre-trade check blocks use of reserved cash. |

## Example 55. FX settlement netting exception

### Scenario

Several same-currency FX settlements are eligible for netting, but one trade is excluded because the counterparty settlement instruction changed after the netting cut-off. Netting must use eligible trades only and keep the excluded trade separately monitored.

| Trade | Direction | Amount | Netting eligible |
|---|---|---:|---|
| FX-1 | Receive USD | 1,200,000 | Yes |
| FX-2 | Pay USD | 850,000 | Yes |
| FX-3 | Pay USD | 200,000 | No |

### Net settlement

```text
eligible_net_usd = 1,200,000 - 850,000 = receive 350,000
excluded_pay_usd = 200,000
gross_liquidity_need_if_excluded_pay_fails = 200,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Settlement | Net only trades that meet counterparty, SSI, cut-off and value-date eligibility. |
| Liquidity | Reserve liquidity for excluded gross-settled trades until settlement confirmation. |
| Operations | Preserve netting set id, eligibility reason, SSI change timestamp, cut-off and settlement status. |
| Risk | Escalate excluded high-value trades that create liquidity or settlement-fail exposure. |
| Reporting | Show netted trades and excluded exceptions separately. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade misses netting cut-off | Trade is excluded from net settlement calculation. |
| SSI changes after cut-off | Exception reason is captured and gross settlement is monitored. |
| Netted settlement completes | Eligible net amount is closed without closing excluded trade. |
| Operations dashboard is generated | Netting set, excluded trade and liquidity reserve are visible. |

## Example 56. FX swap rollover failure

### Scenario

A short-dated FX swap is used to roll a USD funding need against EUR liquidity. The near leg settles, but the far-leg rollover instruction fails because the counterparty rejects the settlement instruction after cut-off. Treasury must distinguish settled cash, open forward exposure, and replacement funding need.

| Attribute | Value |
|---|---:|
| Near-leg USD received | 1,000,000 |
| Far-leg USD payable | 1,003,500 |
| Replacement USD funding quote | 1,006,200 |
| Rollover failure cost | 2,700 |

### Failure cost

```text
rollover_failure_cost = replacement_usd_funding_quote - original_far_leg_usd_payable
rollover_failure_cost = 1,006,200 - 1,003,500 = 2,700
```

### Correct treatment

| Area | Treatment |
|---|---|
| Cash state | Keep near-leg settlement separate from failed far-leg rollover. |
| FX exposure | Keep the open forward/funding exposure visible until replacement trade settles. |
| Treasury workflow | Route rejected instruction to repair, replacement funding, or manual escalation. |
| Reporting | Label cost as rollover failure or operational funding cost, not FX investment return. |
| Evidence | Preserve original swap, far-leg instruction, rejection reason, cut-off and replacement quote. |

### QA assertions

| Test | Expected result |
|---|---|
| Far leg is rejected after cut-off | Rollover failure state opens and replacement funding is required. |
| Replacement trade is booked | Original failure and replacement trade remain linked. |
| Client liquidity view is generated | Available USD reflects actual settlement state, not expected rollover. |
| Failure cost is reported | Cost is labelled separately from market FX P&L. |

## Example 57. Deposit early-break approval dispute

### Scenario

A relationship manager requests an early break of a term deposit to fund a client payment. Treasury approves only if the break penalty and authorization are accepted before the payment cut-off. The client later disputes the approval timestamp.

| Attribute | Value |
|---|---:|
| Deposit principal | 750,000 |
| Accrued interest to date | 6,300 |
| Early-break penalty | 4,800 |
| Net maturity-equivalent proceeds | 751,500 |

### Net break proceeds

```text
net_break_proceeds = principal + accrued_interest - early_break_penalty
net_break_proceeds = 750,000 + 6,300 - 4,800 = 751,500
```

### Correct treatment

| Area | Treatment |
|---|---|
| Authority | Require valid approval source, timestamp and role before breaking the deposit. |
| Cash projection | Project net break proceeds only after approval and product-break workflow state. |
| Client explanation | Show accrued interest, penalty and net proceeds separately. |
| Dispute handling | Preserve approval evidence, quote timestamp, penalty terms and payment cut-off. |
| Reporting | Do not restate the deposit as matured normally when it was broken early. |

### QA assertions

| Test | Expected result |
|---|---|
| Approval is missing | Deposit break is blocked or held in exception state. |
| Approval is after cut-off | Same-day payment funding is not assumed. |
| Penalty quote changes | Net proceeds recalculate from effective quote. |
| Dispute is opened | Original approval evidence remains immutable and linked. |

## Example 58. Virtual-account cash allocation

### Scenario

A pooled operating account receives one bank credit that must be allocated to several virtual accounts. The external bank statement has one cash movement, while internal client reporting needs account-level allocations.

| Virtual account | Expected allocation |
|---|---:|
| VA-001 | 120,000 |
| VA-002 | 85,000 |
| VA-003 | 45,000 |
| Total bank credit | 250,000 |

### Allocation control

```text
allocation_break = bank_credit - sum(virtual_account_allocations)
allocation_break = 250,000 - (120,000 + 85,000 + 45,000) = 0
```

### Correct treatment

| Area | Treatment |
|---|---|
| Bank reconciliation | Reconcile external cash to the pooled physical account. |
| Internal allocation | Allocate to virtual accounts using remittance, account mapping, or approved manual allocation. |
| Reporting | Show each virtual account its allocated cash only. |
| Controls | Block client-ready reporting when allocation does not reconcile to bank credit. |
| Evidence | Preserve remittance advice, allocation rule, manual override and approval lineage. |

### QA assertions

| Test | Expected result |
|---|---|
| Allocation sums to bank credit | Physical and virtual-account ledgers reconcile. |
| Allocation is incomplete | Unallocated residual remains in suspense or exception state. |
| Mapping is stale | Allocation is blocked until mapping is refreshed or approved. |
| Consolidated report is generated | No double counting between pooled account and virtual accounts. |

## Example 59. Payment rail outage rerouting

### Scenario

A priority payment cannot be sent through the preferred rail because the rail is unavailable before cut-off. Operations must reroute through an alternate rail with different fee, cut-off and settlement timing.

| Attribute | Preferred rail | Alternate rail |
|---|---:|---:|
| Payment amount | 500,000 | 500,000 |
| Fee | 15 | 45 |
| Cut-off status | Closed | Open |
| Expected settlement | Same day | Next day |

### Incremental cost

```text
incremental_reroute_fee = alternate_rail_fee - preferred_rail_fee
incremental_reroute_fee = 45 - 15 = 30
```

### Correct treatment

| Area | Treatment |
|---|---|
| Payment state | Preserve original rail failure and new routed instruction. |
| Liquidity | Keep cash reserved until alternate rail settlement is confirmed. |
| Client impact | Surface changed fee, settlement expectation and any approval requirement. |
| Operations | Track rail outage id, reroute decision, cut-off and repair evidence. |
| Reporting | Distinguish reroute cost from ordinary payment fee when material. |

### QA assertions

| Test | Expected result |
|---|---|
| Preferred rail is closed | Payment cannot be released through unavailable rail. |
| Alternate rail is approved | New instruction is generated with its own fee and settlement state. |
| Same-day settlement is no longer possible | Client/payment workflow shows revised value date. |
| Outage report is generated | Original failure, reroute and final settlement are linked. |

## Example 60. Treasury concentration-limit breach

### Scenario

Treasury places cash with multiple banks. A planned deposit would breach the approved concentration limit for one bank after a rating downgrade reduces its limit.

| Attribute | Value |
|---|---:|
| Revised bank limit | 5,000,000 |
| Existing exposure | 4,650,000 |
| Planned placement | 750,000 |
| Excess after placement | 400,000 |

### Limit breach

```text
post_placement_exposure = existing_exposure + planned_placement
post_placement_exposure = 4,650,000 + 750,000 = 5,400,000
excess = 5,400,000 - 5,000,000 = 400,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Limit check | Evaluate concentration against effective-dated treasury limit before placement. |
| Routing | Resize, split, reroute or escalate the placement when post-trade exposure exceeds limit. |
| Risk | Preserve rating source, limit version, approval and exception reason. |
| Reporting | Show existing exposure, planned exposure, headroom and breach amount. |
| Operations | Do not rely on previous limit after downgrade effective date. |

### QA assertions

| Test | Expected result |
|---|---|
| Placement breaches revised limit | Placement is blocked, resized or escalated. |
| Limit source is stale | Placement workflow fails closed or requires approval. |
| Placement is split across banks | Each bank exposure is checked separately. |
| Treasury dashboard is generated | Headroom and breach amount are visible by counterparty. |

## Example 61. Stale bank-balance certification

### Scenario

A bank balance used for liquidity reporting has not been certified by operations after the daily cut-off. The statement balance exists, but it should not be treated as fully certified liquidity until source freshness and reconciliation checks pass.

| Attribute | Value |
|---|---|
| Bank statement balance | 2,750,000 |
| Last statement timestamp | 08:15 |
| Certification cut-off | 10:00 |
| Current time | 11:30 |
| Certification state | Stale |

### Stale amount

```text
certified_liquidity = 0 until balance is certified or approved as usable under fallback policy
stale_reported_balance = 2,750,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Liquidity reporting | Show balance as reported but stale or uncertified. |
| Buying power | Exclude stale uncertified balance from available cash unless fallback policy approves it. |
| Operations | Route to certification, reconciliation, source refresh or exception approval. |
| Evidence | Preserve source timestamp, certification owner, cut-off and fallback decision. |
| Client reporting | Avoid client-ready liquidity claims based on uncertified bank data. |

### QA assertions

| Test | Expected result |
|---|---|
| Balance is after certification cut-off and uncertified | Liquidity state is stale or source-limited. |
| Fallback approval exists | Usability is labelled fallback-approved with evidence. |
| Balance refresh arrives | Certification state updates from new source timestamp. |
| Advisor attempts payment funding | Funding check blocks or warns on uncertified cash according to policy. |

## Example 62. FX overlay funding waterfall

### Scenario

A DPM portfolio runs a currency overlay to maintain target USD exposure while the base reporting currency is EUR. A monthly rebalance requires USD funding, but the platform must choose funding sources in a controlled waterfall rather than automatically converting all available EUR cash.

| Funding source | Available amount | Haircut/buffer | Policy order |
|---|---:|---:|---:|
| Settled USD cash | 180,000 | 0% | 1 |
| USD money-market fund redemption | 250,000 | 2% | 2 |
| EUR cash conversion | 400,000 EUR | 1.5% FX buffer | 3 |
| Credit-line draw | 500,000 | 5% reserve | 4 |

### Funding waterfall

```text
required_usd_funding = 600,000
usable_usd_cash = 180,000
usable_mmf_cash = 250,000 x (1 - 2%) = 245,000
remaining_after_cash_and_mmf = 600,000 - 180,000 - 245,000 = 175,000
eur_conversion_required_before_buffer = 175,000 / (1 - 1.5%) = 177,665
```

### Correct treatment

| Area | Treatment |
|---|---|
| Waterfall | Use funding sources in policy order and preserve the reason when a source is skipped. |
| FX buffer | Apply FX buffer to source-currency conversion and label funding estimate as indicative until trade execution. |
| Liquidity | Do not use MMF proceeds as cash until redemption state and settlement timing allow it. |
| Advisory/DPM | Show overlay funding as mandate execution, not a client external cashflow. |
| Evidence | Preserve target exposure, funding policy, FX quote, redemption state and approval lineage. |

### QA assertions

| Test | Expected result |
|---|---|
| USD cash is insufficient | Waterfall consumes next eligible source in policy order. |
| MMF redemption is gated | Waterfall skips or haircuts MMF source and opens liquidity exception. |
| EUR FX quote is stale | Conversion amount is blocked or labelled indicative. |
| Credit line is used | Draw is visible as financing, not free cash. |

## Example 63. Liquidity stress communication

### Scenario

A market-wide liquidity event reduces same-day access to cash-like instruments. Advisors need a client communication that explains available liquidity, conditional liquidity and restricted liquidity without implying guaranteed access.

| Liquidity bucket | Normal value | Stress-usable value | Reason |
|---|---:|---:|---|
| Bank cash | 350,000 | 350,000 | Settled cash |
| Money-market fund | 500,000 | 300,000 | 40% stress gate |
| Term deposit | 750,000 | 710,000 | Break penalty |
| FX conversion | 250,000 | 0 | Currency rail restriction |

### Stress view

```text
normal_liquidity = 350,000 + 500,000 + 750,000 + 250,000 = 1,850,000
stress_usable_liquidity = 350,000 + 300,000 + 710,000 + 0 = 1,360,000
restricted_or_conditional_liquidity = 490,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Communication | Separate confirmed, conditional and restricted liquidity in client/advisor messaging. |
| Source state | Link each liquidity bucket to gate notice, deposit quote, rail status or bank confirmation. |
| Claims | Avoid guaranteeing same-day liquidity where execution, redemption, break approval or rail status is uncertain. |
| Operations | Track audience, approval, timestamp and next-update cadence. |
| Reporting | Preserve stress scenario assumptions and do not overwrite normal liquidity metrics. |

### QA assertions

| Test | Expected result |
|---|---|
| MMF gate changes | Stress-usable value recalculates and communication version updates. |
| Deposit break approval is missing | Term-deposit liquidity is labelled conditional. |
| FX rail is restricted | FX conversion contributes zero same-day stress liquidity. |
| Advisor sends communication | Message includes assumptions, source timestamp and approved wording. |

## Example 64. Negative-rate client disclosure

### Scenario

A client holds a large EUR cash balance in a booking centre with a negative-rate policy. The charge applies only above the exempt threshold and must be disclosed before the client report is finalized.

| Attribute | Value |
|---|---:|
| Average EUR cash balance | 2,400,000 |
| Exempt threshold | 1,000,000 |
| Annual negative rate | -0.60% |
| Days in period | 30 |
| Day-count basis | 360 |

### Charge

```text
chargeable_balance = 2,400,000 - 1,000,000 = 1,400,000
negative_interest_charge = 1,400,000 x 0.60% x 30 / 360 = 700
```

### Correct treatment

| Area | Treatment |
|---|---|
| Disclosure | Show threshold, chargeable balance, rate, period and booking centre policy. |
| Accrual | Accrue charge by effective-dated rate and split periods when policy changes. |
| Reporting | Label charge as negative interest or cash charge, not investment loss. |
| Advisory | Surface alternatives only as policy-approved liquidity discussion, not tax or investment advice. |
| Evidence | Preserve rate table, threshold, average-balance calculation and client disclosure version. |

### QA assertions

| Test | Expected result |
|---|---|
| Balance is below threshold | No charge is calculated. |
| Rate changes mid-period | Charge is split by effective date. |
| Disclosure is missing | Client-ready report is blocked or exception-labelled. |
| Multi-currency report is generated | Negative charge remains in source currency with reporting-currency translation. |

## Example 65. Multi-currency cash pledge dispute

### Scenario

A client pledges USD and CHF cash against a Lombard facility. A dispute arises because treasury applies different FX haircuts and excludes one restricted cash bucket from collateral value.

| Cash bucket | Amount | FX to USD | Haircut | Eligibility |
|---|---:|---:|---:|---|
| USD free cash | 300,000 | 1.0000 | 5% | Eligible |
| CHF free cash | 500,000 | 1.1000 | 8% | Eligible |
| EUR restricted cash | 200,000 | 1.0800 | n/a | Ineligible |

### Pledge value

```text
usd_collateral_value = 300,000 x (1 - 5%) = 285,000
chf_collateral_value_usd = 500,000 x 1.1000 x (1 - 8%) = 506,000
eligible_collateral_value = 285,000 + 506,000 = 791,000
excluded_restricted_cash_usd = 200,000 x 1.0800 = 216,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Eligibility | Apply pledge eligibility before FX translation and haircut treatment. |
| Dispute | Preserve client claim, policy version, FX rate, haircut source and restriction reason. |
| Buying power | Use only eligible collateral value for credit headroom. |
| Reporting | Show pledged, restricted and excluded cash separately. |
| Resolution | Recalculate only from approved policy or corrected source evidence. |

### QA assertions

| Test | Expected result |
|---|---|
| Restricted EUR cash is pledged by client request | It remains excluded unless eligibility changes with approval. |
| FX rate is stale | Collateral value is blocked or haircut-adjusted by fallback policy. |
| Haircut policy changes | Pledge value recalculates from effective date with lineage. |
| Dispute is opened | Original and revised collateral values remain auditable. |

## Example 66. Payment screening queue ageing

### Scenario

Cross-border payments are held in screening. Operations need an ageing dashboard that distinguishes new holds from SLA breaches and identifies liquidity that remains reserved while payment release is pending.

| Queue bucket | Payments | Reserved cash |
|---|---:|---:|
| 0-2 hours | 18 | 420,000 |
| 2-6 hours | 9 | 310,000 |
| 6-24 hours | 4 | 125,000 |
| Over 24 hours | 2 | 80,000 |

### Ageing metrics

```text
total_held_payments = 18 + 9 + 4 + 2 = 33
total_reserved_cash = 420,000 + 310,000 + 125,000 + 80,000 = 935,000
breach_payment_count = 2
breach_rate = 2 / 33 = 6.06%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Queue state | Track screening reason, hold timestamp, owner, SLA and next action. |
| Liquidity | Keep cash reserved while release, reject or cancel outcome is pending. |
| Escalation | Escalate aged items by SLA, amount, client impact and regulatory sensitivity. |
| Reporting | Do not show held payment as completed or failed until final outcome. |
| Analytics | Separate false positives, data repair, sanctions review and compliance review. |

### QA assertions

| Test | Expected result |
|---|---|
| Payment exceeds SLA | Queue item escalates with ageing bucket and owner. |
| Payment is released | Reserved cash moves to released payment state with timestamp. |
| Payment is rejected | Cash reservation reverses with rejection evidence. |
| Dashboard is generated | Held count, reserved cash and breach rate reconcile to queue data. |

## Example 67. Treasury policy exception attestation

### Scenario

Treasury approves a temporary exception to place cash above normal counterparty limits during a market disruption. The exception must be time-bound, attested and reviewed after expiry.

| Attribute | Value |
|---|---:|
| Normal counterparty limit | 5,000,000 |
| Temporary approved limit | 7,000,000 |
| Actual peak exposure | 6,400,000 |
| Exception duration | 5 business days |

### Exception headroom

```text
normal_limit_breach = 6,400,000 - 5,000,000 = 1,400,000
temporary_limit_headroom = 7,000,000 - 6,400,000 = 600,000
exception_utilization = 6,400,000 / 7,000,000 = 91.43%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Approval | Preserve exception approver, reason, scope, start date, expiry and maximum limit. |
| Monitoring | Track peak exposure, daily utilization and breaches of temporary limit. |
| Expiry | Automatically revert to normal limit after expiry unless renewed with evidence. |
| Attestation | Require post-event review of usage, outcome and any residual exposure. |
| Reporting | Show exception-adjusted compliance separately from normal policy compliance. |

### QA assertions

| Test | Expected result |
|---|---|
| Exposure exceeds normal limit but stays within temporary approval | Position is exception-compliant with approval lineage. |
| Exposure exceeds temporary limit | Breach workflow opens. |
| Exception expires | Normal limit is restored automatically. |
| Attestation is missing after expiry | Governance closure remains incomplete. |

## Example 68. Liquidity buffer model drift

### Scenario

A discretionary mandate uses a model-driven cash buffer to protect near-term fees, settlements and expected withdrawals. The model has not been recalibrated after higher payment volatility, so the recommended investable cash may be overstated.

| Component | Current model | Observed requirement |
|---|---:|---:|
| Settlement buffer | 120,000 | 120,000 |
| Fee and tax reserve | 35,000 | 35,000 |
| Withdrawal buffer | 75,000 | 145,000 |
| Stress uplift | 10% | 18% |

### Drift check

```text
current_model_buffer = (120,000 + 35,000 + 75,000) x 1.10 = 253,000
observed_required_buffer = (120,000 + 35,000 + 145,000) x 1.18 = 354,000
buffer_drift = 354,000 - 253,000 = 101,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Mandate cash | Keep the old model output labelled current-policy until recalibration is approved. |
| Investable cash | Reduce investable cash or mark the excess as model-risk pending review. |
| Source evidence | Preserve withdrawal history, fee/tax schedule, settlement obligations and stress-policy version. |
| Governance | Route material drift to investment, risk or mandate governance before automated rebalancing. |
| Reporting | Explain cash-buffer change as liquidity reserve change, not investment return. |

### QA assertions

| Test | Expected result |
|---|---|
| Observed requirement exceeds model buffer | Drift amount is calculated and surfaced for review. |
| Rebalance consumes disputed buffer | Trade proposal is blocked or warning-labelled by mandate policy. |
| New buffer policy is approved | Future investable cash uses new effective-dated model. |
| Client report is generated | Buffer movement is shown as liquidity reserve change, not portfolio P&L. |

## Example 69. Payment beneficiary whitelist exception

### Scenario

A high-value payment is requested to a beneficiary that is not on the approved whitelist. The client relationship manager claims it is urgent, but operations must separate payment validation, whitelist exception approval and cash reservation.

| Attribute | Value |
|---|---|
| Payment amount | 420,000 |
| Beneficiary status | Not whitelisted |
| Cut-off remaining | 45 minutes |
| Client cash available | 780,000 |
| Exception approval | Pending |

### Payment eligibility

```text
payment_cash_reserved = payment_amount
payment_release_allowed = beneficiary_whitelisted or approved_whitelist_exception
```

### Correct treatment

| Area | Treatment |
|---|---|
| Liquidity | Reserve cash while validation is pending, but do not mark the payment complete. |
| Controls | Require beneficiary verification, callback evidence, exception reason and approval owner. |
| Fraud risk | Do not bypass screening or callback because sufficient cash exists. |
| Reporting | Show payment as pending control approval, not failed or completed. |
| Audit | Preserve original instruction, beneficiary details, whitelist state and approval decision. |

### QA assertions

| Test | Expected result |
|---|---|
| Beneficiary is not whitelisted | Payment release is blocked pending exception approval. |
| Cash is sufficient | Cash is reserved but release state remains control-pending. |
| Exception is rejected | Reservation reverses and payment is cancelled or returned for repair. |
| Exception is approved after cut-off | Revised value date and client communication are updated. |

## Example 70. Deposit rollover suitability conflict

### Scenario

A term deposit is due to mature. A relationship team proposes automatic rollover into a longer tenor with a higher rate, but the client has a documented liquidity need within the new tenor.

| Attribute | Value |
|---|---:|
| Maturing deposit | 1,000,000 |
| Proposed rollover tenor | 12 months |
| Current alternative tenor | 3 months |
| Known liquidity need | 650,000 in 4 months |
| 12-month rate | 4.20% |
| 3-month rate | 3.60% |

### Liquidity conflict

```text
liquidity_need_within_rollover_tenor = true
rollover_excess_over_liquidity_need = 1,000,000 - 650,000 = 350,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Suitability | Do not treat highest rate as automatically suitable when liquidity horizon conflicts. |
| Recommendation | Consider splitting maturity proceeds across cash, short tenor and longer tenor if policy permits. |
| Disclosure | Show rate, tenor, break penalty, liquidity need and alternative placements. |
| Execution | Require client or mandate approval for rollover, split placement or cash retention. |
| Evidence | Preserve maturity instruction, suitability rationale, client liquidity objective and quote timestamp. |

### QA assertions

| Test | Expected result |
|---|---|
| Proposed tenor exceeds known liquidity need horizon | Suitability conflict is raised. |
| Advisor splits proceeds | Each bucket has separate tenor, rate, maturity and approval evidence. |
| Deposit auto-rolls without approval | Exception is raised when policy requires instruction. |
| Client report is generated | Maturity proceeds, rollover amount and retained liquidity are separately labelled. |

## Example 71. Intraday cash forecast confidence scoring

### Scenario

Operations uses an intraday forecast to release payments before all expected credits have arrived. Forecast inputs have different confidence levels depending on source, cut-off and historical reliability.

| Forecast input | Amount | Confidence |
|---|---:|---:|
| Opening certified cash | 500,000 | 100% |
| Expected custody income | 180,000 | 85% |
| Incoming client transfer | 250,000 | 60% |
| Expected FX settlement credit | 300,000 | 75% |

### Confidence-weighted capacity

```text
confidence_weighted_inflows = 180,000 x 85% + 250,000 x 60% + 300,000 x 75%
confidence_weighted_inflows = 153,000 + 150,000 + 225,000 = 528,000
forecast_confidence_capacity = 500,000 + 528,000 = 1,028,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Payment release | Use confidence-weighted capacity for discretionary release, not raw expected inflows. |
| Forecasting | Preserve source, confidence score, cut-off, owner and historical accuracy basis. |
| Risk | Escalate large payments that depend on low-confidence inflows. |
| Operations | Update confidence as confirmations arrive or miss cut-off. |
| Reporting | Keep forecast confidence internal unless client communication explicitly requires it. |

### QA assertions

| Test | Expected result |
|---|---|
| Low-confidence inflow supports payment release | Payment requires escalation or stays pending. |
| Expected credit misses cut-off | Forecast capacity recalculates and dependent payments re-prioritize. |
| Source confirmation arrives | Confidence becomes confirmed cash or certified receivable. |
| Dashboard is generated | Raw forecast, confidence-weighted capacity and confirmed cash are distinct. |

## Example 72. Nostro overdraft fee allocation

### Scenario

A nostro account incurs an overdraft fee because several client payments and settlement debits consumed intraday liquidity before expected credits arrived. Operations must allocate the fee by causality instead of spreading it evenly across all clients.

| Driver | Amount causing overdraft | Allocation basis |
|---|---:|---|
| Client payment A | 300,000 | Released before credit cut-off |
| Client payment B | 150,000 | Released before credit cut-off |
| Securities settlement debit | 250,000 | Contractual settlement |
| Operations timing delay | 100,000 | Internal processing issue |

### Fee allocation

```text
total_overdraft_driver = 300,000 + 150,000 + 250,000 + 100,000 = 800,000
client_a_share = 300,000 / 800,000 = 37.50%
client_b_share = 150,000 / 800,000 = 18.75%
settlement_share = 250,000 / 800,000 = 31.25%
operations_share = 100,000 / 800,000 = 12.50%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Allocation | Allocate only client-chargeable portions under policy and exclude bank/internal-caused portions where required. |
| Evidence | Preserve nostro statement, intraday timeline, payment release decisions, settlement obligations and fee notice. |
| Client reporting | Label any pass-through as financing or operational fee according to policy. |
| Dispute | Keep disputed allocation separate from posted charge until resolved. |
| Controls | Feed recurring causes into intraday liquidity forecasting and cut-off policy review. |

### QA assertions

| Test | Expected result |
|---|---|
| Fee includes internal timing delay | Internal-caused portion is not charged to client unless policy permits. |
| Client disputes allocation | Original fee, proposed allocation and disputed amount remain auditable. |
| Settlement debit is mandatory | Allocation follows documented settlement funding policy. |
| Report is generated | Fee, driver and resolution status reconcile to nostro evidence. |

## Example 73. FX conversion best-execution evidence

### Scenario

A client converts a large CHF balance into USD. The platform must retain evidence that the execution rate, quote source, spread and timing were reasonable under the applicable best-execution policy.

| Attribute | Value |
|---|---:|
| Source currency | CHF |
| Target currency | USD |
| Converted amount | 2,000,000 CHF |
| Executed rate | 1.1120 |
| Reference mid-rate | 1.1145 |
| Policy spread tolerance | 35 bps |

### Execution spread

```text
spread_bps = abs(reference_mid_rate - executed_rate) / reference_mid_rate x 10,000
spread_bps = abs(1.1145 - 1.1120) / 1.1145 x 10,000 = 22.43 bps
within_policy = 22.43 <= 35
```

### Correct treatment

| Area | Treatment |
|---|---|
| Execution evidence | Preserve quote source, timestamp, executed rate, mid-rate, spread, dealer/venue and order instruction. |
| Suitability | Distinguish client-requested conversion, funding conversion, DPM hedge execution and forced conversion. |
| Reporting | Show realized FX conversion and fees/spread treatment according to statement policy. |
| Exceptions | Route stale quotes, wide spreads, manual overrides or off-market timing to review. |
| Analytics | Do not treat best-execution spread as investment performance unless reporting policy explicitly includes it. |

### QA assertions

| Test | Expected result |
|---|---|
| Spread is within tolerance | Execution is evidence-compliant with stored quote data. |
| Mid-rate source is stale | Best-execution check is blocked or exception-labelled. |
| Manual dealer override is used | Approval and reason are required. |
| Client asks for execution explanation | Report can show timestamp, reference basis and spread evidence without exposing restricted dealer data. |

## Example 74. Client cash yield tier change

### Scenario

A client cash account moves into a higher yield tier during the month after a large deposit settles. Interest accrual must apply the correct tier by effective date, not the month-end balance for the entire period.

| Period | Balance | Annual rate | Days |
|---|---:|---:|---:|
| Before tier change | 450,000 | 1.20% | 12 |
| After tier change | 1,250,000 | 2.05% | 18 |

### Accrual calculation

```text
pre_change_interest = 450,000 x 1.20% x 12 / 360 = 180.00
post_change_interest = 1,250,000 x 2.05% x 18 / 360 = 1,281.25
monthly_interest = 180.00 + 1,281.25 = 1,461.25
```

### Correct treatment

| Area | Treatment |
|---|---|
| Tiering | Apply rate tiers by effective-dated balance band, product, currency and booking centre. |
| Accrual | Split accrual periods when the qualifying balance or tier changes. |
| Reporting | Show rate basis, period, balance band and accrued interest separately from investment return. |
| Controls | Preserve rate table version, source approval and any discretionary pricing override. |
| QA | Test boundary balances exactly at tier thresholds and across month-end transitions. |

### QA assertions

| Test | Expected result |
|---|---|
| Balance crosses tier threshold mid-period | Accrual splits by effective date. |
| Rate table changes retrospectively | Prior accrual is restated only through approved correction workflow. |
| Client has discretionary rate override | Override source, approval and expiry are retained. |
| Statement is generated | Yield tier and period basis are visible or explainable according to reporting policy. |

## Example 75. Payment cut-off extension approval

### Scenario

A high-priority payment misses the standard market cut-off, but operations obtains a one-off cut-off extension from the payment operations manager and correspondent bank.

| Attribute | Value |
|---|---|
| Standard cut-off | 16:00 |
| Payment request time | 16:18 |
| Approved extension | 16:30 |
| Payment amount | 850,000 |
| Correspondent acknowledgement | Received |

### Cut-off decision

```text
standard_cutoff_missed = payment_request_time > standard_cutoff
extension_window_open = payment_request_time <= approved_extension_time
release_allowed = extension_window_open and correspondent_acknowledgement_received
```

### Correct treatment

| Area | Treatment |
|---|---|
| Approval | Preserve approver, extension reason, expiry time, payment id and correspondent acknowledgement. |
| Liquidity | Reserve cash when payment is accepted into the extended window. |
| Controls | Prevent reuse of one-off extension for unrelated payments. |
| Client communication | Show revised value date or pending state when extension is not confirmed. |
| Audit | Link final release to the extension approval and screening evidence. |

### QA assertions

| Test | Expected result |
|---|---|
| Payment arrives after standard cut-off | Payment is blocked unless extension is approved. |
| Extension is approved for one payment | Only scoped payment can use the extension. |
| Correspondent acknowledgement is missing | Payment remains pending or moves to next value date. |
| Extension expires | Standard cut-off rule applies again automatically. |

## Example 76. Intraday liquidity stress-test backtesting

### Scenario

Treasury compares a prior intraday stress forecast with actual cash movements. The forecast underestimated peak liquidity need, so the stress model needs review.

| Component | Forecast | Actual |
|---|---:|---:|
| Opening liquidity | 4,000,000 | 4,000,000 |
| Peak outgoing payments | 2,700,000 | 3,350,000 |
| Expected incoming credits | 1,200,000 | 850,000 |
| Required buffer | 500,000 | 500,000 |

### Backtest variance

```text
forecast_min_liquidity = 4,000,000 - 2,700,000 + 1,200,000 - 500,000 = 2,000,000
actual_min_liquidity = 4,000,000 - 3,350,000 + 850,000 - 500,000 = 1,000,000
stress_forecast_error = forecast_min_liquidity - actual_min_liquidity = 1,000,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Backtesting | Preserve forecast version, assumptions, actual movements, cut-off misses and variance reason. |
| Model governance | Trigger review when variance breaches materiality or repeated underestimation threshold. |
| Payment policy | Feed backtesting into payment prioritization, buffer policy and escalation rules. |
| Reporting | Explain variance as liquidity model performance, not portfolio performance. |
| Controls | Avoid automatically changing buffers without approved effective-dated policy. |

### QA assertions

| Test | Expected result |
|---|---|
| Actual liquidity need exceeds forecast | Backtest variance is calculated and reviewed. |
| Variance breaches threshold | Model review or policy exception opens. |
| Forecast assumptions are missing | Backtest is incomplete and cannot certify model performance. |
| New buffer is approved | Future forecasts use effective-dated model version. |

## Example 77. Cross-currency liquidity buffer translation

### Scenario

A portfolio holds liquidity buffers in USD, EUR and CHF, but the mandate reports required liquidity in USD. FX rates and haircuts must be applied before declaring the buffer sufficient.

| Currency | Cash | FX to USD | Haircut |
|---|---:|---:|---:|
| USD | 500,000 | 1.0000 | 0% |
| EUR | 300,000 | 1.0800 | 5% |
| CHF | 250,000 | 1.1200 | 8% |

### Translated buffer

```text
usd_buffer = 500,000 x 1.0000 x (1 - 0%) = 500,000
eur_buffer = 300,000 x 1.0800 x (1 - 5%) = 307,800
chf_buffer = 250,000 x 1.1200 x (1 - 8%) = 257,600
translated_liquidity_buffer = 1,065,400
```

### Correct treatment

| Area | Treatment |
|---|---|
| FX translation | Use source-backed FX rates, timestamp, reporting currency and haircut policy. |
| Liquidity | Distinguish nominal cash from haircut-adjusted liquidity buffer. |
| Mandate | Compare translated buffer to mandate liquidity floor and near-term obligations. |
| Stress | Apply stronger haircuts for restricted, volatile or non-core currencies when policy requires. |
| Reporting | Label translation rate, haircut and supportability state. |

### QA assertions

| Test | Expected result |
|---|---|
| FX rate is stale | Buffer is marked stale or excluded by policy. |
| Currency has restriction | Additional haircut or exclusion applies. |
| Mandate floor is tested | Floor uses haircut-adjusted translated amount. |
| Report is generated | Nominal cash and translated liquidity buffer are not confused. |

## Example 78. Returned-payment fee remediation

### Scenario

A cross-border payment is returned due to beneficiary-bank data mismatch. The correspondent deducts a fee, and the client asks whether the fee should be refunded.

| Attribute | Value |
|---|---:|
| Original payment | 125,000 |
| Correspondent return fee | 85 |
| Internal repair fee | 25 |
| Client-caused data error | False |

### Fee remediation

```text
total_return_cost = correspondent_return_fee + internal_repair_fee
total_return_cost = 85 + 25 = 110
client_refund_amount = total_return_cost if bank_or_provider_error else 0
```

### Correct treatment

| Area | Treatment |
|---|---|
| Return reason | Preserve return message, beneficiary-bank reason, original instruction and repair state. |
| Fee decision | Classify fee as client-caused, bank-caused, provider-caused or shared according to evidence. |
| Cash | Separate returned principal, deducted fees, remediation credit and reissued payment. |
| Reporting | Show returned payment lifecycle without duplicating payment outflow. |
| Controls | Feed repeated return reasons into standing-instruction and beneficiary-data quality monitoring. |

### QA assertions

| Test | Expected result |
|---|---|
| Payment is returned | Principal, fee and return reason are recorded separately. |
| Bank/provider error is confirmed | Fee refund or compensation workflow opens. |
| Client data caused return | Fee is not automatically waived unless policy allows. |
| Payment is resent | Resent instruction links to original return and repair evidence. |

## Example 79. FX standing-instruction drift

### Scenario

A client has a standing instruction to convert excess EUR cash to USD weekly. The instruction was created under an old target-cash policy and now conflicts with a revised mandate cash buffer.

| Attribute | Old instruction | Current policy |
|---|---:|---:|
| EUR excess threshold | 50,000 | 150,000 |
| Weekly EUR balance | 120,000 | 120,000 |
| Conversion amount under old rule | 70,000 | 0 |

### Drift amount

```text
old_rule_conversion = max(0, eur_balance - old_threshold) = 70,000
current_policy_conversion = max(0, eur_balance - current_threshold) = 0
standing_instruction_drift = old_rule_conversion - current_policy_conversion = 70,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Instruction governance | Preserve original instruction, mandate policy version, threshold, consent and expiry/review date. |
| Drift detection | Compare active standing instructions to current mandate cash and FX policy. |
| Execution | Block, warn or route conversion for review when instruction conflicts with current policy. |
| Reporting | Explain cancelled or reduced conversion as instruction-policy drift, not failed FX execution. |
| Controls | Require periodic recertification of standing FX instructions. |

### QA assertions

| Test | Expected result |
|---|---|
| Standing instruction conflicts with current policy | Conversion is blocked, warning-labelled or routed for approval. |
| Instruction is recertified | New threshold and effective date are stored. |
| Conversion already executed | Impact assessment identifies FX trade, cash and report outputs. |
| Client report is generated | Policy-driven non-execution is distinct from operational failure. |
