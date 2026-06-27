# Durable Truth, Source-Controlled Docs, and Wiki Publication

## Purpose

This file explains how to keep documentation truthful and durable.

Durable truth should be stored in source control, reviewed with changes, and published consistently.

---

## Durable Truth

Durable truth is information that future engineers should be able to trust.

Examples:

- service responsibility
- API contracts
- supported features
- runbooks
- architecture decisions
- deployment steps
- quality gates
- data-product contracts
- security assumptions
- known limitations

---

## Source-Controlled Documentation

Source-controlled docs allow:

- review
- history
- traceability
- rollback
- linking to commits
- CI validation
- collaboration
- audit evidence

Docs that affect engineering truth should be versioned like code.

---

## Wiki Publication Model

If a team uses a wiki:

```text
source docs in repository
  -> reviewed PR
  -> merged to main
  -> published to wiki
```

The wiki is a publication surface. The repo remains the source of truth.

---

## Documentation Drift

Drift happens when docs and implementation disagree.

Prevent drift by:

- updating docs in same PR as code
- linking docs to tests/evidence
- validating links
- reviewing supported-feature claims
- using doc freshness dates where helpful
- archiving obsolete docs
- avoiding manual wiki-only edits

---

## Truth Status

Use status labels:

| Status | Meaning |
|---|---|
| Current | Matches implementation. |
| Partial | Some implementation exists; gaps documented. |
| Planned | Future work. |
| Deprecated | Still useful but being replaced. |
| Archived | Historical only. |
| Unknown | Needs owner review. |

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Important truth only in chat | Lost knowledge. |
| Wiki edited manually only | Source drift. |
| Docs merged after code weeks later | Gap period. |
| Planned feature described as current | False claims. |
| Archived docs not labelled | Stale reuse. |
| No owner | No maintenance. |

---

## Review Checklist

- Is this durable truth?
- Is it in source control?
- Is wiki publication source-driven?
- Is status clear?
- Does it match implementation?
- Are old docs archived?
- Are manual wiki edits avoided?
- Are links validated?
- Is owner defined?

---

## Summary

Durable truth must be reviewable, versioned, and discoverable.

If it matters to engineering, it belongs in source-controlled documentation.
