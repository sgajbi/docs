# Cross-Team Dependency, Integration, and Delivery Governance

## Purpose

This file explains how to manage cross-team dependencies and integration risk.

Enterprise platforms rarely deliver in isolation. Multiple teams own services, data, APIs, infrastructure, and user journeys.

---

## Dependency Types

| Type | Example |
|---|---|
| API dependency | BFF depends on domain service route. |
| Data dependency | Analytics depends on source snapshot. |
| Runtime dependency | Service depends on database/queue. |
| Delivery dependency | Team A must finish contract before Team B integrates. |
| Security dependency | Entitlement policy required before release. |
| Operational dependency | Runbook/dashboard needed before go-live. |
| Migration dependency | Cutover sequence across systems. |

---

## Dependency Register

Track:

```text
dependency:
owner:
consumer:
contract:
due date:
risk:
status:
blocker:
fallback:
evidence:
```

---

## Integration Governance

Good integration governance includes:

- contract-first design
- versioning
- consumer-driven tests
- integration environment
- mock/stub strategy
- dependency readiness review
- shared timeline
- fallback plan
- escalation path

---

## Release Coordination

For multi-team release:

- align scope
- freeze contracts
- confirm environments
- confirm migrations
- run integrated tests
- validate dashboards
- agree rollback
- support command center where needed
- document go/no-go criteria

---

## Dependency Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Dependency only tracked in chat | Forgotten blockers. |
| API contract changes late | Integration delay. |
| No fallback for delayed dependency | Release blocked. |
| No consumer tests | Breaks found late. |
| Ownership unclear | Escalation slow. |
| Integrated testing only at end | Late surprises. |

---

## Review Checklist

- Are dependencies listed?
- Is owner clear?
- Is contract defined?
- Is timeline realistic?
- Is fallback defined?
- Is integration test planned?
- Are risks visible?
- Are blockers escalated?
- Is release coordination active?
- Is evidence captured?

---

## Summary

Cross-team governance makes dependencies explicit.

The goal is not more meetings; it is fewer surprises.
