# Product Knowledge Enrichment Standard

This standard defines how product packs should be improved after import.

The goal is not to store raw notes. The goal is to build a professional, reusable knowledge base for private-banking product work, platform design, API modelling, UI design, QA, advisory, reporting, and stakeholder communication.

## Core Rule

Imported material is the base layer. The repository should add a project-ready layer on top where useful.

Good enrichment should:

1. make product concepts easier to apply in real projects,
2. connect product knowledge to platform modelling decisions,
3. clarify source ownership and system boundaries,
4. improve API, UI, reporting, QA, and operational design,
5. use professional practitioner and stakeholder language,
6. avoid narrow preparation-only framing,
7. preserve clear disclaimers where content touches investing, advice, tax, legal, regulation, or trading.

## What To Add

For each product pack, add or strengthen these areas when useful:

| Enrichment Area | What Good Looks Like |
|---|---|
| Product mental model | A concise explanation of what the client legally owns and what drives value/risk. |
| Product taxonomy | Clear product variants and the business difference between them. |
| Position model | Legal holding, accounting position, analytical exposure, and look-through distinctions. |
| Transaction model | Economic transaction types and what should not be booked as a transaction. |
| Lifecycle events | Events that change status, entitlement, payoff, valuation, exposure, or settlement. |
| Valuation model | Price/NAV/model/quote hierarchy, stale state, source quality, and fallback posture. |
| Risk model | Market, credit, rate, spread, FX, liquidity, concentration, volatility, counterparty, or product-specific risks. |
| Performance model | Income, price return, FX return, internal activity, external cashflow, contribution, attribution, and P&L treatment. |
| Source ownership | Which system or party owns instrument, transaction, holding, price, NAV, corporate action, fixing, risk, performance, and suitability truth. |
| API implications | Endpoint responsibilities, examples, errors, supportability, lineage, and what not to infer. |
| UI implications | What users need to see, decide, act on, and understand as blocked/degraded. |
| QA scenarios | High-value scenarios that catch real product and platform defects. |
| Controls | Reconciliation, audit, stale data, overrides, operational exceptions, and failure behavior. |
| Practitioner explanations | Questions and answers for stakeholder conversations, product reviews, and project handoffs. |
| Cross-product comparisons | Similarities and differences versus adjacent products. |
| Worked examples | Product terms, timeline, transactions, cashflows, valuation, risk, reporting, QA, and implementation boundary. |
| Source and calculation matrix | Source owner, dependent calculations, data-quality states, reporting outputs, QA controls, and unsupported boundaries. |

## Where To Add Enrichment

Prefer this order:

1. Product pack README for purpose, reading order, provenance, and high-level notes.
2. Product-area map under `docs/products/` when the guidance applies across the product family.
3. Cross-product review under `docs/products/practical-product-implementation-review.md` when the guidance applies across many product families.
4. Source/calculation/reporting guidance under `docs/products/source-ownership-calculation-reporting-matrix.md` when the change affects implementation ownership, calculations, reporting, QA, or degraded states.
5. Cross-product reference under `docs/reference/` when the guidance applies to multiple product families.
6. Imported source file only when the source wording is unprofessional, narrow, stale, or unclear.

Do not overload imported source files with broad platform commentary when a separate map would be clearer.

## Tone Standard

Use language suitable for:

1. product owners,
2. architects,
3. engineers,
4. QA,
5. operations,
6. advisors,
7. portfolio analysts,
8. business stakeholders,
9. sales or pre-sales preparation,
10. client-demo preparation.

Avoid:

1. narrow preparation-only framing,
2. generic motivational prose,
3. unsupported product claims,
4. advice language,
5. regulatory or tax conclusions,
6. treating analytical exposure as legal holding,
7. implying a platform supports a feature unless implementation-backed.

## Enrichment Review Checklist

Before committing a product-pack update, check:

1. Does the pack have provenance?
2. Is the reading order clear?
3. Is there a product-area map if the product needs project-level interpretation?
4. Are adjacent products cross-linked?
5. Are source-ownership questions explicit?
6. Are API/UI implications present where useful?
7. Are QA scenarios practical and defect-oriented?
8. Are disclaimers present?
9. Is narrow preparation-only wording removed?
10. Do relative links resolve?
11. Does the root/product/reference index point to the new material?
12. Is the coverage matrix updated if the product library changed?
13. Does the practical product implementation review need a new example, capability boundary, or candidate extension?
14. Does the source-ownership/calculation/reporting matrix need a new source truth, calculation dependency, data-quality state, or QA scenario?

## Professional Review Rubric

For deeper review, use the product documentation rubric at
[`../products/product-documentation-review-rubric.md`](../products/product-documentation-review-rubric.md).
It defines the required review dimensions for:

1. product understanding and taxonomy,
2. payoff and cashflow behavior,
3. valuation, pricing, and stale-state treatment,
4. performance, contribution, attribution, and risk analytics,
5. portfolio modelling and exposure treatment,
6. advisory, suitability, DPM, and mandate relevance,
7. reporting, calculation, QA, and implementation boundaries,
8. implementation-backed capabilities and future candidates.

Use the rubric to turn imported content into a stronger long-term knowledge base without turning
the product docs into branded product marketing.

When implementation evidence is useful, translate it through
[`../products/implementation-backed-capability-map.md`](../products/implementation-backed-capability-map.md)
so the product docs stay platform-aware, evidence-aware, and non-branded.

## Ongoing Improvement Principle

Every new product pack should make the repository more useful than a folder of imported notes.

When in doubt, add one small, high-value companion map or checklist that helps convert product knowledge into better project work.
