# Observability, Logging, Metrics, Tracing, and Platform Diagnostics

## Purpose

This file explains runtime observability for containerized and Kubernetes-style platforms.

Application observability and platform observability must work together.

---

## Observability Layers

| Layer | Signals |
|---|---|
| Application | logs, metrics, traces, supportability states |
| Container | restarts, CPU, memory, exit codes |
| Pod | readiness, liveness, scheduling, events |
| Node | capacity, pressure, failures |
| Cluster | deployment health, autoscaling, networking |
| Dependency | DB, cache, queue, stream, external API |
| Product flow | end-to-end user journey and business state |

---

## Logs

Runtime logs should be:

- structured
- JSON where platform expects it
- correlated
- source-safe
- searchable
- consistent across services

Avoid stdout noise, raw payloads, secrets, and dynamic sensitive fields.

---

## Metrics

Runtime metrics include:

- request rate
- error rate
- latency
- CPU/memory
- restarts
- readiness failures
- dependency failures
- queue depth
- worker lag
- supportability state
- saturation

---

## Traces

Traces should connect:

```text
ingress
  -> gateway
  -> service
  -> dependency
  -> worker/event where possible
```

Trace context should be propagated and safe.

---

## Platform Diagnostics

Useful diagnostics:

- pod status
- deployment rollout status
- recent events
- container logs
- readiness results
- restart count
- resource usage
- HPA status
- dependency connectivity
- config version
- image digest

---

## Observability Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| App logs unstructured text only | Hard to search. |
| Metrics labels use dynamic IDs | Cardinality explosion. |
| Traces stop at gateway | Cross-service debugging weak. |
| Dashboard has non-existing metrics | False confidence. |
| No restart alert | Crash loops missed. |
| No dependency metrics | Root cause unclear. |
| Diagnostics expose secrets | Security incident. |

---

## Review Checklist

- Are logs structured?
- Are metrics exposed?
- Are trace headers propagated?
- Are runtime metrics collected?
- Are pod restarts monitored?
- Is readiness failure visible?
- Are dashboards updated?
- Are alerts actionable?
- Are diagnostics safe?
- Are runbooks linked?

---

## Summary

Runtime observability lets teams understand how the service behaves in its real environment.

A service that cannot be diagnosed is not production-ready.
