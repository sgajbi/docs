# Product Documentation Review Rubric

Use this rubric when polishing any product pack, companion map, calculation guide, QA matrix, or reporting note.

The standard is practical: a product document should help a business user understand the product, a BA write better requirements, a developer model the domain safely, QA design meaningful tests, operations identify reconciliation breaks, and advisory/reporting teams explain the output without overstating support.

## Review Outcome

After review, each product area should have:

1. a clear product mental model,
2. a practical taxonomy,
3. payoff and cashflow explanations,
4. valuation and pricing treatment,
5. performance, contribution, attribution, and risk treatment,
6. portfolio exposure and modelling guidance,
7. advisory, suitability, DPM, and mandate implications,
8. reporting requirements and labels,
9. calculation requirements and source inputs,
10. edge cases and implementation considerations,
11. implementation-backed capability boundaries,
12. future capability candidates,
13. worked examples that make the product easier to understand and test.

## Product Review Dimensions

| Dimension | What To Improve | Evidence Of Good Quality |
|---|---|---|
| Product understanding | Explain what the client legally owns, why it exists, and what business purpose it serves. | A reader can explain the product in plain language without confusing legal holding, economic exposure, and reporting classification. |
| Taxonomy and coverage | Separate product family, subtype, wrapper, legal form, service model, and adjacent products. | In-scope, adjacent, partial, and out-of-scope product variants are explicit. |
| Payoff behavior | Describe what drives gain, loss, income, redemption, conversion, delivery, or termination. | Conditional, path-dependent, leveraged, contingent, and capped/floored outcomes are not simplified into linear return. |
| Cashflow behavior | Distinguish booked, projected, contractual, discretionary, conditional, net, gross, internal, and external flows. | Transaction, lifecycle event, settlement, accrual, entitlement, obligation, fee, and tax treatment are separated. |
| Valuation and pricing | Identify price/NAV/quote/model/appraisal hierarchy, source owner, date, stale state, and fallback. | Reports can show source date, valuation basis, supportability state, and whether a value is tradable, estimated, projected, or guaranteed. |
| Performance and attribution | Explain price return, income, FX, fees, taxes, external flows, contribution, attribution, and benchmark context. | Unsupported residuals and partial look-through are labelled instead of hidden in unexplained return. |
| Risk analytics | Cover market, credit, rate, spread, FX, liquidity, concentration, counterparty, leverage, volatility, collateral, and product-specific risks. | Risk denominator and exposure basis are clear. Market value is not used as a substitute for notional, delta, commitment, collateral, or liquidity exposure. |
| Portfolio modelling | Separate legal position, accounting position, analytical exposure, look-through, notional, sensitivity, commitment, and pledge state. | Portfolio views can reconcile from source holdings to exposure reports without inventing missing economic facts. |
| Advisory and suitability | Identify what must be explained, checked, escalated, or documented before recommendation or review. | Complexity, liquidity, cost, concentration, leverage, tax, restriction, and client-authority issues are visible. |
| DPM and mandate relevance | Explain target allocation, eligibility, restrictions, drift, rebalance, tolerance bands, model versions, overrides, and exceptions. | Model output is not treated as executable until funding, settlement, restrictions, and source data pass. |
| Reporting | Define client-statement, portfolio-review, income, activity, risk, exposure, exception, and operating-report requirements. | Labels distinguish current, projected, stale, partial, estimated, unsupported, restricted, and source-limited values. |
| Calculation requirements | State required source inputs, formulas, dates, currencies, units, conventions, rounding, and failure behavior. | QA can build deterministic positive and negative tests from the document. |
| Edge cases | Capture product events that break generic modelling. | Stale NAVs, gates, corporate actions, defaults, calls, physical delivery, surrender, pledge changes, margin calls, and restatements are handled. |
| Implementation-backed capability | Separate generic platform capability from source-backed product-specific support. | The document avoids claiming support unless source ownership, methodology, controls, UI/API behavior, and QA evidence are present. |
| Future candidate capability | Identify valuable extensions without presenting them as current support. | Roadmap candidates include required data, owner, controls, reporting output, and acceptance tests. |
| Worked examples | Add examples across normal, stressed, partial, and unsupported states. | Examples include objective, terms, timeline, cashflows, calculations, reporting output, QA assertions, and boundaries. |

## Implementation-Backed Capability Lens

Product docs should stay vendor-neutral, but they should learn from implementation reality. When reviewing app documentation or code, translate support evidence into neutral capability language:

| Capability Area | Neutral Documentation Treatment |
|---|---|
| Portfolio, holding, account, transaction, cashflow, valuation, and source-data foundations | Treat these as source-of-record prerequisites for product analytics, reporting, and downstream workflows. |
| Performance analytics | Document TWR, MWR, benchmark, contribution, attribution, composite, return-series, supportability, and lineage requirements. |
| Risk analytics | Document risk metrics, stress/scenario results, concentration, mandate risk health, historical attribution limits, and source supportability. |
| Advisory workflow | Document proposal simulation, policy evidence, suitability checks, consent posture, alternatives, and advisory-only boundaries. |
| DPM and mandate workflow | Document model targets, construction alternatives, rebalance simulation, waves, proof packs, outcome review, mandate health, monitoring exceptions, and execution boundaries. |
| Reporting workflow | Document portfolio review composition, report jobs, render/archive handoff, section readiness, upstream evidence, and report-output limitations. |
| Gateway or front-office composition | Document BFF, UI, and workflow composition as downstream realization, not as the owner of portfolio, performance, risk, or suitability truth. |
| AI or narrative assistance | Document grounded evidence, support-only narrative, forbidden-use boundaries, and raw-payload controls. |

Do not copy internal app names into product docs unless the target document is explicitly an internal implementation map. For general product knowledge, write the capability, source owner, support boundary, and evidence requirement.

## Review Workflow

1. Read the product pack README, companion map, calculation catalog row, capability boundary row, QA matrix row, and worked examples.
2. Check whether the pack covers every review dimension above.
3. Remove duplicate or preparation-only phrasing and convert it into practitioner language.
4. Add missing cross-links instead of duplicating long explanations.
5. Add at least one worked example when a product has complex payoff, cashflow, valuation, or support boundary behavior.
6. Use [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md) to keep support claims neutral, evidence-aware, and non-branded.
7. Update [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md) when a gap or enrichment area changes.
8. Update [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) when a support claim, partial state, or future candidate changes.
9. Update [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md) when a formula or QA assertion is added.
10. Update [`cross-product-worked-examples.md`](cross-product-worked-examples.md) when a new example applies beyond one product pack.
11. Run repository checks for relative links, narrow preparation-only wording, unsupported brand references, and whitespace.

## Minimum Quality Bar

A product pack is not polished if it:

1. explains the product but not how it is booked, valued, reported, and tested,
2. describes payoff but not cashflow timing,
3. describes valuation but not source ownership or stale state,
4. describes performance but not external-flow classification,
5. describes risk but not denominator or exposure basis,
6. describes advisory use but not suitability, cost, liquidity, or authority constraints,
7. describes DPM use but not mandate, model, restriction, funding, and post-trade residual drift,
8. describes reporting output but not labels, exceptions, and lineage,
9. describes current support without evidence,
10. lists future ideas without data, controls, and acceptance criteria.

## Disclaimer

This rubric is for education, product analysis, documentation, platform design, reporting design, QA, operations, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.
