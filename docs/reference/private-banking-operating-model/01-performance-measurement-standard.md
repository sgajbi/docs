# Performance Measurement Standard

This standard consolidates the former wiki material on TWR, MWR, contribution, attribution, return decomposition, benchmark alignment, leverage, fees, taxes, data quality, and performance reporting.

Use it when designing performance APIs, dashboards, client statements, DPM composite reporting, performance QA, discrepancy investigations, or performance methodology documentation.

## Core Principle

Performance must answer a specific question.

| Question | Preferred Metric | Reason |
|---|---|---|
| How did the portfolio or strategy perform independent of client flow timing? | Time-weighted return | Measures investment result after neutralizing external flows. |
| What was the client's actual experience based on cashflow timing? | Money-weighted return / IRR | Reflects when the client contributed, withdrew, or funded capital. |
| What amount of money was made or lost? | P&L | Shows currency impact, not percentage return. |
| What drove the return? | Return decomposition, contribution, attribution | Explains income, price, FX, fees, taxes, allocation, selection, and residual effects. |
| Can the number be trusted? | Data-quality and audit state | Shows source lineage, valuation state, cashflow classification, and restatement status. |

Do not mix these answers without labels. A report that shows TWR, MWR, P&L, contribution, and attribution must explain what each metric means and what it does not mean.

## TWR Standard

Time-weighted return measures the performance of assets or a strategy while neutralizing external client flow timing.

Use daily chain-linking for production-quality portfolio reporting when daily valuations and cashflows are available:

```text
TWR_period = product(1 + daily_return_t) - 1
```

Daily return must use a documented cashflow timing convention:

| Convention | Formula Shape | Use |
|---|---|---|
| End-of-day flow | `r = (EMV - BMV - external_flow) / BMV` | Default when flows are assumed not invested during the day. |
| Beginning-of-day flow | `r = (EMV - BMV - external_flow) / (BMV + external_flow)` | Use when flows are available for the full day. |
| Timestamped or weighted flow | Flow is weighted by intraday timing. | Use only when source timestamps and methodology support it. |

Required controls:

1. The convention must be fixed by report, portfolio type, or methodology version.
2. Convention changes require a version break or restatement decision.
3. Large flows should create a methodology check where intraday timing could materially change the result.
4. Missing beginning or ending market value should block or degrade the daily return.
5. External and internal flows must be classified before return calculation.

## MWR And IRR Standard

Money-weighted return measures client experience and is sensitive to cashflow timing.

Use MWR or IRR when the business question is about investor outcome, especially:

1. private markets,
2. illiquid sleeves,
3. structured products with defined cashflows,
4. insurance and annuity cashflow streams,
5. portfolios with large client-directed deposits and withdrawals,
6. since-inception client outcome reporting.

Required controls:

1. Use complete dated cashflows and terminal value.
2. Do not publish annualized short-period IRR without a minimum-period policy.
3. Detect no-solution, multiple-solution, non-convergence, and unstable-result cases.
4. If IRR is unavailable, show an explicit degraded state rather than a fabricated result.
5. For private markets, mark IRR partial when historical cashflow history or terminal NAV is incomplete.

## Flow Classification

Performance depends on correct flow classification.

| Flow Type | Performance Treatment |
|---|---|
| External contribution | Adjusts TWR denominator; affects MWR cashflow. |
| External withdrawal | Adjusts TWR denominator; affects MWR cashflow. |
| Buy or sell | Internal investment activity; not an external client flow. |
| Dividend, coupon, interest, rent, distribution | Investment income unless explicitly treated otherwise by methodology. |
| Tax and withholding | Separate from gross income and fees. |
| Management, advisory, custody, and service fee | Net performance effect; methodology must state gross or net treatment. |
| Loan drawdown or repayment | Financing activity; preserve liability and cash movement separately. |
| Margin or collateral movement | Financing or collateral movement; not product exposure by itself. |
| Capital call or private-market distribution | Investment cashflow; impacts IRR and commitment analytics. |
| Correction or reversal | Must link to original event and trigger recomputation policy. |

## Return Decomposition

Performance explanations should separate:

1. price return,
2. income return,
3. FX return,
4. fees,
5. taxes,
6. financing cost,
7. corporate-action effect,
8. cash drag,
9. derivative or hedge effect,
10. residual or unsupported movement.

Residual return is a control signal, not a reporting bucket to hide unexplained movement.

## Contribution And Attribution

| Concept | Meaning | Required Inputs | Failure Mode |
|---|---|---|---|
| Contribution | How much a position, sleeve, product family, or segment added to portfolio return. | Weights, returns, cashflows, classification, FX policy. | Contributions do not add up because of timing, linking, or missing classifications. |
| Attribution | Why return differed from benchmark or target. | Benchmark, asset-class mapping, sector/country/currency factors, returns, weights. | Attribution dimensions are unsupported or stale but shown as complete. |
| Multi-period linking | Converts daily effects into monthly or annual explanations. | Daily contribution or attribution series and linking method. | Simple summation creates mismatch against linked return. |
| Composite contribution | Explains return across many mandates or accounts. | Composite membership, weights, returns, fee schedule, benchmark. | Survivorship bias, entry timing, and fee-class mismatch distort track record. |

## Benchmark Rules

Benchmark reporting should define:

1. benchmark type: single, blended, custom, policy, hedged, unhedged, or peer,
2. currency basis,
3. return type: price, total, gross, net, hedged, or unhedged,
4. rebalancing rule,
5. effective dates,
6. holiday and missing-index treatment,
7. benchmark versioning,
8. benchmark ownership and approval.

Do not compare a portfolio total return to a price-only benchmark without a clear label.

## Leverage And Negative Base Rules

Leveraged portfolios require at least two views:

| View | Base | Purpose |
|---|---|---|
| Asset-only return | Gross assets or investment assets. | Shows performance of the investment assets without financing amplification. |
| Net-equity return | Assets minus liabilities. | Shows the client outcome after leverage and financing. |

Suppress or degrade percentage return when the denominator is near zero, zero, or negative. In those cases, absolute P&L, exposure, and risk explanation are usually more meaningful than a percentage return.

## Fee, Tax, And Net/Gross Rules

Reports must label:

1. gross return before service fees,
2. net return after service fees,
3. transaction-cost treatment,
4. withholding-tax treatment,
5. financing-cost treatment,
6. accrued fee treatment,
7. performance fee treatment,
8. product fee treatment where source-backed.

If fees are charged periodically but measured daily, accrue them daily where methodology requires smooth net performance. Do not let fee posting dates create misleading performance shocks.

## Data Quality And Restatement

Performance should carry data-quality state:

| State | Required Behavior |
|---|---|
| Missing price, NAV, or FX | Block or mark affected returns partial. |
| Stale valuation | Show valuation date and stale state. |
| Missing corporate-action factor | Hold or suppress affected return until resolved. |
| Backdated transaction | Recompute affected periods or record restatement policy. |
| Incorrect flow classification | Reclassify and recompute dependent returns. |
| Locked reporting period | Require controlled restatement or error materiality decision. |
| Outlier return | Trigger diagnostics before publishing. |

## Performance QA Checklist

1. TWR and MWR answer different questions and are labelled separately.
2. External flows do not include dividends, coupons, fees, taxes, buys, sells, or internal transfers.
3. Daily returns reconcile to beginning value, ending value, and classified flows.
4. Multi-period linked return reconciles to daily chain-linked return.
5. Contribution sums or linking residuals are explained.
6. Benchmark currency, return type, and calendar are aligned.
7. Large cashflows and near-zero bases trigger control behavior.
8. Leveraged portfolios show asset-only and net-equity views where required.
9. Gross and net performance labels are explicit.
10. Restatements preserve old value, new value, reason, source, owner, and approval.

## Related Guides

Use this standard together with:

1. [`../../products/product-performance-attribution-risk-guide.md`](../../products/product-performance-attribution-risk-guide.md)
2. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
3. [`../../products/product-lifecycle-cashflow-and-event-guide.md`](../../products/product-lifecycle-cashflow-and-event-guide.md)
4. [`../../products/product-calculation-example-catalog.md`](../../products/product-calculation-example-catalog.md)
5. [`../../products/tax-regulatory-and-reporting.md`](../../products/tax-regulatory-and-reporting.md)

## Disclaimer

This document is for methodology, platform design, analytics, reporting, documentation, and QA work. It is not investment, legal, tax, accounting, regulatory, fiduciary, credit, trading, or client advice.
