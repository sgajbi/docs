# Incident Management, Severity, Triage, and Command Center

## Purpose

This file explains incident management for enterprise production systems.

Incident response should be structured, role-based, evidence-driven, and communication-aware.

---

## Incident Lifecycle

```text
detect
  -> triage
  -> classify severity
  -> assign incident commander
  -> mitigate
  -> communicate
  -> recover
  -> validate
  -> close
  -> review
  -> improve
```

---

## Severity Model

| Severity | Typical Meaning |
|---|---|
| Sev 1 | Critical outage, major business impact, security/data risk, no workaround. |
| Sev 2 | Significant degradation, important workflow blocked, limited workaround. |
| Sev 3 | Localized issue, workaround available, moderate impact. |
| Sev 4 | Low impact issue, question, minor defect, planned support. |

Severity should consider business impact, user impact, data risk, regulatory risk, and time sensitivity.

---

## Incident Roles

| Role | Responsibility |
|---|---|
| Incident commander | Coordinates response and decisions. |
| Technical lead | Leads diagnosis and mitigation. |
| Communications lead | Sends updates to stakeholders. |
| Scribe | Records timeline, actions, decisions. |
| Business lead | Confirms business impact and acceptance. |
| Support lead | Manages user tickets and known issue flow. |
| Security lead | Engaged for security/privacy events. |

---

## Triage Questions

- What is impacted?
- Who is impacted?
- When did it start?
- Is it still happening?
- What changed recently?
- Which dashboards show impact?
- Is data integrity at risk?
- Is client confidentiality at risk?
- Is workaround available?
- What mitigation is safest?

---

## Incident Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| No incident commander | Confused response. |
| Everyone investigates separately | Duplicated effort. |
| No timeline | Poor RCA. |
| Severity unclear | Wrong urgency. |
| No business impact assessment | Poor communication. |
| Mitigation not recorded | Learning lost. |
| Security/privacy not escalated | Control breach. |

---

## Review Checklist

- Is severity model defined?
- Are incident roles defined?
- Is incident commander assigned?
- Is timeline recorded?
- Are business impacts assessed?
- Are dashboards used?
- Are mitigations safe?
- Are communications regular?
- Is recovery validated?
- Is post-incident review scheduled?

---

## Summary

Incident management reduces chaos.

Clear roles, severity, communication, and evidence make recovery faster and learning stronger.
