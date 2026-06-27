# Product Capability Scope, Supported Features, and Evidence

## Purpose

This file explains how to document product capability and supported-feature status.

Enterprise buyers need clarity on what works today, what is partial, what is planned, and what is not supported.

---

## Capability Catalog

A capability catalog should include:

- capability name
- business purpose
- user personas
- supported workflows
- supported APIs/UI surfaces
- dependencies
- current status
- evidence
- known limitations
- roadmap notes

---

## Status Vocabulary

| Status | Meaning |
|---|---|
| Supported | Implemented, tested, documented, and evidence-backed. |
| Partially supported | Some capability exists; gaps are documented. |
| Pilot | Available for controlled evaluation. |
| Demo only | Works only in synthetic or showcase context. |
| Planned | Future roadmap. |
| Unsupported | Not available or intentionally out of scope. |
| Deprecated | Being replaced or retired. |

---

## Evidence Map

Every supported capability should link to:

- code/module
- API contract
- tests
- CI evidence
- runbook
- documentation
- demo/POC evidence where applicable
- known gaps

---

## Safe Claim Example

Poor:

```text
The platform supports advanced portfolio reporting.
```

Better:

```text
The platform supports portfolio review report generation for configured synthetic portfolios, including holdings, allocation, performance summary, PDF rendering, archive metadata, and golden-report tests. Risk commentary is planned and not currently supported.
```

---

## Capability Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Feature list without status | Buyer over-assumes. |
| Demo capability presented as production | Trust risk. |
| No evidence per feature | Claims weak. |
| Roadmap mixed with current support | Scope confusion. |
| Unsupported limitations hidden | Pilot surprise. |
| Capability names too technical | Business value unclear. |

---

## Review Checklist

- Is capability business meaningful?
- Is supported status clear?
- Is evidence linked?
- Are dependencies listed?
- Are limitations documented?
- Are unsupported cases listed?
- Are roadmap items separated?
- Is demo-only scope labelled?
- Is buyer-facing language accurate?

---

## Summary

A product capability catalog is a trust artifact.

It helps buyers and teams understand exactly what can be relied on today.
