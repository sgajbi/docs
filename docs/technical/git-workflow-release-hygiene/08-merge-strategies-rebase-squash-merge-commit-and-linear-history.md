# Merge Strategies: Rebase, Squash, Merge Commit, and Linear History

## Purpose

This file explains common merge strategies and their trade-offs.

The right merge strategy depends on repository policy, review style, release traceability, and team workflow.

---

## Rebase Merge

Rebase merge applies commits linearly on top of the target branch.

Benefits:

- linear history
- preserves commit sequence
- easier bisecting
- clean main branch

Trade-offs:

- commits should be meaningful
- force-push/rebase discipline needed
- review context may be affected if misused

---

## Squash Merge

Squash merge combines all PR commits into one commit.

Benefits:

- simple main history
- good for small PRs
- hides messy feature branch commits

Trade-offs:

- loses detailed commit sequence
- harder to trace stepwise refactors
- large PR squashed into one commit can be too opaque

---

## Merge Commit

Merge commit preserves branch history and creates a merge node.

Benefits:

- preserves full branch context
- useful for large feature branches
- clear PR grouping

Trade-offs:

- non-linear history
- can become noisy if overused
- bisecting may be harder

---

## Choosing Strategy

| Situation | Good Strategy |
|---|---|
| Small focused PR | Squash or rebase |
| Carefully crafted commit series | Rebase |
| Large multi-commit feature | Merge commit or rebase depending policy |
| Regulated release branch | Merge commit may preserve context |
| Repo prefers linear history | Rebase or squash |
| Messy WIP branch | Squash after review |

---

## Merge Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Squash huge unrelated PR | History opaque. |
| Rebase after review without notice | Review context lost. |
| Merge commit for every tiny change | Noisy history. |
| Strategy differs by personal preference only | Inconsistent history. |
| Merge with failing checks | Broken main. |
| Merge without deleting branch | Hygiene issue. |

---

## Review Checklist

- Does repo define merge policy?
- Is PR size suitable for chosen strategy?
- Are commits meaningful if preserved?
- Is history useful for future debugging?
- Are checks green?
- Is branch deleted after merge?
- Are release tags aligned?

---

## Summary

Merge strategy is not just style.

It shapes the long-term readability and auditability of the repository.
