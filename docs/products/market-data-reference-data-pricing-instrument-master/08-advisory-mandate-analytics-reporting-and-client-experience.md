# 08 - Advisory, Mandate, Analytics, Reporting and Client Experience

## 1. Why market data quality matters to advisory

Advisor recommendations depend on product and market data. Wrong or stale data can cause unsuitable recommendations, wrong risk explanations, inaccurate performance and broken mandate checks.

Market data directly affects:

- portfolio valuation,
- product suitability,
- risk profiling,
- concentration checks,
- liquidity assessment,
- scenario analysis,
- proposal impact,
- cost/yield estimates,
- collateral/buying power,
- client report accuracy.

## 2. Advisory use cases

| Advisory use case | Data requirement |
|---|---|
| Product recommendation | approved product status, risk rating, target market, price/NAV |
| Portfolio review | holdings, valuation, performance, risk, asset allocation |
| Proposal generation | latest price, FX, fees, restrictions, settlement cash |
| Suitability check | product complexity, client profile, concentration, liquidity |
| Yield/income estimate | coupon, dividend, fund distribution, tax/withholding assumptions |
| Downside scenario | price history, volatility, stress assumptions, barriers |
| Switching proposal | sell price, buy price, fees, tax lots, settlement lag |
| Collateral-backed investment | LTV, haircut, collateral value, in-flight orders |

## 3. DPM and mandate use cases

| Mandate control | Data dependency |
|---|---|
| asset allocation limits | classification hierarchy and valuation |
| issuer concentration | entity master, issuer/guarantor linkage |
| currency limits | FX and exposure currency |
| liquidity minimum | liquidity classification, dealing frequency, cash availability |
| credit rating minimum | rating feeds and effective dating |
| restricted securities | watchlist/restricted list data |
| derivatives exposure | notional, delta, Greeks, margin/collateral |
| private asset cap | NAV, commitment and liquidity classification |
| leverage limit | loans, margin, exposure and collateral data |

## 4. Analytics use cases

| Analytics | Data dependency |
|---|---|
| TWR/MWR | valuations, cashflows, FX, corporate actions |
| contribution | market values, returns, classifications |
| attribution | benchmark weights/returns, portfolio weights/returns |
| risk | historical prices, volatilities, correlations, factor data |
| concentration | issuer, sector, country, currency and look-through data |
| stress testing | shocks, sensitivities, Greeks, yield curves |
| income projection | coupon/dividend/distribution schedule |
| cash ladder | settlement dates, maturities, capital calls, coupons |

## 5. Client reporting use cases

| Report section | Data requirements |
|---|---|
| holdings | name, quantity, price, value, currency, price date |
| allocation | classification, look-through, market value |
| performance | valuation series, cashflows, corporate actions |
| income | coupons, dividends, distributions, withholding tax |
| risk | volatility, drawdown, concentration, rating, duration |
| transactions | trade details, settlement, fees, taxes |
| portfolio review | commentary-ready labels and exception flags |
| disclosures | stale price, estimated value, source limitations |

## 6. Degraded-state reporting

A mature platform does not hide data problems. It reports degraded states professionally.

| Data issue | Reporting treatment |
|---|---|
| missing price | exclude from total only with explicit exception or carry last with stale flag based policy |
| stale NAV | show latest available NAV date |
| estimated private market value | label as latest reported NAV / estimated value |
| missing look-through | show fund-level classification and state look-through unavailable |
| suspended instrument | show suspended status and last available value |
| illiquid structured note | show issuer/evaluated value and liquidity warning |
| missing benchmark data | mark benchmark-relative analytics incomplete |

## 7. User experience principles

Advisors need data quality signals before speaking to clients.

Useful UI indicators:

- price date and source,
- stale/estimated/override badge,
- product master completeness status,
- pricing basis explanation,
- unavailable analytics reason,
- pending corporate action alert,
- missing NAV alert,
- mandate breach data source,
- report quality summary before final PDF generation.

## 8. Common stakeholder explanations

### Why is the portfolio value different from yesterday?

Explain price movements, FX movements, transactions, income, fees, corporate actions and valuation-source changes separately.

### Why is a fund not updated today?

Explain NAV publication lag, fund holiday, suspension, stale-policy treatment and report impact.

### Why is a structured note value lower than purchase price?

Explain issuer valuation, market movement, volatility, remaining tenor, barrier distance, bid/offer spread and issuer credit risk.

### Why does mandate exposure differ from legal holdings?

Explain look-through, notional exposure, derivatives delta exposure, collateral exposure, commitments and benchmark-relative classification.

## 9. Governance rule

Any client-facing number should be explainable in business language and traceable in system language. If the advisor cannot explain the source, date, basis and quality of a number, the platform has not completed the job.
