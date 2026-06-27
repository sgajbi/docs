# Secure Observability, Logs, Metrics, Traces, Diagnostics, and Evidence

## Purpose

This file explains how to keep observability and evidence safe.

Observability is necessary for operations, but unsafe observability can leak sensitive data.

---

## Secure Logging

Logs should use:

- event name
- operation
- status
- reason code
- dependency name
- duration bucket
- correlation id
- safe exception class

Logs should avoid:

- request/response bodies
- client names
- account numbers
- portfolio identifiers unless approved
- secrets
- tokens
- raw entitlement errors
- SQL values
- prompts/model outputs
- document storage paths

---

## Secure Metrics

Metric labels must be bounded and safe.

Never use:

- client id
- account id
- portfolio id
- transaction id
- document id
- trace id
- correlation id
- raw error text
- prompt text

Use:

- operation
- status class
- state
- reason code
- dependency
- freshness bucket

---

## Secure Traces

Trace attributes should not include sensitive data.

Safe:

- route template
- operation
- status code
- dependency
- reason code

Unsafe:

- request body
- response body
- account id
- raw SQL values
- prompt/response text
- tokens

---

## Diagnostics

Diagnostics should be protected, authorized, and source-safe.

Show:

- status
- supportability state
- reason code
- dependency posture
- safe evidence ref
- lifecycle state
- retry/replay eligibility

Do not show:

- raw payloads
- secrets
- unrestricted identifiers
- storage paths
- raw stack traces
- prompts/model outputs

---

## Evidence Artifacts

Evidence artifacts should be filtered.

Examples:

- certification output
- dashboard export
- screenshot
- test evidence
- release evidence
- support bundle
- audit pack

They must not include sensitive or restricted content unless formally approved and protected.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Full request logging | Data leak. |
| Metrics with identifiers | Privacy and cardinality risk. |
| Diagnostics public | Critical exposure. |
| Evidence pack includes internal paths | Security leakage. |
| Screenshot uses real client data | Distribution risk. |
| Alert includes raw payload | Incident-tooling leak. |
| Trace includes prompt | AI data leak. |

---

## Review Checklist

- Are logs structured and safe?
- Are metric labels bounded?
- Are traces safe?
- Are diagnostics protected?
- Are screenshots synthetic?
- Are evidence artifacts filtered?
- Are alerts source-safe?
- Are no-sensitive-content gates present?
- Are tests present for forbidden fields?

---

## Summary

Observability should improve operations without weakening confidentiality.

Safe telemetry is a security control.
