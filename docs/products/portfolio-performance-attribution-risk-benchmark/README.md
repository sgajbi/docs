# Portfolio Performance, Attribution, Risk, Benchmarks and Mandate Analytics Reference Pack

## Purpose

This pack is the cross-product analytics layer for a wealth management, private banking, advisory, discretionary portfolio management (DPM), portfolio reporting and platform architecture knowledge base.

It is designed to complement the earlier product packs covering:

- Notes and structured notes
- Bonds and fixed income
- Funds, ETFs and unit trusts
- Equities and corporate actions
- Derivatives
- Structured products
- Private markets and alternatives
- Cash, deposits, money market and FX
- Loans, Lombard lending, margin and collateral
- Insurance, annuities and protection-linked products
- Commodities, precious metals, REITs, infrastructure and real assets
- Trusts, estate planning, family office and wealth structures
- Tax-aware and regulatory reporting

The purpose is to explain how those product-level transactions, positions, valuations and cashflows become portfolio-level analytics, client reporting, advisory insight and mandate monitoring.

## Intended audience

- Wealth platform architects
- Portfolio analytics engineers
- Performance calculation teams
- Business analysts and product owners
- Advisory and DPM teams
- Client reporting teams
- QA and reconciliation teams
- Risk, suitability and mandate-control teams
- Migration and data-conversion teams
- Durable practitioner reference for wealth technology and portfolio-management SME work

## How to use this pack

Read the files in order if you are studying. Use the individual files as reference if you are designing or reviewing a platform.

| File | Purpose |
|---|---|
| `01-performance-analytics-fundamentals-and-taxonomy.md` | Analytics landscape and vocabulary |
| `02-portfolio-performance-twr-mwr-cashflows-and-edge-cases.md` | TWR, MWR, cashflow classification and edge cases |
| `03-contribution-attribution-brinson-carino-currency-and-multi-asset.md` | Return contribution, attribution, smoothing and multi-asset attribution |
| `04-benchmarks-composites-mandates-and-relative-analytics.md` | Benchmarks, composites, relative performance and mandate analytics |
| `05-risk-analytics-exposure-concentration-and-stress-testing.md` | Risk, exposure, concentration and stress analytics |
| `06-data-model-calculation-engine-and-lineage-design.md` | Data model, calculation engine, lineage and architecture |
| `07-product-specific-analytics-treatment-across-asset-classes.md` | Product-specific performance and reporting treatment |
| `08-client-reporting-advisory-dpm-and-mandate-deep-dive.md` | Advisory, DPM, reporting and client experience |
| `09-platform-controls-reconciliation-test-scenarios-and-glossary.md` | Controls, reconciliation, QA scenarios and glossary |
| `10-source-notes-and-further-reading.md` | Standards, references and source notes |
| `11-worked-examples-and-implementation-patterns.md` | Numeric examples for TWR, MWR, contribution, attribution, currency, benchmarks, risk, composites, degraded states and QA |

## Core mental model

A portfolio analytics platform converts raw investment facts into explainable client and advisor insight.

```text
Transactions + positions + prices + FX + reference data + mandates
        ->
Valuation, cashflow classification, income, P&L, exposure
        ->
Performance, attribution, risk, benchmark-relative analytics
        ->
Client reporting, advisory dashboards, suitability, mandate monitoring
```

The analytics layer must be:

- Correct at transaction level
- Consistent across products
- Reproducible across reporting dates
- Explainable to advisors and clients
- Auditable for operations and regulators
- Configurable by jurisdiction, mandate, product and client segment
- Robust to late prices, backdated transactions, corporate actions and restatements

## Important principle

Do not treat performance, attribution and risk as independent calculations. They depend on the same foundation:

- Accurate holdings
- Correct transaction classification
- Reliable valuations
- Consistent FX rates
- Proper income treatment
- Benchmark alignment
- Mandate definitions
- Strong lineage and versioning

If the foundation is wrong, the analytics will be precise but incorrect.
