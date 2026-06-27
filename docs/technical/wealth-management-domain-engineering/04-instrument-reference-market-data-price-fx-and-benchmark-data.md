# Instrument, Reference, Market Data, Price, FX, and Benchmark Data

## Purpose

This file explains reference and market data concepts used by wealth platforms.

Analytics and reporting are only as good as their instrument, price, FX, and benchmark inputs.

---

## Instrument Master

Instrument data may include:

- instrument id
- ISIN / CUSIP / ticker
- asset class
- product type
- currency
- issuer
- sector
- country
- rating
- maturity
- coupon
- risk classification
- eligibility flags
- pricing source

---

## Price Data

Price data includes:

- instrument id
- price
- price date/time
- price currency
- source
- quality status
- stale indicator
- vendor/source id
- correction status

---

## FX Data

FX data includes:

- currency pair
- rate
- rate date/time
- source
- quote convention
- base/terms currency
- quality status
- stale indicator

FX is essential for portfolio currency, reporting currency, and cross-currency analytics.

---

## Benchmark Data

Benchmark data may include:

- benchmark id
- benchmark name
- benchmark currency
- constituent weights
- constituent returns
- rebalancing schedule
- effective dates
- linked portfolio/mandate
- source
- methodology

---

## Reference Data Governance

Reference data should define:

- owner
- source system
- update frequency
- quality checks
- lineage
- correction process
- consumer contracts
- fallback behavior

---

## Common Issues

- stale price
- missing price
- missing FX rate
- wrong currency
- instrument classification mismatch
- benchmark weights not effective-dated
- index return missing
- rating/sector/country inconsistency
- duplicate instrument identifiers

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Instrument classification hardcoded in UI | Inconsistent analytics. |
| Missing FX silently defaults to 1 | Incorrect valuation. |
| Benchmark composition not effective-dated | Wrong relative performance. |
| Price freshness hidden | Misleading reports. |
| Multiple services own instrument truth | Semantic drift. |
| Market data not lineage-backed | Difficult to explain outputs. |

---

## Review Checklist

- Who owns instrument master?
- Are prices source-owned?
- Are FX rates source-owned?
- Is freshness visible?
- Are benchmark weights effective-dated?
- Are missing/stale inputs represented?
- Are classifications canonical?
- Is lineage available?
- Are data-quality rules tested?

---

## Summary

Reference and market data are platform foundations.

They must be governed, lineage-backed, and quality-aware.
