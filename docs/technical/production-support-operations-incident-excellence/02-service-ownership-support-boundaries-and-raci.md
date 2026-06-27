# Service Ownership, Support Boundaries, and RACI

## Purpose

This file explains how to define service ownership and support boundaries.

Operational support fails when ownership is unclear, especially across application, platform, data, security, and business teams.

---

## Service Ownership

Every service should have:

- owning team
- product owner
- technical owner
- production support owner
- SRE/platform contact
- security owner or contact
- data owner where applicable
- documentation owner
- escalation owner

---

## Ownership Statement

Template:

```text
Service:
Purpose:
Owned capabilities:
Non-owned capabilities:
Critical dependencies:
Support owner:
Production escalation:
Runbooks:
Dashboards:
Known limitations:
```

---

## RACI

| Activity | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| Incident triage | Support | Support lead | Engineering/SRE | Business |
| Defect fix | Engineering | Engineering lead | QA/Product | Support |
| Platform outage | SRE/platform | Platform owner | App teams | Business |
| Data freshness issue | Data/app support | Data owner | Source system | Business |
| Security incident | Security | Security lead | Engineering/support | Leadership |
| Release rollback | Engineering/SRE | Release owner | Product/support | Business |

Adapt per organization.

---

## Dependency Ownership

Dependencies should be classified:

- owned by same team
- owned by internal team
- external vendor
- platform service
- source system
- downstream consumer
- shared service

For each dependency, define escalation route.

---

## Boundary Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Everyone assumes someone else owns it | Incident delay. |
| App team owns source data quality without authority | No resolution path. |
| Platform team blamed for application issue | Misrouted escalation. |
| No dependency escalation contacts | Long outage. |
| RACI exists but not used | Process theatre. |
| Ownership changes not documented | Stale support model. |

---

## Review Checklist

- Is accountable owner clear?
- Is responsible support team clear?
- Are dependencies owned?
- Are non-owned areas explicit?
- Is RACI documented?
- Are escalation contacts current?
- Are support boundaries reflected in runbooks?
- Are ownership changes reviewed?
- Is support handoff complete?

---

## Summary

Ownership clarity reduces incident duration.

A system is supportable when the right team can be engaged quickly with clear authority.
