# Observability Testing, Certification, and CI Gates

## Purpose

This file explains how to test observability and prevent telemetry regressions.

Observability should be tested because logs, metrics, traces, dashboards, and supportability states are part of the service contract.

---

## What To Test

Test:

- metric names exist
- metric labels are bounded
- forbidden labels rejected
- structured log event vocabulary
- reason codes
- supportability states
- readiness behavior
- dependency failure behavior
- diagnostics safety
- dashboard metric references
- alert metric references
- no-sensitive-content rules

---

## Metric Tests

Metric tests can prove:

- expected metrics emitted
- labels match allowed list
- no dynamic labels
- no sensitive labels
- state/reason values bounded
- histograms used for latency

---

## Log Tests

Log tests can prove:

- structured fields exist
- event names are known
- reason codes are known
- raw payloads excluded
- exception handling safe
- correlation id present where needed

---

## Readiness Tests

Readiness tests should prove:

- healthy dependency -> ready
- required dependency down -> not ready
- optional dependency down -> ready but degraded where appropriate
- migration missing -> not ready
- config missing -> startup failure or not ready

---

## Dashboard and Alert Validation

Automated validation can check:

- referenced metrics exist
- labels exist
- alert expressions are syntactically valid
- runbook links exist
- dashboards do not reference planned metrics
- sensitive labels absent

---

## Certification Evidence

Observability certification may include:

- API health proof
- readiness proof
- metrics scrape proof
- dashboard validation
- alert validation
- runbook validation
- no-sensitive-content proof
- degraded-state proof
- trace propagation proof

---

## CI Gate Modes

| Mode | Use |
|---|---|
| Report-only | New observability check or baseline. |
| No-new-regression | Prevent worsening while cleanup continues. |
| Blocking | Mature, high-confidence check. |

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Observability only manually reviewed | Regression likely. |
| Dashboard validation absent | Broken dashboard discovered during incident. |
| No tests for forbidden labels | Sensitive leakage. |
| Readiness behavior untested | Bad traffic routing. |
| Alert expression unvalidated | Alert never fires. |
| Report-only forever | No maturity improvement. |

---

## Review Checklist

- Are observability tests present?
- Are sensitive labels blocked?
- Are metrics validated?
- Are logs validated?
- Is readiness tested?
- Are dashboards validated?
- Are alerts validated?
- Are runbooks linked?
- Are gates report-only or blocking?
- Is promotion plan defined?

---

## Summary

Observability is implementation, not decoration.

If telemetry matters during incidents, it deserves tests and gates.
