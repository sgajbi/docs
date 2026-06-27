# Production Support Checklists and KT Playbook

## Purpose

This file provides practical checklists for production support, incident response, operations readiness, runbooks, DR, data operations, and support KT.

Use it for service onboarding, release readiness, incident response, support handoff, operational reviews, and leadership assessments.

---

## Service Onboarding Checklist

- [ ] Service owner defined.
- [ ] Technical owner defined.
- [ ] Support owner defined.
- [ ] Business criticality defined.
- [ ] Support hours defined.
- [ ] Severity model mapped.
- [ ] Dependencies listed.
- [ ] Dashboards available.
- [ ] Runbooks available.
- [ ] Escalation path defined.

---

## Monitoring Checklist

- [ ] Availability monitored.
- [ ] Latency monitored.
- [ ] Error rate monitored.
- [ ] Dependency health monitored.
- [ ] Runtime resources monitored.
- [ ] Queue/job health monitored.
- [ ] Data freshness monitored.
- [ ] Reconciliation monitored.
- [ ] Security events monitored.
- [ ] Dashboards current.

---

## Alert Checklist

- [ ] Critical alerts actionable.
- [ ] Alert owner assigned.
- [ ] Runbook linked.
- [ ] Severity mapped.
- [ ] Noise reviewed.
- [ ] Sensitive labels avoided.
- [ ] Business impact considered.
- [ ] Escalation route defined.
- [ ] Alert tested.

---

## Incident Checklist

- [ ] Severity classified.
- [ ] Incident commander assigned.
- [ ] Technical lead assigned.
- [ ] Communications lead assigned.
- [ ] Timeline started.
- [ ] Business impact assessed.
- [ ] Mitigation identified.
- [ ] Stakeholders updated.
- [ ] Recovery validated.
- [ ] Postmortem scheduled.

---

## Runbook Checklist

- [ ] Purpose clear.
- [ ] Symptoms listed.
- [ ] Dashboards linked.
- [ ] First checks safe.
- [ ] Diagnosis steps clear.
- [ ] Mitigations safe.
- [ ] Recovery/replay documented.
- [ ] Escalation clear.
- [ ] Data-safety notes included.
- [ ] Last reviewed date present.

---

## Change Readiness Checklist

- [ ] Release notes ready.
- [ ] Deployment plan ready.
- [ ] Rollback plan ready.
- [ ] Migrations tested.
- [ ] Dashboards updated.
- [ ] Alerts updated.
- [ ] Runbooks updated.
- [ ] Support aware.
- [ ] Known issues documented.
- [ ] Post-deploy checks defined.

---

## Data Operations Checklist

- [ ] Source freshness tracked.
- [ ] Ingestion monitored.
- [ ] Data-quality rules monitored.
- [ ] Reconciliation defined.
- [ ] Break owners assigned.
- [ ] Replay/rerun safe.
- [ ] Lineage captured.
- [ ] Downstream impact known.
- [ ] Data runbooks available.

---

## Batch/Worker Checklist

- [ ] Job states durable.
- [ ] Queue depth monitored.
- [ ] Worker lag monitored.
- [ ] Retries bounded.
- [ ] Dead letters visible.
- [ ] Replay idempotent.
- [ ] Operator actions audited.
- [ ] Batch SLA monitored.
- [ ] Recovery runbooks available.

---

## Security Support Checklist

- [ ] Support access least privilege.
- [ ] Privileged access audited.
- [ ] Sensitive logs prohibited.
- [ ] Support bundles filtered.
- [ ] Security incident path defined.
- [ ] Privacy incident path defined.
- [ ] Secrets absent from runbooks.
- [ ] Access reviews scheduled.
- [ ] Escalation criteria clear.

---

## DR/BCP Checklist

- [ ] RTO/RPO defined.
- [ ] Backup configured.
- [ ] Restore tested.
- [ ] DR runbook available.
- [ ] Secrets/config included.
- [ ] Archive/document recovery included.
- [ ] DR drill scheduled.
- [ ] BCP process agreed.
- [ ] Evidence retained.

---

## Support KT Sequence

A strong support KT sequence:

1. service purpose and business criticality
2. ownership and RACI
3. architecture and dependencies
4. normal service health
5. dashboards and alerts
6. runbooks and operator tools
7. incident process
8. data operations
9. batch/job recovery
10. security/privacy support rules
11. release/rollback process
12. DR/BCP process
13. known issues and limitations
14. post-incident improvement loop

---

## Engineering Lead Questions

- What happens when the service fails?
- Who is paged?
- What dashboard shows impact?
- What runbook applies?
- What is safe for support to do?
- What requires engineering?
- What requires business decision?
- What data-quality state can users see?
- How do we recover?
- How do we prevent recurrence?

---

## Summary

Production support excellence is built through ownership, observability, runbooks, safe tooling, communication, and continuous learning.

Use these checklists to make support readiness visible and repeatable.
