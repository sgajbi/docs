# Idempotency, Concurrency, Locking, and Duplicate Prevention

## Purpose

This file explains how to prevent duplicate side effects and race conditions.

Enterprise systems must expect retries, double submissions, concurrent users, workers, and partial failures.

---

## Idempotency

Use idempotency for write-like operations:

- create job
- submit calculation
- request report
- acknowledge action
- replay work item
- publish external handoff
- create workflow state
- start batch

---

## Idempotency Record

Store:

- idempotency key
- operation
- caller scope
- request hash
- current status
- result reference
- created at
- expiry
- conflict state

---

## Duplicate Handling

| Case | Behavior |
|---|---|
| Same key, same request, completed | Return previous result. |
| Same key, same request, running | Return current status. |
| Same key, different request | 409 conflict. |
| Missing key when required | 400 invalid request. |
| Expired key | Treat according to policy. |

---

## Concurrency Controls

Controls:

- unique constraints
- optimistic locking
- version fields
- compare-and-set update
- transaction isolation
- row locks where necessary
- queue leases
- idempotency keys
- state transition checks

---

## Locking

Use locks carefully.

Lock types:

- database row lock
- advisory lock
- distributed lock
- queue lease
- optimistic version check

Avoid long locks around external calls.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Frontend disables button only | Backend duplicates still possible. |
| Retry creates second job | User/support confusion. |
| No unique constraint | Race condition. |
| Lock held during HTTP call | Slow contention. |
| State transition not checked | Invalid lifecycle. |
| Worker lease not renewed safely | Duplicate processing. |
| Idempotency key not scoped | Cross-user conflict. |

---

## Review Checklist

- Is operation write-like?
- Is idempotency required?
- Is key scope clear?
- Is request hash stored?
- Are duplicates handled?
- Are conflicts handled?
- Are unique constraints present?
- Are lifecycle transitions atomic?
- Are locks short-lived?
- Are concurrent workers safe?
- Are tests present?

---

## Summary

Idempotency and concurrency controls make retries and parallelism safe.

Without them, resilience mechanisms can create data corruption.
