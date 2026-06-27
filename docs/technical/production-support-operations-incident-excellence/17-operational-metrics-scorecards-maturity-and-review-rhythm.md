# Operational Metrics, Scorecards, Maturity, and Review Rhythm

## Purpose

This file explains how to measure and improve production support maturity.

Operational excellence improves when teams review meaningful signals regularly.

---

## Operational Metrics

Useful metrics:

- incident count by severity
- mean time to detect
- mean time to acknowledge
- mean time to mitigate
- mean time to restore
- alert noise
- runbook coverage
- change failure rate
- rollback count
- data freshness breaches
- reconciliation breaks
- batch SLA breaches
- support ticket volume
- repeat incident count
- postmortem action closure

---

## Service Health Scorecard

Scorecard dimensions:

```text
ownership
SLO/SLA
monitoring
alert quality
runbooks
incident response
problem management
change readiness
data operations
security support
DR/backup
documentation
```

---

## Maturity Levels

| Level | Description |
|---|---|
| L0 | Reactive, undocumented support. |
| L1 | Basic owners and manual support. |
| L2 | Runbooks and dashboards available. |
| L3 | Alerts, SLOs, and incident process active. |
| L4 | Evidence-backed operations and continuous improvement. |
| L5 | Predictive, automated, resilient, low-toil support model. |

---

## Review Cadence

| Cadence | Activity |
|---|---|
| Daily during hypercare | Incidents, defects, data quality, user feedback. |
| Weekly | Alert noise, support tickets, open problems, runbook gaps. |
| Monthly | SLOs, scorecards, incident trends, postmortem actions. |
| Quarterly | DR drills, maturity roadmap, operating model, risk review. |
| After incidents | RCA, corrective actions, runbook/test/dashboard updates. |

---

## Metrics Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Metrics used for blame | Teams hide issues. |
| Only incident count measured | Leading risks missed. |
| No action from reviews | Review theatre. |
| Runbook coverage not measured | Support gaps persist. |
| Postmortem actions not tracked | Repeat incidents. |
| Alert noise ignored | On-call fatigue. |

---

## Review Checklist

- Are operational metrics defined?
- Are scorecards current?
- Are SLOs reviewed?
- Are alert-noise trends reviewed?
- Are repeat incidents identified?
- Are postmortem actions tracked?
- Are runbook gaps closed?
- Are maturity levels honest?
- Is improvement roadmap active?

---

## Summary

Operational excellence is a continuous improvement system.

Metrics should drive action, not create reporting theatre.
