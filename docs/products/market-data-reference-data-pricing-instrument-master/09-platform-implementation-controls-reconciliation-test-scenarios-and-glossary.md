# 09 - Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Recommended service boundaries

| Service | Responsibility |
|---|---|
| instrument-master-service | canonical instruments, identifiers, classifications |
| entity-master-service | issuers, counterparties, LEIs, hierarchy |
| market-data-ingestion-service | raw feed intake and validation |
| pricing-service | golden price selection, hierarchy, stale checks, overrides |
| fx-service | FX rates, cross rates, conversion policies |
| curve-service | curves, benchmark fixings, discount factors |
| benchmark-service | index levels, weights, returns, benchmark mapping |
| corporate-action-service | event capture, entitlements, elections, lifecycle booking |
| data-quality-service | checks, exceptions, dashboards |
| snapshot-service | freeze calculation/report inputs |

## 2. API capabilities

Useful APIs:

- search instrument,
- get instrument by ID,
- map identifier to instrument,
- get price for valuation purpose,
- get FX conversion rate,
- get curve as of date,
- get benchmark return series,
- get corporate action events,
- get data quality exceptions,
- create manual override,
- approve/reject override,
- get valuation snapshot lineage.

## 3. Reconciliation controls

| Reconciliation | Example |
|---|---|
| instrument reconciliation | custodian vs product master |
| price reconciliation | vendor A vs vendor B |
| NAV reconciliation | fund admin vs custodian |
| FX reconciliation | treasury rate vs market data vendor |
| corporate action reconciliation | expected entitlement vs received cash/security |
| classification reconciliation | product governance vs reporting hierarchy |
| benchmark reconciliation | provider return vs recomputed return |
| portfolio valuation reconciliation | platform vs custodian statement |

## 4. Data quality checks

| Check | Assertion |
|---|---|
| mandatory field | required fields present by product type |
| identifier uniqueness | no duplicate active instrument for same identifier/context |
| currency validity | ISO currency exists and minor units valid |
| price reasonableness | price movement within tolerance or exception raised |
| stale data | valuation date within policy threshold |
| cross-source variance | difference within allowed tolerance |
| corporate action impact | split/dividend/merger bookings match terms |
| benchmark continuity | index return matches level change |
| curve completeness | required tenors present |
| source priority | golden copy selected by active policy |

## 5. Test scenarios

### Instrument master

1. Create equity with ISIN, ticker, MIC and sector.
2. Map same ISIN to two listings with different MICs.
3. Load bond with missing maturity and verify trade/report block.
4. Load fund share class with distribution policy and NAV currency.
5. Load structured note with multiple underlyings and observation schedule.
6. Deactivate delisted instrument and keep historical positions reportable.

### Pricing

1. Use exchange close for listed equity.
2. Use fund admin NAV for fund valuation.
3. Use evaluated price for bond.
4. Use issuer bid for structured note.
5. Reject price with 100x currency error.
6. Carry prior price with stale flag when policy permits.
7. Apply approved manual override with audit evidence.

### FX and curves

1. Convert USD portfolio to SGD base currency.
2. Invert SGD/USD to USD/SGD correctly.
3. Triangulate minor currency through USD.
4. Fail valuation when required FX rate is missing.
5. Build curve with missing tenor and mark incomplete.
6. Recalculate floating coupon after rate fixing update.

### Corporate actions

1. Apply 2-for-1 split and preserve total market value before price movement.
2. Book cash dividend with withholding tax.
3. Process rights entitlement and exercise.
4. Process merger with cash and stock consideration.
5. Apply spin-off with cost-basis allocation.
6. Reverse wrongly booked event and reprocess.

### Reporting and analytics

1. Generate report with stale private market NAV label.
2. Suppress benchmark-relative analytics when benchmark data missing.
3. Recompute performance after backdated corporate action.
4. Show data-quality summary before report approval.
5. Reconcile portfolio valuation to custodian with price breaks.

## 6. Production dashboards

Useful dashboards:

- missing critical instrument data,
- stale price/NAV by asset class,
- price override queue,
- largest price moves,
- cross-source price variances,
- missing FX rates,
- curve construction failures,
- corporate action pending elections,
- failed entitlement bookings,
- benchmark data missing,
- reports generated with degraded data.

## 7. Glossary

| Term | Meaning |
|---|---|
| Golden copy | authoritative selected value for a given purpose/context |
| Stale price | price older than policy threshold |
| Evaluated price | valuation provided by model/vendor rather than direct trade price |
| MIC | Market Identifier Code for trading venue/market |
| LEI | Legal Entity Identifier for legal entities |
| ISIN | International Securities Identification Number |
| FIGI | Financial Instrument Global Identifier |
| NAV | Net Asset Value for funds/vehicles |
| Curve | set of rates/discount factors by tenor |
| Fixing | official rate/level used for settlement or reset |
| Corporate action | issuer event affecting securities/cash/rights |
| Entitlement | amount of cash/security due to holder from event |
| Lineage | trace from output value back to inputs/source/rules |
| Override | manually approved replacement value |
| Quality status | classification of data usability |

## 8. Implementation principles

- Use Decimal for financial amounts and rates where precision matters.
- Store raw and normalized data separately.
- Make source, timestamp and quality status mandatory.
- Do not overwrite history without versioning.
- Keep calculation snapshots immutable.
- Make degraded states explicit.
- Build data QA into pipelines, not as a separate afterthought.
- Preserve audit evidence for manual changes.
