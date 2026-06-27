# Runbooks, Incident Response, and Problem Management

## Purpose

This file explains how to write runbooks and manage incidents and problems.

Runbooks turn operational knowledge into repeatable response. Incidents turn failures into learning. Problem management turns repeated issues into permanent fixes.

---

## Runbook Structure

A good runbook includes:

```text
Purpose:
Scope:
Symptoms:
Impact:
Dashboards:
Alerts:
First checks:
Diagnosis steps:
Common causes:
Mitigation:
Recovery/replay:
Rollback:
Escalation:
Data-safety notes:
Post-incident actions:
```

---

## First Checks

Runbook first checks should be safe and quick:

- check service health
- check readiness
- check recent deploy
- check dependency dashboard
- check error rate
- check queue depth
- check logs by correlation id
- check supportability state
- check known incidents

---

## Incident Flow

```text
detect
  -> acknowledge
  -> triage
  -> assess impact
  -> mitigate
  -> communicate
  -> recover
  -> verify
  -> document
  -> problem review
  -> improve controls
```

---

## Incident Communication

Communicate:

- what is impacted
- who is impacted
- current severity
- mitigation status
- next update time
- known workaround
- recovery confirmation

Avoid speculative root cause until confirmed.

---

## Problem Management

Problem management asks:

- why did it happen?
- why was it not prevented?
- why was it not detected earlier?
- did runbook work?
- what evidence was missing?
- what permanent fix is needed?
- what gate/test/alert should be added?
- what documentation should change?

---

## Post-Incident Actions

Possible actions:

- add test
- add alert
- tune dashboard
- improve runbook
- add supportability state
- improve retry/backoff
- add idempotency
- fix dependency timeout
- add migration check
- improve release evidence
- update architecture decision

---

## Runbook Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Runbook says "contact developer" only | No self-service support. |
| Commands are unsafe or outdated | Incident worsens. |
| No dashboard link | Slow diagnosis. |
| No data-safety notes | Sensitive-data exposure. |
| No rollback guidance | Longer outage. |
| Incident review blames person only | System does not improve. |
| Action items not tracked | Problems recur. |

---

## Review Checklist

- Does runbook match current runtime?
- Are symptoms clear?
- Are dashboards linked?
- Are alerts linked?
- Are first checks safe?
- Are mitigation steps clear?
- Is rollback included?
- Is escalation path clear?
- Are data-safety notes present?
- Are post-incident improvements tracked?

---

## Summary

Runbooks and incidents are part of engineering quality.

A good production system learns from failure and improves its controls.
