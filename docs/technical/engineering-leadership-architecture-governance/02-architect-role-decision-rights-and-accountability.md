# Architect Role, Decision Rights, and Accountability

## Purpose

This file explains the role of an architect in enterprise engineering.

An architect should guide design quality, ownership, standards, trade-offs, and long-term technical direction without becoming a delivery bottleneck.

---

## Architect Responsibilities

An architect should:

- define system boundaries
- guide service ownership
- review major design decisions
- maintain architecture principles
- identify cross-team impacts
- approve or guide key technical trade-offs
- ensure non-functional requirements are considered
- support production-readiness reviews
- align platform standards
- mentor engineers and tech leads
- document decisions through ADRs/RFCs

---

## Decision Rights

Architects should have clear decision rights for:

- service boundary changes
- major API/event/data-product contracts
- platform integration patterns
- database ownership and migration strategy
- security and entitlement model
- runtime and deployment model
- cross-team dependency design
- technology choices
- architectural exceptions
- deprecation and modernization paths

---

## Decision Categories

| Decision Type | Example |
|---|---|
| Reversible | Internal implementation approach. |
| Hard to reverse | Data model, API contract, service boundary. |
| High-risk | Security model, production migration, dependency change. |
| Cross-team | Shared API, platform standard, data product. |
| Strategic | Technology stack, modernization, operating model. |

Hard-to-reverse and high-risk decisions need more formal governance.

---

## Accountability

Architect accountability includes:

- clarity of design
- visibility of trade-offs
- correctness of ownership model
- quality of decision records
- alignment with standards
- escalation of unresolved risk
- support for delivery teams
- learning from production outcomes

Architects are not accountable for doing all implementation, but they are accountable for ensuring significant designs are reviewable and coherent.

---

## Architect Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Architect as approval bottleneck | Delivery slows. |
| Architect disconnected from code | Designs become unrealistic. |
| No decision records | Rationale lost. |
| Architecture by slide only | Implementation drift. |
| Avoiding trade-off calls | Teams remain blocked. |
| Ignoring operations | Production risk grows. |
| Mandating standards without examples | Adoption fails. |

---

## Review Checklist

- Are decision rights clear?
- Is this decision reversible?
- Does it affect multiple teams?
- Does it need ADR/RFC?
- Are alternatives documented?
- Are consequences clear?
- Is delivery team aligned?
- Is production impact considered?
- Is security impact considered?
- Is decision evidence-backed?

---

## Summary

Architects create clarity.

The best architects make good decisions easier for teams and risky decisions harder to make accidentally.
