# Market Data, Reference Data, Pricing and Instrument Master Design Reference Pack

## Purpose

This pack documents the data foundation behind a private-banking / wealth-management platform. It complements the product packs for notes, bonds, funds, equities, derivatives, private markets, lending, reporting, rebalancing and investment accounting.

The goal is to explain how market data, reference data, pricing, FX, curves, benchmarks, corporate actions, identifiers, vendor feeds, data lineage and data-quality controls should be understood and modelled in a professional wealth platform.

## Source Provenance

| Source | Details |
|---|---|
| Source pack | `C:\Users\Sandeep\Downloads\market_data_reference_data_pricing_instrument_master_reference_pack.zip\market_data_reference_data_pricing_instrument_master_reference_pack` |
| Import date | 2026-06-27 |
| Local treatment | Imported, normalized, cleaned of narrow prep framing, enriched with worked examples, and indexed into the product knowledge library. |

## Target audience

- Product owners, business analysts and architects working on wealth platforms
- Portfolio analytics, advisory, DPM, reporting and performance teams
- Engineering teams building product master, pricing, valuation, risk and reporting services
- QA and operations teams validating data quality, stale pricing, reconciliation and migration
- Senior wealth technology, platform and domain SME reference work

## Recommended reading order

| File | Purpose |
|---|---|
| `01-market-data-reference-data-fundamentals-and-taxonomy.md` | Core concepts, data families and domain vocabulary. |
| `02-instrument-master-identifiers-symbology-and-classification.md` | Instrument master design, ISIN, FIGI, MIC, LEI, CFI, tickers and identifiers. |
| `03-pricing-valuation-hierarchy-fair-value-and-price-quality.md` | Price types, valuation hierarchy, bid/ask/mid, stale price, source priority and overrides. |
| `04-fx-rates-interest-rate-curves-yield-curves-and-benchmarks.md` | FX, rates, curves, benchmark/index data and fixing calendars. |
| `05-corporate-actions-income-events-and-reference-data-lifecycle.md` | Corporate action and income-event data as reference/market data. |
| `06-data-model-source-ownership-golden-copy-and-lineage-design.md` | Canonical data model, source ownership, golden copy, lineage and versioning. |
| `07-product-specific-data-requirements-across-asset-classes.md` | Data requirements by product family. |
| `08-advisory-mandate-analytics-reporting-and-client-experience.md` | How data quality affects advisory, DPM, analytics and reports. |
| `09-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md` | Architecture, QA, reconciliation, controls and glossary. |
| `10-source-notes-and-further-reading.md` | Source notes and further reading. |
| `11-worked-examples-and-implementation-patterns.md` | Practical examples for instrument master golden copy, stale-price hierarchy, FX rate selection, curve inputs, corporate-action revisions and product-specific data requirements. |

## Recommended repo location

```text
docs/products/market-data-reference-data-pricing-instrument-master/
```

## Core design principle

A wealth platform should not treat market data as a passive feed. Market data and reference data are active control surfaces for portfolio valuation, risk, performance, suitability, mandate compliance, buying power, collateral, reporting, tax, accounting and client experience.

Every value shown to an advisor or client should be traceable to:

1. the source feed or manual override,
2. the effective date/time,
3. the instrument, venue and currency context,
4. the transformation rules applied,
5. the quality status,
6. the valuation and reporting policy that consumed it.

## Disclaimer

This material is for education, product analysis, platform design, architecture, documentation and QA work. It is not investment, accounting, legal, tax, regulatory or client advice.
