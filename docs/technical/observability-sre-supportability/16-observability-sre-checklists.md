# Observability and SRE Checklists

## Purpose

This file provides practical observability and SRE checklists for design reviews, PR reviews, production-readiness reviews, and operational maturity assessments.

---

## Logging Checklist

- [ ] Logs are structured.
- [ ] Event names are stable.
- [ ] Operation name is included.
- [ ] Status is included.
- [ ] Reason code is bounded.
- [ ] Correlation ID is included where needed.
- [ ] Dependency name is included where relevant.
- [ ] Duration is captured.
- [ ] Sensitive data is excluded.
- [ ] Raw request/response bodies are excluded.

---

## Metrics Checklist

- [ ] Request metrics exist.
- [ ] Latency metrics exist.
- [ ] Dependency metrics exist.
- [ ] Error metrics exist.
- [ ] Supportability metrics exist.
- [ ] Worker metrics exist where applicable.
- [ ] Labels are bounded.
- [ ] Sensitive labels are blocked.
- [ ] Metric names are stable.
- [ ] Dashboards use implemented metrics.

---

## Tracing Checklist

- [ ] Trace context accepted.
- [ ] Trace context generated when missing.
- [ ] Trace context propagated downstream.
- [ ] Malformed trace headers handled safely.
- [ ] Span names stable.
- [ ] Attributes are safe.
- [ ] Async work correlated.
- [ ] Trace IDs not used as metric labels.

---

## Health and Readiness Checklist

- [ ] `/health` exists.
- [ ] `/health/live` exists where applicable.
- [ ] `/health/ready` exists.
- [ ] Liveness is lightweight.
- [ ] Readiness checks required dependencies.
- [ ] Optional dependency failure maps to degraded state if appropriate.
- [ ] Migration/config readiness considered.
- [ ] Readiness tests exist.

---

## Supportability Checklist

- [ ] States are defined.
- [ ] Reason codes are bounded.
- [ ] API responses expose supportability where useful.
- [ ] UI states align with backend supportability.
- [ ] Metrics count supportability states.
- [ ] Logs include reason codes.
- [ ] Degraded states are tested.
- [ ] Unknown is not treated as success.

---

## Dashboard Checklist

- [ ] Audience is defined.
- [ ] Service health visible.
- [ ] Request rate visible.
- [ ] Error rate visible.
- [ ] Latency visible.
- [ ] Dependencies visible.
- [ ] Supportability visible.
- [ ] Version/deploy info visible.
- [ ] Alerts linked.
- [ ] Runbooks linked.

---

## Alert Checklist

- [ ] Alert is actionable.
- [ ] Severity defined.
- [ ] Owner defined.
- [ ] Threshold justified.
- [ ] Duration set.
- [ ] Dashboard linked.
- [ ] Runbook linked.
- [ ] Routing correct.
- [ ] Noise reviewed.
- [ ] Alert message source-safe.

---

## Runbook Checklist

- [ ] Purpose clear.
- [ ] Scope clear.
- [ ] Symptoms listed.
- [ ] Impact described.
- [ ] Dashboard links included.
- [ ] Alert links included.
- [ ] First checks included.
- [ ] Diagnosis steps safe.
- [ ] Mitigation steps clear.
- [ ] Rollback/replay included where relevant.
- [ ] Escalation path defined.
- [ ] Data-safety notes included.

---

## Diagnostic API Checklist

- [ ] Access restricted.
- [ ] Audit emitted.
- [ ] Safe fields only.
- [ ] Reason codes bounded.
- [ ] Replay eligibility visible.
- [ ] Raw payloads excluded.
- [ ] Internal paths excluded.
- [ ] Runbook linked.
- [ ] Tests block sensitive leakage.

---

## Async Worker Checklist

- [ ] Work states explicit.
- [ ] Queue depth measured.
- [ ] Worker lag measured.
- [ ] Retry count measured.
- [ ] Dead-letter state monitored.
- [ ] Replay audited.
- [ ] Retention measured.
- [ ] Worker shutdown safe.
- [ ] Runbook exists.

---

## Data Product Observability Checklist

- [ ] Freshness measured.
- [ ] Quality state measured.
- [ ] Certification state measured.
- [ ] Telemetry recency measured.
- [ ] Consumer errors measured.
- [ ] Access denials measured.
- [ ] Evidence references safe.
- [ ] Alerts exist for stale/certification failure.
- [ ] Operating report exists where useful.

---

## Security Checklist

- [ ] Logs exclude sensitive data.
- [ ] Metrics labels exclude sensitive IDs.
- [ ] Traces exclude payloads.
- [ ] Dashboards filtered.
- [ ] Alerts safe.
- [ ] Diagnostics protected.
- [ ] Screenshots sanitized.
- [ ] CI artifacts filtered.
- [ ] No-sensitive-content gate present.

---

## Production Readiness Checklist

- [ ] Observability implemented before release.
- [ ] Dashboards reviewed.
- [ ] Alerts tested or validated.
- [ ] Runbooks reviewed.
- [ ] On-call owner identified.
- [ ] Escalation path documented.
- [ ] SLOs defined where needed.
- [ ] Known gaps documented.
- [ ] Support team briefed.
- [ ] Incident feedback loop defined.

---

## Summary

Checklists make observability review repeatable.

Use them to ensure every service can be diagnosed, supported, and improved safely.
