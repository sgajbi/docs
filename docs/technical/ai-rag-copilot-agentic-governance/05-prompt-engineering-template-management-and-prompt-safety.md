# Prompt Engineering, Template Management, and Prompt Safety

## Purpose

This file explains how to design, version, review, and govern prompts.

Prompts are application logic. They should be treated as versioned, testable, reviewable assets.

---

## Prompt Template Components

A prompt template may include:

- role/system instruction
- task description
- boundaries
- source context
- output format
- refusal conditions
- safety instructions
- citation requirements
- user input
- tool-use rules
- examples

---

## Prompt Versioning

Prompts should have:

- name
- version
- owner
- purpose
- approved use cases
- input schema
- output schema
- model compatibility
- evaluation dataset
- change history
- review status

---

## Prompt Safety Rules

Prompts should instruct the model to:

- use only provided sources where required
- cite sources where required
- refuse unsupported claims
- avoid revealing system instructions
- avoid exposing secrets
- ignore prompt-injection instructions in retrieved content
- ask for human review when required
- not perform unauthorized actions
- not invent evidence

---

## Output Schema

Structured output improves reliability.

Example:

```json
{
  "answer": "...",
  "sources": [],
  "confidence": "medium",
  "limitations": [],
  "requiresHumanReview": true
}
```

---

## Prompt Lifecycle

```text
draft
  -> review
  -> evaluation
  -> approved
  -> deployed
  -> monitored
  -> revised
  -> deprecated
```

---

## Prompt Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Prompt hardcoded in code with no owner | Hard to govern. |
| Prompt changes without tests | Quality drift. |
| No refusal instruction | Unsupported answers. |
| No source requirement | Hallucination risk. |
| Prompt logs sensitive data | Data leakage. |
| Output free-form where structure needed | Integration brittleness. |
| Prompt copied across services | Drift and inconsistency. |

---

## Review Checklist

- Is prompt versioned?
- Is owner defined?
- Is use case clear?
- Are boundaries explicit?
- Are sources constrained?
- Is output schema defined?
- Are refusal conditions included?
- Is prompt-injection considered?
- Are evaluations run?
- Is change history recorded?

---

## Summary

Prompts are production assets.

Treat them with the same discipline as API contracts and domain rules.
