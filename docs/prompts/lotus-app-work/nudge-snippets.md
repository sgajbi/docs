# Nudge Snippets

Use these only after the main task prompt has already established repository, RFC, scope, and operating rules.

## General Quality Nudge

```text
Keep simplifying and tightening the implementation. Improve readability, modularity, maintainability, reliability, and test quality as part of the slice. Avoid cosmetic changes and superficial tests. Leave the codebase cleaner than you found it.
```

## Code Standard Nudge

```text
Write modular, clean, highly readable code with strong domain understanding. Tests must be meaningful and comprehensive, not superficial. APIs should use carefully modeled private-banking vocabulary, complete documentation, clear examples, and proper error behavior.
```

## Continue Implementation Nudge

```text
Continue slice by slice. Do not move forward until the current slice is implemented, validated, reviewed, documented, and in a solid state. Keep CI under control, push meaningful commits when appropriate, and fix failures promptly.
```

## RFC Second-Last Slice Nudge

```text
Before closure, perform a proper hardening review. Verify implementation completeness, API certification pattern, platform governance, data-mesh posture, Swagger quality, error handling, security treatment, tests, docs, wiki, supported-features, and evidence. Tighten loose ends before final closure.
```

## RFC Final Closure Nudge

```text
If all slices are complete, close the RFC properly: update documentation, context, wiki, supported-features, and branch hygiene; publish wiki if needed; verify no durable truth is stranded; sync main; confirm clean status. If any gap remains, finish it before claiming closure.
```

## UI Quality Nudge

```text
Every UI feature must be backed by supported Gateway/backend functionality. Improve usability, decision clarity, flow, consistency, performance, and polish. Reduce technical leakage and make the screen useful for front-office users.
```

## Backend Hardening Nudge

```text
Strengthen architecture, API quality, tests, security, observability, supportability, performance, and documentation. Remove dead code and duplication. Keep business logic out of routers and infrastructure. Preserve source ownership boundaries.
```

## Branch Hygiene Nudge

```text
Before claiming completion, run stranded-truth reconciliation, verify wiki publication where needed, confirm local and remote main are aligned, delete obsolete feature branches, and leave the worktree clean.
```

## No-Squash Linear History Nudge

```text
Use normal meaningful commit history. Do not squash unless explicitly instructed. Keep each commit scoped, truthful, and easy to review.
```

## LinkedIn Closure Nudge

```text
Draft post-completion LinkedIn content only from what was actually implemented and proven. Keep it employer-safe, non-confidential, non-promotional, and free of unsupported product or regulatory claims.
```
