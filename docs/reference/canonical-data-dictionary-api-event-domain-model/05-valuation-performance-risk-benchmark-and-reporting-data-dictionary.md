# 05 - Valuation, Performance, Risk, Benchmark and Reporting Data Dictionary

## 1. Valuation record

| Field | Type | Description |
|---|---|---|
| valuation_id | string | valuation record |
| portfolio_id | string | portfolio |
| position_id | string | position |
| instrument_id | string | instrument |
| valuation_date | date | valuation date |
| price_id | string | source price |
| price_value | decimal | price used |
| price_currency | ISO currency | price currency |
| fx_rate_id | string | FX used |
| local_market_value | decimal | MV in instrument/local currency |
| base_market_value | decimal | MV in portfolio base currency |
| accrued_income | decimal | accrued amount |
| valuation_method | enum | market_price, evaluated_price, model, nav, cost, estimate |
| fair_value_level | enum | level_1, level_2, level_3, not_applicable |
| stale_flag | boolean | stale input used |
| estimated_flag | boolean | estimate used |
| source_system | string | valuation source |
| calculation_version | string | valuation logic version |

## 2. Performance return

| Field | Type | Description |
|---|---|---|
| performance_id | string | return record |
| portfolio_id | string | portfolio |
| period_start | date | start |
| period_end | date | end |
| return_type | enum | TWR, MWR, simple, benchmark, relative |
| return_value | decimal | return |
| currency | ISO currency | reporting currency |
| gross_net | enum | gross, net |
| cashflow_treatment | string | methodology |
| calculation_version | string | version |
| data_quality_status | enum | complete, partial, degraded |
| source_snapshot_id | string | snapshot used |

## 3. Performance contribution

| Field | Type | Description |
|---|---|---|
| contribution_id | string | contribution record |
| portfolio_id | string | portfolio |
| period_start | date | start |
| period_end | date | end |
| segment_type | enum | asset_class, currency, sector, instrument, region |
| segment_value | string | segment |
| beginning_weight | decimal | starting weight |
| segment_return | decimal | segment return |
| contribution_value | decimal | contribution |
| residual_value | decimal | residual/non-allocable |
| calculation_version | string | version |

## 4. Attribution

| Field | Type | Description |
|---|---|---|
| attribution_id | string | attribution record |
| portfolio_id | string | portfolio |
| benchmark_id | string | benchmark |
| period_start | date | start |
| period_end | date | end |
| segment_type | enum | asset_class, sector, currency |
| allocation_effect | decimal | allocation effect |
| selection_effect | decimal | selection effect |
| interaction_effect | decimal | interaction |
| currency_effect | decimal | currency effect |
| total_effect | decimal | total |
| methodology | string | Brinson, Carino, etc. |
| calculation_version | string | version |

## 5. Risk metric

| Field | Type | Description |
|---|---|---|
| risk_metric_id | string | risk record |
| portfolio_id | string | portfolio |
| as_of_date | date | date |
| metric_type | enum | volatility, sharpe, drawdown, beta, VaR, CVaR, concentration |
| metric_value | decimal | value |
| metric_unit | string | percent, amount, ratio |
| lookback_period | string | e.g. 1Y, 3Y |
| confidence_level | decimal | VaR/CVaR |
| methodology | string | calculation methodology |
| data_quality_status | enum | complete, partial, degraded |
| calculation_version | string | version |

## 6. Exposure

| Field | Type | Description |
|---|---|---|
| exposure_id | string | exposure record |
| portfolio_id | string | portfolio |
| as_of_date | date | date |
| exposure_type | enum | legal, market_value, notional, delta_adjusted, lookthrough, collateral |
| dimension | enum | asset_class, issuer, currency, country, sector, counterparty |
| dimension_value | string | value |
| exposure_amount | decimal | amount |
| exposure_pct | decimal | percentage |
| source | string | source/calculation |
| lookthrough_flag | boolean | look-through used |

## 7. Benchmark

| Field | Type | Description |
|---|---|---|
| benchmark_id | string | benchmark |
| name | string | name |
| benchmark_type | enum | index, blended, custom, model, peer |
| currency | ISO currency | currency |
| owner | string | owner |
| methodology | string | construction methodology |
| effective_from | date | start |
| effective_to | date | end |
| status | enum | active, retired |

## 8. Report snapshot

| Field | Type | Description |
|---|---|---|
| report_snapshot_id | string | snapshot |
| portfolio_id | string | portfolio |
| report_type | enum | statement, portfolio_review, tax, mandate, client_pack |
| as_of_date | date | report date |
| generation_timestamp | datetime | generated |
| data_snapshot_ids | array | linked data snapshots |
| calculation_versions | object | calc versions used |
| template_version | string | report template |
| status | enum | draft, approved, delivered, archived, restated |
| archive_uri | string | report archive location |
| exception_summary | array | report exceptions |

## 9. Validation rules

- valuation must specify price and FX lineage.
- performance must specify period and methodology.
- risk metrics must specify lookback/methodology where relevant.
- report snapshot must be reproducible.
- benchmark composition must be effective-dated.
- degraded analytics must be flagged.
- AI-generated explanations must reference calculation outputs.

## 10. Key takeaway

Analytics and reporting data must be versioned, sourced and reproducible.
