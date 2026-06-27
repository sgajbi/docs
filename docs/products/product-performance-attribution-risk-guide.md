# Product Performance, Attribution And Risk Guide

This guide defines how to think about performance, contribution, attribution, and risk analytics across the covered product families.

Use it when designing portfolio performance reports, contribution views, risk dashboards, benchmark comparisons, advisory reviews, DPM monitoring, data contracts, or QA scenarios. It complements [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md), [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md), and [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md).

## Core Principle

Performance and risk analytics must preserve the difference between:

1. legal holding,
2. accounting position,
3. market value,
4. cashflow classification,
5. income,
6. price return,
7. FX return,
8. fees and taxes,
9. look-through exposure,
10. contingent or notional exposure.

If the platform cannot prove one of these layers from source data, the report should show a partial or support-limited analytics state.

## Return Component Model

| Component | Meaning | Common Source Inputs | Failure Behavior |
|---|---|---|---|
| Price return | Change in value from price, NAV, quote, appraisal, or model movement. | Position, price/NAV/quote/appraisal, valuation date, FX rate. | Mark stale or unavailable when valuation is stale, missing, estimated, or unsupported. |
| Income return | Coupon, dividend, distribution, interest, rental income, annuity income, or confirmed product coupon. | Cash ledger, issuer/admin/insurer notice, accrual schedule, tax/fee details. | Separate accrued/estimated from confirmed paid income. |
| FX return | Reporting-currency effect from currency movement. | Local value, reporting currency, FX rates, rate timestamps, translation policy. | Mark unsupported when FX source or timestamp is missing. |
| Fee and tax effect | Product fees, custody fees, management fees, withholding tax, surrender charges, financing cost. | Transaction ledger, custodian, administrator, insurer, lender, fee schedule. | Do not bury fees or tax inside unexplained residual return. |
| External flow | Client contribution, withdrawal, transfer, or other external portfolio movement. | Cash ledger, transfer records, portfolio membership. | Incorrect external-flow classification distorts TWR/MWR. |
| Internal activity | Buy, sell, switch, roll, rebalance, coupon reinvestment, margin movement, loan drawdown/repayment. | Transaction ledger, order/trade system, lifecycle events. | Do not classify internal investment activity as external performance flow. |
| Residual or unsupported | Unexplained movement from missing data, stale valuation, partial history, or unsupported subtype. | Derived after reconciliation. | Report as exception; do not allocate silently to a product or manager. |

## Product Analytics Matrix

| Product Family | Performance Treatment | Contribution And Attribution | Risk Analytics | Common Analytics Trap |
|---|---|---|---|---|
| Bonds | Separate clean-price movement, accrued interest, coupon income, FX, default/write-down, and amortisation. | Attribute by duration, curve, spread, issuer, rating, sector, currency, maturity bucket, and income. | Duration, convexity, spread, rating, issuer, maturity, callability, liquidity, default/recovery. | Treating accrued interest or pull-to-par as unexplained price return. |
| Cash, deposits, money market and FX | Separate cash interest, deposit accrual, MMF NAV return, realized FX, unrealized FX translation, and forward MTM. | Attribute cash drag, liquidity yield, currency contribution, hedge contribution, deposit income, and funding cost. | Bank/issuer concentration, currency mismatch, settlement, liquidity, MMF gate, deposit breakage, counterparty. | Treating ledger cash as available liquidity or treating FX forward notional as cash. |
| Commodities, precious metals and real assets | Separate spot movement, roll yield, storage/fee drag, FX, derivative P&L, ETP tracking error, and issuer/provider effect. | Attribute by commodity family, wrapper, spot return, roll return, collateral/financing, currency, and issuer/provider. | Volatility, unit/grade, custody/provider, roll, basis, leverage, delivery, counterparty, collateral haircut. | Comparing futures-based product return to spot price without roll and fee explanation. |
| Equities | Separate price return, dividend income, tax, corporate-action effects, FX, fees, and realized/unrealized P&L. | Attribute by security, sector, country, currency, factor, dividend, corporate action, and stock selection. | Market beta, single-name, sector, country, currency, liquidity, restriction, corporate-action, concentration. | Treating withholding tax as investment loss or ignoring corporate-action cost-basis effects. |
| Funds | Use NAV return and distributions; separate reinvested/cash distributions, fees, FX, stale NAV, and look-through limits. | Attribute by fund, manager, asset class, share class, look-through exposure, currency, distribution, and benchmark. | Manager, strategy, fund liquidity, NAV staleness, gates, suspensions, side pockets, fees, concentration. | Reporting look-through attribution when holdings file is stale or unavailable. |
| Insurance and annuities | Separate policy charges, premium flows, investment-linked account movement, surrender charges, policy loans, and annuity income. | Attribute only source-backed ILP sub-fund exposure; report protection and estate benefits outside investment performance. | Insurer credit, lapse, premium sufficiency, liquidity, charges, guarantee basis, policy loan, longevity/mortality. | Treating death benefit or non-guaranteed projection as portfolio performance value. |
| Loans, Lombard, margin and collateral | Treat interest and fees as financing cost; separate loan exposure, collateral movement, margin calls, and liquidation effects. | Attribute leverage effect, interest cost, collateral concentration, funding cost, and forced-sale impact where policy supports. | Utilization, LTV, haircut, collateral concentration, FX haircut, margin-call shortfall, liquidation, rate reset. | Treating collateral market value as borrowing capacity or treating margin movements as investment return without policy support. |
| Private markets | Use cashflow-based metrics such as IRR, TVPI, DPI, RVPI; separate NAV lag, distributions, recallable capital, and restatements. | Attribute by manager, strategy, vintage, geography, sector, paid-in, unfunded, NAV change, distribution type. | Illiquidity, unfunded commitments, valuation uncertainty, vintage, manager, leverage, side pocket, transfer restriction. | Fabricating IRR or attribution when historical cashflows are partial. |
| Real estate, REITs and infrastructure | Separate listed price return, distribution income, NAV/appraisal movement, FX, leverage effects, and income quality. | Attribute by wrapper, property/infrastructure sector, geography, income, leverage, valuation change, manager, and liquidity. | Property sector, tenant/offtaker, geography, leverage, rates, refinancing, occupancy, appraisal lag, concession/regulatory risk. | Treating listed REIT performance as pure equity with no real-asset exposure context. |
| Derivatives | Separate premium, MTM, realized settlement, variation margin, collateral income/cost, exercise/expiry, and hedge link. | Attribute by underlying, delta/DV01/CS01, volatility, carry, hedge effectiveness, counterparty, and strategy purpose. | Notional, Greeks, leverage, margin, collateral, counterparty, fixing, expiry, liquidity, stress loss. | Reporting small MTM as small risk while ignoring large notional or delta exposure. |
| Structured products | Separate issuer quote movement, coupon conditions, barrier/autocall state, underlying movement, issuer credit, and liquidity discount. | Attribute by issuer, underlying basket, payoff state, coupon, barrier distance, volatility, currency, and product subtype. | Issuer, underlying, barrier, worst-of basket, volatility, liquidity, complexity, physical delivery, scenario loss. | Treating conditional coupons as bond-like guaranteed income. |
| Structured notes | Separate note valuation, coupon schedule, credit event state, underlying look-through, issuer credit, and delivered-asset transition. | Attribute by issuer, note subtype, underlyings, coupon status, maturity/call ladder, credit event, and delivery state. | Issuer concentration, underlying risk, coupon condition, credit event, maturity/call, liquidity, delivered asset. | Reporting direct underlying ownership before physical delivery occurs. |

## TWR And MWR Guidance

| Metric | Use | Required Inputs | Products Where It Needs Care |
|---|---|---|---|
| Time-weighted return | Measures manager or strategy performance excluding external client cashflow timing. | Beginning/end values, sub-period valuations, external cashflows, dates, corrections. | All products; stale NAV/private/appraisal products need clear valuation-date policy. |
| Money-weighted return / IRR | Measures investor experience including timing and size of cashflows. | Complete dated cashflows and terminal value. | Private markets, insurance, annuities, structured products, loans, portfolios with large external flows. |
| Simple income yield | Shows current or historical income relative to value. | Income amount, value basis, period, annualization policy. | Bonds, deposits, funds, REITs, structured products, annuities; must label guaranteed versus non-guaranteed. |
| Contribution | Shows how much a position/product added to portfolio return. | Weight, return, cashflows, FX, classification. | All products; derivatives, cash, loans, and private markets need careful denominator and exposure policy. |
| Attribution | Explains why return differed from benchmark or target. | Benchmark, classifications, weights, returns, factors, look-through data. | Equities, bonds, funds, DPM portfolios; unsupported for products without source-backed classification. |

## Risk Denominator Rules

| Risk View | Denominator To Use | Avoid |
|---|---|---|
| Market-value allocation | Current market value or supported valuation basis. | Using death benefit, notional, unfunded commitment, or facility limit as market value. |
| Economic exposure | Delta/notional/look-through/underlying exposure where source-backed. | Treating legal holding market value as full risk exposure for derivatives or structured products. |
| Liquidity exposure | Amount accessible by settlement, redemption, surrender, maturity, transfer, or sale after restrictions. | Treating all market value as liquid. |
| Credit exposure | Issuer, counterparty, lender, insurer, bank, manager, or borrower exposure basis. | Only showing asset class without issuer/counterparty/insurer context. |
| Commitment exposure | Commitment, unfunded, callable amount, facility exposure, premium obligation, or future funding need. | Treating future obligations as zero because current market value is low. |
| Collateral exposure | Pledged market value, eligible value, haircut-adjusted lending value, and shortfall. | Using total holdings when only part is pledged or eligible. |

## Degraded Analytics States

| State | Example | Required Reporting Behavior |
|---|---|---|
| Stale valuation | Old NAV, stale issuer quote, old appraisal, stale insurer value. | Show valuation date and mark affected performance/risk as stale or partial. |
| Missing classification | No sector, strategy, region, issuer, underlying, or manager mapping. | Exclude from that attribution dimension and show coverage gap. |
| Partial look-through | Fund or structured product only partially mapped to holdings or underlyings. | Report look-through coverage percentage and residual unknown bucket. |
| Missing cashflow history | Migrated private-market, policy, or loan without complete history. | Block or label MWR/IRR and income history as partial. |
| Unsupported subtype | Product exists but payoff/lifecycle model is not implemented. | Store/display basic record only; block unsupported analytics claims. |
| Restricted data | Beneficiary, health, claim, legal pledge, or private document restricted by role. | Hide restricted fields and show permission-limited state. |
| Disputed source | Custodian, administrator, issuer, or internal ledger conflict. | Queue exception and avoid final analytics output until resolved. |

## QA Assertions

High-value analytics QA should verify:

1. price return, income return, FX return, fees, taxes, and external flows are classified separately,
2. stale valuation blocks or labels dependent performance and risk metrics,
3. contribution does not force unsupported residuals into supported product buckets,
4. look-through attribution shows coverage percentage,
5. derivative risk uses notional and sensitivity, not only market value,
6. private-market IRR is blocked when cashflow history is partial,
7. insurance protection benefits are excluded from investable performance,
8. collateral lending value is separate from market value,
9. structured-product coupons are conditional until confirmed,
10. client reports show source date, value basis, and supportability state.

## Disclaimer

This guide is for education, product analysis, documentation, performance methodology, risk methodology, reporting design, QA, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.
