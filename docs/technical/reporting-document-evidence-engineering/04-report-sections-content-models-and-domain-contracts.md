# Report Sections, Content Models, and Domain Contracts

## Purpose

This file explains how report content should be modelled and assembled.

A report should not be a random collection of raw API responses. It should have a governed content model built from domain-owned contracts.

---

## Report Sections

Common wealth-report sections:

- cover page
- client/account/portfolio summary
- holdings
- asset allocation
- performance summary
- performance detail
- benchmark comparison
- risk analytics
- concentration
- transactions
- income and cashflows
- advisory proposal
- suitability evidence
- fees and charges
- disclosures
- appendix

---

## Content Model

A report content model is structured data prepared for rendering.

It should include:

- report metadata
- section list
- section data
- supportability per section
- warnings
- source references
- display labels
- numeric precision rules
- language/locale
- disclaimers

---

## Domain Contracts

Each section should consume data from the authoritative domain.

Examples:

| Report Section | Domain Owner |
|---|---|
| Holdings | Portfolio/source data service |
| Performance | Performance analytics service |
| Risk | Risk analytics service |
| Advisory proposal | Advisory workflow service |
| Document archive | Archive service |
| Client data | Client master service |

---

## Section-Level Supportability

A section may be:

- ready
- partial
- stale
- degraded
- unavailable
- permission-blocked
- unsupported

The report should decide whether to:

- include section
- include section with warning
- omit section
- block report generation
- require human approval

---

## Content Model Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Renderer calls domain APIs directly | Boundary confusion. |
| Template contains business rules | Logic hidden in layout. |
| Report section uses raw DB query | Source governance bypass. |
| Missing section shown as empty | Misleading report. |
| No section supportability | Client cannot understand limitations. |
| Numeric formatting inconsistent | Report quality issue. |

---

## Review Checklist

- Are report sections defined?
- Is each section source-owned?
- Is content model versioned?
- Are warnings represented?
- Are precision/format rules defined?
- Are permission-blocked sections handled?
- Is renderer logic-free?
- Are domain contracts tested?
- Is section-level evidence captured?

---

## Summary

Report content should be a governed model.

Rendering should format evidence-backed content, not invent business meaning.
