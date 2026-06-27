# Commit Discipline: Small, Meaningful, and Reviewable History

## Purpose

This file explains how to write commits that support review, traceability, and future maintenance.

A commit should be a meaningful unit of change.

---

## Good Commit Characteristics

A good commit is:

- small
- focused
- buildable where practical
- clearly named
- logically grouped
- free of unrelated formatting noise
- free of generated artifacts unless intentional
- easy to revert if needed

---

## Commit Message Format

A practical format:

```text
<type>: <short summary>
```

Examples:

```text
feat: add report job replay command
fix: handle stale source readiness state
test: cover idempotency conflict path
docs: update API supportability notes
refactor: extract portfolio access policy
ci: add OpenAPI quality gate
```

---

## Commit Types

| Type | Use |
|---|---|
| `feat` | New capability. |
| `fix` | Defect fix. |
| `test` | Test addition or correction. |
| `docs` | Documentation update. |
| `refactor` | Structural change without intended behavior change. |
| `ci` | Workflow or validation change. |
| `chore` | Maintenance. |
| `security` | Security-specific hardening. |
| `perf` | Performance improvement. |

---

## Atomic Commits

An atomic commit changes one thing.

Good sequence:

```text
test: add regression for duplicate report submission
fix: enforce idempotency key on report submission
docs: document report submission idempotency behavior
```

Poor sequence:

```text
fix stuff
```

---

## Commit Hygiene Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Giant commit | Hard to review and revert. |
| Mixed formatting and logic | Diff noise. |
| Vague message | Future confusion. |
| Commit lowers gate | Quality erosion. |
| Commit includes local cache | Repo pollution. |
| Commit includes secrets | Security incident. |
| Refactor changes behavior silently | Regression risk. |

---

## Review Checklist

- Is each commit focused?
- Is message meaningful?
- Are generated files intentional?
- Are secrets absent?
- Is behavior preserved or explained?
- Is refactor separated from feature?
- Is documentation commit aligned with code?
- Can a commit be reverted safely?

---

## Summary

Good commits make engineering history understandable.

Small, meaningful commits reduce review effort and improve release confidence.
