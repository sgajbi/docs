# Commodities, Precious Metals and Real Assets Trading Products Reference Pack

## Purpose

This pack is a practitioner-oriented knowledge base for studying, explaining, designing, implementing and supporting commodities and real-asset-linked wealth products.

It is written for wealth management, private banking, advisory, discretionary portfolio management, portfolio analytics, client reporting, product control, operations, QA and technology delivery teams.

The focus is not only product definition. The pack explains how commodities are represented in portfolios, how their lifecycle differs by wrapper, how positions and transactions should be modelled, how valuation and risk analytics work, and how advisors and mandate teams should think about suitability, diversification, liquidity, concentration and reporting.

## Product scope

This pack covers:

- Physical precious metals: gold, silver, platinum and palladium bars/coins.
- Precious metal accounts: allocated and unallocated metal accounts.
- Commodity-linked exchange-traded products: ETFs, ETCs, ETNs, certificates and commodity index trackers.
- Commodity derivatives: futures, options, forwards, swaps, NDF-like contracts, commodity-linked structured products.
- Energy products: crude oil, refined products, natural gas, power and related derivatives.
- Industrial metals: copper, aluminium, nickel, zinc, iron ore, steel-related contracts.
- Agricultural commodities: grains, soft commodities, livestock and related futures/ETPs.
- Real-asset trading exposures: gold as collateral, warehouse receipts, commodity equities, resource funds, REIT/real-asset adjacent exposures.
- Advisory, mandate, analytics, reporting and platform implementation considerations.

## Files

1. [01-commodities-fundamentals-taxonomy-and-wealth-context.md](01-commodities-fundamentals-taxonomy-and-wealth-context.md)
2. [02-precious-metals-bullion-accounts-custody-and-collateral.md](02-precious-metals-bullion-accounts-custody-and-collateral.md)
3. [03-energy-industrial-metals-agriculture-and-real-asset-exposures.md](03-energy-industrial-metals-agriculture-and-real-asset-exposures.md)
4. [04-commodity-derivatives-etps-structured-products-and-product-combinations.md](04-commodity-derivatives-etps-structured-products-and-product-combinations.md)
5. [05-lifecycle-transactions-position-and-event-modelling.md](05-lifecycle-transactions-position-and-event-modelling.md)
6. [06-data-model-valuation-performance-risk-and-analytics.md](06-data-model-valuation-performance-risk-and-analytics.md)
7. [07-advisory-mandate-suitability-and-client-reporting-deep-dive.md](07-advisory-mandate-suitability-and-client-reporting-deep-dive.md)
8. [08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md](08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md)
9. [09-source-notes-and-further-reading.md](09-source-notes-and-further-reading.md)
10. [10-worked-examples-and-implementation-patterns.md](10-worked-examples-and-implementation-patterns.md)

## How to use this pack

For business study, start with files 01, 02, 03 and 04.

For platform design, focus on files 05, 06 and 08.

For advisory, DPM mandates, portfolio analytics and client reporting, focus on files 06 and 07.

For practitioner review, read files 01, 04, 06 and 07, then use the glossary in file 08.

For worked examples, implementation boundaries and QA scenarios, use file 10.

## Design principle

Do not model “commodity exposure” as one uniform product. A client can access the same commodity through very different wrappers, each with different accounting, valuation, lifecycle and risk behavior.

Example: gold exposure can appear as:

- a physical gold bar held in custody;
- an allocated gold account;
- an unallocated precious metal account;
- a gold ETF;
- a gold ETC or ETN;
- a gold futures position;
- a gold option;
- a commodity-linked structured note;
- a gold mining equity or fund.

The underlying economic exposure may be similar, but the position, transaction, valuation, liquidity, credit risk and reporting treatment can be very different.

## Source provenance

Imported from C:\Users\Sandeep\Downloads\commodities_precious_metals_real_assets_reference_pack.zip\commodities_precious_metals_real_assets_reference_pack on 2026-06-27. The imported material was normalized for reusable practitioner framing.
