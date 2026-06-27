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
