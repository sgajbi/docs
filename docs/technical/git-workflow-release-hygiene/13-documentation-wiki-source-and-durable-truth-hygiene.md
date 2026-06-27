# Documentation, Wiki Source, and Durable Truth Hygiene

## Purpose

This file explains how documentation should stay aligned with implementation and Git history.

Documentation is part of delivery truth. If it changes, it should be reviewed, validated, and merged like code.

---

## Durable Truth

Durable truth should live in source-controlled files such as:

- README
- architecture docs
- API docs
- runbooks
- ADRs
- supported-feature docs
- quality scorecards
- wiki source
- release notes
- evidence maps

Chat messages, local notes, and PR comments are not enough for durable truth.

---

## Documentation With Code

When implementation changes, update related docs in the same PR where practical.

Examples:

| Change | Docs To Update |
|---|---|
| API contract | API docs, OpenAPI, examples, wiki API surface. |
| Runtime behavior | runbook, README, operations docs. |
| New capability | supported-feature docs, architecture notes. |
| Security behavior | security docs, runbook, control matrix. |
| CI gate | validation docs, README commands, quality scorecard. |
| Data product | producer/consumer contracts, catalog docs. |

---

## Wiki Source

If repo uses `wiki/` as source:

- edit `wiki/` files in PR
- validate links/content
- publish after merge if required
- avoid editing published wiki manually without syncing source
- treat wiki clone as publication target, not primary truth

---

## Documentation Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Docs claim future behavior as current | False support. |
| Docs updated after code in separate unmerged branch | Drift. |
| Wiki edited manually only | Source truth lost. |
| README has stale commands | Onboarding failure. |
| Runbook references old metrics | Incident failure. |
| PR comments contain important decision only | Knowledge lost. |
| Documentation copied everywhere | Duplication and inconsistency. |

---

## Review Checklist

- Did implementation truth change?
- Are docs updated in same PR?
- Are commands current?
- Are examples current and synthetic?
- Are supported claims evidence-backed?
- Is wiki source updated?
- Are links valid?
- Are runbooks aligned?
- Are future items marked planned?
- Is duplication avoided?

---

## Summary

Documentation is not separate from engineering.

It is how a repository remembers current truth.
