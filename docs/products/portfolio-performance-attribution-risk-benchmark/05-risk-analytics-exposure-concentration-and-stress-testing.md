# 05 - Risk Analytics, Exposure, Concentration and Stress Testing

## 1. Risk analytics purpose

Risk analytics helps advisors, portfolio managers and clients understand how much uncertainty, downside, concentration, leverage and liquidity risk the portfolio carries.

Risk is not one number. A portfolio can have low volatility but high liquidity risk, low market risk but high credit concentration, or attractive income but high leverage and drawdown risk.

## 2. Major risk categories

| Risk type | Meaning |
|---|---|
| Market risk | Loss from price, rate, spread, FX or commodity movement |
| Equity risk | Stock market movement |
| Interest-rate risk | Bond/loan value affected by rate changes |
| Credit risk | Issuer/counterparty/reference entity default or spread widening |
| FX risk | Currency movement versus reporting currency |
| Liquidity risk | Cannot sell or redeem without delay/loss |
| Concentration risk | Too much exposure to one issuer, sector, currency or theme |
| Leverage risk | Borrowing/derivatives magnify losses |
| Counterparty risk | OTC derivative or structured product counterparty fails |
| Valuation risk | Model price/stale NAV may not reflect realizable value |
| Operational risk | Incorrect data, failed corporate action, missed capital call |
| Mandate risk | Portfolio breaches investment policy or suitability limits |

## 3. Exposure analytics

Exposure can differ from market value.

| Product | Market value | Exposure consideration |
|---|---:|---|
| Equity | Market value | Usually same as market value |
| Bond | Market value | Duration, credit, issuer exposure |
| Fund | NAV value | Look-through asset exposure if available |
| Option | Premium/market value | Delta-adjusted or notional exposure |
| Future | Usually small margin balance | Full notional exposure |
| FX forward | Mark-to-market may be small | Currency notional exposure |
| Structured note | Note market value | Underlying, issuer and payoff exposure |
| Private fund | NAV | Committed/unfunded exposure |
| Loan | Liability balance | Leverage and collateral exposure |
| Insurance | Cash/surrender value | Coverage and issuer risk |

## 4. Long, short, gross and net exposure

Definitions:

```text
Long Exposure = Sum positive market or notional exposures
Short Exposure = Absolute value of negative exposures
Gross Exposure = Long Exposure + Short Exposure
Net Exposure = Long Exposure - Short Exposure
```

Example:

| Exposure | Amount |
|---|---:|
| Long equities | 1,000,000 |
| Short futures | -300,000 |
| Gross exposure | 1,300,000 |
| Net exposure | 700,000 |

Gross exposure shows risk scale. Net exposure shows directional exposure.

## 5. Volatility

Volatility measures variability of returns.

Daily volatility:

```text
sigma_daily = standard deviation of daily returns
```

Annualized volatility:

```text
sigma_annual = sigma_daily x sqrt(252)
```

Volatility is useful but incomplete. It treats upside and downside variation similarly and may understate illiquid/private asset risk.

## 6. Drawdown

Drawdown measures decline from prior peak.

```text
Drawdown_t = Current Value / Prior Peak Value - 1
```

Maximum drawdown is the worst drawdown over the period.

| Metric | Meaning |
|---|---|
| Current drawdown | Loss from recent peak |
| Maximum drawdown | Worst peak-to-trough loss |
| Drawdown duration | Time under water |
| Recovery time | Time to regain previous peak |

## 7. Sharpe ratio

Sharpe ratio measures return per unit of total volatility.

```text
Sharpe = (Portfolio Return - Risk-free Rate) / Volatility
```

Limitations:

- Sensitive to period and return frequency
- Not ideal for non-normal returns
- Can be misleading for illiquid assets with smoothed returns
- Does not distinguish upside and downside volatility

## 8. Sortino ratio

Sortino focuses on downside volatility.

```text
Sortino = (Portfolio Return - Target Return) / Downside Deviation
```

Useful for client portfolios where downside matters more than upside variation.

## 9. Beta and alpha

Beta measures sensitivity to benchmark or market.

```text
Portfolio Return = Alpha + Beta x Benchmark Return + Error
```

| Metric | Meaning |
|---|---|
| Beta > 1 | More sensitive than benchmark |
| Beta < 1 | Less sensitive than benchmark |
| Alpha | Return not explained by benchmark exposure |
| R-squared | How much return variation benchmark explains |

## 10. Tracking error and information ratio

Tracking error measures volatility of active return.

```text
Tracking Error = StdDev(Portfolio Return - Benchmark Return)
```

Information ratio:

```text
Information Ratio = Active Return / Tracking Error
```

Useful for DPM and benchmark-relative mandates.

## 11. Value at Risk (VaR)

VaR estimates potential loss over a horizon at a confidence level.

Example:

> 1-day 99% VaR of USD 100,000 means the model estimates there is a 1% chance of losing more than USD 100,000 in one day under model assumptions.

Methods:

| Method | Description |
|---|---|
| Historical simulation | Uses historical return scenarios |
| Variance-covariance | Uses volatility/correlation and normal assumptions |
| Monte Carlo | Simulates risk factors and portfolio revaluation |

VaR limitations:

- Does not show how bad loss can be beyond VaR threshold
- Sensitive to history/model assumptions
- May understate tail risk
- Hard for illiquid/private assets

## 12. CVaR / Expected Shortfall

Expected shortfall estimates average loss beyond the VaR threshold.

It is often more informative for tail risk than VaR.

## 13. Stress testing

Stress testing asks: what happens if specific market shocks occur?

Examples:

| Scenario | Shock |
|---|---|
| Equity sell-off | Global equities -20% |
| Rate shock | Yield curve +100 bps |
| Credit shock | Credit spreads +150 bps |
| FX shock | USD strengthens 10% |
| Oil shock | Oil -30% or +30% |
| Liquidity shock | Private assets marked down 15% |
| Structured note barrier | Underlying falls below knock-in barrier |
| Lombard margin stress | Collateral value falls and LTV breaches |
| Combined crisis | Equity down, credit spreads wider, volatility up, FX shock |

## 14. Concentration analytics

Concentration risk is critical in wealth portfolios.

Common concentration checks:

| Dimension | Example |
|---|---|
| Security | Single holding > 10% |
| Issuer | Issuer group exposure > 15% |
| Counterparty | Structured note issuer exposure > 20% |
| Sector | Technology > 35% |
| Country | China exposure > 25% |
| Currency | Non-base currency > 40% |
| Asset class | Alternatives > 30% |
| Product complexity | Structured products > 25% |
| Illiquid assets | Lock-up assets > 20% |
| Credit rating | Below investment grade > 10% |
| Maturity bucket | Bonds maturing >10Y > 30% |
| Private-market commitment | Unfunded commitments > liquidity buffer |

## 15. Issuer, counterparty and reference-entity exposure

These must be separated.

Example structured note:

| Exposure type | Example |
|---|---|
| Issuer exposure | Bank issuing note |
| Underlying exposure | Apple shares linked to payoff |
| Counterparty exposure | OTC derivative counterparty if applicable |
| Reference entity | For credit-linked note |
| Custodian exposure | Assets held with custodian |

If the system only stores instrument issuer, it may miss underlying concentration.

## 16. Liquidity analytics

Liquidity reporting should classify assets by realistic liquidation horizon.

| Liquidity bucket | Examples |
|---|---|
| Same day | Cash, T-bills, liquid ETFs |
| T+1/T+2 | Listed equities, liquid bonds |
| Weekly | Some funds, less liquid bonds |
| Monthly/Quarterly | Hedge funds, some private funds |
| Locked/Illiquid | Private equity, real estate funds, insurance surrender-constrained products |
| Conditional | Structured notes with issuer bid only |

Liquidity should consider:

- Market depth
- Redemption terms
- Notice periods
- Gates
- Lock-ups
- Side pockets
- Bid/ask spread
- Exit penalties
- Settlement cycle
- Custody restrictions
- Pledged collateral status

## 17. Leverage analytics

Leverage can arise from:

- Lombard loans
- Margin lending
- Short positions
- Futures
- Options
- Swaps
- Structured products
- FX forwards
- Subscription lines in private funds

Metrics:

| Metric | Formula / concept |
|---|---|
| Loan-to-value | Loan exposure / eligible collateral value |
| Gross exposure / NAV | Total gross exposure relative to portfolio value |
| Net exposure / NAV | Directional exposure relative to portfolio value |
| Margin utilization | Used margin / available margin |
| Collateral coverage | Eligible collateral / secured exposure |
| Interest coverage | Income/cash buffer versus financing cost |

## 18. Fixed income risk metrics

| Metric | Meaning |
|---|---|
| Duration | Sensitivity to interest rates |
| Modified duration | Approximate % price change per 1% yield change |
| DV01/PV01 | Currency loss/gain for 1 bp rate move |
| Convexity | Curvature of price/yield relationship |
| Yield to maturity | Return if held to maturity under assumptions |
| Spread duration | Sensitivity to credit spread changes |
| OAS | Option-adjusted spread |
| Average rating | Credit quality indicator |
| Maturity profile | Cashflow timing and reinvestment risk |

## 19. Options/derivatives risk metrics

| Greek | Meaning |
|---|---|
| Delta | Sensitivity to underlying price |
| Gamma | Sensitivity of delta |
| Vega | Sensitivity to volatility |
| Theta | Time decay |
| Rho | Sensitivity to rates |
| DV01 | Rates sensitivity for swaps/futures |
| CS01 | Credit spread sensitivity |

## 20. Risk reporting by audience

| Audience | Detail level |
|---|---|
| Client | Simple: risk level, drawdown, allocation, concentration, liquidity |
| Advisor | Moderate: drivers, warnings, stress scenarios, mandate breaches |
| Portfolio manager | Detailed: factor risk, tracking error, attribution, exposures |
| Risk/compliance | Full controls, breach history, limit utilization |
| Operations | Data quality, reconciliation, missing prices, stale NAVs |

## 21. Expert summary

Risk analytics must combine market value, notional exposure, look-through, product features, liquidity, leverage and mandate context. A low-volatility portfolio can still be risky if it is concentrated, leveraged, illiquid or exposed to complex products. Wealth platforms must therefore support both quantitative risk metrics and business-readable risk explanations.
