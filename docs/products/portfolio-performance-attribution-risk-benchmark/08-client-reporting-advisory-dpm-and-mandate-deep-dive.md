# 08 - Client Reporting, Advisory, DPM and Mandate Deep Dive

## 1. Purpose

Portfolio analytics becomes valuable only when it is communicated clearly to clients, advisors, portfolio managers and governance teams.

This file explains how performance, attribution, risk and mandate analytics should appear in wealth management workflows and client reporting.

## 2. Client reporting objectives

Client reporting should answer:

1. What is my portfolio worth?
2. How did it perform?
3. Why did it perform that way?
4. What income did I receive?
5. What risks am I taking?
6. Is the portfolio aligned to my goals/mandate?
7. What changed since the last report?
8. What action, if any, is recommended?

## 3. Portfolio review structure

A professional portfolio review report can include:

| Section | Content |
|---|---|
| Executive summary | Total wealth, return, key drivers, major changes |
| Allocation | Asset class, currency, region, sector, liquidity |
| Performance | TWR/MWR, period returns, benchmark comparison |
| Contribution | Top contributors/detractors |
| Attribution | Allocation/selection/currency effects versus benchmark |
| Risk | Volatility, drawdown, concentration, VaR/stress |
| Income | Dividends, coupons, distributions, interest, yield |
| Holdings | Current positions, market value, cost, P&L |
| Transactions | Activity summary |
| Mandate | Target versus actual, breaches, drift |
| Liquidity | Cash, near cash, locked assets, capital calls |
| Liabilities | Loans, margin, collateral coverage |
| Outlook/actions | Rebalance, hedge, reduce concentration, raise liquidity |
| Disclosures | Methodology, valuation, benchmark, fee/tax basis |

## 4. Advisor dashboard

Advisor dashboard should be more action-oriented than client report.

Useful widgets:

- Return versus benchmark
- Top 5 contributors/detractors
- Allocation drift
- Mandate warnings/breaches
- Concentration alerts
- Liquidity shortfall
- Upcoming maturities/coupons/capital calls
- Structured product observation dates
- Bonds callable/maturing soon
- FX exposure and hedge ratio
- Loan/LTV and collateral utilization
- Stale price/NAV warnings
- Missing classification warnings
- Suggested rebalance opportunities

## 5. DPM dashboard

DPM teams need manager-control analytics:

- Strategy/composite performance
- Model versus actual drift
- Account dispersion
- Trade implementation slippage
- Client restriction impact
- Cash drag
- Active risk/tracking error
- Attribution by decision layer
- Mandate breach queue
- Rebalance queue
- Concentration by strategy
- Liquidity and capital-call forecast
- Performance outlier accounts

## 6. Advisory suitability usage

Analytics supports suitability by showing whether product or portfolio risk matches client profile.

Examples:

| Suitability question | Analytics support |
|---|---|
| Is the product too complex? | Product complexity exposure |
| Is risk too high? | Volatility, drawdown, VaR, risk rating |
| Is portfolio concentrated? | Issuer/security/sector concentration |
| Is liquidity adequate? | Liquidity buckets, lock-ups, capital calls |
| Is leverage appropriate? | LTV, loan exposure, gross exposure |
| Is income objective met? | Portfolio yield and income projection |
| Is client overexposed to currency? | Currency exposure and FX stress |
| Is advisory recommendation aligned? | Target/actual allocation and risk change |

## 7. Mandate monitoring

Mandate analytics should include:

| Output | Description |
|---|---|
| Limit status | OK/warning/breach |
| Limit utilization | Actual exposure versus limit |
| Drift | Actual minus target |
| Breach cause | Market movement, trade, cashflow, classification change |
| Breach age | Days since breach started |
| Required action | Rebalance, waiver, client approval, restriction update |
| Grace period | Time allowed to correct |
| Evidence | Data used to determine breach |

## 8. Mandate rule examples

### Balanced DPM mandate

| Rule | Example |
|---|---|
| Equity allocation | 40-60% |
| Fixed income allocation | 30-50% |
| Alternatives | Max 15% |
| Cash | 0-10%, minimum 2% |
| Single issuer | Max 10% |
| Single fund | Max 20% |
| Non-base currency | Max 30% unhedged |
| Structured products | Max 20% |
| Complex products | Permitted only if mandate allows |
| Credit rating | Average IG, HY max 10% |
| Duration | 3-7 years |
| Leverage | Not allowed or max LTV |

### Income mandate

| Rule | Example |
|---|---|
| Minimum expected yield | 4% |
| Equity income allocation | Max 35% |
| HY bonds | Max 20% |
| REITs | Max 20% |
| Cash buffer | Minimum 5% |
| Illiquid products | Max 10% |
| Currency | Income currency aligned to client needs |

### Capital preservation mandate

| Rule | Example |
|---|---|
| Cash/short duration | Minimum 30% |
| Equity | Max 20% |
| Alternatives | Max 5% |
| Structured products | Principal-protected only |
| Credit quality | Minimum A average |
| Duration | Max 3 years |
| Drawdown alert | 5% warning |

## 9. Reporting performance responsibly

Client reports must avoid misleading presentation.

Good practices:

- Show period and date range clearly.
- Show benchmark and benchmark basis.
- Show gross/net basis.
- Explain whether returns include fees/taxes.
- State valuation source for illiquid/model-priced products.
- Show stale NAV dates.
- Do not overstate precision for private assets.
- Use consistent annualization rules.
- Avoid cherry-picking periods.
- Reconcile return to contribution and market value movement where possible.

## 10. Explainable return bridge

A client-friendly return bridge:

```text
Opening portfolio value
+/- External contributions/withdrawals
+ Investment income
+ Realized and unrealized market movement
+/- FX impact
- Fees and taxes
= Closing portfolio value
```

This bridges money movement and performance.

## 11. Performance commentary framework

A good commentary structure:

1. Overall performance and benchmark comparison.
2. Main positive drivers.
3. Main negative drivers.
4. Asset allocation impact.
5. Security/fund selection impact.
6. Currency impact.
7. Income contribution.
8. Risk or mandate changes.
9. Portfolio action or recommendation.

Example:

> The portfolio returned 3.2% during the quarter, outperforming the benchmark by 0.6%. Equity allocation contributed positively, mainly from US technology holdings, while fixed income detracted slightly due to rising yields. Currency exposure added value as USD strengthened against the portfolio base currency. The portfolio remains within mandate limits, although equity exposure is close to the upper band and may require monitoring.

## 12. Risk commentary framework

Explain risk in business language:

| Metric | Client-friendly explanation |
|---|---|
| Volatility | How much returns fluctuate |
| Drawdown | Largest fall from a previous high |
| VaR | Model-estimated downside loss under normal/stressed assumptions |
| Concentration | Reliance on a small number of holdings or issuers |
| Liquidity | How quickly assets can be converted to cash |
| Leverage | Borrowing or derivatives that can magnify gains/losses |
| Currency risk | Impact of FX movements on base-currency value |

## 13. Product-event reporting

Advisory/reporting should surface upcoming events:

| Product | Event |
|---|---|
| Bond | Coupon, maturity, call, rating change |
| Note | Observation, coupon, autocall, barrier proximity, maturity |
| Fund | Distribution, redemption gate, NAV delay |
| Private fund | Capital call, distribution, NAV update |
| Option | Expiry, exercise/assignment risk |
| FX forward | Value date, rollover, settlement |
| Loan | Interest reset, margin call, maturity |
| Insurance | Premium due, surrender charge change, maturity |

## 14. Management reporting

Management needs aggregate controls:

- AUM by asset class and region
- Revenue by product/category
- Client risk distribution
- Mandate breach counts
- Product concentration by issuer
- Structured product exposure by issuer/underlying
- Loan collateral utilization
- Stale price/NAV ageing
- Performance outliers
- Advisor book risk
- Suitability exceptions
- Complaint/report restatement cases

## 15. Reporting hierarchy

Reports may be generated at:

- Account level
- Portfolio level
- Client relationship level
- Household/family level
- Trust/entity level
- Beneficiary view
- Advisor book level
- Strategy/composite level
- Region/location level

Aggregation rules must avoid double counting.

## 16. Client segmentation

Different clients need different depth:

| Segment | Reporting focus |
|---|---|
| Retail/mass affluent | Simple performance, allocation, risk and goals |
| Affluent | Product detail, income, P&L, advisory actions |
| HNW | Multi-asset, tax, FX, structured products, alternatives |
| UHNW/family office | Entity hierarchy, private markets, trusts, consolidated reporting |
| Institutional-like | GIPS/composite, attribution, risk factor, mandate monitoring |

## 17. Report snapshot model

A client report should be reproducible.

Store:

- Report date and generation time
- Portfolio/entity hierarchy
- Holdings used
- Transactions used
- Prices and FX used
- Benchmark returns used
- Calculation results
- Methodology/policy versions
- Disclosures shown
- User who generated report
- Corrections/restatements

## 18. Advisory action framework

Analytics should support actions:

| Signal | Possible action |
|---|---|
| Equity overweight | Rebalance/reduce exposure |
| Cash too high | Invest excess cash |
| Liquidity shortfall | Raise cash before capital call/loan interest |
| FX exposure high | Hedge or diversify currency |
| Issuer concentration | Reduce concentrated position |
| Structured note barrier close | Review downside scenario |
| Bond duration high | Shorten duration or hedge rates |
| Loan LTV high | Add collateral/repay/reduce risk assets |
| Performance under benchmark | Review allocation/selection drivers |
| Mandate breach | Correct or document waiver |

## 19. Client-facing caveats

Examples of useful caveats:

- Past performance does not guarantee future results.
- Returns may be shown before or after fees depending on report basis.
- Illiquid/private assets may be valued using latest available NAV and may lag reporting date.
- Structured product valuations may be issuer-provided or model-based and may differ from exit value.
- Benchmark comparison is for reference and may not reflect client-specific constraints.
- FX movements can materially affect base-currency returns.

## 20. Expert summary

The best wealth analytics platforms do more than calculate numbers. They convert complex product-level activity into a coherent story: what changed, what performed, why it happened, what risk exists, whether the mandate remains aligned, and what action is needed. The same underlying data should support client reports, advisor dashboards, DPM controls and management oversight.
