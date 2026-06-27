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
