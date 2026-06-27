# 06 - Observability, SRE And Operational Support

Operational supportability is an engineering feature. A service is not mature because it starts; it is mature when operators can understand, diagnose and recover supported behavior.

For deeper observability, SRE, runbook and operational supportability guidance, use [`observability-sre-supportability/`](observability-sre-supportability/README.md).

## Observability Pillars

| Pillar | Purpose |
|---|---|
| Logs | Explain important events and decisions. |
| Metrics | Quantify service health, latency, throughput and failure rate. |
| Traces | Follow a request across services. |
| Health checks | Tell orchestration and operators whether the service is alive and ready. |
| Runbooks | Explain how humans diagnose and respond. |

## Structured Logging

Application logs should be structured JSON with bounded fields.

Required fields:

1. timestamp,
2. level,
3. service,
4. environment,
5. correlation id or request id,
6. trace id when tracing is enabled,
7. event name,
8. message.

Good contextual fields:

1. endpoint,
2. HTTP method,
3. status class,
4. latency bucket,
5. operation,
6. dependency,
7. supportability state.

Do not log:

1. raw client identifiers,
2. account numbers,
3. holdings,
4. request or response bodies,
5. prompts or model outputs,
6. secrets,
7. trace ids as metric labels.

## Metrics

Minimum service metrics:

1. request count,
2. request latency,
3. error rate,
4. dependency latency,
5. dependency error count,
6. queue depth or consumer lag for async work,
7. background job success/failure counters.

Use stable labels:

```text
service
environment
endpoint_template
method
status_class
operation
dependency
```

Avoid high-cardinality labels such as portfolio id, account id, client name, trace id, document id or free-text error message.

## Tracing

Distributed tracing should propagate:

1. correlation id,
2. request id,
3. W3C trace context headers where supported.

Trace spans should capture service boundaries, dependency calls and material workflow steps. Do not put sensitive payload data in trace attributes.

## Health And Readiness

Use separate endpoints:

| Endpoint | Meaning |
|---|---|
| `/health/live` | Process is alive. Should be lightweight. |
| `/health/ready` | Service can handle supported traffic. Should check critical dependencies. |
| `/health` | Compatibility endpoint when needed. |

Readiness should fail or degrade when critical dependencies are unavailable, migrations are missing, or required configuration is invalid.

## SLO Thinking

Useful SLOs are tied to user or operator outcomes:

1. API availability,
2. p95 or p99 latency,
3. report generation completion,
4. ingestion freshness,
5. data-product trust posture,
6. job recovery time,
7. incident acknowledgement and resolution.

An SLO without measurement is an aspiration. A measured SLO without runbook ownership is still weak.

## Incident Workflow

Standard incident flow:

```text
detect -> triage -> contain -> diagnose -> mitigate -> recover -> verify -> document -> prevent recurrence
```

Evidence to preserve:

1. timeline,
2. impacted users or workflows,
3. error rates and latency,
4. logs and traces,
5. deployment version,
6. configuration changes,
7. mitigation actions,
8. follow-up tests or gates.

## Runbook Template

Every critical service should have runbook material covering:

1. service purpose,
2. dependency map,
3. startup command,
4. health and readiness checks,
5. key logs and metrics,
6. common failures,
7. diagnosis commands,
8. rollback or mitigation,
9. escalation owner,
10. evidence artifacts.

## Supportability States

Use explicit states instead of vague labels.

| State | Meaning |
|---|---|
| ready | Supported output is current and complete. |
| partial | Some source inputs or downstream fields are missing but output is usable with warnings. |
| degraded | Source or runtime issue affects confidence. |
| blocked | Output should not be used for the workflow. |
| unavailable | Required dependency or source is unavailable. |
| permission_blocked | Caller lacks access. |

## SRE Review Questions

1. Can an operator tell if the service is live and ready?
2. Can a failed request be traced across services?
3. Are metrics useful without exposing sensitive data?
4. Are degraded states visible to clients?
5. Is there a runbook for common failures?
6. Is recovery tested?
