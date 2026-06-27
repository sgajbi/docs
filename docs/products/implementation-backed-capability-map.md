# Implementation-Backed Capability Map

This map translates implementation evidence from a private-banking platform ecosystem into neutral product-knowledge guidance.

It is not a branded feature list. Use it to distinguish:

1. capabilities that are commonly implementation-backed in a mature platform,
2. product-specific capabilities that require source contracts and methodology proof,
3. support-limited areas that should be labelled clearly,
4. future candidates worth considering.

## Capability Ownership Model

| Capability Area | Implementation-Backed Meaning | Product Documentation Boundary |
|---|---|---|
| Portfolio source truth | Portfolio, account, holding, transaction, cashflow, valuation, market/reference, mandate, supportability, and analytics-input source products can be represented with governed source ownership. | Source truth does not by itself prove performance, risk, suitability, reporting, execution, or narrative conclusions. |
| Performance analytics | TWR, MWR, benchmark performance, contribution, attribution, composite performance, returns series, benchmark exposure context, supportability, and lineage can be treated as methodology-owned analytics outputs when source inputs are complete. | Product docs must still label partial cashflow history, stale valuations, missing classifications, missing benchmark mappings, and unsupported attribution dimensions. |
| Risk analytics | Portfolio risk, drawdown, rolling metrics, concentration, scenario/stress evaluation, mandate risk health, and selected historical attribution can be modelled as risk-owned outputs with lineage. | Risk views must preserve denominator, exposure lens, scenario definition, supportability, and partial-state behavior. |
| Advisory workflow | Proposal simulation, proposal lifecycle state, policy evidence, suitability posture, alternatives, consent, review, and execution-readiness posture can be modelled as advisory workflow evidence. | Advisory workflow evidence does not equal DPM authority, order execution, fill, settlement, product approval, or client-ready communication. |
| DPM and mandate workflow | Mandate health, construction alternatives, rebalance simulation, wave workflows, proof packs, outcome review, monitoring exceptions, and model-governance evidence can be modelled as management workflow evidence. | DPM workflow evidence must not claim OMS execution, fill confirmation, settlement truth, client communication, or suitability approval unless those facts are separately sourced. |
| Reporting workflow | Portfolio summary, portfolio review, report job lifecycle, section readiness, render/archive handoff, evidence lineage, and supportability can be modelled as reporting composition. | Reporting can aggregate and label upstream evidence; it must not invent source truth, performance methodology, risk methodology, tax conclusions, suitability conclusions, or legal conclusions. |
| Gateway or API composition | Product-facing APIs can compose downstream routes, enforce request conventions, preserve supportability, and expose workflow state. | API composition is not domain ownership. It should not recalculate portfolio, performance, risk, advisory, DPM, reporting, or execution truth locally. |
| Front-office UI rendering | UI surfaces can render portfolio, performance, risk, advisory, DPM, reporting, and evidence states from governed APIs. | UI presence does not prove methodology support. The UI should show source freshness, supportability, blocked states, and ownership boundaries. |
| Narrative assistance | Grounded support narratives can consume bounded evidence, source refs, supportability posture, and review controls. | Narrative assistance must not create advice, approvals, client messages, orders, execution claims, missing source facts, or hidden methodology. |

## Implementation-Backed Versus Candidate Capability

Use the following product-neutral posture when writing product docs, requirements, or QA scenarios.

| Capability | Treat As Implementation-Backed When | Treat As Future Candidate Until |
|---|---|---|
| Source-backed portfolio snapshot | Holdings, cash, transactions, valuations, FX, classifications, source dates, and supportability are sourced and reproducible. | Source ownership, freshness, reconciliation, and identity rules are defined and validated. |
| TWR and MWR | Valuations, external cashflows, FX, portfolio membership, corrections, and lineage are complete enough for the requested period. | Partial history, stale valuations, or missing external-flow classification are resolved. |
| Contribution and attribution | Position returns, weights, classifications, benchmark mappings, FX treatment, and residual policy are complete. | Look-through, classification, benchmark, or cashflow gaps are source-backed. |
| Benchmark and relative analytics | Benchmark identity, return series, exposure context, rebalance policy, currency basis, and effective dates are governed. | Benchmark mapping, return source, and exposure composition are formally sourced. |
| Risk and stress analytics | Risk input series, exposure denominators, scenario definitions, concentration dimensions, and supportability metadata are available. | Scenario methodology, source exposure, and reporting labels are certified for the product family. |
| Advisory proposal | Proposal version, source snapshot, rationale, alternatives, suitability/policy evidence, consent posture, expiry, and audit trail are captured. | Client approval, product eligibility, policy checks, and source snapshots are versioned. |
| DPM rebalance workflow | Model version, mandate, drift, construction alternative, funding, restrictions, approvals, proof evidence, and post-trade outcome review are tracked. | Execution, fills, settlement, client communication, and external OMS acknowledgements are source-backed. |
| Portfolio review report | Upstream evidence, section readiness, key figures, advisor-only notes, client-ready sections, lineage, render/archive state, and supportability are present. | Tax, suitability, recommendation, or legal conclusions are sourced from their authoritative owners. |
| AI-supported explanation | Evidence packets, source refs, forbidden-use controls, review status, and no-raw-payload posture are enforced. | Human review, evidence lineage, redaction, retention, and downstream-use limits are implemented. |

## Product Family Support Posture

| Product Family | Generic Capabilities Usually Backed | Product-Specific Capabilities That Need Proof | Useful Candidate Extensions |
|---|---|---|---|
| Cash, deposits, money market and FX | Account cash, transactions, FX translation, settlement dates, reporting labels. | Available cash, projected cash, deposit breakage, MMF liquidity state, FX forward/NDF lifecycle, hedge ratio. | Funding-gap engine, deposit breakage calculator, cash ladder, hedge-effectiveness workflow. |
| Bonds | Position, price, market value, coupon cash, issuer, rating, maturity. | Accrued interest, clean/dirty price, yield-to-worst, callable schedules, default/recovery, credit-spread attribution. | Issuer-event timeline, call probability workflow, credit-event operations runbook. |
| Equities | Listed holdings, trades, prices, dividends, sector/country/currency tags. | Tax-lot realized P&L, corporate-action election, restrictions, ADR/GDR mapping, suspended/delisted state. | Corporate-action decision workflow, tax-lot optimizer, multi-listing normalization. |
| Funds | Units, NAV, subscriptions/redemptions, distributions, share class, manager. | Dealing calendar, cut-off, gates, suspensions, stale NAV posture, look-through lineage, share-class suitability. | Look-through quality score, order cut-off engine, liquidity-state workflow. |
| Derivatives | Contract, notional, market value, counterparty, expiry, underlying. | Greeks, margin/collateral, fixings, exercise/expiry, reset schedules, CSA terms, hedge effectiveness. | Margin ledger, OTC confirmation matching, central sensitivity engine. |
| Structured products and notes | Issuer, wrapper, underlyings, maturity, issuer quote, coupon/redemption cashflows. | Payoff terms, barrier/autocall state, observation results, scenario payoff, physical delivery, credit event handling. | Term-sheet parser, observation scheduler, payoff schema, delivered-asset workflow. |
| Private markets | Commitment, capital calls, distributions, NAV, valuation date. | Paid-in/unfunded/recallable logic, distribution classification, NAV restatement, side pockets, transfer restrictions, IRR/multiples. | Notice ingestion, cash-call forecast, restatement lineage, PME engine. |
| Real estate, REITs and infrastructure | Listed security/fund holdings, price/NAV, distributions, sector/geography tags. | Property/infrastructure look-through, appraisal value, leverage, redemption queue, direct-property documents. | Appraisal restatement workflow, real-asset exposure taxonomy, income-quality dashboard. |
| Commodities, precious metals and real assets | Holding quantity, price, market value, currency, wrapper classification. | Unit validation, physical/allocated/unallocated distinction, storage fees, futures roll, delivery, collateral haircuts. | Commodity exposure engine, metal inventory support, futures roll workflow. |
| Insurance and annuities | Policy record, insurer, premium cashflows, basic value display. | Value basis, surrender value, guarantees/projections, policy loans, claims, beneficiaries, annuity payout schedule. | Policy contract model, ILP look-through, lapse workflow, retirement-income projection. |
| Loans, Lombard, margin and collateral | Facility, drawdown, repayment, interest, collateral references, utilization. | Pledge graph, haircut/LTV, availability, reservations, margin calls, cure/liquidation workflow. | Cross-pledge allocation, collateral optimization, forced-liquidation simulation. |
| Tax, regulatory reporting and wealth structuring | Client/entity records, product tags, income/cashflow records, reporting perimeter. | Jurisdiction-specific tax treatment, beneficial ownership, authority, trustee/executor roles, reportable-event rules. | Report-output templates, jurisdiction configuration, authority-aware access control. |

## How To Use This Map

1. Start with the product pack and companion map.
2. Use [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md) when translating app READMEs, API contracts, validation evidence, UI proof, report payloads, or runbooks into neutral product guidance.
3. Check whether the requested feature is generic, source-backed, support-limited, or future candidate.
4. Add the source owner, calculation method, degraded state, report output, and QA scenario before calling the feature supported.
5. When a capability is not yet supportable, write the candidate extension and promotion requirements.
6. Keep user-facing docs clear about partial, stale, missing, unsupported, restricted, or estimated states.

## Disclaimer

This map is for product analysis, platform design, reporting design, QA, operations, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.
