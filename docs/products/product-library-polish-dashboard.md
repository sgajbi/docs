# Product Library Polish Dashboard

This dashboard is the concise operating view for the product knowledge base. It shows where the library is strong, where the next professional polish should go, and how each product family should be reviewed for private-banking, wealth-management, advisory, DPM, reporting, analytics, QA and implementation work.

Use this dashboard with:

1. [`product-documentation-review-rubric.md`](product-documentation-review-rubric.md),
2. [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md),
3. [`product-worked-example-index.md`](product-worked-example-index.md),
4. [`cross-product-transaction-position-data-model.md`](cross-product-transaction-position-data-model.md),
5. [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md).

The coverage matrix remains the deep tracking artifact. This file should stay readable enough for planning and review.

## Review Standard

Every product pack should be strong across these dimensions.

| Dimension | Professional Standard |
|---|---|
| Product understanding | Business purpose, target user, product family, subtype, wrapper, issuer/provider, legal holding and out-of-scope boundaries are clear. |
| Taxonomy and coverage | The pack separates product family, instrument type, account/contract wrapper, exposure type and service model. |
| Payoff behavior | Linear, conditional, optional, path-dependent, credit-linked, insurance-like, liability-like and residual-value behavior is explained where relevant. |
| Cashflow behavior | Orders, settlement, income, fees, taxes, margin, collateral, capital calls, premiums, redemptions, claims, maturities and corrections are separated. |
| Valuation and pricing | Source owner, valuation basis, price/NAV/rate hierarchy, stale state, override, model value, tradability and disclosure implications are explicit. |
| Analytics and risk | Performance, contribution, attribution, exposure, concentration, liquidity, issuer, counterparty, Greeks, duration, LTV, commitment and stress treatment are not confused. |
| Portfolio modelling | Position, transaction, cash, lifecycle-event, valuation, exposure, pledge, commitment and reporting views are aligned to the cross-product model. |
| Advisory and suitability | Eligibility, complexity, liquidity, risk rating, concentration, horizon, mandate fit, client authority, disclosure and approval controls are documented. |
| DPM and mandate relevance | The pack explains how the product affects model portfolios, restrictions, drift, rebalancing, mandate monitoring and investment committee oversight. |
| Reporting and calculations | Formulas, report labels, source inputs, degraded-state behavior, reconciliation points and user explanations are testable. |
| Implementation and QA | APIs, UI states, source ownership, support boundaries, edge cases, golden examples, regression tests and operational controls are practical. |

## Status Legend

| Status | Meaning |
|---|---|
| Strong | Ready as a reusable product reference; future work is incremental example depth or source-backed refinement. |
| Targeted | Broadly usable, but one or two focused workflows or calculations would materially improve it. |
| Needs slice | Requires a deliberate polish pass before relying on it as a mature reference. |

## Product-Family Dashboard

| Product Area | Posture | Strongest Coverage | Next Useful Polish Slice |
|---|---|---|---|
| Bonds | Strong | Taxonomy, lifecycle, accrued interest, clean/dirty settlement, yield, duration, credit events, callable features, collateral eligibility and worked examples. | Add compact worked examples for complex correction chains: call reinstatement, escrow reversal, ABS statement withdrawal, sovereign notice failure and private-placement clawback. |
| Cash, deposits, money market and FX | Strong | Ledger cash, available cash, value dates, deposits, money market products, FX spot/forward/swap, liquidity, buying power, payment repair and treasury controls. | Add end-to-end examples for payment bounceback, sweep duplicate suppression, deposit amended notice and FX hedge allocation dispute. |
| Client, account, portfolio and relationship master data | Strong | Client hierarchy, account/portfolio structures, households, beneficial ownership, booking centres, mandates, access control and relationship lifecycle. | Add more edge cases for dormant accounts, power-of-attorney expiry, minor-to-adult transition and collateral-only portfolios. |
| Commodities, precious metals and real assets | Strong | Physical metals, metal accounts, commodity ETPs, derivatives, storage, unit controls, collateral, delivery risk and real-asset exposure. | Add examples for assay disputes, vault transfers, inventory financing, commodity ETP tax events and source-of-metal restrictions. |
| Data governance, quality, lineage and controls | Strong | Source ownership, golden copy, data-quality rules, reconciliation, lineage, migration controls, certification and operating evidence. | Add practical scorecard examples for stale data, partial lineage, source outage fallback and report certification. |
| Derivatives | Strong | Options, futures, forwards, swaps, CDS, Greeks, margin, collateral, lifecycle events, OTC controls, valuation and hedge/overlay treatment. | Add examples for option waiver renewal denial, futures invoice dispute reversal, cleared swap collateral ageing and volatility-surface fallback review. |
| Entitlements, digital channels and advisor workbench | Strong | RBAC/ABAC, relationship-based access, delegation, workbench workflows, digital channels, secure messaging, interaction history and audit. | Add more journeys for legal delegates, EAM access, family-office portals, consent expiry and access recertification campaigns. |
| Equities | Strong | Trading, settlement, dividends, corporate actions, cost basis, P&L, performance, risk, market operations and advanced lifecycle events. | Add compact examples for restricted stock, proxy-vote corrections, manufactured dividends, short-sale margin and spin-off cost allocation appeals. |
| Funds | Strong | Mutual funds, ETFs, share classes, subscriptions/redemptions, NAV, fees, distributions, look-through, liquidity, advisory and worked examples. | Add examples for swing pricing, fund gates, side pockets, share-class conversions, ETF in-kind flows and trailer-fee restatements. |
| Insurance and annuities | Strong | Policy contracts, ILPs, protection, annuities, premiums, claims, surrender, policy loans, beneficiaries, valuation and reporting. | Add examples for beneficiary disputes, premium holidays, partial surrender, policy loan interest, annuity payment suspension and lapse reinstatement. |
| Investment accounting and ledger | Strong | Books of record, transaction legs, cash, positions, lots, accruals, fees, withholding, realized/unrealized P&L, FX and reconciliation. | Add more examples for multi-book adjustments, backdated accrual correction, transferred lots, amortization policy change and fee reversal. |
| Loans, Lombard, margin and collateral | Strong | Credit lines, drawdowns, interest, fees, collateral pledges, LTV, haircuts, buying power, margin calls and liquidation workflows. | Add examples for collateral substitution failure, cross-pledge dispute, private-market NAV haircut, margin-call cure failure and loan repricing. |
| Market data, reference data, pricing and instrument master | Strong | Identifiers, classifications, pricing hierarchy, stale-price controls, FX rates, curves, benchmarks, corporate actions and source ownership. | Add examples for symbology collision, proxy pricing, benchmark restatement, price challenge workflow and override expiry. |
| Model portfolios, rebalancing, advisory and suitability | Strong | Model portfolios, SAA/TAA, drift, proposals, suitability, restrictions, client consent, DPM controls and pre-trade checks. | Add examples for household-level staging, cash sleeve overlays, optimizer constraints, wave rebalancing and FX-hedged model drift. |
| Portfolio performance, attribution, risk and benchmarks | Strong | TWR, MWR, contribution, attribution, benchmarks, composites, exposure, concentration, stress, drawdown and reporting lineage. | Add examples for multi-period attribution linking, private-market IRR edge cases, benchmark carve-outs, fee/tax bases and factor risk. |
| Portfolio reporting and client experience | Strong | Statements, portfolio reviews, holdings, activity, cashflows, income, performance, risk, exceptions, PDF delivery and archive controls. | Add examples for multilingual statements, correction notices, family-office consolidated packs, report entitlements and commentary workflow. |
| Private markets | Strong | Commitments, capital calls, distributions, NAV lag, multiples, IRR, liquidity, gates, restatements, secondaries and reporting. | Add examples for continuation vehicles, capital-call forecasting, co-investments, in-kind distributions, side pockets and covenant breaches. |
| Product governance, APU and target market | Strong | Due diligence, approved universe, target market, eligibility, risk rating, complexity, liquidity, lifecycle review and pre-trade controls. | Add examples for outcome monitoring, target-market drift, vulnerable-client safeguards, exception approvals and out-of-target remediation. |
| Real estate, REITs and infrastructure | Strong | Listed REITs, private real estate, direct property, infrastructure, income, valuation, liquidity, leverage and project/concession risks. | Add examples for property valuation appeal, REIT index restatement, concession reserve reversal, renewable hedge clawback and data-centre power covenant breach. |
| Structured products | Strong | Payoff building blocks, wrappers, barriers, autocalls, underlyings, scenario analytics, valuation, advisory controls and reconciliation. | Add examples for corporate-action adjustments, secondary unwind, bespoke OTC wrapper, scenario-model validation and structured fund treatment. |
| Structured notes | Strong | Note wrappers, coupon behavior, barriers, autocalls, credit-linked notes, dual-currency notes, physical settlement and note lifecycle. | Add examples for range accrual notes, participation notes, partial secondary sale, issuer default after accrual and tax lots after delivery. |
| Tax, regulatory and cross-border reporting | Strong | Income classification, withholding, tax lots, CRS/FATCA/QI concepts, client statements, report corrections and jurisdiction controls. | Add examples for amended-report sequencing, reclaim appeal partial acceptance, schema waiver renewal rejection and delegated advisor expiry. |
| Trade lifecycle, settlement, custody and operations | Strong | Proposal-to-order lifecycle, execution, confirmation, settlement, custody, asset servicing, exceptions, market claims and reconciliation. | Add examples for custody freeze release reversal, oversubscription refund interest, tri-party substitution repair and proxy-vote withdrawal request. |
| Trusts, estate, family office and wealth structuring | Strong | Trusts, foundations, estates, family offices, holding vehicles, beneficial ownership, authority, access control and reporting hierarchy. | Add examples for redirected-payment duplicate suppression, sponsor default refund interest, cash-call objection settlement and authority appeal reinstatement. |

## Cross-Product Reference Posture

| Reference Area | Posture | What It Provides | Next Useful Polish Slice |
|---|---|---|---|
| Transaction, position and lifecycle model | Strong | Cross-product transaction types, legs, lifecycle events, positions, cash, valuation, corporate actions, corrections, controls and API guidance. | Keep aligned with implementation evidence; convert into schemas or API examples if implementation work begins. |
| Worked examples and case studies | Strong | Product-family examples, calculation case studies, reporting outputs, QA scenarios, golden cases and implementation patterns. | Add scenario packs for the highest-value edge cases listed in this dashboard. |
| Product taxonomy and vocabulary | Strong | Shared language for product family, subtype, wrapper, legal holding, exposure, lifecycle, valuation, liquidity and supportability states. | Add a smaller executive glossary for non-technical stakeholders. |
| Source ownership and calculation dependencies | Strong | Source-owner maps, calculation inputs, reporting dependencies, data-quality states and implementation boundaries. | Add example lineage packets for one trade, one corporate action, one valuation override and one report correction. |
| Product capability boundaries | Strong | Generic baseline support, source-backed extensions, support-limited states, future candidates, reporting claims and QA evidence. | Periodically refresh from implementation evidence and open product gaps. |
| Product review rubric and role guide | Strong | Review dimensions and role-based navigation for business users, BAs, developers, architects, QA, operations, advisory and product development. | Add one-page KT sequence for onboarding a new product analyst or engineering lead. |

## Priority Backlog

The next useful work is not to add more broad topics. The library already has broad coverage. The highest-value improvements are compact, realistic examples that show how product, cash, lifecycle, source, reporting and supportability records connect.

| Priority | Slice | Why It Matters |
|---|---|---|
| 1 | Correction-chain examples across bonds, cash/FX, derivatives and reporting. | These are hard to model and often expose whether transaction, lifecycle, cash and report evidence are correctly connected. |
| 2 | Source-lineage packets for trade, corporate action, valuation override and report correction. | They help BAs, developers, QA and operations understand evidence requirements. |
| 3 | Mandate and suitability edge cases across structured products, private markets, lending and model portfolios. | These connect product knowledge to advisory and DPM controls. |
| 4 | Degraded-state reporting examples for stale NAV, stale price, blocked cash, unreconciled corporate action and unsupported product terms. | These improve client reporting, UI design, QA and operational support. |
| 5 | Implementation-backed capability refresh. | This keeps neutral product documentation aligned with real supported behavior without branding the KB. |

## Completion Criteria For Product-Pack Polish

A product pack is ready to call professionally polished when it can answer these questions without relying on tribal knowledge:

1. What is the product, what business purpose does it serve, and what variants are in or out of scope?
2. What positions, transactions, cashflows, lifecycle events, valuations and exposures must the platform represent?
3. What calculations are required for income, cost, accrued interest, valuation, performance, attribution, risk, suitability and reporting?
4. What source systems or documents own the product terms, lifecycle events, valuations, cash movements, tax treatment and reporting labels?
5. What advisory, DPM, mandate, liquidity, concentration, complexity and disclosure controls apply?
6. What should the client report or advisor screen show when data is complete, partial, stale, overridden, unsupported or under correction?
7. What are the normal, edge, degraded, correction and migration test scenarios?
8. Which capabilities are generic baseline support, source-backed extensions, support-limited states or future candidates?

## Maintenance Rule

Update this dashboard when:

1. a product pack gains material new examples,
2. the coverage matrix changes a product area's next enrichment guidance,
3. implementation evidence changes a support boundary,
4. a new cross-product standard changes how product packs should be reviewed,
5. a product area moves from targeted coverage to strong coverage.

Keep this file concise. Put long scenario catalogs, detailed source-owner notes and exhaustive edge-case lists in the coverage matrix or worked-example indexes.
