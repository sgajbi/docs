# Validation Lanes: Feature, PR, Main, and Platform E2E

## Purpose

This file explains how to structure validation lanes so teams get fast feedback without weakening release confidence.

Different checks belong in different lanes because they have different cost, speed, and risk profiles.

---

## Lane Model

```text
Local checks
  -> Remote feature lane
  -> Pull request merge gate
  -> Main releasability gate
  -> Platform end-to-end validation
```

---

## Local Checks

Purpose:

- fast developer feedback
- avoid obvious CI failures
- support focused development

Typical checks:

- format
- lint
- typecheck
- focused unit tests
- targeted contract tests
- docs preview
- local smoke where cheap

Local checks should be easy to run.

---

## Remote Feature Lane

Purpose:

- fast remote proof on feature branch
- shared visibility
- basic confidence before PR review

Typical checks:

- lint
- typecheck
- unit tests
- fast contract checks
- repository hygiene
- lightweight security checks
- workflow lint where useful

This lane should be fast and reliable.

---

## Pull Request Merge Gate

Purpose:

- protect main branch
- prove review readiness
- block known high-risk regressions

Typical checks:

- full lint/format/typecheck
- unit and domain tests
- API contract tests
- integration tests
- OpenAPI gate
- event/data-product contract gates
- security audit
- dependency checks
- migration smoke
- Docker build
- repository hygiene
- docs contract gate

This lane should block merge for meaningful failures.

---

## Main Releasability Gate

Purpose:

- prove main branch remains releasable
- run heavier validation
- produce release-grade evidence
- detect issues after merge integration

Typical checks:

- extended integration tests
- E2E tests
- certification sweeps
- Docker runtime proof
- performance smoke
- migration proof
- SBOM/provenance
- release evidence generation

This lane may be scheduled, manual, or triggered on main.

---

## Platform End-To-End Validation

Purpose:

- prove cross-service product flows
- validate canonical runtime
- confirm gateway/UI/domain/platform integration
- capture demo or release proof

Use when changes affect:

- UI routes
- BFF contracts
- domain APIs consumed by product
- data-product discovery
- trust telemetry
- service addressing
- observability propagation
- security/caller context
- seeded demo flows

---

## Lane Placement Guidance

| Check | Local | Feature | PR | Main | Platform E2E |
|---|---:|---:|---:|---:|---:|
| Format/lint | Yes | Yes | Yes | Optional | No |
| Typecheck | Yes | Yes | Yes | Optional | No |
| Unit tests | Focused | Yes | Yes | Optional | No |
| Contract tests | Focused | Yes | Yes | Yes | Maybe |
| Integration tests | Focused | Optional | Yes | Yes | Maybe |
| E2E/browser | Optional | No | Conditional | Yes | Yes |
| Docker build | Optional | Optional | Yes | Yes | Maybe |
| Security audit | Optional | Yes | Yes | Yes | No |
| SBOM/provenance | No | No | Optional | Yes | No |
| Release evidence | No | No | Optional | Yes | Yes |
| Product proof | No | No | Conditional | Conditional | Yes |

---

## Lane Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Every check in every lane | Slow delivery and low adoption. |
| Heavy checks only manual | Late surprises. |
| PR gate too weak | Main branch becomes unstable. |
| Main gate ignored | Release confidence weak. |
| Platform E2E run for every small backend change | Waste and slow feedback. |
| No clear lane mapping | Developers do not know what to run. |
| Lane names inconsistent across repos | Governance and onboarding friction. |

---

## Review Checklist

- Are validation lanes documented?
- Are local commands available?
- Are PR blockers meaningful?
- Are heavy checks placed appropriately?
- Is platform E2E used for cross-service changes?
- Is main releasability defined?
- Are scheduled/manual gates documented?
- Are lane names consistent?
- Are artifacts retained?
- Are failures actionable?

---

## Summary

Validation lanes balance speed and confidence.

Fast checks help developers move. Strong gates protect main, release, and production.
