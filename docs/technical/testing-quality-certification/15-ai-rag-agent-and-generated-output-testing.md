# AI, RAG, Agent, and Generated Output Testing

## Purpose

This file explains how to test AI-assisted workflows, RAG, copilots, agentic tools, and generated output.

AI features require testing beyond normal API correctness because outputs can be probabilistic, context-sensitive, and security-sensitive.

---

## AI Test Areas

Test:

- prompt template structure
- approved knowledge sources
- retrieval scope
- entitlement-aware retrieval
- prompt injection resistance
- grounding and citations
- hallucination boundaries
- unsafe output filtering
- tool authorization
- human review workflow
- model-risk evidence
- no raw prompt/response leakage
- fallback behavior

---

## RAG Testing

RAG tests should cover:

- relevant document retrieved
- restricted document not retrieved
- stale document warning
- missing source behavior
- citation/source reference included
- answer refuses unsupported claim
- prompt injection document ignored
- caller scope enforced

---

## Agent Testing

Agent tests should cover:

- allowed tool call
- denied tool call
- write tool requires approval
- idempotency for tool action
- unsafe action blocked
- tool failure handled
- audit event emitted
- no secret in tool call
- no raw sensitive output stored

---

## Generated Output Testing

Test output for:

- grounded facts
- no unsupported claims
- safe language
- no client-ready publication without approval
- no restricted data
- no invented evidence
- required disclaimers where relevant
- review state captured

---

## Evaluation Sets

Use evaluation datasets for:

- expected answers
- refusal cases
- prompt injection
- retrieval correctness
- tone/style
- factual grounding
- safety boundaries
- regression tracking

Evaluation data must be synthetic or approved.

---

## AI Testing Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Manual prompt checks only | Non-repeatable. |
| No prompt injection tests | Policy bypass. |
| RAG ignores entitlements | Data leak. |
| AI output accepted as truth | Governance failure. |
| No evaluation dataset | Quality drift invisible. |
| Raw prompts logged | Sensitive-data leakage. |
| Tool calls not authorized | Unsafe actions. |

---

## Review Checklist

- Are knowledge sources approved?
- Is retrieval scoped?
- Are prompt injection tests present?
- Are outputs grounded?
- Are refusals tested?
- Are tool calls authorized?
- Are write actions human-reviewed?
- Is model-risk evidence captured?
- Are prompts/responses safe?
- Are evals run in CI or release gate?

---

## Summary

AI testing must prove safety, grounding, entitlement, and human-control behavior.

AI quality is not only whether the output sounds good.
