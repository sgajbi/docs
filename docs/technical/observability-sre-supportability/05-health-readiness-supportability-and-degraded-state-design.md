# Health, Readiness, Supportability, and Degraded State Design

## Purpose

This file explains how services should communicate runtime and business supportability.

Health tells the platform whether a process is alive. Readiness tells whether it can serve traffic. Supportability tells consumers whether a response is usable.

---

## Health Endpoints

| Endpoint | Purpose |
|---|---|
| `/health` | Basic service status or metadata. |
| `/health/live` | Process is alive and should not be restarted. |
| `/health/ready` | Service can safely receive supported traffic. |
| `/metrics` | Runtime metrics. |

---

## Liveness

Liveness should answer:

```text
Should the platform restart this process?
```

Liveness should not fail just because a temporary dependency is unavailable, unless the process cannot recover.

---

## Readiness

Readiness should answer:

```text
Can this service handle supported requests right now?
```

Readiness may check:

- required database
- required queue/broker
- required configuration
- migration state
- critical upstream dependency
- required model/rules loaded
- worker availability where applicable

---

## Supportability

Supportability answers:

```text
Is this specific response or workflow usable?
```

States:

| State | Meaning |
|---|---|
| READY | Usable and supported. |
| PARTIAL | Some sections missing. |
| STALE | Data outside freshness window. |
| DEGRADED | Reduced quality. |
| UNAVAILABLE | Required dependency unavailable. |
| PERMISSION_BLOCKED | Caller not allowed. |
| UNSUPPORTED | Not supported for request. |
| UNKNOWN | Cannot prove state. |

---

## Degraded State Response

Example:

```json
{
  "supportability": {
    "state": "STALE",
    "reason": "SOURCE_STALE",
    "freshnessBucket": "STALE"
  },
  "warnings": [
    {
      "code": "SOURCE_STALE",
      "severity": "WARNING",
      "message": "Source data is outside the expected freshness window."
    }
  ]
}
```

---

## Health Versus Supportability

| Concept | Scope |
|---|---|
| Health | Process/service status. |
| Readiness | Traffic routing status. |
| Supportability | Business response usability. |
| Alert | Human action signal. |
| SLO | Expected service level. |

A service can be healthy but return a stale data response. Do not collapse all states into health.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Readiness always 200 | Broken service gets traffic. |
| Liveness checks optional dependency | Restart loops. |
| Supportability hidden in logs only | UI cannot show truthful state. |
| Stale data returned as ready | Incorrect user decisions. |
| Permission-blocked returned as empty data | User confusion and audit gap. |
| Unknown treated as success | False confidence. |

---

## Review Checklist

- Are health endpoints defined?
- Is liveness lightweight?
- Is readiness meaningful?
- Are required dependencies checked?
- Are supportability states defined?
- Are degraded states visible to consumers?
- Are reason codes bounded?
- Are states represented in metrics?
- Are states tested?
- Are runbooks aligned?

---

## Summary

Health is about the process. Supportability is about the business response.

A mature service models both.
