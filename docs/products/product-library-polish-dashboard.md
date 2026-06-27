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
| Bonds | Strong | Taxonomy, lifecycle, accrued interest, yield, duration, credit risk, valuation, QA, clean/dirty settlement, callable yield-to-worst, downgrades, default/recovery, inflation-linked uplift, amortising schedules, convertibles, asset-backed prepayments and factor restatements, multi-currency performance, sinkable schedules, make-whole calls, distressed exchanges, tender offers, consent solicitations, tax-lot sales, accrued-interest corrections, contingent convertibles, perpetual reset spreads, hedged bond attribution, multi-book accounting, callable step-up extension risk, covered-bond pool deterioration, sukuk profit distributions, municipal tax-equivalent yield, green-bond label failure, ladder reinvestment and benchmark-duration hedging. | Repo-funded bond positions, covered-bond resolution events, sukuk asset-sale events, callable step-up non-call campaigns, green-bond label remediation and duration-hedge roll management. |
| Cash, deposits, money market and FX | Strong | Ledger cash, available cash, deposits, money market instruments, repos, FX spot, forwards, swaps, liquidity, multi-currency buying power, failed FX settlement, cash sweeps, approved overdraft funding, negative interest, cross-currency settlement holidays, sweep unwind during stress, credit-line-funded purchases, overdraft pricing tiers, intraday liquidity restrictions, payment cut-off failures, stress escalation, cash pooling, nostro reconciliation, source-calendar governance, payment repair, screening holds, multi-bank concentration, intraday credit caps, CLS/PVP settlement controls and treasury placement workflows. | CLS fallback disputes, payment recall, cross-bank liquidity stress, treasury counterparty downgrade, same-day liquidity allocation across client groups and payment sanctions false-positive remediation. |
| Client, account, portfolio and relationship master data | Strong | Client hierarchy, accounts, portfolios, households, beneficial ownership, mandates, booking centres, access control, joint accounts, EAM relationships, trust/company structures, closed-account reporting, account transfers, privacy masking and advisor coverage changes. | Dormant accounts, power-of-attorney expiry, minor-to-adult transition, collateral-only portfolios, omnibus client mapping and client data rectification. |
| Commodities, precious metals and real assets | Strong | Physical metals, metal accounts, commodity ETPs, derivatives, structured exposure, collateral, unit controls, storage fees, paid-in-kind fees, option exercise, warehouse receipts, index roll yield and delivery-risk workflows. | Assay disputes, vault transfers, provider fee disputes, commodity ETP tax events, source-of-metal restrictions, inventory financing and natural-resource royalty streams. |
| Data governance, quality, lineage and controls | Strong | Source ownership, golden copy, lineage, reconciliation, quality rules, certification, migration and audit evidence. | Quality-score thresholding, lineage graph export, report certification, event replay and source outage fallback. |
| Derivatives | Strong | Options, futures, forwards, swaps, CDS, Greeks, margin, collateral, advisory controls, lifecycle events, multi-leg strategies, hedge effectiveness, OTC novation/compression, central clearing, collateral optimization, portfolio Greeks, variance swaps, barrier options, cross-currency swaps, exercise windows, collateral disputes, clearing-broker default, strategy-level attribution, SIMM-style margin inputs, volatility-surface shocks, callable swaps, equity swap dividend adjustments, futures delivery notices, prime-broker give-ups, early termination breakage and close-out netting sets. | CVA/DVA/FVA input ownership, compression tear-up allocations, cross-currency collateral optionality, lifecycle event replay, barrier observation disputes, uncleared-margin phase-in, exchange position limits, cleared OTC porting and valuation model-change approvals. |
| Entitlements, digital channels and advisor workbench | Strong | RBAC/ABAC, relationship-based access, delegation, advisor workbench, digital channels, workflow and audit. | Legal delegate journeys, EAM access, family-office portals, mobile step-up, consent expiry and certification campaigns. |
| Equities | Strong | Trading, settlement, corporate actions, dividends, cost basis, P&L, performance, risk and market operations. | Spin-off cost allocation, merger consideration, manufactured dividends, short-sale margin and restricted-list controls. |
| Funds | Strong | Fund taxonomy, share classes, orders, NAV, fees, distributions, look-through, private fund flows, gates, equalization, share-class conversion, fund mergers, notice-period holdbacks, transfer-agent rejects, fund-of-funds look-through, performance-fee crystallization, in-specie redemptions, custody re-registration, suspension reopenings, stale look-through override expiry, cross-border eligibility changes, tax-status changes, trailer-fee rebates, anti-dilution levy disputes, hard-to-value side-pocket exits, closure distributions and master-feeder restructurings. | Omnibus transfer-agent allocations, ETF creation/redemption baskets, liquidity bucket reclassification, swing-factor governance, fund-of-funds fee layering, side-letter liquidity terms, class-action proceeds and fund-platform migration events. |
| Insurance and annuities | Strong | Policy contracts, ILPs, protection, annuities, premiums, claims, surrender, loans, beneficiaries and reporting labels. | Indexed annuity crediting, variable annuity riders, premium holidays, reinstatement and cross-border beneficiary restrictions. |
| Investment accounting and ledger | Strong | Books of record, transaction legs, lots, accruals, fees, withholding, cost basis, P&L, FX and audit lineage. | Multi-book accounting, amortized cost, tax-lot elections, fee rebates, statement corrections and derivative collateral postings. |
| Loans, Lombard, margin and collateral | Strong | Credit lines, pledges, cross-pledging, LTV, haircuts, buying power, margin calls, liquidation and collateral controls. | Multi-currency facilities, structured covenants, rehypothecation constraints, pledged policies and private-market NAV lending. |
| Market data, reference data, pricing and instrument master | Strong | Identifiers, classification, pricing hierarchy, stale-price controls, FX rates, curves, benchmarks and corporate actions. | Vendor onboarding, price challenge workflow, benchmark restatement, symbology collision, proxy pricing and override expiry. |
| Model portfolios, rebalancing, advisory and suitability | Strong | Model portfolios, SAA/TAA, drift, rebalancing, proposals, suitability, restrictions, consent and DPM controls. | Optimizer constraints, wave rebalancing, household-level staging, cash sleeve overlays and FX-hedged models. |
| Portfolio performance, attribution, risk and benchmarks | Strong | TWR, MWR, contribution, attribution, benchmark analytics, composites, tracking error, drawdown and stress. | Multi-period attribution linking, private-market IRR edge cases, factor risk, benchmark carve-outs and fee/tax bases. |
| Portfolio reporting and client experience | Strong | Statements, reviews, report datasets, holdings, activity, cashflows, income, risk, exceptions, PDF rendering and delivery. | Multilingual statements, family-office consolidated reports, commentary workflows, report entitlements and correction notices. |
| Private markets | Strong | Commitments, capital calls, distributions, NAV lag, multiples, IRR, gates, restatements, secondaries and liquidity. | Co-investments, continuation vehicles, covenant breaches, in-kind distributions, side pockets and capital-call forecasting. |
| Product governance, APU and target market | Strong | Due diligence, approved universe, target market, eligibility, complexity, liquidity, lifecycle review, pre-trade integration, numeric target-market scoring, channel/mandate matrix evaluation, disclosure version expiry, booking-centre suspension impact and historical audit replay. | Product outcome monitoring, post-sale target-market drift, exception approval workflows, vulnerable-client safeguards and distribution outside target-market remediation. |
| Real estate, REITs and infrastructure | Strong | Listed REITs, private real estate, direct property, infrastructure funds, income, valuation, liquidity, leverage analytics, property-level debt maturity, tenant concentration, occupancy, lease expiry, concession renewal, renewable power-price exposure, direct ownership transfers, development projects, construction drawdowns, rent-free periods, capex reserves, property sale completion, valuation committee overrides, green-building certification and infrastructure revenue clawbacks. | Zoning changes, compulsory acquisition, property insurance claims, environmental remediation, loan covenant breaches, tenant default workouts, availability deductions and regulated asset base resets. |
| Structured products | Strong | Payoff building blocks, wrappers, underlyings, lifecycle events, valuation, scenario analytics, advisory controls and QA. | Structured funds, bespoke OTC trades, secondary-market unwind, corporate-action adjustments and scenario-model validation. |
| Structured notes | Strong | Note wrappers, barriers, autocalls, coupon behavior, credit-linked notes, dual currency notes and physical settlement. | Range accrual notes, participation notes, issuer default after accrual, partial secondary sale and tax lots after delivery. |
| Tax, regulatory reporting and cross-border reporting | Strong | Income classification, withholding, tax lots, cost basis, CRS/FATCA/QI concepts, client reporting, jurisdiction-specific configuration, report outputs, amended filings, withholding reclaims, beneficial-owner reporting, transferred tax lots, multi-currency gain bases, withholding-pool reconciliation, delivery entitlements, correction notices and late tax breakdowns. | Wash-sale style controls, cross-border tax-lot method conflicts, mid-year residency changes, qualified intermediary pool reporting, estate/trust distribution tax statements and filing rejection workflows. |
| Trade lifecycle, settlement, custody and operations | Strong | Advisory-to-order lifecycle, execution, confirmation, settlement, custody, asset servicing, reconciliation, exceptions, allocation corrections, FX settlement, income reversals, trade cancellations, fee/tax postings, break closure, multi-market allocations, settlement netting, custody account transfers, securities lending recalls, failed corporate-action elections, nostro breaks, omnibus sub-account allocations and market claims. | Intraday settlement cut-offs, partial custody transfers, buy-in exposure, tax-lot transfer portability, cross-custodian SSI migration and operational incident communication. |
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

1. CLS fallback disputes, payment recall, cross-bank liquidity stress, treasury counterparty downgrade and payment sanctions false-positive remediation,
2. repo-funded bond positions, covered-bond resolution events, sukuk asset-sale events, callable step-up non-call campaigns and duration-hedge roll management,
3. omnibus transfer-agent allocation, ETF creation/redemption basket, liquidity bucket reclassification, swing-factor governance and fund-of-funds fee-layering examples,
4. derivative CVA/DVA/FVA input ownership, compression tear-up allocation, cross-currency collateral optionality, lifecycle event replay and valuation model-change approval examples,
5. real estate zoning change, compulsory acquisition, insurance claim, environmental remediation and loan covenant breach examples,
6. tax wash-sale style, cross-border lot-method conflict, mid-year residency change, QI pool reporting and filing rejection examples,
7. intraday settlement cut-off, buy-in exposure, tax-lot transfer portability and cross-custodian SSI migration examples,
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
