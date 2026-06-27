# AI Architecture Patterns: RAG, Copilot, Agent, and Workflow

## Purpose

This file explains common AI architecture patterns and when to use them.

Enterprise AI architecture should preserve domain authority, data governance, user control, and evidence.

---

## Pattern 1: RAG Assistant

```text
User question
  -> entitlement check
  -> retrieve approved knowledge chunks
  -> construct grounded prompt
  -> model response
  -> source references
  -> safety checks
  -> answer
```

Use for:

- knowledge search
- policy explanation
- engineering documentation assistance
- support/runbook questions
- domain glossary explanation

---

## Pattern 2: Copilot

```text
User workflow
  -> AI suggestion
  -> user review
  -> edit/approve/reject
  -> audit/evidence
```

Use for:

- drafting
- summarizing
- explaining
- suggesting next actions
- reviewing content

Copilot should support user judgment, not replace it.

---

## Pattern 3: Agentic Workflow

```text
Goal
  -> plan
  -> tool selection
  -> authorization
  -> tool call
  -> observe result
  -> next step
  -> human approval where required
  -> audit trail
```

Use for bounded internal workflows only when tool calls are safe and governed.

---

## Pattern 4: AI Evaluation Service

```text
candidate output
  -> evaluator prompt/model/rules
  -> quality/safety scores
  -> decision or review flag
```

Use for:

- answer quality scoring
- hallucination checks
- policy compliance checks
- prompt regression tests
- generated content review

---

## Pattern 5: AI Workflow Pack

A workflow pack defines:

- prompt templates
- model configuration
- allowed tools
- allowed sources
- output schema
- safety checks
- evaluation criteria
- owner
- version
- evidence

---

## Architecture Boundaries

AI layer should not own:

- source data
- portfolio truth
- calculation truth
- entitlement truth
- report archive truth
- trade execution authority
- regulatory approval

AI layer can assist with explanation, drafting, classification, and controlled workflow steps.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| AI directly reads all databases | Entitlement and data-governance breach. |
| Agent tools have broad permissions | Unsafe actions. |
| RAG retrieves unapproved documents | Leakage and wrong answers. |
| Copilot output auto-published | No human control. |
| AI service owns business truth | Authority confusion. |
| Prompt templates not versioned | Unstable behavior. |

---

## Review Checklist

- Which AI pattern is used?
- Is domain authority preserved?
- Are sources approved?
- Is retrieval entitlement-aware?
- Are tools bounded?
- Is human review required?
- Is output schema defined?
- Is evidence captured?
- Is model risk assessed?
- Are failure modes safe?

---

## Summary

Choose the simplest AI architecture that satisfies the use case and risk controls.

Do not use agents where a grounded assistant or simple workflow is enough.
