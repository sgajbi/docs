# 06 - Portfolio Analytics Architecture, Calculation Services and Workflows

## 1. Analytics architecture goal

Portfolio analytics turns holdings, transactions, cashflows, prices, FX, benchmarks, product attributes and mandate rules into meaningful outputs:

- valuation
- allocations
- P&L
- income
- TWR/MWR performance
- contribution
- attribution
- benchmark-relative analytics
- risk metrics
- exposure and concentration
- mandate checks
- portfolio review sections
- advisor explanations

The architecture must make these outputs correct, reproducible and explainable.

## 2. Calculation engine principles

| Principle | Meaning |
|---|---|
| Deterministic | Same inputs and method version produce same output. |
| Versioned | Formula/method changes are tracked and reportable. |
| Explainable | Result can be decomposed into input drivers. |
| Recalculable | Backdated changes can trigger controlled recalculation. |
| Auditable | Input snapshots and calculation metadata are retained. |
| Product-aware | Different asset classes have different valuation, income and exposure behavior. |
| Degraded-state aware | Missing prices, FX or product data should produce controlled exceptions. |
| Scalable | Large portfolios and historical periods should not require synchronous UI waits. |

## 3. Analytics domains

| Domain | Inputs | Outputs |
|---|---|---|
| Valuation | Positions, prices, FX, accruals | Market value, accrued income, valuation flags |
| Allocation | Valued holdings, taxonomy | Asset class, currency, geography, sector, issuer allocation |
| P&L | Cost basis, market value, transactions, FX | Realized/unrealized P&L, FX P&L |
| Performance | Valuation time series, cashflows | TWR, MWR, period returns, cumulative returns |
| Contribution | Returns and weights | Contribution by asset class/security/currency |
| Attribution | Portfolio/benchmark weights and returns | Allocation, selection, interaction, currency effects |
| Risk | Returns, exposures, prices, sensitivities | Volatility, drawdown, beta, VaR, stress, concentration |
| Mandate | Holdings, exposures, rules | Pass/fail, breach details, drift, restriction status |
| Reporting | All analytics outputs | Portfolio review, statements, dashboards, narratives |

## 4. Layered calculation architecture

```text
Input data layer
  |-- Transactions
  |-- Positions
  |-- Cashflows
  |-- Prices / FX / NAV / Curves
  |-- Product and benchmark data
  `-- Client / mandate / reporting configuration

Curation and snapshot layer
  |-- Data-quality validation
  |-- Product classification enrichment
  |-- Look-through enrichment
  |-- FX normalization
  `-- Input snapshot creation

Calculation layer
  |-- Valuation service
  |-- Allocation service
  |-- Performance service
  |-- Attribution service
  |-- Risk service
  |-- Concentration service
  |-- Mandate service
  `-- Reporting aggregation service

Output layer
  |-- APIs
  |-- Events
  |-- Reporting snapshots
  |-- Advisor dashboards
  |-- Client statements
  `-- Exception dashboards
```

## 5. Synchronous vs asynchronous calculations

| Calculation | Typical mode | Reason |
|---|---|---|
| Current allocation summary | Synchronous or cached | Needed for UI. |
| Current holdings valuation | Synchronous if cached, async if heavy | Depends on size and price dependencies. |
| Historical TWR for long periods | Asynchronous or precomputed | Requires time series and cashflow linking. |
| Attribution | Asynchronous/precomputed | Benchmark and decomposition heavy. |
| Risk scenarios/stress | Asynchronous | Computationally expensive. |
| Portfolio review PDF | Asynchronous | Requires snapshot, charts, rendering and archive. |
| Mandate pre-trade check | Synchronous | Needed before order placement. |
| Post-trade mandate monitoring | Event-driven/asynchronous | Triggered by positions/prices/transactions. |

## 6. Input snapshot design

Analytics should run on explicit input snapshots:

```text
calculation_input_snapshot
  snapshot_id
  portfolio_id
  as_of_date
  valuation_basis
  transaction_set_version
  position_set_version
  price_set_version
  fx_set_version
  product_taxonomy_version
  benchmark_version
  mandate_rule_version
  created_at
```

This protects official results from changing when upstream data is corrected later.

## 7. Recalculation workflow

Backdated data changes require controlled recalculation.

Examples:

- late transaction
- corrected quantity
- corrected price
- restated fund NAV
- benchmark correction
- corporate-action adjustment
- cost-basis correction
- product classification change

Recommended workflow:

```text
Data correction event
  -> impact analysis
  -> affected portfolio/period detection
  -> recalculation job creation
  -> recalculation execution
  -> result comparison
  -> control approval if material
  -> publish new result version
  -> report impact notification
```

## 8. Calculation job model

| Field | Purpose |
|---|---|
| jobId | Unique job. |
| jobType | Valuation, performance, risk, report, mandate. |
| portfolioScope | Portfolio/account/client group. |
| period | Calculation period. |
| inputSnapshotId | Frozen inputs. |
| calculationVersion | Method version. |
| status | Pending, running, completed, failed, cancelled. |
| triggerReason | EOD, user request, data correction, migration, report request. |
| priority | Operational ordering. |
| resultId | Output link. |
| lineage | Source and dependencies. |
| errorCode | Failure classification. |

## 9. Product-specific analytics architecture

| Product | Special architecture requirement |
|---|---|
| Bonds | Clean/dirty price, accrued interest, duration, yield, credit spread. |
| Structured notes | Issuer valuation, barrier/observation state, payoff scenarios, look-through exposure. |
| Funds | NAV lag, distribution treatment, share classes, look-through if available. |
| Equities | Corporate actions, dividends, splits, multi-listing, ADR. |
| Derivatives | Notional, delta exposure, Greeks, margin/collateral, expiry. |
| Private markets | Commitments, unfunded, NAV lag, IRR, capital calls/distributions. |
| Lending/collateral | LTV, haircuts, pledged values, exposure, buying power. |
| Insurance | Policy value, surrender value, premiums, guarantees, riders. |
| Real estate/REITs | Appraisal NAV, leverage, distributions, liquidity. |
| Cash/FX | Cash basis, FX translation, value date, settlement risk. |

## 10. Mandate analytics architecture

Mandate checks should run at multiple points:

| Stage | Check type |
|---|---|
| Proposal | Suitability, target allocation, product eligibility, client restrictions. |
| Pre-trade | Cash/buying power, concentration, product restrictions, leverage, asset-class bands. |
| Post-trade | Actual exposure and drift after execution. |
| EOD | Full mandate monitoring using official positions/prices. |
| Reporting | Mandate status, breaches, explanation, remediation. |

Mandate rules should be versioned, effective-dated and explainable.

## 11. Analytics API design

Example API set:

```text
GET /portfolios/{id}/valuation-summary
GET /portfolios/{id}/allocation-summary
GET /portfolios/{id}/performance?period=YTD&method=TWR
GET /portfolios/{id}/performance-breakdown?dimension=assetClass
GET /portfolios/{id}/attribution?benchmarkId=BM1&period=YTD
GET /portfolios/{id}/risk-summary
GET /portfolios/{id}/concentration?dimension=issuer
POST /portfolios/{id}/mandate-checks
POST /analytics/recalculation-jobs
GET /analytics/jobs/{jobId}
```

Responses should include method version and data-quality status.

## 12. Analytics event design

Useful events:

```text
ValuationCalculated
PerformanceCalculated
AttributionCalculated
RiskCalculated
MandateBreachDetected
MandateBreachResolved
RecalculationRequested
RecalculationCompleted
ReportInputSnapshotCreated
```

## 13. Explainability model

Analytics should provide drill-down:

| Output | Explainability |
|---|---|
| Return | Opening MV, closing MV, cashflows, income, fees, FX, sub-periods. |
| Contribution | Weight, return, contribution by dimension. |
| Attribution | Portfolio weight, benchmark weight, allocation effect, selection effect, residual. |
| Risk | Input returns, exposure basis, confidence, lookback, methodology. |
| Mandate breach | Rule, threshold, actual value, breached holdings, remediation suggestion. |
| Report exception | Missing input, stale source, block/warning reason. |

## 14. Calculation quality controls

- Input completeness check.
- Price and FX freshness check.
- Cashflow classification check.
- Outlier return detection.
- Negative/zero market value handling.
- Corporate-action adjustment validation.
- Benchmark availability check.
- Reconciliation to accounting book of record.
- Golden test portfolios.
- Regression test pack for known edge cases.
- Materiality thresholds for report restatement.

## 15. Architecture anti-patterns

| Anti-pattern | Problem |
|---|---|
| Calculation logic inside UI | Inconsistent results and no lineage. |
| Report engine recalculates ad hoc | Non-reproducible statements. |
| No calculation version | Cannot explain historical changes. |
| Performance uses mutable current positions | Incorrect historical results. |
| No degraded-data state | Misleading output. |
| Every product handled generically only | Asset-class-specific risks are hidden. |
| Manual spreadsheet overrides outside platform | Audit and reconciliation failure. |

## 16. Analytics architecture checklist

- Inputs are versioned and snapshot-able.
- Calculations are method-versioned.
- Results are traceable to input snapshots.
- Product-specific treatments are explicit.
- Degraded-data rules are defined.
- Recalculation workflow is implemented.
- Outputs are API/event/report ready.
- QA includes golden portfolios and edge cases.
- Mandate and advisory uses are supported.
- Performance and reporting outputs are reproducible.
