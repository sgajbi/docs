# 01 - Performance Analytics Fundamentals and Taxonomy

## 1. What portfolio analytics means in wealth management

Portfolio analytics is the discipline of converting investment activity and holdings into meaningful measures of return, risk, exposure, income, mandate compliance and client outcomes.

In a wealth platform, analytics usually supports five major business outcomes:

| Outcome | Examples |
|---|---|
| Performance measurement | TWR, MWR, annualized return, cumulative return, since-inception return |
| Performance explanation | Contribution, attribution, security effect, allocation effect, currency effect |
| Risk measurement | Volatility, drawdown, beta, VaR, CVaR, duration, credit risk, liquidity risk |
| Mandate monitoring | Asset allocation bands, product eligibility, leverage, concentration, restricted lists |
| Client reporting | Portfolio review, statements, advisory packs, DPM reporting, management dashboards |

For an advisor, analytics answers: **How is the portfolio doing, why, and what should we do next?**

For a platform architect, analytics answers: **What data, rules, calculations and controls are needed to produce correct, explainable and repeatable results?**

## 2. Analytics domains

| Domain | Main question | Typical output |
|---|---|---|
| Valuation | What is the portfolio worth? | Market value, accrued income, NAV, exposure |
| Performance | What return was achieved? | TWR, MWR, annualized return |
| Contribution | Which holdings contributed to return? | Asset/security contribution |
| Attribution | Why did portfolio differ from benchmark? | Allocation, selection, interaction, currency |
| Risk | How risky is the portfolio? | Volatility, VaR, drawdown, stress loss |
| Exposure | What is the portfolio exposed to? | Asset class, issuer, currency, sector, country |
| Income | What income was earned? | Dividends, coupons, distributions, interest |
| Mandate analytics | Is the portfolio within rules? | Breaches, warnings, drift, recommended rebalance |
| Benchmark analytics | How did it do versus benchmark? | Excess return, tracking error, information ratio |
| Client reporting | How should results be explained? | Review pack, charts, commentary, disclosures |

## 3. Three levels of portfolio analytics

### Level 1: Accounting / holdings analytics

This answers what the client owns and what it is worth.

Examples:

- Cash balances
- Security positions
- Nominal and quantity
- Cost and market value
- Accrued income
- Realized and unrealized P&L
- Asset allocation
- Currency exposure
- Income earned

### Level 2: Performance and risk analytics

This answers how the portfolio performed and what risks were taken.

Examples:

- TWR and MWR
- Cumulative and annualized return
- Contribution by holding, asset class or sector
- Volatility and drawdown
- Sharpe and Sortino
- Beta and tracking error
- VaR and stress scenarios

### Level 3: Decision and advisory analytics

This answers what action or explanation is needed.

Examples:

- Why did the portfolio underperform?
- Which holdings drove performance?
- Is the portfolio consistent with the mandate?
- Is the client overexposed to one issuer, sector, region or currency?
- Is liquidity sufficient for withdrawals, fees, capital calls or loan interest?
- Is a rebalance needed?
- Are there suitability concerns?

## 4. Performance versus P&L

Performance and P&L are related but not the same.

| Concept | Meaning | Example |
|---|---|---|
| P&L | Absolute profit or loss in currency | Portfolio gained USD 50,000 |
| Return | Profit/loss relative to capital base | Portfolio returned 5% |
| TWR | Return excluding timing impact of external cashflows | Manager/strategy performance |
| MWR | Return considering timing and size of client cashflows | Investor experience |
| Contribution | Portion of return from a holding/category | Equity sleeve contributed 3% |
| Attribution | Explanation of active return versus benchmark | Stock selection added 1% |

Example:

A portfolio gains USD 50,000. That is P&L. Whether that is good depends on whether the starting value was USD 500,000 or USD 5,000,000, whether the client added/withdrew cash, and what the benchmark returned.

## 5. Return types

| Return type | Use |
|---|---|
| Holding-period return | Return over one period |
| Daily return | Foundation for linking periods |
| Cumulative return | Total return across a period |
| Annualized return | Return expressed per year |
| Time-weighted return | Manager/strategy return independent of external cashflow timing |
| Money-weighted return | Investor return considering cashflow timing |
| Gross return | Before fees/taxes depending on policy |
| Net return | After fees/taxes depending on policy |
| Local return | Return in security or local currency |
| Base-currency return | Return translated into portfolio reporting currency |
| Real return | Return adjusted for inflation |
| Excess return | Portfolio return minus benchmark return |
| Relative return | Performance versus benchmark |

## 6. Return components

Total return usually has several components:

| Component | Examples |
|---|---|
| Price return | Equity price movement, bond price change, NAV change |
| Income return | Dividend, coupon, distribution, interest |
| FX return | Currency movement versus base currency |
| Accrual return | Accrued coupon or interest movement |
| Realized P&L | Sale/redemption/exercise profit or loss |
| Unrealized P&L | Mark-to-market movement |
| Fees/expenses | Advisory fee, management fee, custody fee, transaction fee |
| Tax drag | Withholding tax, stamp duty, transaction tax |
| Residual/non-allocable | Rounding, timing, pricing, FX or modelling residual |

A good platform should expose these components clearly enough to explain performance without overwhelming the advisor or client.

## 7. Portfolio hierarchy

Analytics can be calculated at multiple levels:

| Level | Example |
|---|---|
| Client group | Family / household / relationship |
| Legal entity | Trust, foundation, company, individual |
| Portfolio/account | DPM account, advisory account, custody account |
| Mandate | Balanced, income, growth, preservation |
| Sleeve | Equity sleeve, fixed income sleeve, alternatives sleeve |
| Asset class | Equities, bonds, funds, cash, alternatives |
| Security | Apple, US Treasury, fund share class |
| Lot | Purchase lot / tax lot |
| Transaction | Buy, sell, coupon, dividend, capital call |

## 8. Product hierarchy for analytics

A canonical product hierarchy is needed for consistent reporting:

```text
Total Portfolio
 +-- Cash and Liquidity
 +-- Fixed Income
 |    +-- Sovereign bonds
 |    +-- Corporate bonds
 |    +-- Structured notes
 |    +-- Money market instruments
 +-- Equities
 |    +-- Developed market equity
 |    +-- Emerging market equity
 |    +-- REITs
 |    +-- ADR/GDR
 +-- Funds and ETFs
 +-- Alternatives
 |    +-- Private equity
 |    +-- Private credit
 |    +-- Hedge funds
 |    +-- Real assets
 +-- Derivatives
 +-- Structured Products
 +-- Insurance / Annuities
 +-- Loans / Liabilities
```

The hierarchy must be configurable. A REIT may be shown under equities, real estate or alternatives depending on the reporting policy.

## 9. Measurement basis

Analytics must be clear about basis.

| Basis | Question |
|---|---|
| Trade date | When did the investment decision happen? |
| Settlement date | When did cash/security settle? |
| Accounting date | When was it booked? |
| Valuation date | What market value was used? |
| Performance date | Which date is included in return calculation? |
| Reporting date | Which date is shown to client? |

In wealth reporting, most platforms use trade-date positions for investment analytics and settlement-date cash for operational liquidity. Buying power may need both.

## 10. Gross, net and client-facing return

The platform must define what is included in each return basis.

| Return basis | Typical treatment |
|---|---|
| Gross of fees | Excludes advisory/management fees; may include transaction costs depending policy |
| Net of fees | Includes management/advisory fees and possibly custody fees |
| Net of withholding tax | Income shown after withholding tax |
| Pre-tax return | Excludes client-specific tax effects |
| After-tax return | Includes tax effects where supported |
| Model/strategy return | Strategy-level performance, often gross or model basis |
| Client actual return | Actual portfolio performance including client-specific cashflows and costs |

A high-quality system should store the policy used to calculate each metric.

## 11. Common analytics outputs

| Output | Description |
|---|---|
| Total wealth | Market value of assets minus liabilities |
| Portfolio return | TWR/MWR over selected period |
| Period returns | MTD, QTD, YTD, 1Y, 3Y, 5Y, since inception |
| Allocation | Market value weights by category |
| Drift | Actual allocation minus target allocation |
| Contribution | Return contribution by holding/category |
| Attribution | Active return explanation versus benchmark |
| Income | Earned, received and projected income |
| Realized P&L | Closed trade gains/losses |
| Unrealized P&L | Open holding gains/losses |
| Risk metrics | Volatility, Sharpe, drawdown, beta, VaR |
| Concentration | Issuer/security/sector/currency/country exposures |
| Liquidity | Cash, near-cash, liquidation horizon, lock-up |
| Leverage | Loans, derivatives, gross/net exposure |
| Mandate status | Within range, warning, breach |

## 12. Analytics design principles

1. Separate **facts** from **interpretation**.
2. Separate **transactions** from **events**.
3. Separate **accounting position** from **look-through exposure**.
4. Store calculation inputs and versions.
5. Make benchmark selection effective-dated.
6. Make product classification effective-dated and configurable.
7. Recalculate when backdated transactions, prices, FX, corporate actions or mappings change.
8. Use decimals for money and high-precision calculations.
9. Explain residuals rather than hiding them.
10. Treat client-facing reports as controlled outputs, not ad-hoc analytics.

## 13. Expert summary

Portfolio analytics is not only about formulas. It is a controlled interpretation layer over product master, transactions, positions, prices, FX, benchmarks, mandates and client context. The same return number can mean different things depending on cashflow treatment, fee basis, FX translation, benchmark mapping, valuation source and product classification. A professional wealth platform must make those assumptions explicit, versioned and auditable.
