# Product Knowledge Coverage Matrix

This matrix tracks the current product-library coverage and where enrichment should deepen next.

## Current Product Packs

| Product Area | Pack | Companion Map | Current Strength | Useful Next Enrichment |
|---|---|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of bond taxonomy, lifecycle, accrued interest, yield, duration, credit risk, valuation, controls, and QA. | Add issuer/credit-events source-ownership matrix and more client-reporting examples for maturity ladder, yield, duration, and spread exposure. |
| Derivatives | [`derivatives/`](derivatives/README.md) | [`derivatives-and-overlay-products.md`](derivatives-and-overlay-products.md) | Strong coverage of options, futures, forwards, swaps, CDS, valuation, Greeks, exposure, margin, collateral, mandate controls, and QA. | Add product-by-product exposure measure examples and a margin/collateral accounting vs performance treatment guide. |
| Equities | [`equities/`](equities/README.md) | [`direct-equities-and-market-operations.md`](direct-equities-and-market-operations.md) | Strong coverage of direct equity lifecycle, settlement, corporate actions, cost basis, P&L, performance, risk, and market operations. | Add a corporate-action processing decision tree and source-ownership model for issuer, exchange, custodian, and market-data inputs. |
| Funds | [`funds/`](funds/README.md) | [`pooled-investment-products.md`](pooled-investment-products.md) | Strong coverage of fund taxonomy, share classes, order lifecycle, NAV, fees, distributions, look-through, risk, and private fund flows. | Add a dealing-calendar/cut-off workflow map and explicit treatment for stale NAV, gates, suspensions, and side pockets in UI/API states. |
| Structured products | [`structured-products/`](structured-products/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong coverage of structured wrappers, payoff building blocks, lifecycle/event modelling, valuation, Greeks, scenario analytics, advisory, mandate controls, reconciliation, and QA. | Add a cross-wrapper source-ownership matrix for notes, deposits, certificates, warrants, structured funds, accumulators, decumulators, and OTC structures. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Strong note-wrapper deep dive covering structured-note taxonomy, lifecycle, payoff terms, barriers, autocalls, valuation, risk, physical settlement, advisory, and QA. | Keep as the note-specific implementation deep dive and cross-link future note payoffs back to the broader structured-products pack. |

## Cross-Product References

| Reference | Use |
|---|---|
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
18. cross-product comparisons.

## Library Improvement Backlog

Potential future packs:

1. cash and deposits,
2. FX spot, forwards, swaps, and deposits as a dedicated pack,
3. private markets and alternatives,
4. commodities and precious metals,
5. insurance-linked and capital-protected products,
6. mandates and advisory workflows,
7. client reporting and portfolio review methodology,
8. performance and attribution methodology,
9. risk and concentration methodology,
10. source-data and market-data governance.
