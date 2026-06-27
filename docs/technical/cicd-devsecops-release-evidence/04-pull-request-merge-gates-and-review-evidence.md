# Pull Request Merge Gates and Review Evidence

## Purpose

This file explains how PR merge gates should protect the main branch and produce useful review evidence.

A PR is not ready because code was written. A PR is ready when change, behavior, tests, security, docs, and risks are explained and validated.

---

## PR Merge Gate Purpose

The PR merge gate should:

- protect main branch
- enforce review
- validate behavior
- protect contracts
- check security
- prevent broken runtime packaging
- ensure docs stay aligned
- produce durable evidence

---

## Common Required Checks

Typical PR checks:

- lint
- format
- typecheck
- unit tests
- domain tests
- contract tests
- integration tests
- coverage
- OpenAPI validation
- event/data-product validation
- security scan
- dependency audit
- migration smoke
- Docker build
- repository hygiene
- documentation checks

---

## PR Evidence Template

```markdown
## Summary

## Why this change is needed

## Architecture / boundary impact

## API / event / data-product impact

## Tests and validation

## Security and privacy impact

## Observability and operations impact

## Migration / configuration impact

## Rollback or mitigation plan

## Known gaps and follow-up work
```

---

## PR Review Areas

Review should cover:

- correctness
- architecture boundaries
- domain ownership
- API compatibility
- test quality
- security posture
- observability impact
- performance risk
- migration safety
- docs truth
- release readiness

Review is not just syntax and formatting.

---

## Required Review Conditions

Useful branch protection:

- required PR review
- required status checks
- no direct push to main
- CODEOWNERS where useful
- signed commits/tags where policy requires
- workflow changes reviewed carefully
- security-sensitive files require extra attention

---

## Merge Readiness

A PR is merge-ready when:

- scope is clear
- reviewers understand impact
- blocking checks pass
- failures are understood
- tests are meaningful
- docs are updated
- security impact is handled
- rollback path is known
- residual risks are documented

---

## PR Gate Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Merge with failing checks without explanation | Hidden risk. |
| PR description empty | Reviewers lack context. |
| Giant PR | Low review quality. |
| Tests added only for coverage | Weak behavior proof. |
| Docs updated without implementation | False truth. |
| Gate lowered to pass one PR | Governance erosion. |
| Workflow changes unreviewed | Supply-chain risk. |

---

## Review Checklist

- Is the PR small enough?
- Are changes explained?
- Are tests relevant?
- Are contracts updated?
- Are docs updated?
- Are CI checks meaningful?
- Are security implications described?
- Is observability updated?
- Is rollback possible?
- Are gaps listed?

---

## Summary

The PR merge gate protects the shared codebase.

It should make reviewers confident that the change is correct, safe, understandable, and supportable.
