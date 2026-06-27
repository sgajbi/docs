# Agentic Workflows, Tool Use, Authorization, and Action Safety

## Purpose

This file explains how to govern AI agents that can call tools or perform actions.

Agentic systems introduce higher risk because they can change state, trigger workflows, or access systems through tools.

---

## Agent Workflow

```text
user goal
  -> planner
  -> tool selection
  -> authorization check
  -> tool call
  -> result observation
  -> next action
  -> human approval when required
  -> audit trail
```

---

## Tool Governance

Every tool should define:

```text
tool name:
purpose:
allowed users:
allowed agent workflows:
input schema:
output schema:
side effects:
authorization:
idempotency:
audit:
rate limits:
failure behavior:
human approval:
```

---

## Tool Risk Levels

| Tool Type | Risk |
|---|---|
| Read-only public search | Low |
| Read internal docs | Medium |
| Read restricted data | High |
| Create draft | Medium |
| Send message/email | High |
| Modify records | High |
| Approve workflow | Critical |
| Execute trade/payment | Usually blocked or extremely controlled |

---

## Action Safety Controls

Controls:

- tool allowlist
- input schema validation
- authorization check
- human approval
- idempotency key
- dry-run mode
- action preview
- confirmation
- audit event
- rollback/compensation plan
- rate limit
- unsafe-action blocklist

---

## Agent Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Agent can use any API | Unbounded action risk. |
| Tool call without authorization | Security breach. |
| No human approval for write action | Unsafe automation. |
| No audit of action | Accountability gap. |
| No idempotency | Duplicate side effects. |
| Tool output blindly trusted | Error propagation. |
| Agent can access secrets | Critical risk. |

---

## Review Checklist

- Are tools allowlisted?
- Is each tool risk-classified?
- Is authorization enforced?
- Is human approval required where needed?
- Are write actions idempotent?
- Are tool inputs validated?
- Are tool outputs checked?
- Are actions audited?
- Is rollback/compensation defined?
- Are unsafe actions blocked?

---

## Summary

Agents need stronger governance than chat assistants.

The moment AI can act, authorization, idempotency, audit, and human approval become critical.
