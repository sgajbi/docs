# Branch, PR, Merge, Versioning, and Release Hygiene

## Purpose

This file explains source-control and release hygiene for enterprise delivery.

Good hygiene makes change history understandable, reviewable, traceable, and supportable.

---

## Branch Strategy

Common patterns:

- short-lived feature branches
- protected main
- release branches where needed
- hotfix branches for urgent fixes
- no long-lived unmerged work without reason

Branch names should be meaningful:

```text
feature/add-contract-gate
fix/readiness-dependency-check
docs/release-evidence-runbook
quality/dependency-audit-baseline
```

---

## Commit Hygiene

Good commits:

- are small
- have clear messages
- preserve buildability where practical
- avoid unrelated changes
- avoid generated noise
- do not hide gate lowering

Poor commits:

```text
fix
changes
final
stuff
wip
```

---

## Merge Strategy

Possible strategies:

| Strategy | Use |
|---|---|
| Rebase merge | Linear history and individual commits preserved. |
| Squash merge | Simple history for small PRs. |
| Merge commit | Preserve feature branch context for larger work. |

Choose intentionally and document.

---

## Tags and Versions

Release tags and versions should be tied to:

- commit SHA
- artifact
- release notes
- SBOM/provenance
- deployment record
- rollback record

---

## Branch Cleanup

After merge:

- delete feature branch
- close stale PRs
- confirm main checks
- publish docs/wiki if needed
- update release notes
- track follow-up backlog

---

## Hygiene Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Long-lived feature branch | Merge conflicts and stale truth. |
| PR merged but branch left active | Confusion and accidental reuse. |
| Version not tied to artifact | Traceability weak. |
| Hotfix bypasses PR | Audit gap. |
| Docs changed on branch but never merged | Lost knowledge. |
| Release branch diverges silently | Production drift. |

---

## Review Checklist

- Is branch name clear?
- Are commits meaningful?
- Is PR scope reviewable?
- Is merge strategy appropriate?
- Is versioning clear?
- Are release tags tied to artifacts?
- Are stale branches cleaned?
- Are docs/wiki published after merge?
- Is follow-up backlog captured?

---

## Summary

Source-control hygiene is delivery memory.

A clean history helps future engineers understand what changed, why, how it was proved, and what was released.
