# Caller Context, Authorization, and Entitlement-Aware APIs

## Purpose

This file explains how APIs should handle caller context, authorization, entitlements, permission-blocked states, and audit posture.

In banking and wealth platforms, not every authenticated user can see or do everything. APIs must enforce and communicate that safely.

## Caller Context

Caller context describes who is calling and under what scope.

Common fields:

- actor id
- caller application
- tenant id
- region
- booking center
- role
- capabilities
- portfolio scope
- client scope
- correlation id
- trace context

## Header Examples

```text
X-Actor-Id: synthetic-actor
X-Tenant-Id: tenant-sg
X-Region: APAC
X-Caller-Application: advisor-portal
X-Role: advisor
X-Correlation-Id: synthetic-correlation-id
traceparent: 00-...
```

Header names should be standardized across services.

## Authentication Versus Authorization

Authentication:

```text
Who are you?
```

Authorization:

```text
What are you allowed to do?
```

Entitlement:

```text
Which clients, accounts, portfolios, regions, products, actions, or documents are in your permitted scope?
```

## Permission-Blocked API Behavior

When caller lacks permission:

- use 403 if the entire operation is blocked
- use section-level `PERMISSION_BLOCKED` if partial product response is appropriate
- avoid raw entitlement failure details
- avoid revealing hidden resource existence where policy requires
- log safe audit event
- emit bounded metrics

## Caller Context Propagation

In multi-service flows:

```text
UI -> BFF -> domain service -> shared capability
```

The BFF should forward approved caller context. Domain services should enforce their own rules rather than trusting UI-only checks.

## Audit Events

Write or sensitive read actions may require audit events.

Audit event may include:

- operation
- actor
- caller application
- tenant/region
- resource type
- action
- result
- reason code
- timestamp
- correlation id

Avoid:

- raw payloads
- client names
- sensitive content
- secrets
- prompts or generated responses unless approved

## Entitlement-Aware Response Design

Example section response:

```json
{
  "section": "advisorBrief",
  "state": "PERMISSION_BLOCKED",
  "reason": "CALLER_NOT_ENTITLED",
  "data": null
}
```

This lets UI render a safe blocked state.

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Authorization only in frontend | Backend can be bypassed. |
| BFF assumes all upstream calls allowed | Domain-level leakage. |
| Raw entitlement message returned | Sensitive policy leakage. |
| Permission failure shown as empty data | User confusion and weak audit. |
| Caller context names differ per API | Integration drift. |
| No audit for write action | Weak traceability. |
| Metrics label includes actor id | Sensitive and high-cardinality label. |

## Review Checklist

- Is caller context required?
- Are header names standardized?
- Is authorization enforced server-side?
- Does BFF forward approved context?
- Does domain service validate scope?
- Are permission-blocked states safe?
- Are audit events required?
- Are logs and metrics sensitive-data safe?
- Are tests present for allowed and denied paths?
- Does OpenAPI document caller-context requirements?

## Summary

Entitlement-aware APIs protect client trust.

They must enforce access, communicate blocked states safely, and preserve auditability without leaking sensitive policy details.
