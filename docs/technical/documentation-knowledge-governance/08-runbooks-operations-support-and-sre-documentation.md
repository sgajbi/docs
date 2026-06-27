# Runbooks, Operations Support, and SRE Documentation

## Purpose

This file explains how to write operational documentation that helps support teams diagnose, mitigate, recover, and escalate issues.

Runbooks are production tools.

---

## Runbook Structure

A good runbook includes:

```text
Purpose
Scope
Symptoms
Impact
Dashboards
Alerts
First checks
Diagnosis steps
Mitigation
Recovery/replay
Rollback
Escalation
Data-safety notes
Post-incident actions
```

---

## Operational Docs

Operational docs may include:

- service runbook
- deployment guide
- rollback guide
- migration runbook
- incident playbook
- dashboard guide
- alert catalogue
- support FAQ
- dependency map
- DR/backup guide
- operator command guide

---

## First Checks

First checks should be safe and practical:

- health/readiness
- recent deploy
- error rate
- latency
- dependency status
- queue depth
- worker lag
- logs by correlation id
- supportability states
- known incidents

---

## Runbook Quality

A runbook should be:

- current
- actionable
- safe
- concise
- linked to dashboards/alerts
- aligned with runtime
- tested during drills or incidents
- reviewed after changes

---

## Runbook Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| “Contact developer” only | No support self-service. |
| Commands outdated | Incident delay. |
| No dashboard link | Slow diagnosis. |
| No data-safety notes | Sensitive-data risk. |
| No escalation path | Ownership confusion. |
| No rollback/replay guidance | Recovery slow. |
| Runbook not updated after incident | Learning lost. |

---

## Review Checklist

- Does runbook match runtime?
- Are symptoms clear?
- Are dashboards linked?
- Are alerts linked?
- Are first checks safe?
- Are mitigation steps clear?
- Is rollback included?
- Is escalation defined?
- Are data-safety notes present?
- Is review cadence defined?

---

## Summary

A runbook is successful when someone can use it during pressure without tribal knowledge.
