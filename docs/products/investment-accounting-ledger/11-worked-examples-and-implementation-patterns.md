# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns investment-accounting and ledger concepts into practical examples for platform design, reporting, reconciliation, migration, QA and production support.

## Example 1. Equity buy posting with cash, position and lot

### Scenario

A client buys listed shares in the secondary market.

| Attribute | Value |
|---|---:|
| Quantity | 1,000 |
| Execution price | 25.00 |
| Commission | 50 |
| Tax | 20 |
| Settlement currency | SGD |

### Ledger view

```text

gross consideration = 1,000 x 25.00 = 25,000
total cash settlement = 25,000 + 50 + 20 = 25,070

```

| Posting | Amount | Direction | Accounting meaning |
|---|---:|---|---|
| Security position | 1,000 shares | Increase | New holding quantity |
| Cash | 25,070 | Decrease | Settlement cash outflow |
| Cost basis | 25,070 | Increase | If fees and tax are capitalized by policy |
| Fee expense | 50 | Increase | If fee is expensed by policy |
| Tax expense | 20 | Increase | If tax is expensed by policy |

### Implementation pattern

The transaction should not be stored as one flat amount. Store an order/trade reference, execution facts, cash leg, security leg, fee leg, tax leg, lot creation and source lineage.

### QA assertions

| Test | Expected result |
|---|---|
| Fee capitalization policy changes | Cost basis and expense reporting follow policy. |
| Cash leg is missing | Cash reconciliation fails. |
| Lot is not created | Future realized P&L cannot be calculated correctly. |
| Trade is cancelled | All legs reverse with lineage to the original transaction. |

## Example 2. Trade-date and settlement-date accounting

### Scenario

A client sells a bond on trade date T, settling on T+2.

| Attribute | Value |
|---|---:|
| Nominal sold | 200,000 |
| Clean sale price | 98.50 |
| Accrued interest received | 1,200 |
| Settlement date | T+2 |

### Views

| Date basis | Position | Cash | Use |
|---|---|---|---|
| Trade date | Bond exposure reduced | Sale proceeds pending | Performance, risk and economic exposure |
| Settlement date | Settled custody position reduced | Cash settled | Custody, available cash and ledger cash |

### Correct treatment

Trade-date accounting and settlement-date accounting can both be valid, but they answer different questions. The platform should store dates explicitly instead of deriving one from the other.

### QA assertions

| Test | Expected result |
|---|---|
| Settlement fails | Trade-date exposure may remain reduced while settlement-date position remains unchanged. |
| Report uses custody basis | Settled position is used. |
| Performance uses economic basis | Trade-date position is used. |
| Cash availability is calculated before settlement | Pending proceeds are not treated as settled cash unless credit policy allows it. |

## Example 3. Lot selection and realized P&L

### Scenario

A client sells 150 shares from two existing lots.

| Lot | Quantity | Cost per share |
|---|---:|---:|
| Lot A | 100 | 20.00 |
| Lot B | 100 | 24.00 |

Sale price is 30.00 per share. The lot method is FIFO.

### FIFO calculation

```text

shares sold from Lot A = 100
shares sold from Lot B = 50

sale proceeds = 150 x 30.00 = 4,500
cost relieved = (100 x 20.00) + (50 x 24.00)
cost relieved = 2,000 + 1,200 = 3,200

realized P&L = sale proceeds - cost relieved
realized P&L = 4,500 - 3,200 = 1,300

```

### QA assertions

| Test | Expected result |
|---|---|
| Lot method changes to average cost | Realized P&L changes and must be policy-driven. |
| Historical report is regenerated | Original lot method and cost basis version are used. |
| Partial sale consumes a lot | Remaining lot quantity is updated. |
| Corporate action adjusted lot cost | Sale uses adjusted lot cost, not original raw cost. |

## Example 4. Multi-currency unrealized P&L split

### Scenario

A USD reporting portfolio holds a EUR equity.

| Attribute | Purchase | Current |
|---|---:|---:|
| Quantity | 1,000 | 1,000 |
| Local price | 50 EUR | 55 EUR |
| EUR/USD FX rate | 1.10 | 1.05 |

### Calculation

```text

local cost = 1,000 x 50 = 50,000 EUR
local value = 1,000 x 55 = 55,000 EUR
local P&L = 5,000 EUR

base cost = 50,000 x 1.10 = 55,000 USD
base value = 55,000 x 1.05 = 57,750 USD
base P&L = 2,750 USD

price contribution in USD approximation = 5,000 x 1.10 = 5,500 USD
FX contribution approximation = 2,750 - 5,500 = -2,750 USD

```

### Reporting treatment

The investment gained in local currency, but adverse currency movement reduced the base-currency gain. Reports should separate local price effect and FX translation effect when possible.

### QA assertions

| Test | Expected result |
|---|---|
| Local price rises and FX falls | Base P&L can be lower than local P&L. |
| FX rate missing | Base-currency value is source-limited. |
| Reporting currency differs from portfolio base | Additional translation layer is explicit. |
| FX contribution methodology changes | Report labels and methodology version update. |

## Example 5. Management fee accrual and charging

### Scenario

A discretionary mandate charges a quarterly management fee.

| Attribute | Value |
|---|---:|
| Average assets for quarter | 2,000,000 |
| Annual fee rate | 0.80% |
| Quarter days | 90 |
| Day-count basis | 360 |

### Fee accrual

```text

fee = average assets x annual fee rate x days / day-count basis
fee = 2,000,000 x 0.80% x 90 / 360
fee = 4,000

```

### Lifecycle

| Event | Treatment |
|---|---|
| Daily or monthly accrual | Expense accrues and payable increases. |
| Fee invoice | Accrual is confirmed or adjusted. |
| Cash charge | Cash decreases and payable is cleared. |
| Fee waiver | Expense and payable are reduced with approval lineage. |

### QA assertions

| Test | Expected result |
|---|---|
| Portfolio value changes during quarter | Average asset basis follows methodology. |
| Fee is waived | Waiver is traceable to approval and reason. |
| Fee is charged in different currency | FX rate and cash currency are explicit. |
| Report shows gross and net performance | Fee treatment is consistent with methodology. |

## Example 6. Reconciliation break and correction reversal

### Scenario

The platform books a dividend as 10,000, but the custodian confirms 9,000 after withholding tax.

| Source | Gross | Tax | Net |
|---|---:|---:|---:|
| Platform expected | 10,000 | 0 | 10,000 |
| Custodian confirmed | 10,000 | 1,000 | 9,000 |

### Correction pattern

Do not overwrite the original transaction silently. Create a tax leg or correction transaction with source lineage.

| Posting | Amount | Direction |
|---|---:|---|
| Dividend income gross | 10,000 | Keep |
| Withholding tax | 1,000 | Add expense / tax withheld |
| Cash received | 9,000 | Correct net cash |
| Reconciliation break | Closed | Linked to source correction |

### QA assertions

| Test | Expected result |
|---|---|
| Custodian tax arrives late | Correction adjusts net cash and tax reporting. |
| Report was already issued | Impacted reporting period is flagged for restatement policy. |
| Original transaction is overwritten | Audit QA fails. |
| Reconciliation break is closed | Closure references correcting transaction and source evidence. |
