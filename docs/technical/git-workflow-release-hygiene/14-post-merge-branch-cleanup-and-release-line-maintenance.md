# Post-Merge Branch Cleanup and Release Line Maintenance

## Purpose

This file explains what should happen after a PR is merged.

The definition of done should include post-merge hygiene, not only merge approval.

---

## Post-Merge Checklist

After merge:

- confirm main branch checks
- delete feature branch
- update local branches
- publish wiki/docs if required
- verify generated evidence if needed
- confirm release notes/changelog if needed
- close linked issue/story
- record follow-up backlog
- check no stale PRs remain
- monitor early runtime signals if deployed

---

## Branch Cleanup

Clean branches reduce confusion.

Review:

- merged branches
- abandoned feature branches
- stale release branches
- duplicate experiment branches
- branches with unmerged docs
- branches with old CI failures

---

## Release Line Maintenance

For release branches:

- keep fixes minimal
- back-merge to main
- tag release candidates
- preserve evidence
- close branch after release window
- document supported versions
- avoid divergence

---

## Post-Merge Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Feature branch left active | Accidental reuse. |
| Docs merged but wiki not published | Users see stale docs. |
| Hotfix not back-merged | Regression. |
| Main check failure ignored | Release confidence weak. |
| Follow-up risk not tracked | Known gaps forgotten. |
| Release branch kept forever | Maintenance burden. |

---

## Review Checklist

- Is branch deleted?
- Are main checks green?
- Is wiki/docs publication done?
- Is release note updated?
- Are issues closed?
- Are follow-ups captured?
- Are hotfixes back-merged?
- Are stale branches reviewed?
- Is release branch still needed?

---

## Summary

Post-merge hygiene keeps repository truth clean.

Merge is not the end of delivery; it is the start of durable truth.
