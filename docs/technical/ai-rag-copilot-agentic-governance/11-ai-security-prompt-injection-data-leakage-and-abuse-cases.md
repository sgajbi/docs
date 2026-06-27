# AI Security, Prompt Injection, Data Leakage, and Abuse Cases

## Purpose

This file explains major AI security risks and control patterns.

AI systems expand the attack surface through prompts, retrieved content, tools, model outputs, and generated artifacts.

---

## Major Risks

| Risk | Example |
|---|---|
| Prompt injection | Retrieved document tells model to ignore policy. |
| Data leakage | Model outputs restricted context to unauthorized user. |
| Tool misuse | Agent sends or modifies data without approval. |
| Hallucinated evidence | Model invents citation or source. |
| Sensitive logging | Prompts or responses stored in logs. |
| Policy bypass | User asks model to reveal instructions or secrets. |
| Over-permissioned retrieval | RAG fetches data outside caller scope. |
| Unsafe output | AI generates regulated advice without review. |

---

## Prompt Injection Controls

Controls:

- treat retrieved content as untrusted
- separate system instructions from retrieved text
- instruct model to ignore source instructions
- filter or classify sources
- use allowlisted tools
- validate output
- test with injection examples
- require human review for sensitive use

---

## Data Leakage Controls

Controls:

- entitlement-aware retrieval
- field-level filtering
- prompt minimization
- no raw prompt logging
- safe output filtering
- evidence classification
- secure vector store
- access-audited retrieval
- no secrets in context

---

## Abuse Cases

Write abuse cases such as:

- user tries to retrieve another client's data
- prompt asks model to reveal system prompt
- document contains malicious instructions
- agent is asked to bypass approval
- generated report includes unsupported claim
- tool output includes secret
- stale source used as current guidance

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Trust retrieved text as instruction | Prompt injection. |
| Index all documents | Data leakage. |
| Log full prompts | Sensitive-data exposure. |
| Allow arbitrary tool calls | Unsafe action. |
| No abuse-case tests | Risk untested. |
| AI output trusted without grounding | Hallucination. |
| No human approval for high-risk output | Governance failure. |

---

## Review Checklist

- Are prompt-injection risks considered?
- Are retrieved sources treated as untrusted?
- Is retrieval entitlement-aware?
- Are prompts minimized?
- Are prompts/responses logged safely?
- Are tools allowlisted?
- Are high-risk outputs reviewed by humans?
- Are abuse cases tested?
- Are unsafe outputs blocked?
- Is evidence safe?

---

## Summary

AI security requires treating prompts, retrieved content, and tool calls as attack surfaces.

Design controls before exposing the capability.
