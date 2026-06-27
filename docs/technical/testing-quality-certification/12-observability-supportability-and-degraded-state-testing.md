# Observability, Supportability, and Degraded-State Testing

## Purpose

This file explains how to test observability, readiness, supportability, and degraded behavior.

Operational behavior is part of the product and should be tested.

---

## What To Test

Test:

- structured log fields
- metric names
- metric labels
- trace propagation
- health endpoint
- readiness endpoint
- dependency failure readiness
- supportability states
- partial response
- stale response
- degraded response
- unavailable dependency
- permission-blocked response
- diagnostics safety

---

## Readiness Tests

Test cases:

- all required dependencies available -> ready
- required database unavailable -> not ready
- required migration missing -> not ready
- optional dependency unavailable -> ready but degraded for affected route
- config missing -> startup failure or not ready

---

## Metrics Tests

Assert:

- metric emitted
- labels are bounded
- no forbidden labels
- supportability state counted
- dependency failure counted
- latency histogram exists where required

---

## Log Tests

Assert:

- log event name known
- operation present
- reason code present
- correlation id present where needed
- raw payload absent
- sensitive fields absent

---

## Degraded State Tests

Test:

- partial upstream failure
- stale source data
- missing optional section
- dependency timeout
- permission-blocked section
- unsupported input

Assert user/API sees truthful state.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Observability not tested | Telemetry regresses silently. |
| Dashboard references untested metrics | Incident failure. |
| Readiness always mocked green | Traffic routing risk. |
| Degraded states manually checked only | Real-world failures unproved. |
| Sensitive labels not tested | Metrics leak data. |

---

## Review Checklist

- Are health/readiness tested?
- Are dependency failure states tested?
- Are metrics validated?
- Are logs validated?
- Are trace headers tested?
- Are degraded states tested?
- Are permission-blocked states tested?
- Are diagnostics source-safe?
- Are dashboard/alert references validated?

---

## Summary

If operators depend on telemetry, telemetry must be tested.

Operational confidence is built through automated supportability evidence.
