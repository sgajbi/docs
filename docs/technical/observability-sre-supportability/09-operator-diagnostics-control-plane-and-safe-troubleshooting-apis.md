# Operator Diagnostics, Control Plane, and Safe Troubleshooting APIs

## Purpose

This file explains how to design safe diagnostic and control-plane APIs for operators.

Operator APIs help support teams inspect runtime state, troubleshoot failures, replay work, and recover safely.

---

## Diagnostic API Purpose

Diagnostics should answer:

- what is the current state?
- what happened recently?
- what dependency failed?
- is replay possible?
- what evidence exists?
- what is the safe next action?

---

## Useful Diagnostic Fields

Safe diagnostic fields may include:

- operation
- lifecycle state
- supportability state
- reason code
- latest safe event
- dependency state
- retry count
- last attempted timestamp
- next retry timestamp
- replay eligibility
- evidence reference
- runbook link

---

## Fields To Avoid

Diagnostics should not expose:

- raw request body
- raw response body
- secrets
- client names
- account numbers
- unrestricted portfolio identifiers
- raw entitlement payloads
- internal storage paths
- prompts/model outputs
- stack traces unless restricted and approved

---

## Control-Plane Actions

Examples:

- run readiness check
- run ingestion once
- replay failed work item
- retry report job
- rerender document
- regenerate from source
- run retention cleanup
- inspect runtime status

Control-plane actions must be:

- authorized
- audited
- idempotent where possible
- bounded
- observable
- documented

---

## Diagnostic API Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Diagnostic route public | Security incident. |
| Raw payload in diagnostics | Data leakage. |
| Replay action unaudited | Unsafe operations. |
| Diagnostic state differs from actual runtime | Support confusion. |
| No runbook link | Operator still stuck. |
| Control action not idempotent | Duplicate side effects. |
| Diagnostics expose storage paths | Internal information leak. |

---

## Review Checklist

- Is diagnostic API needed?
- Is access restricted?
- Are fields safe?
- Are control actions audited?
- Are replay actions idempotent?
- Are reason codes bounded?
- Are diagnostics linked to runbooks?
- Are metrics/logs emitted?
- Are tests present for sensitive-data rejection?
- Are diagnostics documented?

---

## Summary

Operator APIs are powerful production tools.

Design them like privileged product features: safe, authorized, audited, and useful.
