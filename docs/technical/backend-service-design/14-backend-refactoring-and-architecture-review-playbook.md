# Backend Refactoring and Architecture Review Playbook

## Purpose

This file provides a practical playbook for improving backend service quality without destabilizing behavior.

Use it when refactoring a service toward clearer boundaries, better tests, stronger contracts, safer runtime behavior, and more professional documentation.

## Refactoring Principles

1. Preserve behavior unless change is intentional.
2. Improve one measurable area at a time.
3. Keep commits small and reviewable.
4. Add tests before or during movement.
5. Do not lower gates to pass.
6. Update docs when truth changes.
7. Mark unknowns honestly.
8. Prefer reversible slices.
9. Keep the service buildable.
10. Validate locally, then use CI for heavier proof.

## Common Refactoring Goals

| Goal | Example |
|---|---|
| Thin controllers | Move business rules from route handler to use case/domain policy. |
| Domain isolation | Remove framework imports from domain modules. |
| Port extraction | Hide HTTP/database calls behind interfaces. |
| DTO cleanup | Separate request/response models from domain objects. |
| Repository cleanup | Stop returning ORM objects upward. |
| Test improvement | Add domain and use-case tests before restructuring. |
| Observability cleanup | Replace raw prints/ad hoc logs with structured events. |
| Security cleanup | Centralize caller context and sensitive-data filtering. |
| Migration safety | Add migration smoke and rollback guidance. |
| Documentation truth | Align README, runbook, and supported status with code. |

## Refactoring Sequence

### Step 1: Baseline

Gather:

- current README
- architecture docs
- route map
- tests
- CI status
- quality scorecard
- runbooks
- known issues

Identify current truth and unknowns.

### Step 2: Choose One Slice

Good slice examples:

- extract one domain policy
- isolate one repository
- add one contract test family
- clean one route family
- add idempotency to one write workflow
- add readiness check for one dependency
- add supportability state to one response

Avoid giant refactors.

### Step 3: Add Safety Tests

Before moving logic, add tests that prove behavior.

Prefer:

- domain tests for rules
- use-case tests for orchestration
- contract tests for API shape
- integration tests for adapters

### Step 4: Move Logic

Move logic to the right layer.

Keep mapping explicit:

```text
route -> command -> use case -> domain policy -> port -> adapter
```

### Step 5: Validate

Run:

- focused tests
- lint/typecheck
- contract tests
- integration tests where needed
- Docker/runtime proof where runtime changed

### Step 6: Update Docs

Update:

- README
- architecture docs
- runbooks
- API docs
- supported-feature docs
- quality scorecard
- evidence map

### Step 7: Explain PR Evidence

PR should explain:

- what changed
- what behavior was preserved
- what tests prove it
- what risk remains
- what follow-up is needed

## Architecture Review Method

Use this structure:

```text
1. Capability ownership
2. Boundary correctness
3. Layering and dependency direction
4. API contract quality
5. Persistence and transaction safety
6. Async/event/idempotency posture
7. Error and supportability model
8. Security and sensitive-data controls
9. Observability and runbooks
10. Test coverage by layer
11. CI/release evidence
12. Documentation truth
```

## Review Questions

- Is the service mission clear?
- Is domain authority explicit?
- Is business logic in the right layer?
- Are dependencies behind ports?
- Are DTO/domain/persistence models separated?
- Are state changes transactionally safe?
- Are async workflows durable?
- Are errors safe and meaningful?
- Is caller context enforced?
- Are logs/metrics source-safe?
- Are tests meaningful?
- Are docs implementation-backed?

## Refactoring Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Move files without tests | Behavior may change silently. |
| Add abstraction without repeated pattern | Complexity increases. |
| Rewrite entire service | High risk and hard review. |
| Update docs before implementation | False truth. |
| Lower gate thresholds | Quality debt hidden. |
| Mix formatting with logic change | Review harder. |
| Leave compatibility aliases undocumented | Future confusion. |
| Treat CI failure as flaky without proof | Real defect ignored. |

## Refactoring Definition Of Done

- behavior preserved or intentional changes documented
- relevant tests added
- CI gates healthy
- contracts updated
- docs updated
- runbooks updated where operational behavior changed
- residual risks documented
- follow-up backlog clear
- no generated artifacts accidentally tracked
- service remains buildable and runnable

## Summary

Good refactoring is controlled improvement.

The goal is not to make code look different. The goal is to make the service easier to understand, safer to change, easier to test, and easier to operate.
