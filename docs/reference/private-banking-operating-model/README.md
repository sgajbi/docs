# Private Banking Operating Model Reference

This pack consolidates the useful content from the former `privatebanking-docs` wiki into this documentation repository.

The source wiki contained many short pages with overlapping treatment of performance measurement, advisory workflows, discretionary mandates, suitability, order handling, custody, corporate actions, lending, and operating controls. The material has been migrated as curated standards instead of a raw page dump so the repo stays deduplicated, searchable, and consistent with the current documentation structure.

## Source Provenance

| Source | Details |
|---|---|
| Source repository | `https://github.com/sgajbi/privatebanking-docs.wiki.git` |
| Source branch | `master` |
| Source commit inspected | `983475a6bcda46d5e84bb0db080c86898fef64d7` |
| Source Markdown pages | 131 |
| Migration date | 2026-06-27 |
| Migration approach | Consolidated and rewired into standards; raw duplicate pages were not copied. |

## Reading Order

| File | Use |
|---|---|
| [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md) | Performance methodology, TWR/MWR, cashflow timing, return decomposition, fees, leverage, data quality, and reporting controls. |
| [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md) | Advisory versus discretionary mandate operating model, decision rights, consent, model portfolios, rebalancing, breaches, reviews, and transitions. |
| [`03-suitability-product-governance-standard.md`](03-suitability-product-governance-standard.md) | Client profiling, risk capacity, knowledge and experience, product gates, concentration limits, house views, recommendations, and defensible advice. |
| [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md) | Order management, best execution, trade lifecycle, custody chain, corporate actions, Lombard lending, collateral, margin calls, and controls. |
| [`migration-inventory.md`](migration-inventory.md) | Source wiki coverage, consolidation mapping, duplicate handling, and follow-up enrichment backlog. |

## How To Use This Pack

Use these standards when designing or reviewing:

1. performance reports,
2. portfolio review packs,
3. advisory proposal workflows,
4. DPM mandate workflows,
5. suitability and appropriateness checks,
6. product gating,
7. order and execution controls,
8. custody and corporate-action handling,
9. Lombard lending and collateral monitoring,
10. source data, audit trail, and QA evidence.

This pack should be used with:

1. [`../../products/product-performance-attribution-risk-guide.md`](../../products/product-performance-attribution-risk-guide.md)
2. [`../../products/portfolio-performance-attribution-risk-benchmark/`](../../products/portfolio-performance-attribution-risk-benchmark/README.md)
3. [`../../products/portfolio-construction-and-rebalancing-guide.md`](../../products/portfolio-construction-and-rebalancing-guide.md)
4. [`../../products/model-portfolios-rebalancing-advisory-suitability/`](../../products/model-portfolios-rebalancing-advisory-suitability/README.md)
5. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
6. [`../../products/product-lifecycle-cashflow-and-event-guide.md`](../../products/product-lifecycle-cashflow-and-event-guide.md)
7. [`../../products/advisory-mandate-reporting-decision-guide.md`](../../products/advisory-mandate-reporting-decision-guide.md)
8. [`../../products/product-governance-apu-target-market/`](../../products/product-governance-apu-target-market/README.md)
9. [`../../products/trade-lifecycle-operations/`](../../products/trade-lifecycle-operations/README.md)
10. [`../../products/investment-accounting-ledger/`](../../products/investment-accounting-ledger/README.md)
11. [`../../products/market-data-reference-data-pricing-instrument-master/`](../../products/market-data-reference-data-pricing-instrument-master/README.md)
12. [`../../products/data-governance-quality-lineage-controls/`](../../products/data-governance-quality-lineage-controls/README.md)
13. [`../../products/portfolio-reporting-statement-client-experience/`](../../products/portfolio-reporting-statement-client-experience/README.md)
14. [`../../products/client-account-portfolio-master-data/`](../../products/client-account-portfolio-master-data/README.md)
15. [`../../products/entitlements-digital-channels-advisor-workbench/`](../../products/entitlements-digital-channels-advisor-workbench/README.md)
16. [`../../products/lending-collateral-and-buying-power.md`](../../products/lending-collateral-and-buying-power.md)
17. [`../../products/direct-equities-and-market-operations.md`](../../products/direct-equities-and-market-operations.md)
18. [`../private-banking-platform-knowledge-map.md`](../private-banking-platform-knowledge-map.md)

## Migration Standard

The former wiki content is not copied one page at a time because that would recreate duplicates and make the docs harder to maintain. A topic is considered migrated when:

1. the durable concept is present in one of the curated standards,
2. the source wiki cluster is listed in the migration inventory,
3. overlapping pages are collapsed into one canonical section,
4. links point to current repo-local guides,
5. wording is platform-neutral and private-banking oriented,
6. unsupported implementation claims are framed as requirements, controls, or future capability candidates,
7. validation scans pass for branding leakage, narrow career-only positioning, broken relative links, and Markdown hygiene.

## Disclaimer

This pack is for education, platform design, documentation, analytics, reporting, QA, governance, and operating-model work. It is not investment, legal, tax, regulatory, accounting, fiduciary, credit, trading, or client advice.
