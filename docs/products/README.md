# Product Documentation

Product documentation belongs here.

Each product family should have its own folder with a local `README.md`, a clear reading order, and source provenance.

## Product Packs

| Product Pack | Location | Purpose |
|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | Wealth-management reference for bond fundamentals, lifecycle, transaction and position modelling, valuation, yield, duration, credit risk, advisory, implementation, and QA. |
| Cash, deposits, money market and FX | [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Wealth-management reference for cash balances, deposits, money market instruments, money market funds, FX spot, forwards, swaps, NDFs, liquidity, buying power, income, settlement, advisory, reporting, implementation, and QA. |
| Commodities, precious metals and real assets | [`commodities-precious-metals-real-assets/`](commodities-precious-metals-real-assets/README.md) | Wealth-management reference for physical metals, metal accounts, commodity ETPs, commodity derivatives, structured commodity exposure, real assets, collateral, advisory, reporting, implementation, and QA. |
| Derivatives | [`derivatives/`](derivatives/README.md) | Wealth-management reference for options, futures, forwards, swaps, CDS, OTC derivatives, lifecycle events, valuation, Greeks, exposure, margin, collateral, advisory, mandate controls, implementation, and QA. |
| Equities | [`equities/`](equities/README.md) | Wealth-management reference for direct equities, trade lifecycle, settlement, corporate actions, dividends, cost basis, valuation, P&L, performance, risk, advisory, DPM, reporting, implementation, and QA. |
| Funds | [`funds/`](funds/README.md) | Wealth-management reference for funds, ETFs, share classes, lifecycle, subscriptions/redemptions, NAV, fees, look-through, performance, advisory, implementation, and QA. |
| Insurance and annuities | [`insurance-annuities/`](insurance-annuities/README.md) | Wealth-management reference for insurance policies, ILPs, protection products, annuities, premiums, claims, surrender, policy loans, beneficiaries, advisory, reporting, implementation, and QA. |
| Loans, Lombard, margin and collateral | [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | Wealth-management reference for loans, overdrafts, credit lines, Lombard lending, margin lending, collateral pledges, LTV, haircuts, buying power, margin calls, advisory, reporting, implementation, and QA. |
| Private markets | [`private-markets/`](private-markets/README.md) | Wealth-management reference for private equity, private credit, real assets, hedge funds, commitments, capital calls, distributions, NAV lag, liquidity, advisory, implementation, and QA. |
| Real estate, REITs and infrastructure | [`real-estate-reits-infrastructure/`](real-estate-reits-infrastructure/README.md) | Wealth-management reference for listed REITs, business trusts, property securities, private real estate funds, direct property, infrastructure funds, income, valuation, risk, advisory, reporting, implementation, and QA. |
| Structured products | [`structured-products/`](structured-products/README.md) | Wealth-management reference for structured wrappers, payoff building blocks, underlyings, lifecycle events, valuation, scenario analytics, advisory controls, implementation, reconciliation, and QA. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | Wealth-management reference for notes, structured notes, lifecycle, data model, valuation, risk, advisory, implementation, and QA. |
| Tax, regulatory reporting and cross-border client reporting | [`tax-regulatory-reporting/`](tax-regulatory-reporting/README.md) | Wealth-management reference for tax-aware reporting, income classification, withholding, tax lots, cost basis, cross-border documentation, regulatory reporting, client statements, controls, implementation, and QA. |
| Trusts, estate planning, family office and wealth structuring | [`trusts-estate-family-office-wealth-structuring/`](trusts-estate-family-office-wealth-structuring/README.md) | Wealth-management reference for trusts, estates, foundations, family offices, holding vehicles, beneficial ownership, authority, mandates, access control, reporting, implementation, and QA. |

## Product Area Maps

| Map | Purpose |
|---|---|
| [`advisory-mandate-reporting-decision-guide.md`](advisory-mandate-reporting-decision-guide.md) | Provides cross-product advisory explanation, mandate-control, reporting, QA, and review-failure guidance for covered product families. |
| [`cash-deposits-money-market-and-fx.md`](cash-deposits-money-market-and-fx.md) | Captures reusable platform-design distinctions for currency cash, settlement, available cash, deposits, money market products, FX, liquidity, buying power, and reporting. |
| [`client-reporting-and-portfolio-review-guide.md`](client-reporting-and-portfolio-review-guide.md) | Defines product-aware client statement, portfolio review, report label, exception panel, client explanation, and report QA guidance. |
| [`commodities-precious-metals-and-real-assets.md`](commodities-precious-metals-and-real-assets.md) | Captures reusable platform-design distinctions for commodities, precious metals, real-asset-linked wrappers, unit controls, exposure, collateral, and reporting. |
| [`cross-product-worked-examples.md`](cross-product-worked-examples.md) | Provides practical worked examples across product families with lifecycle, calculations, reporting, QA, and implementation boundaries. |
| [`derivatives-and-overlay-products.md`](derivatives-and-overlay-products.md) | Captures reusable platform-design distinctions for derivatives, overlays, exposure, lifecycle events, margin, collateral, advisory, and mandate controls. |
| [`direct-equities-and-market-operations.md`](direct-equities-and-market-operations.md) | Captures reusable platform-design distinctions for direct equities, trading, corporate actions, P&L, risk, and market operations. |
| [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Compares bonds and structured notes and captures reusable platform-design distinctions across fixed income and structured products. |
| [`insurance-and-annuities.md`](insurance-and-annuities.md) | Captures reusable platform-design distinctions for policy contracts, protection benefits, ILPs, annuities, cash values, surrender, claims, and reporting. |
| [`lending-collateral-and-buying-power.md`](lending-collateral-and-buying-power.md) | Captures reusable platform-design distinctions for facilities, drawdowns, collateral pools, pledge graphs, LTV, haircuts, buying power, margin calls, and liquidation workflows. |
| [`private-markets-and-alternatives.md`](private-markets-and-alternatives.md) | Captures reusable platform-design distinctions for commitments, capital calls, distributions, lagged NAVs, alternatives, mandate controls, liquidity planning, and reporting. |
| [`pooled-investment-products.md`](pooled-investment-products.md) | Captures reusable platform-design distinctions for funds, ETFs, fund of funds, hedge funds, and private market funds. |
| [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) | Separates generic baseline support from source-backed product-specific capabilities, support-limited states, future candidates, reporting claims, and QA evidence. |
| [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md) | Provides compact formulas, reporting labels, degraded-state behavior, and QA assertions across covered product families. |
| [`product-lifecycle-cashflow-and-event-guide.md`](product-lifecycle-cashflow-and-event-guide.md) | Defines reusable lifecycle, cashflow, event, transaction, accrual, entitlement, obligation, reporting, implementation, and QA guidance across product families. |
| [`product-payoff-scenario-and-stress-guide.md`](product-payoff-scenario-and-stress-guide.md) | Defines reusable payoff, scenario, stress, downside, income reliability, liquidity, advisory, reporting, and QA guidance across product families. |
| [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md) | Defines cross-product return components, contribution, attribution, risk denominators, degraded analytics states, and analytics QA. |
| [`practical-product-implementation-review.md`](practical-product-implementation-review.md) | Provides a cross-product review lens, product-family example matrix, generic platform baseline, candidate extensions, and example-expansion backlog. |
| [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md) | Provides cross-product QA and regression scenarios for normal, degraded, reporting, reconciliation, migration, and release-gate evidence. |
| [`product-source-intake-and-migration-guide.md`](product-source-intake-and-migration-guide.md) | Provides source-intake and migration guidance for field profiling, source ownership, reconciliation, degraded states, and sign-off evidence. |
| [`product-taxonomy-and-vocabulary-guide.md`](product-taxonomy-and-vocabulary-guide.md) | Defines reusable product taxonomy and vocabulary for family, subtype, wrapper, legal holding, exposure, lifecycle, valuation, liquidity, reporting, and supportability states. |
| [`real-estate-reits-and-infrastructure.md`](real-estate-reits-and-infrastructure.md) | Captures reusable platform-design distinctions for listed REITs, property securities, private real estate, infrastructure, income, valuation, liquidity, and reporting. |
| [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md) | Tracks current product coverage, companion maps, and useful next enrichment areas. |
| [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md) | Defines cross-product source ownership, calculation dependencies, reporting requirements, data-quality states, QA scenarios, and implementation boundaries. |
| [`tax-regulatory-and-reporting.md`](tax-regulatory-and-reporting.md) | Captures reusable platform-design distinctions for tax classification, withholding, tax lots, cross-border documentation, regulatory reporting, client statements, controls, and QA. |
| [`wealth-structuring-trusts-and-family-office.md`](wealth-structuring-trusts-and-family-office.md) | Captures reusable platform-design distinctions for trusts, estates, family offices, holding vehicles, beneficial ownership, authority, access control, reporting groups, and QA. |

## Suggested Pack Structure

```text
docs/products/<product-slug>/
  README.md
  01-overview-and-taxonomy.md
  02-lifecycle-and-transactions.md
  03-data-model-and-risk.md
  04-platform-implementation.md
```

Use this structure when it fits. If source material is already well organized, preserve the source sequence and update the local README.
