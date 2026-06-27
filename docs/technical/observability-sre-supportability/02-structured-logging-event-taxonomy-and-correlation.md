# Structured Logging, Event Taxonomy, and Correlation

## Purpose

This file explains how to design logs that are useful, searchable, consistent, and safe.

Logs should help engineers and operators understand what happened without leaking sensitive data.

---

## Structured Logging

Structured logs use machine-readable fields.

Example:

```json
{
  "timestamp": "2026-06-27T10:15:00Z",
  "level": "INFO",
  "service": "portfolio-service",
  "event": "portfolio_summary_read_completed",
  "operation": "portfolio.summary.read",
  "status": "success",
  "reason": "SOURCE_READY",
  "correlation_id": "synthetic-correlation-id",
  "duration_ms": 42
}
```

---

## Log Event Taxonomy

Define a stable event vocabulary.

Example categories:

| Category | Example Event |
|---|---|
| Request | `api_request_started`, `api_request_completed` |
| Dependency | `dependency_call_failed`, `dependency_timeout` |
| Domain | `calculation_completed`, `report_job_created` |
| Worker | `work_item_claimed`, `work_item_failed_retryable` |
| Security | `access_denied`, `write_action_authorized` |
| Supportability | `supportability_state_emitted` |
| Recovery | `replay_requested`, `recovery_completed` |
| Data quality | `source_stale_detected`, `reconciliation_failed` |

---

## Correlation ID

A correlation ID connects logs for the same request or workflow.

Rules:

- accept incoming ID where trusted
- generate if missing
- propagate downstream
- include in logs
- include in safe error responses where appropriate
- do not use as metric label
- do not treat as authentication

---

## Trace ID Versus Correlation ID

| ID | Purpose |
|---|---|
| Correlation ID | Business/support request correlation. |
| Trace ID | Distributed tracing context. |
| Request ID | Single service request instance. |
| Execution ID | Async job/calculation/report identity. |

Do not confuse them.

---

## Reason Codes

Reason codes should be bounded.

Good:

```text
SOURCE_READY
DEPENDENCY_TIMEOUT
PERMISSION_BLOCKED
UNSUPPORTED_INPUT_MODE
IDEMPOTENCY_CONFLICT
```

Bad:

```text
failed because xyz raw error payload from downstream...
```

---

## Safe Log Fields

Good fields:

- service
- operation
- event
- status
- reason code
- dependency name
- duration bucket
- supportability state
- correlation id
- safe exception class

Avoid:

- client names
- account numbers
- portfolio ids unless explicitly approved
- raw request bodies
- raw response bodies
- tokens
- secrets
- prompts/model outputs
- raw entitlement errors
- SQL values
- internal source paths not approved for logs

---

## Logging Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Free-text logs only | Hard to search and dashboard. |
| Raw payload logs | Sensitive-data leakage. |
| Every team invents event names | Inconsistent operations. |
| Dynamic fields as event names | Poor aggregation. |
| Correlation ID missing | Cross-service diagnosis hard. |
| Log level misuse | Noise or hidden severity. |
| Logs only errors, no lifecycle | Cannot understand workflow progress. |

---

## Review Checklist

- Are logs structured?
- Is event vocabulary defined?
- Are reason codes bounded?
- Is correlation ID included?
- Are dependency failures logged safely?
- Are sensitive fields excluded?
- Are log levels meaningful?
- Are worker and async lifecycle events logged?
- Are logs useful for runbook steps?
- Are logging tests or guards present?

---

## Summary

Structured logs are operational evidence.

They should explain behavior consistently without becoming a sensitive-data leak.
