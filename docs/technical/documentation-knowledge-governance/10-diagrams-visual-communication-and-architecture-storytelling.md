# Diagrams, Visual Communication, and Architecture Storytelling

## Purpose

This file explains how to create useful architecture diagrams and visual explanations.

Diagrams should clarify, not decorate.

---

## Diagram Types

| Diagram | Purpose |
|---|---|
| Context diagram | Shows external actors and systems. |
| Container/service diagram | Shows major applications/services. |
| Component diagram | Shows internal modules. |
| Sequence diagram | Shows interaction flow. |
| Data-flow diagram | Shows data movement and ownership. |
| Deployment diagram | Shows runtime topology. |
| Security diagram | Shows trust boundaries and controls. |
| Operational diagram | Shows monitoring, alerts, runbooks, recovery. |
| Migration diagram | Shows current, target, and transition states. |

---

## Good Diagram Rules

A good diagram:

- has title
- has audience
- has scope
- labels ownership
- labels protocols
- shows boundaries
- avoids excessive detail
- distinguishes current/future state
- includes legend
- is versioned
- links to supporting text

---

## Architecture Storytelling

Good architecture communication follows:

```text
business problem
  -> capability boundary
  -> user/system flow
  -> service ownership
  -> data ownership
  -> runtime behavior
  -> failure and support
  -> evidence
  -> open gaps
```

---

## Current Versus Future

Always label:

- current state
- target state
- transitional state
- proposed state
- deprecated state

Do not mix current and future without visual distinction.

---

## Diagram Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| One diagram shows everything | Unreadable. |
| No legend | Misinterpretation. |
| No ownership labels | Boundary confusion. |
| Future state not labelled | False truth. |
| Diagram not stored in source | Hard to maintain. |
| Screenshot-only diagram | Hard to diff/review. |
| No text explanation | Diagram lacks context. |

---

## Review Checklist

- Is audience clear?
- Is scope clear?
- Is title meaningful?
- Are boundaries shown?
- Is ownership labelled?
- Are current/future states separated?
- Is diagram readable?
- Is legend included?
- Is source stored?
- Is supporting text linked?

---

## Summary

Good diagrams are thinking tools.

They help teams align on ownership, flow, risk, and evidence.
