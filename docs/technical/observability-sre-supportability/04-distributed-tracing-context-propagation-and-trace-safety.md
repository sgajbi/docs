# Distributed Tracing, Context Propagation, and Trace Safety

## Purpose

This file explains how distributed tracing helps diagnose cross-service behavior.

Tracing connects work across UI, gateway, domain services, shared capability services, workers, databases, queues, and external dependencies.

---

## Trace Mental Model

A trace is a tree of spans.

```text
user request
  -> gateway span
      -> portfolio service span
      -> performance service span
          -> database span
      -> report service span
```

A trace helps answer:

- where did time go?
- which dependency failed?
- where did the request stop?
- did context propagate?
- which operation caused downstream work?

---

## Trace Context

W3C trace context commonly uses:

```text
traceparent
tracestate
```

Services should:

- accept valid trace context
- generate trace context if missing
- replace malformed trace headers safely
- propagate context downstream
- avoid exposing trace IDs as metric labels

---

## Span Naming

Good span names:

```text
GET /api/v1/portfolio/{portfolioId}/summary
portfolio.summary.read
dependency.performance.get_returns
worker.report_job.execute
```

Avoid dynamic sensitive span names.

---

## Trace Attributes

Safe attributes:

- operation
- service name
- route template
- status code
- dependency name
- result state
- reason code

Avoid:

- client name
- account id
- portfolio id unless explicitly allowed
- request/response body
- secrets
- prompt text
- generated AI response
- raw SQL values

---

## Cross-Service Propagation

Trace propagation should work across:

- UI/BFF
- BFF/domain services
- domain/shared capability services
- workers
- queues/events where supported
- external API calls

For async workflows, record causation/correlation where full trace continuation is not possible.

---

## Trace Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Trace stops at gateway | Backend diagnosis weak. |
| Dynamic identifiers in span names | Cardinality and privacy risk. |
| Request body in trace attributes | Sensitive-data leak. |
| Malformed trace header blindly forwarded | Tooling issues. |
| Trace ID used as metric label | Cardinality explosion. |
| Async work loses correlation | Worker failures hard to connect. |

---

## Review Checklist

- Is trace context accepted?
- Is trace context generated when missing?
- Is context propagated downstream?
- Are malformed headers handled?
- Are span names stable?
- Are attributes safe?
- Are async jobs correlated?
- Are traces linked to logs?
- Are trace IDs excluded from metrics labels?
- Are traces useful for runbook diagnosis?

---

## Summary

Tracing provides the map of distributed work.

Use it to connect services, but keep trace metadata safe and bounded.
