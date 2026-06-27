# Git Workflow Checklists and Engineering Lead Playbook

## Purpose

This file provides practical checklists for Git workflow, branching, commits, PRs, reviews, merge policy, release hygiene, and audit traceability.

Use it for onboarding, PR review, repository governance, release preparation, and engineering leadership.

---

## Branch Checklist

- [ ] Branch type is clear.
- [ ] Branch name is meaningful.
- [ ] Branch is from correct base.
- [ ] Scope is focused.
- [ ] Branch is short-lived.
- [ ] Branch is synced/rebased as policy requires.
- [ ] Branch has no unrelated changes.
- [ ] Branch deleted after merge.

---

## Commit Checklist

- [ ] Commit is focused.
- [ ] Message is meaningful.
- [ ] Type prefix used where applicable.
- [ ] Formatting separated from logic where possible.
- [ ] Generated files intentional.
- [ ] Secrets absent.
- [ ] Behavior change intentional.
- [ ] Commit can be understood later.

---

## PR Checklist

- [ ] Summary clear.
- [ ] Motivation clear.
- [ ] Scope reviewable.
- [ ] Tests and validation listed.
- [ ] Architecture impact described.
- [ ] API/event/data impact described.
- [ ] Security impact described.
- [ ] Observability impact described.
- [ ] Docs updated.
- [ ] Known gaps listed.
- [ ] Rollback/mitigation described.

---

## Review Checklist

- [ ] Domain ownership preserved.
- [ ] Architecture boundary respected.
- [ ] Tests meaningful.
- [ ] Error paths handled.
- [ ] Security safe.
- [ ] Logs/metrics safe.
- [ ] Migrations safe.
- [ ] Runtime impact understood.
- [ ] Docs truthful.
- [ ] CI evidence sufficient.

---

## Merge Checklist

- [ ] Required checks pass.
- [ ] Required reviews complete.
- [ ] Conversations resolved.
- [ ] Merge strategy appropriate.
- [ ] Release notes updated if needed.
- [ ] Branch cleanup planned.
- [ ] Follow-up backlog captured.

---

## Hotfix Checklist

- [ ] Urgency justified.
- [ ] Correct base branch.
- [ ] Smallest safe change.
- [ ] Focused validation run.
- [ ] Expedited review recorded.
- [ ] Rollback known.
- [ ] Production verification done.
- [ ] Back-merged to main.
- [ ] Regression/follow-up created.

---

## Release Hygiene Checklist

- [ ] Version/tag created.
- [ ] Tag maps to commit.
- [ ] Artifact/image digest recorded.
- [ ] Release notes current.
- [ ] Changelog updated.
- [ ] Migration notes included.
- [ ] Known issues listed.
- [ ] Rollback target known.
- [ ] Support handoff complete.

---

## Documentation Hygiene Checklist

- [ ] README updated if commands/scope changed.
- [ ] API docs updated if contract changed.
- [ ] Runbook updated if operations changed.
- [ ] ADR updated if architecture decision changed.
- [ ] Wiki source updated if published docs changed.
- [ ] Supported claims evidence-backed.
- [ ] Future work labelled planned.
- [ ] Links valid.

---

## Audit Traceability Checklist

- [ ] Issue/story/RFC linked.
- [ ] PR linked.
- [ ] Review recorded.
- [ ] CI evidence available.
- [ ] Security exceptions recorded.
- [ ] Release tag available.
- [ ] Deployment record available.
- [ ] Evidence retained.
- [ ] Residual risk tracked.

---

## Engineering Lead Operating Rhythm

| Cadence | Activity |
|---|---|
| Every PR | Review scope, evidence, CI, and documentation truth. |
| Weekly | Review stale branches, flaky checks, and PR cycle time. |
| Every release | Review tags, release notes, evidence, and branch hygiene. |
| Monthly | Review CODEOWNERS, branch protection, and workflow policy. |
| After incident | Review whether Git/PR/CI evidence helped diagnosis. |
| Quarterly | Review workflow maturity and simplify where possible. |

---

## Summary

Git workflow discipline is engineering leadership in daily practice.

Good branches, commits, PRs, reviews, merges, and release records make teams faster because they make change safer and clearer.
