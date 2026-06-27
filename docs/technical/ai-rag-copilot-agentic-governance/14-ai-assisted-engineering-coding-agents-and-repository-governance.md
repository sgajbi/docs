# AI-Assisted Engineering, Coding Agents, and Repository Governance

## Purpose

This file explains how to use AI coding assistants and repository agents safely.

AI-assisted engineering can accelerate delivery, but it must follow the same engineering standards as human-authored work.

---

## Good Uses

AI can help:

- draft code
- refactor modules
- generate tests
- improve docs
- summarize architecture
- prepare PR descriptions
- find duplicate code
- suggest CI improvements
- create runbooks
- generate examples

---

## Required Guardrails

AI-generated changes must still have:

- clear scope
- branch discipline
- small commits
- tests
- CI validation
- security scan
- human review
- documentation updates
- no secrets
- no unsupported claims
- evidence summary

---

## Agent Operating Contract

Agents should be instructed to:

- read repository context first
- preserve behavior unless told otherwise
- avoid broad rewrites
- make small reviewable changes
- run validation
- update docs when truth changes
- avoid sensitive data
- never lower gates to pass
- report gaps honestly
- prepare PR evidence

---

## Coding-Agent Risks

| Risk | Example |
|---|---|
| Hallucinated APIs | Code calls nonexistent route. |
| Weak tests | Tests assert generated implementation only. |
| Behavior drift | Refactor changes semantics. |
| Security issue | Agent logs sensitive data. |
| Scope creep | Agent changes unrelated files. |
| Documentation overclaim | Planned feature described as implemented. |
| Workflow risk | Agent modifies CI permissions unsafely. |

---

## Review Checklist

- Is AI-generated scope clear?
- Are changes small?
- Are tests meaningful?
- Are docs truthful?
- Are secrets absent?
- Are security-sensitive files reviewed?
- Does CI pass?
- Are unsupported claims avoided?
- Is PR evidence clear?
- Are follow-up gaps documented?

---

## Summary

AI-assisted coding should strengthen engineering systems, not bypass them.

Human review and evidence remain mandatory.
