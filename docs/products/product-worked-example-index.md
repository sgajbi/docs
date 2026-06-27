# Product Worked Example Index

This index is the entry point for practical product examples across the knowledge base.

Use it when you need a concrete example for learning, advisory review, business analysis, implementation design, QA scenarios, reporting checks, analytics treatment, support-boundary decisions, or future product planning.

## How To Use This Index

1. Start with the product-family table when the product is already known.
2. Start with the workflow table when the question is about a platform process such as lifecycle, valuation, reporting, suitability, rebalancing, authority, or reconciliation.
3. Use [`cross-product-worked-examples.md`](cross-product-worked-examples.md) for compact multi-product examples.
4. Use [`cross-product-worked-examples-calculations-case-studies/`](cross-product-worked-examples-calculations-case-studies/README.md) for deeper cross-product calculations, case studies, QA scenarios and golden cases.
5. Use the local worked-example file in each product pack for deeper product-specific examples and regression tests.

## Product Family Example Files

| Product family | Worked-example file | Primary example coverage |
|---|---|---|
| Bonds | [`bonds/05-worked-examples-and-implementation-patterns.md`](bonds/05-worked-examples-and-implementation-patterns.md) | Clean/dirty settlement, accrued interest, yield, duration, callable bonds, downgrade impact, maturity ladders, default/recovery, inflation-linked uplift, amortising schedules, convertible exposure, asset-backed prepayments and factor restatements, multi-currency performance, sinkable schedules, make-whole calls, distressed exchanges, tender offers, consent solicitations, tax-lot sales, accrued-interest corrections, contingent convertibles, perpetual resets, hedged attribution, multi-book accounting, callable step-up extension risk, covered-bond pool deterioration, sukuk profit distributions, municipal tax-equivalent yield, green-bond label failure, ladder reinvestment, benchmark-duration hedging, repo-funded positions, covered-bond resolution, sukuk asset-sale events, step-up non-call campaigns, green-label remediation and duration-hedge roll management. |
| Cash, deposits, money market and FX | [`cash-deposits-money-market-fx/10-worked-examples-and-implementation-patterns.md`](cash-deposits-money-market-fx/10-worked-examples-and-implementation-patterns.md) | Available cash, deposits, money market funds, FX spot, forwards, NDFs, liquidity ladders, multi-currency buying power, failed FX settlement, cash sweeps, overdraft funding, negative interest, cross-currency settlement holidays, sweep unwind during stress, credit-line-funded purchases, overdraft pricing tiers, intraday liquidity restrictions, payment cut-off failures, liquidity stress escalation, cash pooling, nostro reconciliation, source-calendar governance, payment repair and screening holds, multi-bank cash concentration, CLS/PVP settlement controls, treasury cash placement, CLS fallback disputes, payment recall, cross-bank liquidity stress, treasury counterparty downgrades, same-day liquidity allocation and sanctions false-positive remediation. |
| Client, account, portfolio and relationship master data | [`client-account-portfolio-master-data/11-worked-examples-and-implementation-patterns.md`](client-account-portfolio-master-data/11-worked-examples-and-implementation-patterns.md) | Client/account hierarchy, booking centre, household, beneficial owner, mandate, access control, reporting scope, migration, joint accounts, EAM relationships, trust/company structures, closed-account reporting, account transfers, privacy masking and advisor coverage changes. |
| Commodities, precious metals and real assets | [`commodities-precious-metals-real-assets/10-worked-examples-and-implementation-patterns.md`](commodities-precious-metals-real-assets/10-worked-examples-and-implementation-patterns.md) | Allocated and unallocated gold, futures rolls, ETN issuer risk, pledged metal haircuts, commodity-linked notes, fund look-through, storage-fee accrual, paid-in-kind metal fees, commodity option exercise, warehouse receipts, natural-resource fund look-through, index roll yield and delivery-risk workflows. |
| Data governance, quality, lineage and controls | [`data-governance-quality-lineage-controls/11-worked-examples-and-implementation-patterns.md`](data-governance-quality-lineage-controls/11-worked-examples-and-implementation-patterns.md) | Quality scorecards, lineage, reconciliation, control certification, migration, source outage, report readiness and incident triage. |
| Derivatives | [`derivatives/09-worked-examples-and-implementation-patterns.md`](derivatives/09-worked-examples-and-implementation-patterns.md) | Options, covered calls, futures variation margin, FX forwards, NDFs, swaps, CDS, multi-leg option strategies, hedge effectiveness, OTC novation/compression, central clearing, collateral optimization, portfolio Greeks, variance swaps, barrier options, cross-currency swaps, exercise windows, collateral disputes, clearing-broker default, strategy-level attribution, SIMM-style margin inputs, volatility-surface shocks, callable swaps, equity swap dividend adjustments, futures delivery notices, prime-broker give-ups, early termination breakage, close-out netting sets, independent amount disputes, CVA/DVA/FVA valuation adjustments, compression tear-up allocations, cross-currency collateral optionality, lifecycle event replay, barrier observation disputes, uncleared-margin phase-in, exchange position limits, cleared OTC porting, valuation model-change approvals, margin/collateral and exposure lenses. |
| Entitlements, digital channels and advisor workbench | [`entitlements-digital-channels-advisor-workbench/11-worked-examples-and-implementation-patterns.md`](entitlements-digital-channels-advisor-workbench/11-worked-examples-and-implementation-patterns.md) | RBAC/ABAC, delegated access, advisor workbench, digital delivery, workflow approvals, consent, interaction history and audit. |
| Equities | [`equities/09-worked-examples-and-implementation-patterns.md`](equities/09-worked-examples-and-implementation-patterns.md) | Settlement, dividends, withholding, corporate actions, cost basis, ADRs, multi-currency P&L, contribution and delisting. |
| Funds | [`funds/05-worked-examples-and-implementation-patterns.md`](funds/05-worked-examples-and-implementation-patterns.md) | Subscriptions, redemptions, NAV confirmation, gates, distributions, share classes, ETFs, look-through, capital calls, NAV restatements, equalization, share-class conversion, fund mergers, notice-period holdbacks, transfer-agent rejects, fund-of-funds look-through, performance-fee crystallization, in-specie redemptions, custody re-registration, suspension reopenings, stale look-through override expiry, cross-border eligibility changes, tax-status changes, trailer-fee rebates, levy disputes, side-pocket exits, closure distributions, master-feeder restructures, omnibus transfer-agent allocations, ETF creation/redemption baskets, liquidity bucket reclassification, swing-factor governance, fund-of-funds fee layering, side-letter liquidity terms, class-action proceeds and fund-platform migration. |
| Insurance and annuities | [`insurance-annuities/11-worked-examples-and-implementation-patterns.md`](insurance-annuities/11-worked-examples-and-implementation-patterns.md) | ILPs, charges, surrender, policy loans, lapse, claims, annuity payouts, premium financing, replacement review and reporting labels. |
| Investment accounting and ledger | [`investment-accounting-ledger/11-worked-examples-and-implementation-patterns.md`](investment-accounting-ledger/11-worked-examples-and-implementation-patterns.md) | Transaction legs, trade/settlement views, accruals, fees, withholding, realized/unrealized P&L, FX translation, valuation and reconciliation. |
| Loans, Lombard, margin and collateral | [`loans-lombard-margin-collateral/10-worked-examples-and-implementation-patterns.md`](loans-lombard-margin-collateral/10-worked-examples-and-implementation-patterns.md) | Availability, haircuts, FX haircuts, rating downgrades, margin calls, forced liquidation, reservations, collateral substitution and interest accrual. |
| Market data, reference data, pricing and instrument master | [`market-data-reference-data-pricing-instrument-master/11-worked-examples-and-implementation-patterns.md`](market-data-reference-data-pricing-instrument-master/11-worked-examples-and-implementation-patterns.md) | Instrument identity, identifiers, pricing hierarchy, stale prices, FX rates, curves, benchmarks, corporate actions, source ownership and overrides. |
| Model portfolios, rebalancing, advisory and suitability | [`model-portfolios-rebalancing-advisory-suitability/11-worked-examples-and-implementation-patterns.md`](model-portfolios-rebalancing-advisory-suitability/11-worked-examples-and-implementation-patterns.md) | Model version rollout, drift, funding constraints, tax-aware rebalance, suitability failures, proposal expiry, DPM restrictions and residual drift. |
| Portfolio performance, attribution, risk and benchmarks | [`portfolio-performance-attribution-risk-benchmark/11-worked-examples-and-implementation-patterns.md`](portfolio-performance-attribution-risk-benchmark/11-worked-examples-and-implementation-patterns.md) | TWR, MWR, cashflows, contribution, attribution, benchmarks, drawdown, stress, mandate analytics and data-lineage supportability. |
| Portfolio reporting and client experience | [`portfolio-reporting-statement-client-experience/11-worked-examples-and-implementation-patterns.md`](portfolio-reporting-statement-client-experience/11-worked-examples-and-implementation-patterns.md) | Reporting snapshots, holdings labels, activity classification, degraded data, restatements, PDF rendering, delivery and client review. |
| Private markets | [`private-markets/11-worked-examples-and-implementation-patterns.md`](private-markets/11-worked-examples-and-implementation-patterns.md) | Commitment ledgers, capital calls, distributions, NAV lag, restatements, partial-history IRR, private credit, gates and secondary sales. |
| Product governance, APU and target market | [`product-governance-apu-target-market/11-worked-examples-and-implementation-patterns.md`](product-governance-apu-target-market/11-worked-examples-and-implementation-patterns.md) | Due diligence, approved product universe, target market, eligibility, product risk, disclosure gates, suspension, audit replay, stakeholder artifacts, numeric target-market scoring, channel/mandate matrix evaluation, disclosure version expiry and booking-centre-specific suspension impact. |
| Real estate, REITs and infrastructure | [`real-estate-reits-infrastructure/10-worked-examples-and-implementation-patterns.md`](real-estate-reits-infrastructure/10-worked-examples-and-implementation-patterns.md) | REIT distributions, rights issues, private real estate calls, appraisal restatements, infrastructure distributions, redemption queues, direct property valuation, leverage, property-level debt maturity, tenant concentration, occupancy, lease expiry, concession renewal, renewable power-price exposure, ownership transfers, development projects, construction drawdowns, rent-free periods, capex reserves, property sale completion, valuation committee overrides, green-building certification and infrastructure revenue clawbacks. |
| Structured notes | [`structured-notes/05-worked-examples-and-implementation-patterns.md`](structured-notes/05-worked-examples-and-implementation-patterns.md) | Fixed coupon notes, reverse convertibles, Phoenix notes, credit-linked notes, dual currency notes, barrier reporting and physical delivery. |
| Structured products | [`structured-products/08-worked-examples-and-implementation-patterns.md`](structured-products/08-worked-examples-and-implementation-patterns.md) | Autocallables, memory coupons, principal protection, buffers, dual currency investments, accumulators, decumulators, certificates and valuation controls. |
| Tax, regulatory reporting and cross-border reporting | [`tax-regulatory-reporting/10-worked-examples-and-implementation-patterns.md`](tax-regulatory-reporting/10-worked-examples-and-implementation-patterns.md) | Dividends, withholding, accrued interest, tax lots, corporate-action basis, fund distributions, structured-note delivery, documentation expiry, corrections, jurisdiction configuration, report outputs, amended filings, withholding reclaims, beneficial-owner reporting, tax-lot transfers, multi-currency gain bases, withholding-pool reconciliation, delivery entitlements, client correction notices and late tax-breakdown ingestion. |
| Trade lifecycle, settlement, custody and operations | [`trade-lifecycle-operations/11-worked-examples-and-implementation-patterns.md`](trade-lifecycle-operations/11-worked-examples-and-implementation-patterns.md) | Orders, partial fills, accrued-interest settlement, settlement fails, fund cut-offs, entitlement revisions, data contracts, allocation corrections, linked FX settlement, income reversals, trade cancellations, fee/tax postings, performance restatements, reconciliation break closure, multi-market allocations, settlement netting, custody transfers, securities lending recalls, failed corporate-action elections, nostro breaks, omnibus sub-account allocations and market claims. |
| Trusts, estate, family office and wealth structuring | [`trusts-estate-family-office-wealth-structuring/10-worked-examples-and-implementation-patterns.md`](trusts-estate-family-office-wealth-structuring/10-worked-examples-and-implementation-patterns.md) | Trust distributions, estate account restrictions, holding-company pledges, beneficiary access, family-office reporting, policy roles, authority changes, protector vetoes, directed trusts, investment committees, foundation council changes, beneficiary classes, VCC sub-funds and cross-border tax residency changes. |

## Cross-Product Example Sets

Use [`cross-product-worked-examples.md`](cross-product-worked-examples.md) when a workflow cuts across product, cash, reporting, collateral, tax, lifecycle or authority boundaries.

Use [`cross-product-worked-examples-calculations-case-studies/`](cross-product-worked-examples-calculations-case-studies/README.md) when the workflow needs fuller setup data, expected transaction records, calculation detail, reporting output, AI/support workflow treatment, or a reusable QA golden scenario.

| Example set | Start with |
|---|---|
| Cash availability, liquidity and secured funding | Examples 1, 10, 20 and 27. |
| Fixed-income and structured-income behavior | Examples 2, 19, 26 and 31. |
| Corporate actions and source-owned lifecycle events | Examples 3, 21 and 29. |
| Fund liquidity and valuation stress | Examples 4, 20, 24 and 28. |
| Collateral, leverage and pledged assets | Examples 5, 11, 19, 27 and 30. |
| Private-market and illiquid-asset reporting | Examples 6, 13, 24 and 25. |
| Derivatives, overlays and notional exposure | Examples 7 and 22. |
| Structured-product observations and support boundaries | Examples 8, 9, 23 and 31. |
| Insurance value, policy loans and restrictions | Examples 12 and 30. |
| Portfolio analytics, model portfolios and advisory workflow | Examples 14, 15, 16, 17 and 18. |

## Workflow-To-Example Map

| Workflow or question | Start here |
|---|---|
| Legal holding versus analytical exposure | Structured products, structured notes, derivatives, commodities, real estate, private markets, wealth structuring. |
| Cashflow lifecycle and event booking | Cash/FX, bonds, funds, structured notes, private markets, insurance, loans, trade lifecycle, investment accounting. |
| Payoff and scenario behavior | Structured products, structured notes, derivatives, bonds, insurance, commodities, loans/collateral. |
| Valuation and stale-source handling | Market data, funds, bonds, derivatives, structured products, private markets, real estate, insurance, portfolio reporting. |
| Performance, contribution and attribution | Portfolio performance, investment accounting, equities, bonds, funds, derivatives, private markets, structured products. |
| Risk, exposure and concentration | Portfolio exposure guide, derivatives, structured products, commodities, loans/collateral, real estate, model portfolios. |
| Advisory, suitability and mandate controls | Product governance, model portfolios, structured products, derivatives, private markets, insurance, trusts/wealth structuring. |
| DPM and rebalance workflows | Model portfolios, portfolio construction guide, cash/FX, funds, private markets, tax reporting, trade lifecycle. |
| Client reporting and statement behavior | Portfolio reporting, tax reporting, trusts/wealth structuring, private markets, real estate, insurance, market data. |
| Source ownership and support boundaries | Product capability boundary matrix, implementation-backed capability map, market data, data governance, product governance, source ownership matrix. |
| QA and regression pack design | Every local worked-example file ends with regression scenarios; use [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md) for cross-product release gates. |

## Coverage Rule

Every product pack should keep a local worked-example file that includes:

1. representative lifecycle events,
2. numeric calculations where useful,
3. cashflow behavior,
4. valuation or pricing basis,
5. performance/risk/reporting treatment,
6. advisory, suitability or mandate implications,
7. implementation boundaries,
8. degraded or missing-source states,
9. regression scenarios.

When adding a new product pack, update this index, [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md), the product [`README.md`](README.md), and the repository root [`../../README.md`](../../README.md).
