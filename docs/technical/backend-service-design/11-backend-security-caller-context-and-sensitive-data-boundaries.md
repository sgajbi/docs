# Backend Security, Caller Context, and Sensitive-Data Boundaries

## Purpose

This file explains security concerns inside backend service architecture.

Security must be designed into service boundaries, request handling, caller context, authorization, logging, metrics, persistence, events, and generated evidence.

## Security Responsibilities

Backend services should define:

- authentication assumptions
- caller context model
- authorization policy
- entitlement scope
- write-action controls
- sensitive-data handling
- audit fields
- secure error handling
- secure logging and metrics
- dependency and configuration safety

## Caller Context Model

A typed caller context may include:

```text
actor_id
caller_application
tenant_id
region
booking_center
role
capabilities
portfolio_scope
client_scope
correlation_id
trace_context
```

Do not pass raw headers across the application. Convert them into a typed object.

## Authorization

Authorization should be explicit.

Examples:

- can read portfolio
- can submit proposal
- can request report
- can replay failed job
- can download archived document
- can run operator diagnostic
- can view AI explanation
- can access data product

Authorization decisions should be testable and auditable.

## Write-Action Security

Write actions need stronger control.

For write-capable routes:

- require caller context
- require idempotency key where appropriate
- validate entitlement
- validate lifecycle state
- audit action
- protect against duplicate submission
- return safe errors
- avoid hidden side effects

## Sensitive Data Boundaries

Sensitive data can appear in:

- request payloads
- responses
- logs
- metrics
- traces
- events
- database rows
- generated reports
- screenshots
- CI artifacts
- test fixtures
- AI prompts and responses
- exception messages

Each boundary needs rules.

## Safe Observability

Logs and metrics should not expose sensitive data.

Unsafe:

```text
metric label: portfolio_id=PB123
log field: client_name=John Smith
error detail: raw entitlement failure payload
```

Safer:

```text
metric label: state=permission_blocked
log field: reason=PERMISSION_BLOCKED
support field: correlationId=synthetic-safe-id
```

## Configuration Security

Configuration should:

- keep secrets out of source
- use typed settings
- validate required values
- redact secret values
- avoid insecure defaults
- document local and runtime profiles
- be testable

## Dependency Security

Backend services should use:

- dependency audit
- lockfiles
- approved package policy
- container scan
- base image hygiene
- patch cadence
- no unapproved runtime downloads

## AI and Sensitive Data

If backend service integrates with AI:

- do not send sensitive data unless approved and necessary
- ground prompts in approved evidence
- avoid storing raw prompts/responses unless policy allows
- capture model-risk evidence safely
- separate AI suggestions from authoritative decisions
- require human review for regulated actions
- test prompt/response boundaries
- reject unsafe diagnostic fields

## Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Raw caller headers used everywhere | Inconsistent authorization. |
| UI-only authorization | Backend can be bypassed. |
| Secrets in examples | Credential leakage. |
| Raw payload logging | Sensitive-data incident. |
| Metrics with user/account identifiers | Leakage and cardinality explosion. |
| Operator diagnostics expose storage paths | Internal information leakage. |
| AI output treated as official decision | Governance and suitability risk. |
| Workflow replay without authorization | Operational abuse risk. |

## Review Checklist

- Is caller context typed?
- Are auth assumptions documented?
- Are read and write permissions enforced?
- Are idempotency and audit present for writes?
- Are errors product-safe?
- Are logs sensitive-data safe?
- Are metrics label-safe?
- Are traces policy-compliant?
- Are events safe?
- Are generated artifacts filtered?
- Are secrets excluded?
- Are dependencies audited?
- Are AI boundaries governed?

## Summary

Backend security is not a middleware checkbox.

It is a full-service design concern spanning context, authorization, data, observability, events, persistence, CI, and operations.
