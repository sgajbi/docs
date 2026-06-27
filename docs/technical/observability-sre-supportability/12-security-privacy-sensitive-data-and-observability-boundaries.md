# Security, Privacy, Sensitive Data, and Observability Boundaries

## Purpose

This file explains how to keep observability safe.

Observability can become a data leak if logs, metrics, traces, dashboards, diagnostics, screenshots, or evidence expose sensitive information.

---

## Sensitive Observability Surfaces

Sensitive data may leak through:

- logs
- metric labels
- trace attributes
- dashboards
- alerts
- diagnostic APIs
- screenshots
- CI artifacts
- generated reports
- runbook examples
- incident tickets
- AI prompts and outputs

---

## Data To Avoid

Avoid exposing:

- client names
- account numbers
- raw portfolio identifiers unless approved
- transaction details
- holdings
- request/response bodies
- secrets
- tokens
- internal source paths
- raw entitlement failures
- prompts/model responses
- document storage paths
- trace IDs as metric labels

---

## Safe Alternatives

Use:

- bounded reason codes
- status classes
- freshness buckets
- supportability states
- safe operation names
- dependency names
- safe synthetic identifiers
- correlation id where policy permits
- redacted config state

---

## Metric Label Safety

Never use high-cardinality sensitive labels.

Unsafe:

```text
portfolio_id
client_id
account_id
transaction_id
trace_id
correlation_id
error_message
```

Safe:

```text
operation
status_class
state
reason
dependency
freshness_bucket
```

---

## Diagnostic Security

Diagnostics should be:

- authenticated
- authorized
- audited
- source-safe
- minimal
- tied to runbooks
- tested for sensitive-content rejection

---

## Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Request body logging | Sensitive-data leak. |
| Metrics with account ids | Privacy and cardinality issue. |
| Trace attributes contain prompt text | AI data leak. |
| Dashboard shared with restricted identifiers | Unauthorized disclosure. |
| Alert message includes raw error payload | Incident tooling leak. |
| Runbook examples use real data | Compliance risk. |
| Diagnostic endpoint public | Critical exposure. |

---

## Review Checklist

- Are logs source-safe?
- Are metrics label-safe?
- Are traces safe?
- Are dashboards filtered?
- Are alerts safe?
- Are diagnostics protected?
- Are screenshots sanitized?
- Are CI artifacts checked?
- Are runbook examples synthetic?
- Are sensitive-content gates present?

---

## Summary

Unsafe observability is a security defect.

Telemetry should help operators without exposing protected data.
