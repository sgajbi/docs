# PR Review, Quality Gates, and Engineering Standards Map

## Purpose

This file maps PR review and quality-governance topics to the full guide series.

Use it to build enterprise-grade PR templates, review checklists, and CI/CD gates.

---

## PR Review Dimensions

| Dimension | Guides |
|---|---|
| Code structure | 2 |
| API contract | 3 |
| Data contract | 4 |
| CI/CD | 5 |
| Runtime | 6 |
| Observability | 7 |
| Security | 8 |
| Testing | 9 |
| Git/commits | 10 |
| Performance | 11 |
| Documentation | 12 |
| AI | 14 |
| Domain | 15 |
| Reporting | 16 |
| Migration | 18 |
| Support | 19 |

---

## Standard PR Questions

- Is the change small and reviewable?
- Is the logic in the right layer?
- Are contracts updated?
- Are tests meaningful?
- Are error paths covered?
- Is authorization enforced?
- Are logs/metrics safe?
- Is performance impact understood?
- Is documentation updated?
- Is release/support impact clear?
- Is evidence included?

---

## Quality Gates To Consider

- lint
- typecheck
- unit tests
- domain tests
- API contract tests
- OpenAPI validation
- integration tests
- security scans
- dependency scans
- Docker build
- migration tests
- docs link checks
- no-sensitive-content checks
- coverage trend
- complexity threshold
- certification sweep

---

## Summary

PR review is daily architecture governance.

Use the guides to make review consistent, evidence-based, and teachable.
