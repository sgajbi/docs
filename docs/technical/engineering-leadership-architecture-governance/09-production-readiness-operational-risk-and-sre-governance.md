# Production Readiness, Operational Risk, and SRE Governance

## Purpose

This file explains how engineering leaders should review production readiness and operational risk.

A system is not ready because the feature works locally. It is ready when it can run, fail, recover, and be supported safely.

---

## Production Readiness Areas

| Area | Review Focus |
|---|---|
| Runtime | Container, config, probes, resources, deployment. |
| Observability | Logs, metrics, traces, dashboards, alerts. |
| Supportability | Runbooks, degraded states, diagnostics. |
| Resilience | Timeouts, retries, back-pressure, fallback. |
| Security | Secrets, auth, permissions, sensitive data. |
| Data | Migrations, lineage, quality, retention. |
| Testing | Unit, contract, integration, E2E, certification. |
| Release | Rollout, rollback, change evidence. |
| Operations | On-call, escalation, incident response. |

---

## Production Readiness Review

A readiness review should ask:

- What is supported?
- What is not supported?
- What can fail?
- How will support know?
- What alert fires?
- Which runbook applies?
- How is it rolled back?
- What evidence exists?
- What risk remains?

---

## Operational Risk

Common operational risks:

- no owner
- no runbook
- no alert
- no readiness check
- missing rollback
- manual recovery only
- stale docs
- untested migration
- unsupported data state
- sensitive logs
- dependency with no fallback

---

## Go/No-Go Criteria

A go/no-go decision should consider:

- critical tests
- security findings
- migration readiness
- runtime readiness
- support readiness
- known defects
- rollback plan
- business timing
- incident risk
- stakeholder acceptance

---

## Readiness Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Production readiness checked after deployment | Risk discovered too late. |
| Runbook written but not usable | Incident delay. |
| Alerts missing | Failure invisible. |
| Rollback unclear | Long outage. |
| Known gaps not communicated | Stakeholder surprise. |
| Security finding deferred silently | Hidden risk. |

---

## Review Checklist

- Is runtime proof available?
- Are health/readiness meaningful?
- Are dashboards and alerts ready?
- Is runbook current?
- Is rollback plan defined?
- Are migrations tested?
- Are support owners clear?
- Are security findings resolved or accepted?
- Are known risks documented?
- Is go/no-go evidence-based?

---

## Summary

Production readiness is a leadership responsibility.

Leaders must ensure teams can support what they release.
