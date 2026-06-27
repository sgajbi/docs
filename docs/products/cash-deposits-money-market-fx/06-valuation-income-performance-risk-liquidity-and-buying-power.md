# 06 — Valuation, Income, Performance, Risk, Liquidity and Buying Power

## 1. Valuation overview

| Product | Primary valuation approach |
|---|---|
| Cash | Par amount in currency, translated to base currency using FX rate |
| Deposit | Principal + accrued interest, or fair value if required |
| T-bill / CP / CD | Market/evaluated price, amortised cost or discounted cashflow depending on policy |
| Repo | Principal plus accrued repo interest, adjusted for collateral/margin if required |
| Money market fund | Units × NAV |
| FX spot | Cash legs at agreed rate and base-currency translation |
| FX forward | Mark-to-market versus current forward curve |
| FX swap | Combined near/far leg valuation |
| NDF | Present value of expected cash settlement versus fixing/forward |

## 2. Cash valuation

Cash is valued at par in its own currency.

```text
Local Currency Value = Cash Amount
Base Currency Value = Cash Amount × FX Rate to Base Currency
```

Example:

| Currency | Amount | FX to SGD | SGD value |
|---|---:|---:|---:|
| USD | 100,000 | 1.35 | 135,000 |
| EUR | 50,000 | 1.45 | 72,500 |
| SGD | 80,000 | 1.00 | 80,000 |

Total base-currency cash = SGD 287,500.

## 3. Deposit accrued interest

For simple fixed-rate deposit:

```text
Accrued Interest = Principal × Annual Rate × Accrued Days / Day Count Basis
```

Example:

| Item | Value |
|---|---:|
| Principal | SGD 100,000 |
| Rate | 3.00% |
| Accrued days | 90 |
| Day count | 365 |
| Accrued interest | SGD 739.73 |

## 4. Deposit valuation approaches

| Approach | When used | Treatment |
|---|---|---|
| Principal only | Simple cash reporting | Shows deposit at principal, income separately |
| Principal + accrued interest | Common wealth reporting | Reflects earned interest to date |
| Fair value | Accounting/risk where needed | Discount remaining cashflows at current rates/credit spread |
| Break value | If early withdrawal likely | Principal plus reduced interest minus penalty |

For client reporting, disclose valuation policy clearly. A fixed deposit held to maturity may be reported differently from a negotiable CD traded in the market.

## 5. Money market instrument valuation

### Discount instrument

A T-bill bought at discount and redeemed at par:

```text
Market Value = Face Value × Price %
```

or using discount yield/discount factor depending on market convention.

### Accretion

If amortised cost is used:

```text
Carrying Value Today = Purchase Cost + Accreted Discount To Date
```

### Risk of approximation

Amortised cost can be appropriate for very short-term high-quality instruments under some policies, but it can hide market stress if rates or credit spreads move sharply.

## 6. Money market fund valuation

```text
Market Value = Units × NAV
```

For distributing share classes:

- income is paid as distribution;
- NAV may remain stable or change depending on fund design.

For accumulating share classes:

- income is reflected in NAV growth;
- there may be no separate cash distribution transaction.

## 7. FX valuation

### Foreign cash

Foreign cash produces base-currency P&L when FX rates change.

Example:

| Item | Value |
|---|---:|
| USD cash | 100,000 |
| Initial USD/SGD | 1.35 |
| Initial SGD value | 135,000 |
| New USD/SGD | 1.32 |
| New SGD value | 132,000 |
| FX translation loss | -3,000 SGD |

### FX forward

A forward has no simple cash balance until maturity but has an MTM value.

Simplified concept:

```text
FX Forward MTM = PV(Current market forward value - Contracted forward value)
```

Inputs:

- spot FX rate;
- forward points/forward curve;
- interest rate curves;
- time to maturity;
- discount factors;
- counterparty/collateral treatment;
- bid/ask spread.

## 8. Income treatment

| Product | Income type | Transaction/treatment |
|---|---|---|
| Cash account | Interest | CASH_INTEREST_PAYMENT / accrual |
| Overdraft | Interest expense | CASH_OVERDRAFT_INTEREST |
| Fixed deposit | Interest | DEPOSIT_INTEREST_ACCRUAL/PAYMENT |
| T-bill | Discount accretion or gain | MM_DISCOUNT_ACCRETION / maturity gain |
| Commercial paper | Discount/coupon | Accretion/coupon |
| Repo | Repo interest | REPO_INTEREST_ACCRUAL/PAYMENT |
| MMF distributing | Distribution | MMF_DISTRIBUTION |
| MMF accumulating | NAV appreciation | Valuation/P&L, no cash income transaction unless policy derives income |
| FX forward | MTM/realised P&L | FX_FORWARD_MTM / settlement P&L |

## 9. Performance treatment

| Event | TWR/MWR treatment |
|---|---|
| External cash contribution | External inflow |
| External cash withdrawal | External outflow |
| Deposit placement from portfolio cash | Internal allocation, not external outflow |
| Deposit maturity back to portfolio cash | Internal redemption, not external inflow |
| Cash interest | Investment income |
| MMF subscription from portfolio cash | Internal investment |
| MMF redemption to portfolio cash | Internal redemption |
| MMF distribution | Investment income |
| FX conversion inside portfolio | Internal transaction, not external flow |
| FX translation of foreign cash | Investment/FX effect |
| Forward MTM | Investment effect if contract held in portfolio |
| Forward settlement | Realised investment effect plus cash movement |
| Overdraft interest | Expense |
| Bank/advisory fees | Expense or external flow depending reporting policy |

## 10. Cash drag analytics

Cash drag measures the impact of holding cash instead of target/risk asset allocation.

Simple estimate:

```text
Cash Drag ≈ Cash Weight × (Benchmark/Target Return - Cash Return)
```

Example:

| Item | Value |
|---|---:|
| Portfolio cash weight | 20% |
| Target portfolio return | 8% |
| Cash return | 2% |
| Estimated cash drag | 20% × 6% = 1.2% |

Use with care: cash may be intentional for liquidity, risk reduction or upcoming capital calls.

## 11. Liquidity analytics

Useful metrics:

| Metric | Meaning |
|---|---|
| Immediate cash % | Actual cash as % of portfolio |
| Available cash % | Available cash excluding blocked/reserved amounts |
| Cash equivalents % | Cash + near-cash liquidity products |
| Restricted liquidity % | Pledged/blocked/notice/gated liquidity |
| Negative cash by currency | Overdraft/shortfall exposure |
| Projected cash shortfall | Future date where cash turns negative |
| Days to liquidity | Weighted liquidity timing of holdings |
| Liquidity coverage | Available cash versus expected outflows |
| Capital-call coverage | Liquid assets versus unfunded commitments |

## 12. FX exposure analytics

| Metric | Meaning |
|---|---|
| Gross currency exposure | Total exposure to each currency before hedges |
| Net currency exposure | Exposure after cash, liabilities and hedges |
| Base-currency translated exposure | Currency exposure in reporting currency |
| Hedge ratio | Hedge notional / underlying exposure |
| Unhedged exposure | Net residual currency exposure |
| FX P&L | Gain/loss due to currency movement |
| FX contribution | Currency contribution to portfolio return |
| Settlement currency gap | Cash required in settlement currency versus available |

## 13. Buying power and cash availability

Buying power should not rely only on cash balance.

```text
Buying Power
= Available Cash
+ Available Credit / Margin Capacity
+ Eligible Sell Proceeds if allowed
+ Eligible FX Conversion if allowed
- Pending Orders
- Required Reserves
- Haircuts / Buffers
```

Currency-specific buying power:

```text
Buying Power in Currency X
= Available Cash in X
+ Allowed FX converted value from other currencies
+ Allowed credit in X
- Pending settlement obligations in X
```

## 14. Cash and collateral

Cash can be:

- free cash;
- pledged cash;
- margin cash;
- collateral cash;
- security deposit;
- reserve cash;
- restricted cash.

Only free/available cash should be used for new orders without additional approvals.

## 15. Overdraft and negative cash

Negative cash is economically borrowing.

| Topic | Treatment |
|---|---|
| Position | Negative cash balance in currency |
| Valuation | Negative liability at par, translated to base |
| Interest | Overdraft interest expense |
| Risk | Funding cost, currency risk, credit-line usage |
| Advisory | May be intentional leverage or accidental settlement shortfall |
| Reporting | Should be visible as liability/negative cash, not netted away silently |

## 16. Yield analytics

| Metric | Use |
|---|---|
| Cash yield | Interest earned on cash balances |
| Deposit yield | Contract rate / effective annual yield |
| Weighted liquidity yield | Yield across cash and near-cash products |
| Net cash yield | Cash/deposit/MMF income minus overdraft and fees |
| Yield pickup | Incremental yield versus call cash |
| Break-even FX movement | FX move that offsets foreign deposit yield |

## 17. Example: foreign deposit return

A SGD client places USD 100,000 for one year at 4%.

| Component | Value |
|---|---:|
| Starting USD/SGD | 1.35 |
| Starting SGD value | 135,000 |
| USD maturity value | 104,000 |
| Ending USD/SGD | 1.28 |
| Ending SGD value | 133,120 |
| SGD return | -1.39% |

Although USD interest was positive, SGD return was negative due to FX movement.

## 18. Risk taxonomy

| Risk | Products affected |
|---|---|
| Bank credit risk | Cash, deposits, CDs |
| Sovereign risk | T-bills, government MMFs |
| Corporate credit risk | CP, prime MMFs |
| Interest-rate risk | T-bills, CDs, MMFs, forwards |
| Liquidity risk | Deposits, CP, repos, MMFs |
| FX risk | Foreign cash/deposits/assets/forwards |
| Settlement risk | FX spot/forward/swaps, securities settlements |
| Counterparty risk | FX forwards, swaps, repos, OTC liquidity products |
| Collateral risk | Repos, margin/collateral arrangements |
| Operational risk | Value dates, rates, NAVs, cutoffs, calendars |
| Concentration risk | Large cash with one bank or issuer |

## 19. Reporting controls

Reports should show:

- cash by currency;
- negative cash separately;
- cash equivalents separately from true cash;
- deposits by maturity ladder;
- deposit accrued interest;
- money market funds by NAV date and settlement cycle;
- FX forward exposure and maturity dates;
- restricted/pledged cash;
- projected cash shortfalls;
- base-currency translation method;
- income and yield by product family.

## 20. Practitioner principle

```text
Liquidity is not the same as market value.
Cash is not the same as available cash.
Foreign currency yield is not the same as base-currency return.
Money market funds are not bank deposits.
FX conversion is not an external portfolio cashflow.
```
