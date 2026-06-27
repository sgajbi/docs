# Product Knowledge Coverage Matrix

This matrix tracks the current product-library coverage and where enrichment should deepen next.

## Current Product Packs

| Product Area | Pack | Companion Map | Current Strength | Useful Next Enrichment |
|---|---|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of bond taxonomy, lifecycle, accrued interest, yield, duration, credit risk, valuation, controls, and QA. | Add issuer/credit-events source-ownership matrix and more client-reporting examples for maturity ladder, yield, duration, and spread exposure. |
| Cash, deposits, money market and FX | [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | [`cash-deposits-money-market-and-fx.md`](cash-deposits-money-market-and-fx.md) | Strong coverage of ledger/available/projected cash, deposits, money market instruments, money market funds, repos, FX spot, forwards, swaps, NDFs, liquidity, buying power, performance, advisory, reporting, implementation, controls, and QA. | Add worked examples for unsettled cash, deposit breakage, MMF gate, FX funding, FX forward hedge, and NDF settlement. |
| Derivatives | [`derivatives/`](derivatives/README.md) | [`derivatives-and-overlay-products.md`](derivatives-and-overlay-products.md) | Strong coverage of options, futures, forwards, swaps, CDS, valuation, Greeks, exposure, margin, collateral, mandate controls, and QA. | Add product-by-product exposure measure examples and a margin/collateral accounting vs performance treatment guide. |
| Equities | [`equities/`](equities/README.md) | [`direct-equities-and-market-operations.md`](direct-equities-and-market-operations.md) | Strong coverage of direct equity lifecycle, settlement, corporate actions, cost basis, P&L, performance, risk, and market operations. | Add a corporate-action processing decision tree and source-ownership model for issuer, exchange, custodian, and market-data inputs. |
| Funds | [`funds/`](funds/README.md) | [`pooled-investment-products.md`](pooled-investment-products.md) | Strong coverage of fund taxonomy, share classes, order lifecycle, NAV, fees, distributions, look-through, risk, and private fund flows. | Add a dealing-calendar/cut-off workflow map and explicit treatment for stale NAV, gates, suspensions, and side pockets in UI/API states. |
| Loans, Lombard, margin and collateral | [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | [`lending-collateral-and-buying-power.md`](lending-collateral-and-buying-power.md) | Strong coverage of credit lines, Lombard lending, margin lending, pledges, cross-pledging, LTV, haircuts, availability, buying power, interest, fees, margin calls, forced liquidation, advisory, reporting, implementation, controls, and QA. | Add worked examples for cross-pledge allocation, collateral substitution, rating downgrade haircut changes, forced liquidation, and shared-collateral optimization. |
| Private markets | [`private-markets/`](private-markets/README.md) | [`private-markets-and-alternatives.md`](private-markets-and-alternatives.md) | Strong coverage of private equity, private credit, real assets, hedge funds, commitments, capital calls, distributions, NAV lag, performance multiples, advisory, mandate controls, implementation, reconciliation, and QA. | Add a source-ownership matrix by administrator, GP, custodian, document portal, internal ledger, and valuation committee for each private-market data area. |
| Structured products | [`structured-products/`](structured-products/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of structured wrappers, payoff building blocks, lifecycle/event modelling, valuation, Greeks, scenario analytics, advisory, mandate controls, reconciliation, and QA. | Add a cross-wrapper source-ownership matrix for notes, deposits, certificates, warrants, structured funds, accumulators, decumulators, and OTC structures. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong note-wrapper deep dive covering structured-note taxonomy, lifecycle, payoff terms, barriers, autocalls, valuation, risk, physical settlement, advisory, and QA. | Keep as the note-specific implementation deep dive and cross-link future note payoffs back to the broader structured-products pack. |

## Cross-Product References

| Reference | Use |
|---|---|
| [`practical-product-implementation-review.md`](practical-product-implementation-review.md) | Cross-product checklist for examples, payoff behavior, cashflows, valuation, analytics, advisory, mandate, reporting, QA, implementation boundaries, and candidate extensions. |
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

1. commodities and precious metals,
2. insurance-linked and capital-protected products,
3. mandates and advisory workflows,
4. client reporting and portfolio review methodology,
5. performance and attribution methodology,
6. risk and concentration methodology,
7. source-data and market-data governance.
