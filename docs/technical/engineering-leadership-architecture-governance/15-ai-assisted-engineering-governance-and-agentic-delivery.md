# AI-Assisted Engineering Governance and Agentic Delivery

## Purpose

This file explains how engineering leaders should govern AI-assisted coding, documentation, testing, and repository automation.

AI can accelerate delivery, but it must operate within engineering standards, review, evidence, and security boundaries.

---

## AI-Assisted Engineering Uses

Useful uses:

- code scaffolding
- test generation
- documentation drafting
- refactoring assistance
- API examples
- runbook drafts
- code review preparation
- dependency analysis
- migration planning
- knowledge-base summarization

---

## Governance Principles

AI-assisted changes should follow the same standards as human-written changes:

- clear goal
- branch discipline
- small commits
- tests
- CI validation
- security checks
- review
- documentation updates
- evidence
- no unsupported claims

---

## Agentic Delivery Boundaries

Agents should be instructed to:

- read context first
- preserve behavior unless asked
- make small commits
- run validation
- avoid secrets
- avoid fabricating support
- update docs only when implementation truth changes
- report gaps honestly
- not bypass review

---

## Risks

| Risk | Example |
|---|---|
| Hallucinated implementation | Agent claims feature exists. |
| Unsafe refactor | Behavior changes silently. |
| Test weakness | Generated tests assert implementation, not behavior. |
| Secret leakage | Agent includes `.env` or logs. |
| Documentation overclaim | Planned feature written as current. |
| Large unreviewable PR | Review quality drops. |
| Security bypass | Tool changes workflow permissions. |

---

## AI Governance Controls

Controls:

- agent operating contract
- repository context files
- protected branches
- required PR review
- CI gates
- no-sensitive-content checks
- prompt templates
- human approval for risky changes
- evidence requirements
- generated-code review standards

---

## Review Checklist

- Was AI used within repo standards?
- Is scope clear?
- Are changes reviewable?
- Are tests meaningful?
- Are docs truthful?
- Are secrets absent?
- Are security-sensitive files reviewed?
- Does CI pass?
- Are unsupported claims avoided?
- Are gaps reported honestly?

---

## Summary

AI can speed up engineering, but it must not weaken engineering judgment.

Leaders should use AI to amplify standards, not bypass them.
