# 06 - Data Model, Source Ownership, Golden Copy and Lineage Design

## 1. Core data architecture

A production wealth platform should separate raw data, normalized data, golden copy and derived data.

```text
raw_market_data_event
  -> normalized_market_data_record
    -> golden_market_data_record
      -> valuation_snapshot / analytics_input / reporting_output
```

Each layer has a different purpose.

| Layer | Purpose |
|---|---|
| Raw | preserve source payload exactly as received |
| Normalized | map into platform schema and units |
| Golden copy | select authoritative value based on hierarchy |
| Derived | compute analytics-ready or report-ready values |
| Snapshot | freeze values used by a calculation/report |

## 2. Golden copy principles

Golden copy does not mean "one source for everything". It means there is one authoritative value for a given purpose, date and context.

Example:

| Purpose | Golden source |
|---|---|
| equity valuation | exchange official close |
| collateral valuation | conservative bid/haircut value |
| bond reporting | evaluated mid price |
| bond liquidation estimate | bid price |
| fund valuation | official NAV from fund admin |
| structured note valuation | issuer bid or independent valuation source |
| accounting fair value | valuation policy hierarchy |

## 3. Source ownership matrix

| Data domain | Typical source owner | Platform owner |
|---|---|---|
| ISIN/security identifier | NNA/vendor/custodian | product master team |
| exchange prices | exchange/vendor | market data team |
| fund NAV | fund administrator/vendor | fund data team |
| bond prices | evaluated pricing vendor/dealer | pricing team |
| structured note quotes | issuer/vendor | structured product data team |
| FX rates | market data vendor/treasury | FX data service |
| curves | vendor/internal valuation | risk/valuation service |
| corporate actions | custodian/vendor | asset servicing team |
| ratings | rating agencies/vendor | credit/reference data team |
| product risk rating | product governance | APU/product governance platform |
| collateral haircut | credit risk/lending | buying power engine |

## 4. Lineage model

Every downstream value should be traceable.

```text
value_id
  value_type
  value
  instrument_id
  valuation_date
  source_id
  raw_event_id
  transformation_rule_version
  quality_status
  selected_by_policy
  selected_at
  consumed_by_calculation_id
```

## 5. Effective dating and bitemporality

Reference data needs two timelines:

| Timeline | Meaning |
|---|---|
| effective time | when the data is valid in business reality |
| system/knowledge time | when the platform knew or stored it |

Example: a bond rating downgrade effective on 15 June is received on 18 June. Risk and reporting may need to know both dates.

## 6. Data versioning

Version the following carefully:

- product terms,
- classification mappings,
- source priority rules,
- valuation methodology,
- benchmark methodology,
- FX conversion rules,
- corporate action terms,
- price override policies,
- manual mapping decisions.

## 7. Canonical market data schemas

### Price record

```json
{
  "instrumentId": "SEC123",
  "valuationDate": "2026-06-26",
  "priceType": "CLOSE",
  "quoteBasis": "PRICE_PER_UNIT",
  "price": "125.43",
  "currency": "USD",
  "source": "EXCHANGE_VENDOR",
  "marketIdentifierCode": "XNAS",
  "timestamp": "2026-06-26T16:15:00-04:00",
  "qualityStatus": "VALID",
  "selectionPolicy": "EQUITY_EOD_OFFICIAL_CLOSE_V1"
}
```

### FX record

```json
{
  "fromCurrency": "USD",
  "toCurrency": "SGD",
  "rate": "1.3500",
  "valuationDate": "2026-06-26",
  "rateType": "EOD_CLOSE",
  "source": "FX_VENDOR",
  "directInvertedTriangulated": "DIRECT",
  "qualityStatus": "VALID"
}
```

### Instrument classification record

```json
{
  "instrumentId": "SEC123",
  "classificationScheme": "WEALTH_ASSET_CLASS",
  "classificationValue": "GLOBAL_EQUITY",
  "effectiveFrom": "2026-01-01",
  "source": "PRODUCT_GOVERNANCE",
  "status": "ACTIVE"
}
```

## 8. Data quality status vocabulary

| Status | Meaning |
|---|---|
| VALID | usable without exception |
| MISSING | required value absent |
| STALE | value older than policy threshold |
| SUSPECT | validation warning |
| OVERRIDDEN | manual approved value used |
| ESTIMATED | calculated/estimated value used |
| PROVISIONAL | not yet final |
| REJECTED | failed validation |
| BLOCKED | downstream consumption prohibited |

## 9. Integration patterns

| Pattern | Use |
|---|---|
| batch EOD file | closing prices, NAVs, reference data |
| streaming feed | intraday prices, order/risk checks |
| API lookup | instrument enrichment, on-demand security creation |
| event-driven update | corporate actions, price corrections, rating changes |
| manual workflow | exceptional missing data / overrides |
| snapshot service | freeze valuation inputs for reports/calculations |

## 10. Design rule

Do not allow consuming services to silently invent missing data. They should either consume an approved fallback value with visible quality status or fail with a clear exception and remediation workflow.
