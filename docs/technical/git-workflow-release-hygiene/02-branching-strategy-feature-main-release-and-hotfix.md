# Branching Strategy: Feature, Main, Release, and Hotfix

## Purpose

This file explains common branch types and how to use them safely in enterprise engineering.

A branch strategy should support fast delivery, clear review, stable main, controlled releases, and emergency fixes.

---

## Common Branch Types

| Branch Type | Purpose |
|---|---|
| `main` | Durable integrated truth and releasable baseline. |
| Feature branch | Short-lived branch for one focused change. |
| Release branch | Stabilization line for a release candidate. |
| Hotfix branch | Urgent fix for production or release branch. |
| Experiment branch | Temporary exploration, not durable truth. |
| Documentation branch | Focused docs/knowledge update. |
| Refactor branch | Focused structural improvement. |

---

## Main Branch

Main should be:

- protected
- continuously validated
- releasable or close to releasable
- documented as durable truth
- not used for unreviewed work
- not broken for long
- source for new feature branches

---

## Feature Branches

Feature branches should be:

- short-lived
- named clearly
- scoped to one change area
- regularly synced/rebased as policy requires
- validated before PR
- deleted after merge

Example names:

```text
feature/report-job-replay
fix/readiness-dependency-check
docs/api-contract-cleanup
quality/openapi-gate
refactor/domain-policy-extraction
```

---

## Release Branches

Release branches may be used when:

- release needs stabilization
- production cutover is controlled
- multiple teams coordinate
- hotfixes need controlled backport
- regulatory/change windows require freeze

Rules:

- keep release branch short-lived where possible
- avoid feature work on release branch
- back-merge fixes to main
- tag release candidates
- preserve evidence

---

## Hotfix Branches

Hotfix branches are for urgent fixes.

Rules:

- branch from production/release baseline
- change smallest safe area
- run focused validation
- get expedited review
- document risk and rollback
- merge/backport to main
- follow up with full regression where needed

---

## Branching Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Long-lived feature branch | Drift and merge pain. |
| Direct commits to main | Review and evidence bypass. |
| Release branch receives features | Release risk. |
| Hotfix not merged back to main | Regression later. |
| Branch name vague | Hard to understand. |
| Many stale branches | Confusion. |
| Experiment branch treated as truth | Unsupported implementation leaks. |

---

## Review Checklist

- Is branch type clear?
- Is branch name meaningful?
- Is branch from correct base?
- Is scope focused?
- Is branch short-lived?
- Are release/hotfix rules followed?
- Are fixes back-merged?
- Is branch deleted after merge?
- Are stale branches reviewed?

---

## Summary

A good branch strategy keeps main stable, feature work focused, releases controlled, and emergency fixes traceable.
