# Product Implementation Evidence Review Guide

This guide explains how to use implementation evidence from private-banking applications to improve the product knowledge base without turning the docs into branded product marketing.

Use it when reviewing app READMEs, API contracts, validation evidence, OpenAPI files, UI proof packs, report payloads, or runbooks and deciding what should be reflected in product documentation.

## Core Rule

Implementation evidence should be translated into neutral product-capability language.

Do not write:

- an app owns this product,
- a UI screen proves full product support,
- an endpoint proves advisory, DPM, report, tax, risk, or suitability truth by itself,
- an internal service name as if it were a market-facing product capability.

Write instead:

- which capability is source-backed,
- which source owns the facts,
- which calculation or workflow is methodology-backed,
- which output is composition-only,
- which states are support-limited,
- which promotion requirements remain before product support can be claimed.

## Evidence Classes

| Evidence class | What it can prove | What it does not prove alone |
|---|---|---|
| Source-data README or contract | Portfolio, account, holding, transaction, valuation, cashflow, mandate, product, and supportability facts have an owner. | Performance, risk, advisory, DPM, reporting, tax, legal, or client-communication conclusions. |
| Analytics methodology or API contract | Calculation owner, input contract, output shape, supportability states, and lineage requirements. | That every product subtype has complete source inputs or product-specific methodology. |
| Advisory workflow evidence | Proposal state, alternatives, policy evidence, consent posture, expiry, review state, and advisory-only boundaries. | DPM authority, client order creation, execution, settlement, or client publication. |
| DPM workflow evidence | Mandate health, model target, drift, rebalance simulation, wave, approval, proof-pack, outcome-review, and monitoring posture. | OMS execution, fill confirmation, settlement truth, client communication, or suitability approval unless separately sourced. |
| Reporting evidence | Report composition, section readiness, render/archive handoff, report job state, snapshot lineage, and degraded-state labels. | Source portfolio truth, performance methodology, risk methodology, tax conclusion, or suitability conclusion. |
| UI or gateway evidence | User-facing composition, route exposure, interaction flow, supportability display, and workflow handoff. | Domain ownership or calculation support. |
| AI or narrative evidence | Grounded source references, allowed evidence packets, redaction posture, review state, and forbidden-use boundaries. | Advice, recommendation approval, product eligibility, client-ready communication, or action authority. |
| Validation or QA evidence | A capability was tested for a named scenario, data set, endpoint, or workflow. | General support for adjacent products, countries, booking centres, wrappers, or edge cases outside the evidence. |

## Translation Pattern

When implementation evidence is found, translate it through this sequence:

1. **Observed evidence:** What file, contract, endpoint, payload, test, or runbook proves.
2. **Neutral capability:** What product capability this represents in generic wealth-platform language.
3. **Source owner:** Which domain owns the fact or method.
4. **Product scope:** Which product families and subtypes are covered.
5. **Supportability state:** Supported, source-backed extension, support-limited, future candidate, or unsupported.
6. **Required labels:** Stale, missing, partial, estimated, restricted, unsupported, advisor-only, client-ready, rendered, archived, or replayable.
7. **Documentation update:** Which product pack, companion map, calculation catalog, QA matrix, or worked example needs an update.
8. **Promotion requirements:** What additional source contract, calculation, UI/API behavior, controls, and QA evidence are needed.

## Capability Translation Examples

| Observed implementation evidence | Neutral product-documentation update |
|---|---|
| A source-data service states that it owns portfolio, account, holding, transaction, valuation, mandate, and supportability facts. | Product docs can say portfolio analytics require source-owned holdings, transactions, valuations, classifications, mandates, and supportability metadata before downstream outputs can be trusted. |
| A performance analytics service owns TWR, MWR, contribution, attribution, benchmark context, returns series, and lineage. | Performance sections should define required valuations, external-flow classification, benchmark identity, contribution coverage, attribution residuals, partial history, and report labels. |
| A risk analytics service owns concentration, drawdown, rolling metrics, scenario/stress, historical attribution, and mandate risk health. | Risk sections should specify exposure denominator, scenario definition, concentration dimensions, supportability, and product-specific limitations for derivatives, structured products, collateral, commitments, and insurance. |
| An advisory service persists proposal simulation, alternatives, policy evidence, consent posture, expiry, and review/replay state. | Advisory sections should separate advisory recommendation evidence from DPM authority, product approval, order execution, settlement, and client communication. |
| A DPM service supports rebalance simulation, waves, proof packs, outcome review, mandate monitoring, and supportability evidence. | DPM sections should cover model version, drift, funding, restrictions, approval, proof evidence, execution boundary, post-trade review, blocked states, and source-owner handoffs. |
| A reporting service composes portfolio reviews, report jobs, section readiness, render/archive handoff, and snapshot lineage. | Reporting sections should define what is sourced, partial, advisor-only, client-ready, rendered, archived, replayable, and not recomputed by the reporting layer. |
| A UI or gateway exposes composed front-office routes. | UI/API guidance should describe user-facing labels, degraded states, permission posture, and route composition, but should not claim method ownership. |
| AI evidence packets preserve source refs and forbidden-use boundaries. | Narrative guidance should require grounding, source references, redaction, review status, and no advice/order/client-message authority without explicit workflow approval. |

## Product Documentation Update Targets

| If evidence affects | Update |
|---|---|
| A product-specific calculation | Product pack worked examples and [`product-calculation-example-catalog.md`](product-calculation-example-catalog.md). |
| Source ownership or degraded state | Product pack implementation/control file and [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md). |
| Support boundary | [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md) and [`implementation-backed-capability-map.md`](implementation-backed-capability-map.md). |
| Product coverage status | [`product-knowledge-coverage-matrix.md`](product-knowledge-coverage-matrix.md). |
| Cross-product workflow | [`cross-product-worked-examples.md`](cross-product-worked-examples.md), [`product-lifecycle-cashflow-and-event-guide.md`](product-lifecycle-cashflow-and-event-guide.md), or [`portfolio-construction-and-rebalancing-guide.md`](portfolio-construction-and-rebalancing-guide.md). |
| QA or release evidence | [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md). |
| Role-based navigation | [`product-library-role-guide.md`](product-library-role-guide.md). |

## Support Claim Checklist

Before writing that a product capability is supported, confirm:

1. the source owner is named in neutral terms,
2. required source fields and dates are defined,
3. calculation methodology or workflow state is documented,
4. stale, missing, partial, restricted, unsupported, and estimated states are visible,
5. report labels are defined,
6. API/UI behavior does not overclaim ownership,
7. QA evidence covers normal and degraded scenarios,
8. downstream advisory, DPM, reporting, risk, performance, tax, legal, and execution boundaries are preserved,
9. unsupported adjacent variants are explicitly labelled,
10. promotion requirements are listed for future candidates.

## Future-Candidate Template

Use this template when implementation evidence is not yet enough to claim support.

```text
Candidate capability:
Product family and subtype:
Business purpose:
Required source owner:
Required source fields:
Calculation or workflow method:
Required report labels:
Required API/UI behavior:
Required controls:
Required QA scenarios:
Unsupported until:
```

## Review Cadence

Use this guide when:

1. a product pack is imported,
2. a product pack is polished,
3. a new app capability appears in README or API docs,
4. a report or analytics payload adds supportability metadata,
5. a QA or validation run proves a new scenario,
6. a previously future-candidate capability becomes source-backed,
7. a support claim might leak implementation branding into reusable product docs.

## Disclaimer

This guide is for education, product analysis, documentation, platform design, QA, operations, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, cybersecurity-certification, or client advice.
