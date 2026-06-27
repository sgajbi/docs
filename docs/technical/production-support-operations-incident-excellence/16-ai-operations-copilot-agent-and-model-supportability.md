# AI Operations, Copilot, Agent, and Model Supportability

## Purpose

This file explains operational support for AI-enabled capabilities.

AI features require support for model behavior, prompt versions, retrieval, grounding, cost, tool calls, safety events, and human review workflows.

---

## AI Operational Signals

Track:

- AI request count
- model used
- prompt version
- retrieval source count
- grounding status
- citation availability
- refusal rate
- safety-filter trigger
- tool-call count
- human-review queue
- latency
- token usage
- cost
- error rate
- feedback rate
- unsafe-output reports

---

## AI Incident Types

Examples:

- hallucinated answer
- wrong citation
- stale source used
- prompt injection succeeded
- restricted source retrieved
- tool called incorrectly
- AI output published without approval
- model/provider outage
- cost spike
- latency spike
- prompt/model regression

---

## AI Runbooks

Runbooks should cover:

- disable AI feature flag
- rollback prompt version
- rollback model route
- remove/disable source from index
- rebuild vector index
- block tool
- review unsafe output
- investigate source retrieval
- cost spike triage
- provider outage fallback

---

## Agentic Support

For agents:

- tool calls must be audited
- write actions require approval where needed
- duplicate actions must be idempotent
- unsafe tools must be blockable
- support must inspect safe action history
- rollback/compensation must be defined

---

## AI Ops Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| AI answer issues handled like normal UI bug | Model/source risk missed. |
| Prompt version not logged | Regression hard to diagnose. |
| Raw prompts logged with sensitive data | Privacy risk. |
| Tool calls unaudited | Accountability gap. |
| No feature flag to disable AI | Incident duration. |
| Vector index cannot be rebuilt | Stale/bad retrieval persists. |
| Human review queue not monitored | Publication delays. |

---

## Review Checklist

- Are AI metrics defined?
- Is prompt/model version observable?
- Is retrieval trace available safely?
- Are tool calls audited?
- Is AI feature disable path ready?
- Is prompt/model rollback defined?
- Is index rebuild runbook ready?
- Is human-review queue monitored?
- Are unsafe-output reports triaged?
- Are AI incidents included in support model?

---

## Summary

AI supportability requires observing and controlling model, prompt, retrieval, tool, and human-review behavior.

AI features need operations design before production use.
