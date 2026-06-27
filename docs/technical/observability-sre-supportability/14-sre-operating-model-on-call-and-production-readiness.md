# SRE Operating Model, On-Call, and Production Readiness

## Purpose

This file explains the operating model needed to support services in production.

SRE is not only a team. It is a set of practices for reliability, ownership, observability, incident response, and continuous improvement.

---

## Production Ownership

Every service should define:

- owning team
- on-call or support rota
- escalation path
- service criticality
- runbooks
- dashboards
- alerts
- SLOs
- known limits
- release process
- rollback process

---

## On-Call Readiness

On-call engineers need:

- access
- dashboards
- alerts
- runbooks
- service map
- recent deployment visibility
- dependency ownership
- rollback procedure
- communication channel
- escalation contacts

---

## Production Readiness Review

A production-readiness review should check:

- service mission
- runtime profile
- health/readiness
- logs/metrics/traces
- dashboards
- alerts
- runbooks
- SLOs
- security posture
- dependency posture
- capacity
- rollback
- DR/backup where needed
- support handoff
- known risks

---

## Operating Rhythm

| Cadence | Activity |
|---|---|
| Daily | Review incidents and active alerts. |
| Weekly | Review SLOs, noisy alerts, and top errors. |
| Sprint | Review production readiness for new capabilities. |
| Monthly | Review runbooks, dashboards, and reliability backlog. |
| Quarterly | Review SRE maturity and major risk themes. |
| After incident | Problem review and control improvement. |

---

## SRE Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No clear owner | Incidents bounce between teams. |
| On-call lacks access | Slow response. |
| No runbook | Tribal support. |
| No SLO | Reliability expectations unclear. |
| Alerts route to wrong team | Response delay. |
| Production readiness reviewed after deploy | Risk discovered too late. |
| Incidents not converted to backlog | Repeated failures. |

---

## Review Checklist

- Is owner clear?
- Is on-call model defined?
- Are dashboards available?
- Are alerts actionable?
- Are runbooks current?
- Are SLOs defined?
- Is rollback documented?
- Are dependencies mapped?
- Is support handoff complete?
- Are incident learnings tracked?

---

## Summary

SRE is how engineering quality survives contact with production.

A service is not production-ready until people, process, telemetry, and recovery are ready too.
