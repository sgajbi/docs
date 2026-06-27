# Change, Release, Deployment, and Production Readiness Governance

## Purpose

This file explains how production support connects to release governance.

Operational readiness should be reviewed before production change, not after incidents.

---

## Production Readiness

Review:

- deployment plan
- rollback plan
- feature flags
- migrations
- compatibility
- monitoring
- alerts
- dashboards
- runbooks
- support staffing
- known issues
- business communication
- security approvals
- evidence

---

## Change Types

| Change Type | Operational Focus |
|---|---|
| Feature | User impact, support notes, rollout. |
| API contract | Consumer readiness, versioning, rollback. |
| Database migration | backup, lock risk, rollback/compensation. |
| Runtime change | deployment, probes, resource limits. |
| Security change | access, audit, emergency rollback. |
| Data pipeline | freshness, reconciliation, replay. |
| Report/template | golden output, archive, client communication. |

---

## Deployment Verification

Post-deploy checks:

- service health
- API smoke
- dashboard signals
- error rate
- latency
- batch/job status
- data freshness
- user workflow
- security/auth checks
- logs for critical errors

---

## Rollback

Rollback plan should include:

- trigger
- decision owner
- steps
- validation
- data implications
- communication
- fallback if rollback impossible

---

## Change Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Release without support awareness | Incident response weak. |
| No rollback plan | Long outage. |
| Dashboards updated after release | Blind deployment. |
| Migrations untested | Production risk. |
| Feature flag not documented | Operational confusion. |
| Known issues hidden | Stakeholder trust damage. |

---

## Review Checklist

- Is production-readiness review done?
- Is rollback defined?
- Are dashboards/alerts ready?
- Are runbooks updated?
- Are support teams informed?
- Are migrations tested?
- Are known issues documented?
- Are post-deploy checks defined?
- Is evidence retained?
- Is go/no-go decision clear?

---

## Summary

Release governance and production support are connected.

Every release should improve service, not surprise support.
