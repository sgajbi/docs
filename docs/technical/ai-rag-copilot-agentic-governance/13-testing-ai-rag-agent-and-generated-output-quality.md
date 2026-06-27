# Testing AI, RAG, Agent, and Generated Output Quality

## Purpose

This file explains how to test AI capabilities.

AI tests should prove grounding, safety, usefulness, entitlement, tool governance, and regression stability.

---

## Test Types

| Test Type | Purpose |
|---|---|
| Prompt unit test | Validate template and output schema. |
| Retrieval test | Validate correct chunks retrieved. |
| Entitlement test | Validate restricted content not retrieved. |
| Grounding test | Validate answer supported by source. |
| Refusal test | Validate unsupported questions refused. |
| Prompt-injection test | Validate malicious source/user instruction resisted. |
| Tool-use test | Validate agent tool authorization and audit. |
| Evaluation test | Score answer quality against dataset. |
| Regression test | Protect behavior after prompt/model/index changes. |

---

## RAG Tests

Test:

- relevant source retrieved
- stale source marked
- archived source excluded
- restricted source blocked
- answer cites correct source
- answer refuses unsupported claim
- prompt injection ignored
- no sensitive data leaked

---

## Agent Tests

Test:

- allowed tool call succeeds
- disallowed tool call blocked
- write action requires approval
- duplicate tool call is idempotent
- tool failure handled safely
- audit emitted
- unsafe action refused

---

## Output Quality Tests

Evaluate:

- factual correctness
- source support
- completeness
- clarity
- tone
- format validity
- safety
- refusal correctness
- absence of hallucinated citations

---

## AI Testing Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Manual demo only | Non-repeatable evidence. |
| No eval dataset | Quality drift. |
| No entitlement tests | Data leakage. |
| No prompt-injection tests | Security gap. |
| No regression after prompt change | Unexpected output change. |
| Output judged only by "sounds good" | False quality. |

---

## Review Checklist

- Is eval dataset defined?
- Are RAG retrieval tests present?
- Are entitlement tests present?
- Are grounding tests present?
- Are refusal tests present?
- Are prompt-injection tests present?
- Are tool-use tests present?
- Is output schema validated?
- Are tests run in CI/release gate?
- Are results retained?

---

## Summary

AI quality must be tested through realistic examples, failure cases, and adversarial scenarios.

A polished answer is not proof of a safe AI system.
