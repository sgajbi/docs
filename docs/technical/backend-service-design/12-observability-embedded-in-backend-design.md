# Observability Embedded in Backend Design

## Purpose

This file explains how observability should be built into backend service design.

Observability is not something added after code is complete. A service should be designed to explain what it is doing, why it failed, and whether its result is supportable.

## Observability Design Inputs

Before implementing a workflow, define:

- operation name
- success states
- failure states
- supportability states
- reason codes
- metrics
- log events
- trace propagation
- diagnostic endpoint needs
- runbook impact

## Operation Vocabulary

Every important backend operation should have a stable operation name.

Examples:

```text
portfolio_snapshot.read
performance_calculation.submit
report_job.replay
document_archive.download
source_ingestion.run_once
data_product.catalog.read
```

Stable names support logs, metrics, dashboards, alerts, and runbooks.

## Structured Events

A structured event should include:

- event name
- operation
- status
- reason code
- severity
- dependency if relevant
- duration bucket
- supportability state
- correlation id
- trace context where allowed

Avoid arbitrary log fields.

## Metrics Design

Design metrics around operations and states.

Examples:

```text
backend_requests_total{operation,status_class}
backend_dependency_calls_total{dependency,operation,status_class}
backend_supportability_total{operation,state,reason}
backend_worker_items_total{worker,state}
backend_replay_total{operation,result}
```

Metric labels must be bounded and safe.

## Trace Design

Trace design should answer:

- where did the request enter?
- which services were called?
- which dependency was slow?
- where did the error occur?
- what correlation id maps to logs?
- did the trace propagate across BFF/domain/shared service?

Do not put sensitive business data in trace attributes.

## Diagnostics APIs

Operator diagnostics APIs can help support.

They should show:

- status
- lifecycle state
- latest safe event
- supportability
- dependency posture
- replay eligibility
- safe lineage summary
- timestamps
- reason codes

They should not show:

- raw payloads
- secrets
- client names
- unrestricted identifiers
- internal storage paths
- prompts/responses unless approved

## Runbook Alignment

Every alert or diagnostic should map to a runbook.

Runbook should include:

- symptom
- impact
- dashboard
- first checks
- likely causes
- safe commands
- mitigation
- rollback/replay
- escalation
- data-safety notes

## Observability Tests

Observability can and should be tested.

Test:

- metric names exist
- labels are bounded
- forbidden labels rejected
- supportability state emitted
- log events use known vocabulary
- no sensitive fields in diagnostics
- readiness changes with dependency state
- alerts reference implemented metrics

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Add logs only after incidents | Poor first incident response. |
| Metrics invented in dashboard | Dashboard lies. |
| Dynamic labels | Cardinality and data leakage. |
| No operation vocabulary | Logs cannot be searched consistently. |
| Diagnostics expose raw data | Security risk. |
| No tests for observability | Regression unnoticed. |
| Readiness not tied to dependency | Traffic goes to broken service. |

## Review Checklist

- Is operation vocabulary defined?
- Are log events structured?
- Are reason codes bounded?
- Are metrics safe and useful?
- Is trace propagation handled?
- Are diagnostics safe?
- Are dashboards tied to real metrics?
- Are alerts actionable?
- Are observability tests present?
- Does the runbook match runtime behavior?

## Summary

Observable backend services are easier to support and safer to operate.

If a workflow is important enough to build, it is important enough to explain through logs, metrics, traces, diagnostics, and runbooks.
