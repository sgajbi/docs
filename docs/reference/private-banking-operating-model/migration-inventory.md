# Private Banking Wiki Migration Inventory

This inventory records how the former `privatebanking-docs` wiki was migrated into this repo.

## Source Summary

| Field | Value |
|---|---|
| Source repository | `https://github.com/sgajbi/privatebanking-docs.wiki.git` |
| Source commit | `983475a6bcda46d5e84bb0db080c86898fef64d7` |
| Source Markdown pages inspected | 131 |
| Migration date | 2026-06-27 |
| Import decision | Consolidate durable knowledge into curated standards; do not copy raw duplicate wiki pages. |

## Consolidation Map

| Source Cluster | Source Page Examples | Destination |
|---|---|---|
| Performance fundamentals | `2` through `37`, `38` through `61`, `200` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md) and existing product performance guides. |
| TWR and cashflow timing | `5`, `11`, `13`, `17`, `29`, `53`, `200` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md). |
| MWR, XIRR, private-market outcome returns | `6`, `14`, `34`, `54`, `200` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md) and [`../../products/product-performance-attribution-risk-guide.md`](../../products/product-performance-attribution-risk-guide.md). |
| Contribution and attribution | `7`, `8`, `24`, `25`, `47`, `48`, `130` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md). |
| Benchmarking and composite reporting | `20`, `39`, `50`, `123` through `130` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md). |
| Leverage, negative cash, derivatives margin | `27`, `32`, `41`, `42`, `49`, `82 financing` | [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md), [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md), and [`../../products/lending-collateral-and-buying-power.md`](../../products/lending-collateral-and-buying-power.md). |
| Advisory operating model | `62` through `65`, `72` through `110`, `201` | [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md) and [`03-suitability-product-governance-standard.md`](03-suitability-product-governance-standard.md). |
| DPM operating model | `66` through `81`, `111` through `122`, `201` | [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md). |
| Suitability, profiling, restrictions, product governance | `63`, `74`, `82 product governance`, `83`, `91`, `98`, `99`, `202` | [`03-suitability-product-governance-standard.md`](03-suitability-product-governance-standard.md). |
| Research, house view, advisory documentation, consent | `84` through `88`, `95` through `100`, `106` | [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md) and [`03-suitability-product-governance-standard.md`](03-suitability-product-governance-standard.md). |
| Cross-currency, hedging, themes, risk budgets | `101`, `103`, `109`, `110` | [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md), [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md), and product-specific FX/derivatives guides. |
| Lombard lending and credit risk | `203`, `82 financing` | [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md) and [`../../products/lending-collateral-and-buying-power.md`](../../products/lending-collateral-and-buying-power.md). |
| Custody, corporate actions, asset servicing | `21`, `43`, `204` | [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md), [`../../products/direct-equities-and-market-operations.md`](../../products/direct-equities-and-market-operations.md), and [`../../products/product-lifecycle-cashflow-and-event-guide.md`](../../products/product-lifecycle-cashflow-and-event-guide.md). |
| Order management and best execution | `88`, `205` | [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md). |

## Duplicate Handling

The following source patterns were intentionally consolidated rather than imported as separate pages:

1. Multiple TWR pages repeating the same daily chain-linking, cashflow timing, and edge-case rules.
2. Multiple return decomposition pages repeating income, capital, FX, fee, tax, and residual explanations.
3. Multiple advisory lifecycle pages repeating idea, proposal, suitability, consent, execution, and monitoring flow.
4. Multiple DPM pages repeating mandate onboarding, model governance, rebalancing, breach, review, and transition logic.
5. Multiple product-governance pages repeating risk rating, complexity, restrictions, K&E, and concentration controls.
6. Multiple corporate-action/performance pages now handled by the lifecycle and execution/custody standards.

## Current Canonical Destinations

Use these docs instead of the former wiki pages:

1. [`README.md`](README.md)
2. [`01-performance-measurement-standard.md`](01-performance-measurement-standard.md)
3. [`02-advisory-and-dpm-operating-model.md`](02-advisory-and-dpm-operating-model.md)
4. [`03-suitability-product-governance-standard.md`](03-suitability-product-governance-standard.md)
5. [`04-execution-custody-lending-controls.md`](04-execution-custody-lending-controls.md)
6. [`../../products/product-performance-attribution-risk-guide.md`](../../products/product-performance-attribution-risk-guide.md)
7. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
8. [`../../products/advisory-mandate-reporting-decision-guide.md`](../../products/advisory-mandate-reporting-decision-guide.md)
9. [`../../products/client-reporting-and-portfolio-review-guide.md`](../../products/client-reporting-and-portfolio-review-guide.md)
10. [`../../products/lending-collateral-and-buying-power.md`](../../products/lending-collateral-and-buying-power.md)

## Follow-Up Backlog

Useful future enrichment:

1. Add numeric worked examples for TWR, MWR, contribution linking, and composite dispersion.
2. Add advisory proposal templates connected to suitability, concentration, product complexity, and consent evidence.
3. Add DPM model-portfolio versioning examples and breach lifecycle examples.
4. Add order-management examples for market, limit, stop, stop-limit, OTC quote, and partial-fill behavior.
5. Add corporate-action worked examples for rights issue, merger, spin-off, cash-in-lieu, and manufactured dividend.
6. Add Lombard stress examples for cross-currency collateral, concentration haircut, close-out, and cross-pledge contagion.

## Validation Notes

The migration should continue to satisfy repo standards:

1. no product/reference branding leakage,
2. no career-prep positioning,
3. relative links resolve,
4. source content is deduplicated into canonical docs,
5. domain claims are framed as platform design and operating-model reference, not advice.
