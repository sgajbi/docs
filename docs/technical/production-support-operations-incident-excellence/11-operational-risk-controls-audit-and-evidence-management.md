# Operational Risk, Controls, Audit, and Evidence Management

## Purpose

This file explains operational risk and evidence management for production systems.

Regulated enterprises need proof that operational controls exist, work, and are reviewed.

---

## Operational Risk Areas

| Area | Example Risk |
|---|---|
| Availability | Service unavailable during business hours. |
| Data quality | Stale or incorrect data used. |
| Access control | Unauthorized user accesses client data. |
| Change | Bad release causes outage. |
| Incident | Slow detection or response. |
| Resilience | Backup/DR does not work. |
| Support | No runbook or owner. |
| Security | Sensitive data exposed in logs. |
| Vendor/dependency | External service outage impacts users. |

---

## Control Evidence

Evidence may include:

- service ownership record
- runbook
- alert definition
- dashboard
- incident ticket
- postmortem
- release record
- access review
- backup/restore test
- DR test
- vulnerability scan
- change approval
- operational scorecard

---

## Control Matrix

```text
control:
risk:
implementation:
evidence:
owner:
frequency:
last reviewed:
exceptions:
```

---

## Exceptions

Exceptions should include:

- reason
- risk
- compensating control
- owner
- expiry
- approval
- review cadence

---

## Audit Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Evidence recreated manually after audit request | Inaccuracy. |
| Controls not tied to owner | No accountability. |
| Exceptions never expire | Permanent risk. |
| Runbook exists but unused | Weak control. |
| Incident actions not tracked | Repeat findings. |
| Sensitive evidence shared broadly | Confidentiality risk. |

---

## Review Checklist

- Are operational risks identified?
- Are controls mapped?
- Is evidence available?
- Are owners assigned?
- Are review cadences defined?
- Are exceptions documented?
- Are controls tested?
- Is evidence safe to share?
- Are audit gaps tracked?

---

## Summary

Operational controls are credible when normal engineering workflows generate evidence continuously.
