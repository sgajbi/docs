# Product Knowledge Coverage Matrix

This matrix tracks the current product-library coverage and where enrichment should deepen next.

## Current Product Packs

| Product Area | Pack | Companion Map | Current Strength | Useful Next Enrichment |
|---|---|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of bond taxonomy, lifecycle, accrued interest, yield, duration, credit risk, valuation, controls, and QA. | Add issuer/credit-events source-ownership matrix and more client-reporting examples for maturity ladder, yield, duration, and spread exposure. |
| Cash, deposits, money market and FX | [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | [`cash-deposits-money-market-and-fx.md`](cash-deposits-money-market-and-fx.md) | Strong coverage of ledger/available/projected cash, deposits, money market instruments, money market funds, repos, FX spot, forwards, swaps, NDFs, liquidity, buying power, performance, advisory, reporting, implementation, controls, and QA. | Add worked examples for unsettled cash, deposit breakage, MMF gate, FX funding, FX forward hedge, and NDF settlement. |
| Commodities, precious metals and real assets | [`commodities-precious-metals-real-assets/`](commodities-precious-metals-real-assets/README.md) | [`commodities-precious-metals-and-real-assets.md`](commodities-precious-metals-and-real-assets.md) | Strong coverage of physical metals, metal accounts, commodity ETPs, futures, options, swaps, structured commodity exposure, real-asset proxies, unit controls, collateral, advisory, reporting, implementation, controls, and QA. | Add worked examples for gold purchase/sale, allocated vs unallocated metal, futures margin and roll, commodity ETN issuer risk, pledged gold haircut, and commodity-linked note observation. |
| Derivatives | [`derivatives/`](derivatives/README.md) | [`derivatives-and-overlay-products.md`](derivatives-and-overlay-products.md) | Strong coverage of options, futures, forwards, swaps, CDS, valuation, Greeks, exposure, margin, collateral, mandate controls, and QA. | Add product-by-product exposure measure examples and a margin/collateral accounting vs performance treatment guide. |
| Equities | [`equities/`](equities/README.md) | [`direct-equities-and-market-operations.md`](direct-equities-and-market-operations.md) | Strong coverage of direct equity lifecycle, settlement, corporate actions, cost basis, P&L, performance, risk, and market operations. | Add a corporate-action processing decision tree and source-ownership model for issuer, exchange, custodian, and market-data inputs. |
| Funds | [`funds/`](funds/README.md) | [`pooled-investment-products.md`](pooled-investment-products.md) | Strong coverage of fund taxonomy, share classes, order lifecycle, NAV, fees, distributions, look-through, risk, and private fund flows. | Add a dealing-calendar/cut-off workflow map and explicit treatment for stale NAV, gates, suspensions, and side pockets in UI/API states. |
| Insurance and annuities | [`insurance-annuities/`](insurance-annuities/README.md) | [`insurance-and-annuities.md`](insurance-and-annuities.md) | Strong coverage of policy taxonomy, protection, ILPs, annuities, premiums, claims, surrender, policy loans, beneficiaries, valuation bases, advisory, reporting, implementation, controls, and QA. | Add worked examples for ILP premium and charges, policy loan, surrender quote, missed premium/lapse, death claim, and annuity payout schedule. |
| Loans, Lombard, margin and collateral | [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | [`lending-collateral-and-buying-power.md`](lending-collateral-and-buying-power.md) | Strong coverage of credit lines, Lombard lending, margin lending, pledges, cross-pledging, LTV, haircuts, availability, buying power, interest, fees, margin calls, forced liquidation, advisory, reporting, implementation, controls, and QA. | Add worked examples for cross-pledge allocation, collateral substitution, rating downgrade haircut changes, forced liquidation, and shared-collateral optimization. |
| Model portfolios, rebalancing, advisory proposals and suitability workflows | [`model-portfolios-rebalancing-advisory-suitability/`](model-portfolios-rebalancing-advisory-suitability/README.md) | [`portfolio-construction-and-rebalancing-guide.md`](portfolio-construction-and-rebalancing-guide.md) | Strong coverage of model portfolios, SAA/TAA, mandate design, drift, rebalancing, trade generation, proposals, suitability, restrictions, consent, data model, reporting, controls, and QA. | Add worked examples for model version rollout, tax-aware rebalance, pending-liquidity rebalance, proposal expiry, and post-trade residual drift. |
| Private markets | [`private-markets/`](private-markets/README.md) | [`private-markets-and-alternatives.md`](private-markets-and-alternatives.md) | Strong coverage of private equity, private credit, real assets, hedge funds, commitments, capital calls, distributions, NAV lag, performance multiples, advisory, mandate controls, implementation, reconciliation, and QA. | Add a source-ownership matrix by administrator, GP, custodian, document portal, internal ledger, and valuation committee for each private-market data area. |
| Portfolio performance, attribution, risk, benchmarks and mandate analytics | [`portfolio-performance-attribution-risk-benchmark/`](portfolio-performance-attribution-risk-benchmark/README.md) | [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md) | Strong coverage of performance analytics taxonomy, TWR, MWR, cashflow classification, contribution, attribution, benchmarks, composites, risk, stress, lineage, product-specific analytics, reporting, controls, and QA. | Add numeric worked examples for daily TWR, XIRR failure modes, Brinson attribution, currency attribution, benchmark rebalancing, and composite dispersion. |
| Real estate, REITs and infrastructure | [`real-estate-reits-infrastructure/`](real-estate-reits-infrastructure/README.md) | [`real-estate-reits-and-infrastructure.md`](real-estate-reits-and-infrastructure.md) | Strong coverage of listed REITs, business trusts, property securities, private real estate funds, direct property, infrastructure funds, income, valuation, leverage, liquidity, advisory, reporting, implementation, controls, and QA. | Add worked examples for REIT distribution, REIT corporate action, private real estate capital call, appraisal restatement, infrastructure fund distribution, and redemption queue. |
| Structured products | [`structured-products/`](structured-products/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of structured wrappers, payoff building blocks, lifecycle/event modelling, valuation, Greeks, scenario analytics, advisory, mandate controls, reconciliation, and QA. | Add a cross-wrapper source-ownership matrix for notes, deposits, certificates, warrants, structured funds, accumulators, decumulators, and OTC structures. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong note-wrapper deep dive covering structured-note taxonomy, lifecycle, payoff terms, barriers, autocalls, valuation, risk, physical settlement, advisory, and QA. | Keep as the note-specific implementation deep dive and cross-link future note payoffs back to the broader structured-products pack. |
| Tax, regulatory reporting and cross-border client reporting | [`tax-regulatory-reporting/`](tax-regulatory-reporting/README.md) | [`tax-regulatory-and-reporting.md`](tax-regulatory-and-reporting.md) | Strong coverage of tax-aware reporting taxonomy, income/capital-gain/withholding treatment, tax lots, cost basis, CRS/FATCA/QI concepts, client reporting, data model, controls, and QA. | Add jurisdiction-specific configuration examples and report-output templates for common private-banking reporting regimes where source policy is available. |
| Trusts, estate planning, family office and wealth structuring | [`trusts-estate-family-office-wealth-structuring/`](trusts-estate-family-office-wealth-structuring/README.md) | [`wealth-structuring-trusts-and-family-office.md`](wealth-structuring-trusts-and-family-office.md) | Strong coverage of trusts, foundations, estates, family offices, holding vehicles, beneficial ownership, authority, reporting hierarchy, mandates, access control, governance, implementation, and QA. | Add worked examples for trust distribution, estate freeze, family-office consolidated reporting, holding-company pledge, beneficiary access, and policy ownership roles. |

## Cross-Product References

| Reference | Use |
|---|---|
| [`advisory-mandate-reporting-decision-guide.md`](advisory-mandate-reporting-decision-guide.md) | Cross-product guide for advisory explanation, DPM/mandate controls, reporting requirements, QA evidence, and common review failures. |
| [`client-reporting-and-portfolio-review-guide.md`](client-reporting-and-portfolio-review-guide.md) | Cross-product client statement and portfolio review guide covering report sections, labels, exception panels, client explanations, and report QA. |
| [`cross-product-worked-examples.md`](cross-product-worked-examples.md) | Practical examples for lifecycle, transactions, cashflows, calculations, reporting, QA, and implementation boundaries across product families. |
| [`portfolio-construction-and-rebalancing-guide.md`](portfolio-construction-and-rebalancing-guide.md) | Cross-product construction and rebalancing guide for target allocation, drift, funding, liquidity, tax/cost impact, advisory/DPM treatment, execution readiness, reporting, and QA. |
| [`portfolio-exposure-modelling-guide.md`](portfolio-exposure-modelling-guide.md) | Cross-product exposure model for legal holdings, market value, economic exposure, look-through, notional, sensitivity, issuer, counterparty, currency, liquidity, collateral, commitment, mandate, and reporting views. |
| [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) | Cross-product boundary matrix for generic support, source-backed product-specific capabilities, support-limited states, future candidates, reporting claims, and QA evidence. |
| [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md) | Cross-product calculation examples with formulas, source-input expectations, reporting labels, degraded-state behavior, and QA assertions. |
| [`product-documentation-review-rubric.md`](product-documentation-review-rubric.md) | Professional review rubric for polishing every product pack across business purpose, taxonomy, payoff, cashflows, valuation, analytics, exposure, advisory, DPM, reporting, calculations, edge cases, support boundaries, future candidates, and examples. |
| [`product-lifecycle-cashflow-and-event-guide.md`](product-lifecycle-cashflow-and-event-guide.md) | Cross-product lifecycle and cashflow standard for orders, events, transactions, settlement, accruals, entitlements, obligations, performance treatment, reporting, and QA. |
| [`product-payoff-scenario-and-stress-guide.md`](product-payoff-scenario-and-stress-guide.md) | Cross-product payoff and scenario guide for linear, conditional, convex, income, protection, liability, residual-value, stress, advisory, reporting, and QA treatment. |
| [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md) | Cross-product performance, contribution, attribution, risk denominator, degraded analytics, and analytics QA guidance. |
| [`practical-product-implementation-review.md`](practical-product-implementation-review.md) | Cross-product checklist for examples, payoff behavior, cashflows, valuation, analytics, advisory, mandate, reporting, QA, implementation boundaries, and candidate extensions. |
| [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md) | Cross-product QA matrix for normal, degraded, reporting, reconciliation, migration, and release-gate evidence. |
| [`product-source-intake-and-migration-guide.md`](product-source-intake-and-migration-guide.md) | Cross-product source-intake and migration guidance for field profiling, source ownership, reconciliation, degraded states, and sign-off evidence. |
| [`product-taxonomy-and-vocabulary-guide.md`](product-taxonomy-and-vocabulary-guide.md) | Cross-product vocabulary standard for product family, subtype, wrapper, legal holding, exposure, lifecycle events, valuation basis, liquidity basis, reporting classification, and supportability states. |
| [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md) | Cross-product source ownership, data-quality states, calculation dependencies, reporting requirements, QA scenarios, and implementation-boundary checklist. |
| [`tax-regulatory-and-reporting.md`](tax-regulatory-and-reporting.md) | Tax-aware reporting guide for tax classification, withholding, tax lots, cross-border documentation, regulatory reporting, client reports, controls, and QA. |
| [`wealth-structuring-trusts-and-family-office.md`](wealth-structuring-trusts-and-family-office.md) | Wealth-structuring guide for trusts, estates, family offices, holding vehicles, beneficial ownership, authority, access control, reporting hierarchy, and QA. |
| [`../reference/private-banking-platform-knowledge-map.md`](../reference/private-banking-platform-knowledge-map.md) | General project checklist for source ownership, API design, UI design, and documentation design. |
| [`../reference/wealth-product-framework/`](../reference/wealth-product-framework/README.md) | Cross-product canonical position, transaction, advisory, mandate, analytics, reporting, and quality-standard material. |

## Coverage Dimensions

Use these dimensions when deciding whether a product area is complete enough for project use:

1. product taxonomy,
2. legal position model,
3. analytical exposure model,
4. lifecycle events,
5. economic transactions,
6. valuation and stale-data posture,
7. risk and concentration treatment,
8. performance and P&L treatment,
9. advisory and suitability considerations,
10. mandate and DPM controls,
11. reporting requirements,
12. source ownership,
13. API implications,
14. UI implications,
15. QA and regression scenarios,
16. controls and reconciliation,
17. practitioner/stakeholder questions and answers,
18. cross-product comparisons,
19. worked examples across product subtypes,
20. generic capability versus product-specific implementation boundary.

## Library Improvement Backlog

Potential future packs:

1. mandates and advisory workflows,
2. client-facing explanation packs,
3. report template examples,
4. source-data and market-data governance.
