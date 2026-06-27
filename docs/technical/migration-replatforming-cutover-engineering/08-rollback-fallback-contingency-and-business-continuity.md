# Rollback, Fallback, Contingency, and Business Continuity

## Purpose

This file explains how to design rollback and fallback for migration.

Rollback and fallback must be designed before cutover, not invented during failure.

---

## Rollback Versus Fallback

| Term | Meaning |
|---|---|
| Rollback | Return to previous system/state. |
| Fallback | Use alternative path or degraded process. |
| Contingency | Prepared response for known risk scenario. |
| Business continuity | Ability to continue critical operations. |

---

## Rollback Plan

Define:

- rollback trigger
- rollback decision owner
- rollback steps
- data consistency implications
- user communication
- integration switchback
- validation after rollback
- time limit for rollback feasibility
- irreversible steps

---

## Fallback Options

Examples:

- read-only mode
- manual processing
- old report path
- temporary file interface
- delayed batch
- partial capability enablement
- route subset of users back
- disable new feature flag
- use previous data snapshot with stale warning

---

## Irreversibility

Some steps may be hard to reverse:

- data writes in target
- external notifications
- document delivery
- irreversible schema changes
- customer-facing outputs
- decommissioned source
- DNS/cert changes without rollback

Identify these before cutover.

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Rollback assumed but not tested | False safety. |
| No rollback decision owner | Delay during incident. |
| Data writes continue during rollback | Inconsistency. |
| Business fallback not agreed | Operations stop. |
| Irreversible step not identified | Surprise. |
| Contingency only technical | Business process gap. |

---

## Review Checklist

- Is rollback possible?
- What is rollback trigger?
- Who decides rollback?
- What is rollback time window?
- What data changes must be reconciled?
- What fallback exists?
- What business process continues?
- What steps are irreversible?
- Has rollback been rehearsed?
- Is communication prepared?

---

## Summary

Rollback is not a hope. It is a designed and rehearsed capability.

Where rollback is impossible, fallback and contingency must be stronger.
