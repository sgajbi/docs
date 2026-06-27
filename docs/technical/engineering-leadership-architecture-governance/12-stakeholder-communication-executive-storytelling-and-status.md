# Stakeholder Communication, Executive Storytelling, and Status

## Purpose

This file explains how technical leaders communicate complex engineering work to different stakeholders.

A strong engineering leader can translate technical reality into business-relevant status, risk, and decisions.

---

## Communication Audiences

| Audience | Needs |
|---|---|
| Executives | Outcome, risk, timeline, decision needed. |
| Product owners | Capability, scope, trade-offs, user impact. |
| Business stakeholders | Business continuity, readiness, limitations. |
| Risk/security | Controls, evidence, residual risk. |
| Engineers | Technical detail, decisions, implementation path. |
| Operations | Runbooks, alerts, support, rollback. |
| QA | Test strategy, acceptance, certification. |

---

## Status Structure

A good status update includes:

```text
Outcome:
Current state:
Progress:
Risks:
Dependencies:
Decisions needed:
Next milestone:
Evidence:
```

---

## Technical Risk In Business Language

Translate:

```text
Database migration has no rollback plan.
```

Into:

```text
If the release causes data-shape issues, recovery may take longer because rollback is not yet automated. We are adding migration smoke tests and a compensation plan before release.
```

---

## Executive Storytelling

Useful structure:

```text
1. What business capability is affected?
2. What is the current status?
3. What is the risk or decision?
4. What evidence do we have?
5. What support is needed?
```

---

## Communication Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Too much technical detail for executives | Message lost. |
| Hide risks until late | Trust damage. |
| Say "done" without evidence | False readiness. |
| Only red/amber/green with no explanation | Poor decision support. |
| Blame other teams | Collaboration weakens. |
| No decision ask | Stakeholders cannot help. |

---

## Review Checklist

- Who is the audience?
- What decision or understanding is needed?
- Is status evidence-backed?
- Are risks clear?
- Are dependencies clear?
- Is language appropriate?
- Are limitations honest?
- Is ask explicit?
- Is next step clear?

---

## Summary

Communication is part of engineering leadership.

Good communication makes technical truth usable for decisions.
