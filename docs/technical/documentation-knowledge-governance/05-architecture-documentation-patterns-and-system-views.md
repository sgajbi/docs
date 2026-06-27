# Architecture Documentation Patterns and System Views

## Purpose

This file explains how to document architecture clearly.

Architecture documentation should help teams understand system boundaries, integration, runtime, data flow, decisions, and operational behavior.

---

## Architecture Views

Useful views:

| View | Purpose |
|---|---|
| Context view | Shows users, systems, and external dependencies. |
| Container/service view | Shows major applications and services. |
| Component view | Shows modules within a service. |
| Data flow view | Shows data movement, ownership, lineage. |
| Runtime view | Shows deployment, containers, dependencies. |
| Sequence view | Shows request/workflow interactions. |
| Security view | Shows trust boundaries and controls. |
| Operations view | Shows health, alerts, dashboards, runbooks. |
| Migration view | Shows old/new systems and cutover. |

---

## Boundary Documentation

Every service architecture doc should state:

- what it owns
- what it consumes
- what it exposes
- what it persists
- what it does not own
- failure modes
- supportability states
- operational owner

---

## Integration Documentation

Integration docs should include:

- source system
- target system
- protocol
- contract
- frequency
- data ownership
- retry/replay behavior
- failure handling
- security
- observability

---

## Runtime Architecture

Runtime docs should include:

- service processes
- containers
- ports
- configuration
- dependencies
- health/readiness
- metrics
- secrets
- deployment strategy
- scaling
- rollback

---

## Architecture Doc Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| One giant diagram | Hard to read. |
| Diagram lacks ownership | Boundary confusion. |
| No runtime view | Deployment issues. |
| No failure view | Production risk hidden. |
| Diagrams not versioned | Stale architecture. |
| Future state not labelled | False truth. |
| No decision context | People revisit settled debates. |

---

## Review Checklist

- Is audience clear?
- Is view type clear?
- Are boundaries explicit?
- Are data flows shown?
- Are integrations documented?
- Are runtime concerns included?
- Are failure modes documented?
- Are security boundaries shown?
- Is future state labelled?
- Are diagrams versioned?

---

## Summary

Architecture docs should make systems easier to reason about.

Use multiple focused views instead of one overloaded picture.
