# 06 - Data Model, Calculation Engine and Lineage Design

## 1. Architecture objective

A portfolio analytics platform must produce repeatable, explainable and auditable outputs from changing data.

It must handle:

- Transactions
- Positions
- Cash balances
- Prices and NAVs
- FX rates
- Income events
- Corporate actions
- Product reference data
- Benchmarks
- Mandates
- Look-through holdings
- Risk factors
- Client/entity hierarchy
- Reporting policy
- Calculation versions

## 2. Canonical data flow

```text
Source systems
  +-- Custody / core banking
  +-- Order management
  +-- Product master
  +-- Market data
  +-- Corporate action feeds
  +-- Fund/NAV feeds
  +-- Benchmark providers
  +-- Mandate/suitability systems
  +-- Client/entity systems
        ->
Ingestion and normalization
        ->
Golden data stores
        ->
Valuation and position engine
        ->
Performance, risk, attribution and mandate engines
        ->
Reporting APIs, dashboards, PDF reports, advisor workbench
```

## 3. Core entities

| Entity | Purpose |
|---|---|
| Client | Individual or entity relationship |
| Account | Custody/trading account |
| Portfolio | Reporting/mandate aggregation |
| Mandate | Investment rules and objectives |
| Instrument | Security/product master |
| Position | Current/historical holding |
| Lot | Purchase/tax/cost lot |
| Transaction | Economic movement |
| Cash balance | Currency-level cash position |
| Price | Security/NAV/valuation point |
| FX rate | Currency conversion |
| Income event | Dividend/coupon/distribution |
| Corporate action | Split, merger, spin-off, rights |
| Benchmark | Index/blended/custom benchmark |
| Classification | Asset class/sector/country/currency mapping |
| Risk factor | Factor/curve/spread/volatility input |
| Calculation result | TWR, MWR, risk, attribution output |
| Report snapshot | Frozen client report inputs and outputs |

## 4. Position model

Essential fields:

| Field | Purpose |
|---|---|
| position_id | Unique position record |
| portfolio_id | Portfolio/account context |
| instrument_id | Product/security |
| valuation_date | Date of position |
| quantity | Units/shares/contracts |
| nominal | Bond/note face amount |
| cost_amount | Cost basis |
| market_price | Price/NAV |
| market_value_local | Local currency value |
| market_value_base | Base currency value |
| accrued_income | Accrued coupon/interest |
| currency | Instrument currency |
| asset_class | Reporting category |
| lifecycle_status | Active/matured/defaulted/restricted |
| pledge_status | Free/pledged/restricted |
| liquidity_bucket | Same day/T+2/monthly/locked |
| valuation_source | Exchange/custodian/issuer/model |
| stale_price_flag | Price stale or not |

## 5. Transaction model

Essential fields:

| Field | Purpose |
|---|---|
| transaction_id | Unique transaction |
| transaction_group_id | Link multi-leg transactions |
| portfolio_id/account_id | Context |
| instrument_id | Security/product |
| transaction_type | Buy, sell, dividend, coupon, redemption, fee, etc. |
| trade_date | Investment event date |
| settlement_date | Cash/security settlement date |
| booking_date | Accounting date |
| quantity_delta | Units/shares/contracts change |
| nominal_delta | Face amount change |
| cash_amount | Cash movement |
| cash_currency | Cash currency |
| price | Execution price |
| fees | Fees/commission |
| taxes | Taxes/withholding |
| accrued_interest | Bond accrued interest |
| realized_pnl | P&L on disposal |
| income_amount | Income component |
| external_cashflow_flag | Performance classification |
| performance_treatment | Income/expense/capital/external/internal |
| source_system | Data lineage |
| reversal_reference | Cancel/correct support |

## 6. Classification model

Classifications must be effective-dated.

```text
InstrumentClassification
  instrument_id
  classification_scheme
  classification_value
  effective_from
  effective_to
  source
  confidence
```

Examples:

| Scheme | Value |
|---|---|
| Asset class | Equity |
| Sector | Technology |
| Country | United States |
| Region | North America |
| Currency exposure | USD |
| Issuer group | Apple Inc |
| Product complexity | Complex / non-complex |
| Liquidity bucket | T+2 |
| Risk rating | High |
| Mandate eligibility | Eligible / restricted |

## 7. Market data model

| Field | Purpose |
|---|---|
| instrument_id | Security |
| price_date | Valuation date |
| price | Price/NAV |
| price_type | Close/bid/mid/NAV/issuer/evaluated |
| currency | Price currency |
| source | Vendor/custodian/exchange/issuer |
| timestamp | Received time |
| quality_status | Valid/stale/missing/override |
| valuation_level | Listed/evaluated/model |
| override_reason | Manual adjustment reason |

## 8. FX model

| Field | Purpose |
|---|---|
| base_currency | From currency |
| quote_currency | To currency |
| rate_date | Date |
| rate_type | Spot close/official/custodian/performance |
| rate | FX rate |
| source | Vendor/source |
| timestamp | Lineage |
| quality_status | Valid/stale/missing |

For reproducibility, do not simply call latest FX during report generation. Store the actual FX rate used for calculation.

## 9. Benchmark model

| Entity | Purpose |
|---|---|
| benchmark | Master benchmark |
| benchmark_component | Blended benchmark components |
| benchmark_return | Daily/monthly returns |
| benchmark_constituent | Index constituents if licensed/available |
| portfolio_benchmark_assignment | Effective-dated mapping |
| benchmark_policy | Currency/return type/rebalance methodology |

## 10. Calculation result model

```text
CalculationResult
  result_id
  portfolio_id
  calculation_type
  period_start
  period_end
  base_currency
  method
  policy_id
  version
  result_value
  status
  created_at
  input_snapshot_id
  lineage_reference
```

Calculation types:

- TWR
- MWR
- Contribution
- Attribution
- Volatility
- Drawdown
- VaR
- Exposure
- Concentration
- Mandate breach
- Benchmark-relative return
- Income projection

## 11. Input snapshot and lineage

A production-grade platform should store enough lineage to answer:

- Which transactions were used?
- Which prices were used?
- Which FX rates were used?
- Which classification mapping was used?
- Which benchmark and benchmark version was used?
- Which mandate rules were used?
- Which calculation version was used?
- Was the result restated?
- Which report used this result?

## 12. Calculation engine layers

```text
API / Reporting Layer
        ->
Application Use Cases
        ->
Calculation Orchestrator
        ->
Domain Calculation Engines
  +-- Valuation Engine
  +-- Performance Engine
  +-- Contribution Engine
  +-- Attribution Engine
  +-- Risk Engine
  +-- Benchmark Engine
  +-- Mandate Engine
        ->
Ports / Interfaces
        ->
Repositories / Market Data / Product Data / External Services
```

## 13. Engine separation

| Engine | Responsibility |
|---|---|
| Valuation engine | Market values, accruals, base-currency values |
| Performance engine | TWR, MWR, period returns |
| Contribution engine | Absolute contribution to return |
| Attribution engine | Benchmark-relative active return explanation |
| Benchmark engine | Benchmark returns, blended benchmarks, assignments |
| Risk engine | Volatility, VaR, drawdown, exposures, Greeks |
| Mandate engine | Rule checks, breaches, drift, suitability indicators |
| Reporting engine | Presentation, sections, disclosures and formatting |

## 14. Calculation policy model

Every calculation should reference a policy.

Policy examples:

| Policy dimension | Example |
|---|---|
| Cashflow timing | BOD/EOD |
| Fee basis | Gross/net |
| Tax basis | Pre-tax/net withholding/after-tax |
| Price basis | Close/bid/custodian/model |
| FX basis | Official close/custodian/performance FX |
| Accrual treatment | Include/exclude accrued income |
| Trade date basis | Trade-date versus settlement-date |
| Annualization basis | Calendar days/trading days |
| Classification scheme | Business asset class / regulatory asset class |
| Benchmark basis | Price/gross total/net total return |

## 15. Recalculation dependency graph

Changes and impacts:

| Change | Recalculate |
|---|---|
| Backdated transaction | Positions, performance, contribution, risk, reporting from date |
| Price correction | Valuation, returns, risk from price date |
| FX correction | Base values, returns, exposure from FX date |
| Corporate action | Positions, cost, returns from ex-date |
| Classification change | Allocation, contribution grouping, mandate checks |
| Benchmark change | Relative performance, attribution |
| Mandate rule change | Breach history from effective date |
| Product term update | Valuation/risk/lifecycle for product |
| Client hierarchy change | Aggregated reporting/household analytics |

## 16. Event-driven versus batch architecture

| Approach | Strength | Weakness |
|---|---|---|
| Batch EOD | Stable, controlled, easier reconciliation | Less timely |
| Intraday/event-driven | Real-time advisor insight | More complex consistency/recalculation |
| Hybrid | EOD official + intraday indicative | Requires clear status labels |

A common pattern:

- EOD batch = official performance and reporting
- Intraday = indicative valuation, buying power, risk alerts
- Report snapshot = frozen output

## 17. Idempotency and audit

Calculation jobs must be idempotent.

Required controls:

- Same input snapshot produces same result
- Retry does not duplicate results
- Reversal/correction is explicit
- All overrides require reason and approver
- Calculations have version numbers
- Manual changes are logged
- Reports can be regenerated exactly

## 18. API design

Common APIs:

| API | Purpose |
|---|---|
| `/portfolio/{id}/summary` | Total wealth, allocation, P&L, income |
| `/portfolio/{id}/performance` | TWR/MWR period returns |
| `/portfolio/{id}/contribution` | Contribution by dimension |
| `/portfolio/{id}/attribution` | Benchmark-relative attribution |
| `/portfolio/{id}/risk` | Risk metrics and exposures |
| `/portfolio/{id}/benchmarks` | Benchmark assignments and returns |
| `/portfolio/{id}/mandate-checks` | Rules, drift and breaches |
| `/portfolio/{id}/report` | Client reporting sections |
| `/portfolio/{id}/lineage` | Calculation inputs/audit |

## 19. Performance API considerations

Parameters:

| Parameter | Example |
|---|---|
| period_type | MTD/QTD/YTD/1Y/3Y/SI/custom |
| start_date/end_date | Custom periods |
| currency | USD/SGD/HKD |
| return_method | TWR/MWR |
| basis | gross/net/after-tax |
| include_benchmark | true/false |
| breakdown | daily/monthly/quarterly/asset_class/security |
| classification_scheme | business/regulatory/mandate |
| include_lineage | true/false |

## 20. Expert summary

The analytics data model must preserve both economic truth and reporting interpretation. Positions, transactions, prices and FX are facts. Performance method, fee basis, benchmark choice, classification, mandate rules and presentation sections are policies. Mixing them creates hardcoded logic and inconsistent reporting. A professional design keeps them separate, versioned and traceable.
