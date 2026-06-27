# Git Workflow, PR Review And Release Hygiene

## Purpose

This technical guide explains how to use Git, branches, commits, pull requests, reviews, merge policies, release branches, tags, documentation updates, and evidence records in enterprise-grade engineering teams.

It is written for developers, senior engineers, architects, engineering leads, QA engineers, DevOps engineers, SREs, release managers, and platform teams who need source-control discipline to support controlled delivery, traceability, review quality, and production readiness.

The guide covers:

- Git as engineering delivery memory
- branch strategy and branch hygiene
- commit discipline and meaningful history
- feature branches and change slicing
- pull-request structure and evidence
- review responsibilities and reviewer expectations
- CODEOWNERS and ownership-based review
- merge strategy: merge commit, squash, rebase, and linear history
- main branch protection and release-line discipline
- versioning, tagging, release notes, and changelog hygiene
- hotfix and emergency change workflow
- CI integration and required status checks
- documentation and wiki/source truth alignment
- generated artifact and repository hygiene
- branch cleanup and post-merge discipline
- auditability, traceability, and regulated SDLC evidence
- engineering-lead checklists and operating rhythm

The guide is application-neutral and written as reusable technical knowledge for enterprise banking, wealth-management, platform, product, data, reporting, and AI-enabled engineering teams.

---

## Core Thesis

Git is not only a code storage tool. Git is the durable memory of engineering delivery.

A mature Git workflow answers:

```text
What changed?
Why did it change?
Who reviewed it?
What evidence proves it?
Which contracts changed?
Which risks remain?
Which release includes it?
How can it be rolled back?
What documentation changed?
What branch or PR still needs cleanup?
```

A repository with poor Git hygiene becomes difficult to review, audit, release, support, and improve.

---

## Audience

| Audience | Use |
|---|---|
| Developer | Learn branch naming, commit discipline, PR evidence, and review expectations. |
| Senior engineer | Improve change slicing, review depth, merge hygiene, and release traceability. |
| Architect | Review architecture-impacting PRs, ADR alignment, and contract changes. |
| Engineering lead | Establish delivery discipline, branch policy, review quality, and post-merge hygiene. |
| QA engineer | Connect PRs, tests, release evidence, and regression proof. |
| DevOps engineer | Align branch protection, CI checks, release branches, and artifact governance. |
| SRE | Trace production behavior back to commits, PRs, releases, and rollback plans. |
| Release manager | Use release branches, tags, changelogs, and evidence for controlled release. |
| Security engineer | Review workflow changes, CODEOWNERS, permissions, hotfix, and audit trails. |

---

## Reading Order

| Step | File | Purpose |
|---:|---|---|
| 1 | `README.md` | Pack purpose, thesis, audience, and navigation. |
| 2 | `01-git-as-engineering-delivery-memory.md` | Git as traceability, accountability, release, support, and audit memory. |
| 3 | `02-branching-strategy-feature-main-release-and-hotfix.md` | Branch types, naming, feature branches, release branches, hotfixes, and trunk discipline. |
| 4 | `03-commit-discipline-small-meaningful-and-reviewable-history.md` | Commit sizing, messages, atomic changes, refactor safety, and history quality. |
| 5 | `04-change-slicing-and-feature-branch-delivery.md` | How to split work into safe, reviewable, CI-friendly slices. |
| 6 | `05-pull-request-structure-evidence-and-description-quality.md` | PR templates, evidence, risk notes, validation, and reviewability. |
| 7 | `06-code-review-responsibilities-and-review-depth.md` | Reviewer expectations beyond syntax: architecture, tests, security, operations, and docs. |
| 8 | `07-codeowners-ownership-sensitive-files-and-review-routing.md` | Ownership-based review, sensitive areas, CODEOWNERS, and escalation. |
| 9 | `08-merge-strategies-rebase-squash-merge-commit-and-linear-history.md` | Merge policy trade-offs, history strategy, and audit implications. |
| 10 | `09-main-branch-protection-status-checks-and-green-main-discipline.md` | Protected main, required checks, status gates, broken-main response, and branch discipline. |
| 11 | `10-versioning-tags-changelogs-and-release-notes.md` | Versioning, tags, changelogs, release notes, and artifact traceability. |
| 12 | `11-hotfix-emergency-change-and-expedited-review-workflow.md` | Hotfix flow, emergency controls, evidence, and post-incident cleanup. |
| 13 | `12-generated-artifacts-repository-hygiene-and-sensitive-content.md` | Generated files, caches, build outputs, evidence artifacts, secrets, and repo cleanliness. |
| 14 | `13-documentation-wiki-source-and-durable-truth-hygiene.md` | README, docs, wiki source, runbooks, ADRs, and keeping truth merged and discoverable. |
| 15 | `14-post-merge-branch-cleanup-and-release-line-maintenance.md` | Branch deletion, stale PRs, post-merge checks, wiki publication, and release-line cleanup. |
| 16 | `15-audit-traceability-compliance-and-engineering-evidence.md` | SDLC traceability, audit evidence, control mapping, and regulated delivery records. |
| 17 | `16-git-workflow-checklists-and-engineering-lead-playbook.md` | Practical checklists and operating model for Git and PR governance. |

---

## Design Principles

1. **Main branch should represent durable truth.**
2. **Feature branches should be short-lived and focused.**
3. **Commits should be small, meaningful, and reviewable.**
4. **PRs should explain change, proof, risk, and follow-up.**
5. **Reviews should cover behavior, architecture, tests, security, operations, and documentation.**
6. **CI checks should protect branch policy and release confidence.**
7. **Documentation should be updated in the same branch as implementation truth.**
8. **Generated artifacts should be controlled and intentional.**
9. **Hotfixes should be fast but still traceable and reviewed.**
10. **Post-merge hygiene is part of definition of done.**
11. **Release tags and artifacts should map back to commits and PRs.**
12. **Audit evidence should be produced by normal workflow, not manually recreated later.**

---

## What Good Looks Like

A mature Git workflow has:

- protected main branch
- clear branch naming convention
- short-lived feature branches
- meaningful commits
- reviewable PR size
- PR template with validation evidence
- CODEOWNERS or owner-routing for sensitive areas
- required CI status checks
- clear merge policy
- release tags tied to artifacts
- changelog/release-note discipline
- hotfix workflow
- branch cleanup
- documentation/wiki/source alignment
- generated artifact hygiene
- no secrets or sensitive data in repo
- traceability from issue/story to PR to commit to release
- engineering-lead operating rhythm for review quality

---

## Disclaimer

This material is for technical education, engineering workflow design, SDLC governance, KT, and engineering leadership development.

It is not legal, regulatory, audit, cybersecurity certification, procurement, investment, tax, or client advice.

## Curation Note

Curated into the technical knowledge base on 2026-06-27 and normalized for reusable engineering workflow, PR review, release hygiene and engineering-leadership guidance. Local source paths and package provenance are intentionally not retained.
