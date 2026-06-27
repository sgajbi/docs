# Product Library Role Guide

This guide gives each role a practical path through the product knowledge base.

Use it when the question is not "which product is this?" but "what should I read or produce for my work?"

## Core Navigation

| Need | Start here |
|---|---|
| Understand a product family | The relevant product pack README under [`docs/products/`](README.md#product-packs). |
| Find practical examples | [`product-worked-example-index.md`](product-worked-example-index.md). |
| Understand support boundaries | [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) and [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md). |
| Translate implementation evidence into product docs | [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md). |
| Track product polish status | [`product-library-polish-dashboard.md`](product-library-polish-dashboard.md). |
| Check source ownership and report dependencies | [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md). |
| Build QA coverage | [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md) and each product pack's local worked-example file. |
| Review or polish a product document | [`product-documentation-review-rubric.md`](product-documentation-review-rubric.md). |

## Business User Path

Use this path for product understanding, stakeholder discussion, and business-scope decisions.

| Step | Read | Produce |
|---|---|---|
| Product understanding | Product pack README and file `01` in the relevant pack. | Plain-language product summary. |
| Product taxonomy | Product pack taxonomy file and [`product-taxonomy-and-vocabulary-guide.md`](product-taxonomy-and-vocabulary-guide.md). | In-scope, adjacent, partial, and out-of-scope product list. |
| Business behavior | Local worked-example file and [`product-payoff-scenario-and-stress-guide.md`](product-payoff-scenario-and-stress-guide.md). | Business explanation of payoff, cashflows, downside, income, liquidity, and client impact. |
| Support posture | [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md). | Supported, source-backed, support-limited, and future-candidate capability list. |

Minimum evidence:

- product family and subtype;
- client objective;
- legal holding and economic exposure;
- key risks and limitations;
- required reports or workflows;
- open decisions and unsupported areas.

## Business Analyst Path

Use this path for requirements, backlog refinement, data contracts, acceptance criteria, and workflow design.

| Step | Read | Produce |
|---|---|---|
| Domain model | Product pack data-model file and [`product-taxonomy-and-vocabulary-guide.md`](product-taxonomy-and-vocabulary-guide.md). | Entities, attributes, states, events, and source owners. |
| Lifecycle | Product lifecycle file and [`product-lifecycle-cashflow-and-event-guide.md`](product-lifecycle-cashflow-and-event-guide.md). | Event model, transaction types, cashflow rules, and state transitions. |
| Calculations | Product worked examples and [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md). | Formula inputs, dates, currencies, units, rounding, and failure behavior. |
| Boundary | [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md). | Requirement split between baseline support, product extension, support-limited state, and future candidate. |
| Acceptance | [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md). | Acceptance criteria for happy path, stale source, missing source, correction, permission, reporting, and migration states. |

Minimum evidence:

- source owner for every critical field;
- event-to-transaction mapping;
- calculation basis and degraded-state behavior;
- report labels and user-visible warnings;
- open implementation decisions.

## Developer And Architect Path

Use this path for domain modelling, API design, service boundaries, event contracts, and implementation planning.

| Step | Read | Produce |
|---|---|---|
| Capability boundary | [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) and [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md). | Domain ownership and support-boundary decision. |
| Source and data model | Product data-model file and [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md). | Source contract, canonical model, identifiers, dates, currencies, states, and lineage. |
| Events and workflows | Product lifecycle file and local worked-example file. | Event contract, idempotency keys, state machine, transaction mapping, and reconciliation hooks. |
| APIs and UI states | Product implementation file plus companion maps. | API response shape, supportability flags, stale/missing states, permissions, and UI labels. |
| Regression hooks | Local worked-example regression section. | Unit, integration, contract, migration, and report-output tests. |

Implementation rule:

Do not let downstream APIs, UIs, or narrative tools become the owner of portfolio, price, tax, suitability, risk, performance, or reporting truth. They should compose source-backed evidence and expose supportability.

## QA Path

Use this path for test design, regression packs, release checks, and defect triage.

| Step | Read | Produce |
|---|---|---|
| Product risks | Product pack README and advisory/reporting file. | Risk-based test scope. |
| Worked examples | Local worked-example file and [`product-worked-example-index.md`](product-worked-example-index.md). | Normal, stressed, partial, unsupported, correction, and migration scenarios. |
| Cross-product QA | [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md). | Release-gate scenario matrix. |
| Source and report checks | [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md). | Source-to-calculation-to-report assertions. |

Minimum QA coverage:

- normal calculation;
- stale source;
- missing mandatory field;
- conflicting source values;
- corrected source or restatement;
- partial support state;
- unsupported subtype;
- permission or restricted-data behavior;
- downstream report output;
- reconciliation against source owner.

## Operations Path

Use this path for reconciliation, breaks, corrections, source intake, migration, and production support.

| Step | Read | Produce |
|---|---|---|
| Lifecycle and source truth | Product lifecycle file and [`product-source-ownership-deep-dive.md`](product-source-ownership-deep-dive.md). | Break classification and source-owner route. |
| Data quality | [`product-source-intake-and-migration-guide.md`](product-source-intake-and-migration-guide.md) and data-governance pack. | Intake checklist, field profiling, reconciliation and certification evidence. |
| Corrections | Product worked examples and reporting/client-experience pack. | Correction workflow and report-impact assessment. |
| Operating controls | Relevant implementation/control file in the product pack. | Exception queue, maker-checker process, audit trail and sign-off. |

Minimum evidence:

- source file or document;
- event date, value date, booking date and received date;
- original value, corrected value and reason;
- impacted reports, calculations and workflows;
- approval and audit trail.

## Advisory And DPM Path

Use this path for client recommendations, mandate monitoring, model portfolios, rebalancing, suitability and portfolio review.

| Step | Read | Produce |
|---|---|---|
| Product explanation | Product pack README and local worked examples. | Client/advisor explanation of objective, payoff, cashflows, risks and liquidity. |
| Suitability and product governance | [`advisory-mandate-reporting-decision-guide.md`](advisory-mandate-reporting-decision-guide.md) and [`product-governance-apu-target-market/`](product-governance-apu-target-market/README.md). | Eligibility, target-market, complexity, risk and disclosure evidence. |
| Mandate and rebalance | [`portfolio-construction-and-rebalancing-guide.md`](portfolio-construction-and-rebalancing-guide.md) and [`model-portfolios-rebalancing-advisory-suitability/`](model-portfolios-rebalancing-advisory-suitability/README.md). | Drift, proposed action, funding, restriction, approval and post-trade evidence. |
| Reporting and review | [`client-reporting-and-portfolio-review-guide.md`](client-reporting-and-portfolio-review-guide.md). | Portfolio review narrative with source-backed figures and supportability labels. |

Minimum evidence:

- client or mandate objective;
- product eligibility;
- suitability and knowledge/experience posture;
- liquidity and concentration impact;
- cost, tax and risk explanation;
- proposal or DPM authority evidence;
- source-backed reporting labels.

## Reporting And Analytics Path

Use this path for portfolio reports, performance/risk analytics, client statements, dashboards and narrative support.

| Step | Read | Produce |
|---|---|---|
| Report design | [`client-reporting-and-portfolio-review-guide.md`](client-reporting-and-portfolio-review-guide.md) and portfolio reporting pack. | Report section definitions, labels, exception states and evidence requirements. |
| Performance and risk | [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md) and portfolio performance pack. | Return, contribution, attribution, risk and supportability definitions. |
| Exposure | [`portfolio-exposure-modelling-guide.md`](portfolio-exposure-modelling-guide.md). | Legal holding, market value, look-through, notional, sensitivity, commitment and collateral exposure views. |
| Tax and regulatory labels | [`tax-regulatory-and-reporting.md`](tax-regulatory-and-reporting.md). | Gross/net/tax labels, cost-basis labels and documentation states. |

Minimum evidence:

- valuation date and source;
- calculation methodology;
- cashflow classification;
- benchmark and exposure basis;
- stale, missing, estimated, manual, partial or unsupported states;
- report lineage and archive reference.

## Product Development Path

Use this path when deciding whether to add or extend product capability.

| Step | Read | Produce |
|---|---|---|
| Current coverage | [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md). | Gap statement and affected product families. |
| Support boundary | [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md). | Current support, source-backed extension, support-limited state or future candidate. |
| Evidence needed | [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md). | Required source contracts, methodology, UI/API behavior, controls and QA evidence. |
| Implementation evidence review | [`product-implementation-evidence-review-guide.md`](product-implementation-evidence-review-guide.md). | Neutral support claim, evidence class, source owner, supportability state and promotion requirements. |
| Examples | [`product-worked-example-index.md`](product-worked-example-index.md). | Worked example and acceptance scenario for the candidate capability. |

Promotion checklist:

1. source owner identified,
2. source contract available,
3. calculation or rule method documented,
4. lifecycle and transaction behavior defined,
5. user-facing labels and supportability states defined,
6. reporting impact known,
7. reconciliation and audit trail defined,
8. regression tests written,
9. coverage matrix updated.

## Common Failure Modes

| Failure | Better behavior |
|---|---|
| Treating a legal holding as the same as economic exposure | Separate legal position, accounting position, analytical exposure, look-through, notional, sensitivity and commitment views. |
| Treating projected cash as available cash | Distinguish ledger, settled, projected, reserved, restricted and policy-available cash. |
| Reporting a number without source date | Show valuation date, source timestamp and supportability state. |
| Claiming support from UI presence | Require source ownership, methodology, controls, API behavior, UI state and QA evidence. |
| Hiding stale or missing data | Use explicit stale, missing, estimated, manual, partial, restricted or unsupported labels. |
| Turning examples into generic promises | Keep examples tied to product terms, source inputs, calculation basis and boundary conditions. |

## Maintenance Rule

When a product pack changes, update any affected:

1. local README,
2. local worked-example file,
3. [`product-worked-example-index.md`](product-worked-example-index.md),
4. [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md),
5. calculation, QA, source-ownership or capability-boundary guide if the change affects shared behavior.
