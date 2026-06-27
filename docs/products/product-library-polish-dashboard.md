# Product Library Polish Dashboard

This dashboard tracks the current product-library polish posture. It is a compact operating view, not a replacement for the full coverage matrix.

Use it with:

1. [`product-documentation-review-rubric.md`](product-documentation-review-rubric.md),
2. [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md),
3. [`product-worked-example-index.md`](product-worked-example-index.md),
4. [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md).

## Review Dimensions

Every product area should be reviewed across these dimensions:

| Dimension | Standard |
|---|---|
| Product understanding and taxonomy | Product family, subtype, wrapper, legal holding, service model, adjacent products and out-of-scope areas are clear. |
| Payoff, cashflow and lifecycle | Orders, lifecycle events, settlement, income, fees, taxes, obligations, projected flows and corrections are separated. |
| Valuation and pricing | Source owner, valuation basis, price hierarchy, stale state, fallback, model value and tradability are explicit. |
| Performance, risk and exposure | Return components, contribution, attribution, risk denominator, legal holding, economic exposure and look-through are not confused. |
| Advisory, DPM and suitability | Eligibility, complexity, liquidity, concentration, mandate, authority, disclosure and recommendation controls are documented. |
| Reporting, calculation and QA | Formulas, source inputs, report labels, degraded states, regression tests and reconciliation evidence are testable. |
| Implementation boundary | Generic baseline support, source-backed extensions, support-limited states and future candidates are separated. |

## Status Legend

| Status | Meaning |
|---|---|
| Strong | The pack has dedicated source material, worked examples, QA language, source ownership and support-boundary guidance. |
| Targeted | The pack is broad and usable, but a focused slice would improve a specific edge case, workflow or calculation area. |
| Needs slice | The area needs a deliberate enrichment pass before it should be treated as a mature reusable reference. |

## Product-Family Dashboard

| Product area | Current review posture | Strongest coverage | Next polish slice |
|---|---|---|---|
| Bonds | Strong | Taxonomy, lifecycle, accrued interest, yield, duration, credit risk, valuation, QA, clean/dirty settlement, callable yield-to-worst, downgrades, default/recovery, inflation-linked uplift, amortising schedules, convertibles, asset-backed prepayments and multi-currency performance. | Sinkable bonds, make-whole calls, distressed exchanges, tax-lot reporting, accrued-interest corrections and hedged bond attribution. |
| Cash, deposits, money market and FX | Strong | Ledger cash, available cash, deposits, money market instruments, repos, FX spot, forwards, swaps, liquidity, multi-currency buying power, failed FX settlement, cash sweeps, approved overdraft funding, negative interest, cross-currency settlement holidays, sweep unwind during stress and credit-line-funded purchases. | Overdraft pricing tiers, intraday liquidity restrictions, payment cut-off failures, stress escalation, cash pooling and nostro reconciliation. |
| Client, account, portfolio and relationship master data | Strong | Client hierarchy, accounts, portfolios, households, beneficial ownership, mandates, booking centres, access control, joint accounts, EAM relationships, trust/company structures, closed-account reporting, account transfers, privacy masking and advisor coverage changes. | Dormant accounts, power-of-attorney expiry, minor-to-adult transition, collateral-only portfolios, omnibus client mapping and client data rectification. |
| Commodities, precious metals and real assets | Strong | Physical metals, metal accounts, commodity ETPs, derivatives, structured exposure, collateral, unit controls, storage fees, paid-in-kind fees, option exercise, warehouse receipts, index roll yield and delivery-risk workflows. | Assay disputes, vault transfers, provider fee disputes, commodity ETP tax events, source-of-metal restrictions, inventory financing and natural-resource royalty streams. |
| Data governance, quality, lineage and controls | Strong | Source ownership, golden copy, lineage, reconciliation, quality rules, certification, migration and audit evidence. | Quality-score thresholding, lineage graph export, report certification, event replay and source outage fallback. |
| Derivatives | Strong | Options, futures, forwards, swaps, CDS, Greeks, margin, collateral, advisory controls, lifecycle events, multi-leg strategies, hedge effectiveness, OTC novation/compression, central clearing, collateral optimization and portfolio Greeks. | Variance swaps, barrier options, cross-currency swaps, option exercise windows, collateral disputes, clearing-broker default and strategy-level attribution. |
| Entitlements, digital channels and advisor workbench | Strong | RBAC/ABAC, relationship-based access, delegation, advisor workbench, digital channels, workflow and audit. | Legal delegate journeys, EAM access, family-office portals, mobile step-up, consent expiry and certification campaigns. |
| Equities | Strong | Trading, settlement, corporate actions, dividends, cost basis, P&L, performance, risk and market operations. | Spin-off cost allocation, merger consideration, manufactured dividends, short-sale margin and restricted-list controls. |
| Funds | Strong | Fund taxonomy, share classes, orders, NAV, fees, distributions, look-through, private fund flows, gates, equalization, share-class conversion, fund mergers, notice-period holdbacks, transfer-agent rejects and fund-of-funds look-through. | Fee equalization after partial redemption, performance-fee crystallization, in-specie transfers, custody re-registration, suspension reopenings and stale look-through override expiry. |
| Insurance and annuities | Strong | Policy contracts, ILPs, protection, annuities, premiums, claims, surrender, loans, beneficiaries and reporting labels. | Indexed annuity crediting, variable annuity riders, premium holidays, reinstatement and cross-border beneficiary restrictions. |
| Investment accounting and ledger | Strong | Books of record, transaction legs, lots, accruals, fees, withholding, cost basis, P&L, FX and audit lineage. | Multi-book accounting, amortized cost, tax-lot elections, fee rebates, statement corrections and derivative collateral postings. |
| Loans, Lombard, margin and collateral | Strong | Credit lines, pledges, cross-pledging, LTV, haircuts, buying power, margin calls, liquidation and collateral controls. | Multi-currency facilities, structured covenants, rehypothecation constraints, pledged policies and private-market NAV lending. |
| Market data, reference data, pricing and instrument master | Strong | Identifiers, classification, pricing hierarchy, stale-price controls, FX rates, curves, benchmarks and corporate actions. | Vendor onboarding, price challenge workflow, benchmark restatement, symbology collision, proxy pricing and override expiry. |
| Model portfolios, rebalancing, advisory and suitability | Strong | Model portfolios, SAA/TAA, drift, rebalancing, proposals, suitability, restrictions, consent and DPM controls. | Optimizer constraints, wave rebalancing, household-level staging, cash sleeve overlays and FX-hedged models. |
| Portfolio performance, attribution, risk and benchmarks | Strong | TWR, MWR, contribution, attribution, benchmark analytics, composites, tracking error, drawdown and stress. | Multi-period attribution linking, private-market IRR edge cases, factor risk, benchmark carve-outs and fee/tax bases. |
| Portfolio reporting and client experience | Strong | Statements, reviews, report datasets, holdings, activity, cashflows, income, risk, exceptions, PDF rendering and delivery. | Multilingual statements, family-office consolidated reports, commentary workflows, report entitlements and correction notices. |
| Private markets | Strong | Commitments, capital calls, distributions, NAV lag, multiples, IRR, gates, restatements, secondaries and liquidity. | Co-investments, continuation vehicles, covenant breaches, in-kind distributions, side pockets and capital-call forecasting. |
| Product governance, APU and target market | Strong | Due diligence, approved universe, target market, eligibility, complexity, liquidity, lifecycle review and pre-trade integration. | Numeric rule scoring, disclosure expiry, suspension impact and historical audit replay across booking centres. |
| Real estate, REITs and infrastructure | Strong | Listed REITs, private real estate, direct property, infrastructure funds, income, valuation, liquidity, leverage analytics, property-level debt maturity, tenant concentration, occupancy, lease expiry, concession renewal, renewable power-price exposure and direct ownership transfers. | Development projects, construction drawdowns, rent-free periods, capex reserves, property sale completion, valuation committee overrides and infrastructure revenue clawbacks. |
| Structured products | Strong | Payoff building blocks, wrappers, underlyings, lifecycle events, valuation, scenario analytics, advisory controls and QA. | Structured funds, bespoke OTC trades, secondary-market unwind, corporate-action adjustments and scenario-model validation. |
| Structured notes | Strong | Note wrappers, barriers, autocalls, coupon behavior, credit-linked notes, dual currency notes and physical settlement. | Range accrual notes, participation notes, issuer default after accrual, partial secondary sale and tax lots after delivery. |
| Tax, regulatory reporting and cross-border reporting | Strong | Income classification, withholding, tax lots, cost basis, CRS/FATCA/QI concepts, client reporting, jurisdiction-specific configuration, report outputs, amended filings, withholding reclaims, beneficial-owner reporting and controls. | Wash-sale style controls, tax-lot transfer between custodians, multi-currency gain bases, withholding pool reconciliation and report delivery entitlements. |
| Trade lifecycle, settlement, custody and operations | Strong | Advisory-to-order lifecycle, execution, confirmation, settlement, custody, asset servicing, reconciliation, exceptions, allocation corrections, FX settlement, income reversals, trade cancellations, fee/tax postings and break closure. | Multi-market allocations, settlement netting, custody account transfers, securities lending recalls, failed corporate-action elections and market claims. |
| Trusts, estate, family office and wealth structuring | Strong | Trusts, foundations, estates, family offices, holding vehicles, beneficial ownership, authority, reporting hierarchy, protector vetoes, directed trusts, investment committees, foundation council changes, beneficiary classes, VCC sub-funds and cross-border tax residency changes. | Letters of wishes, reserved powers, charitable purpose foundations, nominee arrangements, philanthropic grants, trust termination and conflict-of-interest controls. |

## Current Cross-Product Strengths

The library now has strong cross-product support for:

1. worked examples across product families,
2. deeper cross-product calculation case studies and golden QA scenarios,
3. role-based navigation,
4. product coverage and enrichment tracking,
5. calculation examples,
6. QA and regression scenarios,
7. source ownership and reporting dependencies,
8. capability boundaries and implementation evidence,
9. lifecycle, cashflow and event modelling,
10. payoff, scenario and stress explanations,
11. performance, attribution, exposure and risk guidance.

## Next High-Value Slices

The next useful enrichment work should prioritize:

1. overdraft pricing tiers, intraday liquidity restrictions, payment cut-off failures, stress escalation and nostro reconciliation,
2. sinkable bond, make-whole call, distressed exchange and hedged bond attribution examples,
3. fund fee equalization after partial redemption, performance-fee crystallization, in-specie transfers and suspension reopenings,
4. variance swaps, barrier options, cross-currency swaps, collateral disputes and strategy-level attribution,
5. real estate development projects, construction drawdowns, capex reserves and property sale completion,
6. tax-lot transfer, multi-currency gain basis, withholding pool reconciliation and report delivery entitlement examples,
7. multi-market allocation, settlement netting, custody transfer and market-claim examples,
8. wealth-structuring letters of wishes, reserved powers, nominee arrangements and conflict-of-interest controls.

## How To Use This Dashboard

| Role | Use |
|---|---|
| Business user | Find the strongest product explanation and the next area where business examples may still be needed. |
| Business analyst | Turn the next polish slice into requirements, data questions and acceptance criteria. |
| Developer or architect | Check whether a requested product feature is already source-backed or needs a new capability boundary. |
| QA | Convert the next polish slice into regression scenarios and degraded-state tests. |
| Product development | Prioritize future enrichment where product understanding, implementation evidence and client reporting value overlap. |

## Maintenance Rule

Update this dashboard when:

1. a new product pack is added,
2. a product worked-example file gains material new examples,
3. the coverage matrix changes its next enrichment guidance,
4. a product area moves from targeted coverage to strong coverage,
5. implementation evidence changes the support boundary for a product capability.
