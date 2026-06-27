# Change Slicing and Feature Branch Delivery

## Purpose

This file explains how to split engineering work into reviewable, safe, CI-friendly slices.

Large changes fail because they are difficult to review, test, merge, and roll back. Good slicing makes delivery safer.

---

## What Is A Slice?

A slice is a small, meaningful change that improves one area and can be reviewed on its own.

Examples:

- add a missing contract test
- extract one domain policy
- introduce one DTO
- add one readiness check
- add idempotency to one route
- update one runbook
- add one CI gate in report-only mode

---

## Slicing Principles

1. Preserve behavior unless intentionally changed.
2. Separate refactor from feature where possible.
3. Separate formatting from logic.
4. Add tests close to the change.
5. Keep PR reviewable.
6. Keep branch buildable.
7. Update docs with truth changes.
8. Avoid giant all-in-one PRs.

---

## Good Slice Sequence

Example backend hardening:

```text
1. Add tests around current route behavior.
2. Extract use case from controller.
3. Extract domain policy.
4. Add contract test for API response.
5. Add observability reason code.
6. Update runbook and README.
7. Add CI guard.
```

Each step can be reviewed.

---

## Slicing Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Big-bang refactor | Review and regression risk. |
| Feature mixed with architecture rewrite | Hard to isolate behavior change. |
| Tests added after all changes | Hard to know what broke. |
| CI gate added before baseline understood | Noise and frustration. |
| Docs updated before code | False truth. |
| Many unrelated files touched | Review fatigue. |

---

## Review Checklist

- Is change one clear slice?
- Can it be reviewed in reasonable time?
- Is behavior change intentional?
- Are tests close to the change?
- Is documentation updated only where truth changed?
- Is CI impact understood?
- Is rollback possible?
- Is follow-up backlog captured?

---

## Summary

Good slicing is engineering risk management.

Small, meaningful changes create faster review, safer merge, and better history.
