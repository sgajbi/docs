# Migration Scope, Inventory, Dependencies, and Wave Planning

## Purpose

This file explains how to define migration scope and plan migration waves.

A migration without a complete inventory will discover scope during cutover, which is dangerous.

---

## Migration Inventory

Inventory should include:

- applications
- services
- APIs
- data stores
- tables/collections
- files
- events
- reports
- documents
- users
- roles/permissions
- batch jobs
- schedules
- interfaces
- downstream consumers
- operational runbooks
- dashboards/alerts
- environments

---

## Dependency Mapping

Track dependencies:

```text
dependency:
owner:
source:
target:
contract:
frequency:
criticality:
migration impact:
fallback:
validation:
```

---

## Wave Planning

Migration waves can be organized by:

- region
- booking center
- client segment
- product type
- portfolio group
- application capability
- user group
- data domain
- integration partner

---

## Wave Readiness

Each wave should define:

- scope
- data volume
- dependencies
- readiness criteria
- reconciliation criteria
- cutover window
- rollback criteria
- support coverage
- sign-off owner
- known exclusions

---

## Scope Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Inventory only covers applications | Reports/jobs/interfaces missed. |
| Dependencies tracked informally | Late blockers. |
| Wave scope changes without governance | Sign-off confusion. |
| No volume estimate | Performance surprise. |
| No downstream consumer list | Breakage after cutover. |
| User groups not mapped | Access/support issues. |

---

## Review Checklist

- Is inventory complete?
- Are data volumes known?
- Are dependencies mapped?
- Are downstream consumers known?
- Are waves defined?
- Are wave criteria clear?
- Are exclusions documented?
- Are owners assigned?
- Are blockers tracked?
- Is sign-off owner clear?

---

## Summary

Migration planning starts with knowing what exists.

A strong inventory prevents cutover surprises.
