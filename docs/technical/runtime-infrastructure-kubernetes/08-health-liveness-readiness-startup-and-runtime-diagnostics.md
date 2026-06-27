# Health, Liveness, Readiness, Startup, and Runtime Diagnostics

## Purpose

This file explains how to design health checks, probes, and diagnostics for enterprise services.

Health endpoints should help the platform route traffic safely and help operators understand runtime posture.

---

## Health Endpoint Types

| Endpoint | Purpose |
|---|---|
| `/health` | Basic service health or metadata. |
| `/health/live` | Process is alive and should not be restarted. |
| `/health/ready` | Service can receive supported traffic. |
| `/metrics` | Metrics for monitoring. |
| `/diagnostics` | Protected operational diagnostics where applicable. |

---

## Liveness

Liveness should answer:

```text
Is the process alive, not deadlocked, and safe to keep running?
```

Liveness should not fail just because a dependency is temporarily down unless the process itself is unrecoverable.

Aggressive liveness probes can create restart loops.

---

## Readiness

Readiness should answer:

```text
Can this service safely serve its supported traffic right now?
```

Readiness may check:

- database connectivity
- required upstreams
- migration state
- cache availability if required
- queue connection if required
- configuration completeness
- feature dependencies
- model/rule load state
- internal worker readiness

---

## Startup Probe

Startup probe protects slow-starting applications from premature liveness failure.

Use when:

- service loads large model/config
- migrations or warmup are slow
- app startup can exceed liveness threshold
- cold start is variable

---

## Diagnostics

Diagnostics should be protected and safe.

Useful diagnostic fields:

- service version
- active profile
- dependency posture
- migration version
- queue state
- worker state
- last successful source refresh
- supportability state
- safe reason codes

Avoid:

- secrets
- raw payloads
- client data
- account identifiers
- internal storage paths
- unrestricted stack traces

---

## Probe Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Readiness always returns 200 | Broken service receives traffic. |
| Liveness checks database | Temporary DB issue restarts healthy app. |
| Startup too slow without startup probe | Crash loop. |
| Diagnostics expose secrets | Security incident. |
| Health endpoint performs heavy work | Probe overload. |
| Readiness ignores migrations | Traffic hits incompatible schema. |
| No distinction between live and ready | Platform cannot make good routing decisions. |

---

## Review Checklist

- Is `/health` defined?
- Is `/health/live` meaningful?
- Is `/health/ready` meaningful?
- Are required dependencies checked in readiness?
- Are transient dependency failures handled appropriately?
- Is startup probe needed?
- Are diagnostics protected?
- Are diagnostics source-safe?
- Are probe timeouts and thresholds reasonable?
- Are readiness failures observable?

---

## Summary

Health and readiness are runtime contracts.

They tell the platform when to restart a process and when to send traffic. Design them carefully.
