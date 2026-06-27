# Service Ownership, Domain Boundaries, and Platform Governance

## Purpose

This file explains how engineering leaders govern service ownership and platform standards.

Unclear ownership is one of the biggest causes of duplicated logic, fragile systems, and support gaps.

---

## Service Ownership

Every service should have:

- owning team
- service purpose
- owned domain/capability
- non-owned areas
- public contracts
- runtime owner
- support owner
- documentation owner
- quality posture

---

## Ownership Statement

Template:

```text
This service owns <capability>.
It is authoritative for <data/rules/workflow>.
It exposes <contracts>.
It consumes <dependencies>.
It does not own <explicit exclusions>.
```

---

## Boundary Governance

Govern boundaries through:

- README ownership section
- architecture docs
- ADRs/RFCs
- contract tests
- data-product declarations
- CODEOWNERS
- architecture reviews
- service maps
- supported-feature docs

---

## Platform Governance

Platform governance should provide:

- standards
- templates
- validators
- reusable workflows
- runtime conventions
- onboarding guides
- evidence formats
- quality gates
- shared context

Platform should not become owner of every application’s business truth.

---

## Governance Without Bureaucracy

Good governance is:

- clear
- lightweight
- evidence-based
- automatable where possible
- proportional to risk
- supportive of delivery
- reviewed and improved

Bad governance is slow, vague, manual, and disconnected from implementation.

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| No service owner | Support and change confusion. |
| BFF owns domain logic | Authority drift. |
| Platform duplicates app truth | Stale governance. |
| Every team invents standards | Inconsistent engineering. |
| Governance only meetings | No executable control. |
| Ownership hidden in people's heads | Onboarding and support risk. |

---

## Review Checklist

- Is service owner clear?
- Is domain authority clear?
- Are non-responsibilities documented?
- Are contracts owned?
- Are support responsibilities clear?
- Are platform standards applied?
- Are exceptions documented?
- Are ownership changes reviewed?
- Are boundaries tested or enforced?

---

## Summary

Ownership clarity is architecture quality.

Governance should make boundaries visible, reusable, and enforceable without blocking delivery unnecessarily.
