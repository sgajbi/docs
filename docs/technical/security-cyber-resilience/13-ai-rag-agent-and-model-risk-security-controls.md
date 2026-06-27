# AI, RAG, Agent, and Model-Risk Security Controls

## Purpose

This file explains security controls for AI-assisted workflows, RAG systems, copilots, and agentic automation.

AI systems introduce new risks around data access, prompt injection, hallucination, tool use, evidence, and human oversight.

---

## AI Security Risks

| Risk | Example |
|---|---|
| Data leakage | Prompt includes restricted client data. |
| Prompt injection | Retrieved content instructs model to ignore policy. |
| Unauthorized retrieval | RAG returns data outside caller scope. |
| Unsafe tool call | Agent performs action without approval. |
| Hallucinated evidence | Model invents source facts. |
| Over-authority | AI output treated as official decision. |
| Logging leakage | Prompts/responses stored in logs. |
| Model drift | Output quality changes without review. |

---

## RAG Controls

RAG should enforce:

- approved knowledge sources
- entitlement-aware retrieval
- source references
- freshness awareness
- prompt sanitization
- output grounding
- no unsupported claims
- human review for regulated content
- retrieval audit
- no raw restricted context in logs

---

## Agent Controls

Agentic workflows require:

- tool allowlist
- tool authorization
- human approval for sensitive actions
- action audit
- idempotency for write tools
- bounded execution
- prompt/output evidence policy
- failure and rollback path
- forbidden action list

---

## AI Output Posture

AI output should be classified:

| State | Meaning |
|---|---|
| Draft | Human review required. |
| Advisory support | Non-authoritative assistance. |
| Evidence-grounded | Source references available. |
| Client-ready | Only after approved review and controls. |
| Blocked | Output cannot be used due to policy. |

Do not treat generated text as authoritative by default.

---

## Model-Risk Evidence

Capture where required:

- workflow id
- prompt template version
- source references
- model/provider
- evaluation result
- guardrail outcome
- human review
- approval state
- output hash/reference
- audit trail

---

## AI Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| AI sees all data regardless of caller | Entitlement breach. |
| Prompts logged raw | Sensitive-data leak. |
| Generated advice sent to client without review | Regulatory/model risk. |
| Agent tools have broad permissions | Unauthorized actions. |
| RAG source references missing | Hallucination hard to detect. |
| Prompt injection ignored | Policy bypass. |
| AI evidence treated as source truth | Governance failure. |

---

## Review Checklist

- Are knowledge sources approved?
- Is retrieval entitlement-aware?
- Are prompts source-safe?
- Are outputs grounded?
- Are tool calls authorized?
- Are write actions human-reviewed?
- Are prompts/responses logged safely?
- Is model-risk evidence captured?
- Are unsupported uses blocked?
- Are tests present for prompt injection and unsafe output?

---

## Summary

AI security is data security, application security, and model-risk governance combined.

AI can assist regulated workflows only when grounded, scoped, reviewed, and evidenced.
