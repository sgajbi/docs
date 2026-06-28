# Implementation-Backed Capability Map

This map translates implementation evidence from a private-banking platform ecosystem into neutral product-knowledge guidance.

It is not a branded feature list. Use it to distinguish:

1. capabilities that are commonly implementation-backed in a mature platform,
2. product-specific capabilities that require source contracts and methodology proof,
3. support-limited areas that should be labelled clearly,
4. future candidates worth considering.

## Evidence Review Snapshot

This snapshot summarizes recurring implementation evidence patterns observed across a portfolio-platform ecosystem. It is intentionally neutral: it records capability boundaries and promotion rules, not application names or marketing claims.

| Evidence Area | Evidence Pattern Observed | Neutral Capability Guidance |
|---|---|---|
| Source data foundation | Foundational services define portfolio, account, holding, transaction, cashflow, valuation, market/reference, mandate, data-quality and supportability ownership. | Product docs can rely on source-owned portfolio facts only when identity, source version, freshness, reconciliation and supportability metadata are present. |
| Performance analytics | Dedicated analytics contracts cover TWR, MWR, contribution, attribution, benchmark context, composite performance, return series, supportability and lineage. | Performance guidance should treat methodology-owned outputs as distinct from source data, and should label partial history, missing flow classification, stale valuations and unsupported attribution dimensions. |
| Risk analytics | Risk contracts cover portfolio risk, drawdown, rolling metrics, concentration, scenario/stress evaluation, risk events, mandate risk context, decomposition and audit lineage. | Risk guidance should preserve exposure denominator, scenario definition, product-family coverage, input supportability and reporting labels. |
| Advisory workflow | Advisory workflow evidence covers proposal simulation, proposal lifecycle, policy evidence, alternatives, review posture, replay, consent-related posture and bounded narrative evidence. | Advisory guidance should separate recommendation evidence from product approval, discretionary authority, execution, settlement and client-ready publication. |
| DPM workflow | Management workflow evidence covers rebalance simulation, mandate health, construction alternatives, waves, proof packs, outcome review, monitoring exceptions, action registers and run supportability. | DPM guidance should separate simulation and workflow readiness from order creation, OMS routing, fill confirmation, settlement truth and client communication. |
| Reporting workflow | Reporting evidence covers summary/review payloads, report job lifecycle, section readiness, render/archive handoff, immutable input snapshots, upstream call lineage, hashing and support-safe metadata. | Reporting guidance should treat reports as evidence composition; reports must not invent portfolio, performance, risk, tax, suitability or legal truth. |
| Gateway and experience API | Experience APIs compose route families, preserve upstream supportability, expose degraded/partial/unavailable/permission-blocked states and keep domain ownership upstream. | API guidance should distinguish composition from domain authority and require bounded status, reason codes, entitlement posture and source lineage. |
| Product UI | UI evidence renders gateway-backed data, supportability states, workflow panels, report status, archived-document metadata and operational posture without calculating domain truth locally. | UI guidance should state that a visible panel proves presentation support only when backed by supported API behavior and source-owned data. |
| AI assistance | AI capability evidence covers governed execution, workflow packs, provider routing, guardrails, evidence references, no-raw-payload controls, review-required posture and forbidden actions. | AI guidance should require grounding, source references, safety controls, review gates and explicit limits on advice, approval, execution, client communication and domain authority. |
| Engineering governance | Repositories use OpenAPI, vocabulary, domain-product, trust-telemetry, security, Docker/runtime, observability, validation-lane and quality-gate evidence. | Product docs should cite support as source-backed only when contracts, tests, telemetry and runbooks can prove normal and degraded behavior. |

## Capability Promotion Matrix

Use this matrix before converting an observed implementation behavior into a product-support claim.

| Capability Stage | Meaning | Minimum Evidence | Documentation Wording |
|---|---|---|---|
| Concept candidate | Useful capability idea, but not yet source-backed. | Business purpose, target users, required source owner, required calculations and required controls. | "Candidate capability; not yet supportable without source and method evidence." |
| Source-backed foundation | Required facts are available from authoritative sources. | Source contracts, identity rules, freshness rules, data-quality state, lineage and reconciliation evidence. | "Source-backed foundation exists for the stated facts." |
| Methodology-backed output | Calculation or workflow method is owned and testable. | Methodology owner, input contract, formula or workflow rule, supportability states and regression evidence. | "Methodology-backed for the stated scope and assumptions." |
| Composed product surface | API, UI or report composes upstream evidence safely. | Experience contract, supportability mapping, entitlement checks, reason codes, report labels and degraded-state behavior. | "Composed surface for upstream evidence; not domain authority." |
| Client-ready or operator-ready output | Output can be used in the intended business process. | Review/sign-off state, evidence packet, source version, archive/render status, delivery controls and operational runbook. | "Ready only for the named audience, purpose, date and evidence scope." |
| Unsupported or support-limited | Capability lacks required source, method, controls or evidence. | Known gap, affected products, blocked claims and promotion requirements. | "Unsupported or support-limited; show reason and next evidence needed." |

## Evidence-To-Documentation Rules

| If Evidence Shows | Documentation May Say | Documentation Must Not Say |
|---|---|---|
| A source service owns portfolio facts. | Portfolio facts require source owner, freshness, data quality, lineage and reconciliation. | Downstream analytics, reports or advice are automatically correct. |
| A methodology service returns a calculation. | Calculation is methodology-backed for the specified input scope and supportability state. | Every product subtype, missing input, stale price or partial history is supported. |
| A report service composes a review payload. | Reporting composes upstream evidence and must carry section readiness and lineage. | Reporting owns portfolio truth, tax conclusions, risk methodology or suitability approval. |
| A gateway exposes a route. | The experience API composes and stabilizes a product-facing contract. | The gateway is the domain owner or calculation engine. |
| A UI renders a panel. | The user experience can present supported upstream states. | The UI proves full backend support or client-ready output by itself. |
| An AI workflow produces a narrative. | AI assistance can summarize bounded evidence under guardrails and review posture. | AI output is advice, approval, order instruction or client communication without separate authority. |
| A validation run passes a scenario. | That scenario is tested for the named data, product and workflow scope. | Adjacent products, jurisdictions, wrappers, booking centres or edge cases are certified. |

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

## Implementation-Backed Product Review Questions

Use these questions when refreshing a product pack from implementation evidence:

1. Which source owns the product facts, lifecycle event, valuation input, transaction leg, cash movement or client/reporting scope?
2. Which methodology owns the calculation, workflow decision, supportability state or analytics output?
3. Which product families, wrappers, booking centres, currencies, client segments and report types are actually covered?
4. Which states are `READY`, `PARTIAL`, `STALE`, `DEGRADED`, `UNAVAILABLE`, `PERMISSION_BLOCKED`, `UNSUPPORTED` or `UNKNOWN`?
5. Which API, UI or report surfaces only compose upstream evidence?
6. Which source fields, calculation policy versions, report labels, reason codes and QA scenarios prove the claim?
7. Which adjacent capabilities are intentionally blocked because source, method, entitlement, review or operational evidence is missing?
8. Which future candidate would become supportable if source contracts, controls, tests and runbooks were added?

## How To Use This Map

1. Start with the product pack and companion map.
2. Use [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md) when translating app READMEs, API contracts, validation evidence, UI proof, report payloads, or runbooks into neutral product guidance.
3. Check whether the requested feature is generic, source-backed, support-limited, or future candidate.
4. Add the source owner, calculation method, degraded state, report output, and QA scenario before calling the feature supported.
5. When a capability is not yet supportable, write the candidate extension and promotion requirements.
6. Keep user-facing docs clear about partial, stale, missing, unsupported, restricted, or estimated states.

## Disclaimer

This map is for product analysis, platform design, reporting design, QA, operations, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.
