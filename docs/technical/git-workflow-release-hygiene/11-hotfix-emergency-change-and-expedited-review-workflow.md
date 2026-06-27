# Hotfix, Emergency Change, and Expedited Review Workflow

## Purpose

This file explains how to handle urgent fixes without abandoning engineering controls.

Emergency changes may need speed, but they still need traceability, review, validation, and follow-up.

---

## Hotfix Principles

1. Fix the smallest safe area.
2. Branch from the correct production/release baseline.
3. Keep evidence proportional to risk.
4. Get expedited review.
5. Preserve audit trail.
6. Back-merge to main.
7. Add regression after stabilization if not before.
8. Document residual risk.

---

## Hotfix Flow

```text
identify incident or urgent defect
  -> create hotfix branch from release/production baseline
  -> implement minimal fix
  -> run focused tests
  -> expedited review
  -> run required hotfix checks
  -> deploy
  -> verify
  -> back-merge to main
  -> add full regression/evidence if deferred
  -> incident/problem review
```

---

## Emergency Evidence

Even urgent changes should record:

- issue/incident reference
- risk
- changed files
- tests run
- reviewers
- deployment target
- rollback plan
- verification
- follow-up items

---

## Hotfix Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Direct production patch with no PR | Audit gap. |
| Hotfix never merged to main | Regression later. |
| No test added | Defect may recur. |
| Broad refactor during hotfix | Incident risk. |
| No rollback plan | Recovery harder. |
| Emergency process used for normal work | Governance erosion. |

---

## Review Checklist

- Is this truly urgent?
- Is branch from correct base?
- Is change minimal?
- Are focused tests run?
- Is review recorded?
- Is rollback known?
- Is deployment verified?
- Is fix back-merged?
- Is regression/follow-up captured?
- Is incident review planned?

---

## Summary

Hotfix discipline balances speed and control.

Emergency does not mean invisible.
