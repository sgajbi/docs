# Timeouts, Retries, Backoff, Jitter, and Circuit Breakers

## Purpose

This file explains how to design safe dependency calls.

External calls fail. Dependencies slow down. Networks hang. Retry behavior can either improve resilience or amplify incidents.

---

## Timeouts

Every external call should have timeouts.

Types:

- connection timeout
- read timeout
- overall request timeout
- database query timeout
- queue operation timeout
- worker processing timeout

No timeout means a failure can hang indefinitely.

---

## Retry Policy

Retries should be bounded.

Define:

```text
max attempts:
initial delay:
backoff:
jitter:
retryable errors:
non-retryable errors:
timeout per attempt:
overall timeout:
metrics:
```

---

## Retryable Versus Non-Retryable

Retryable:

- network timeout
- temporary 503
- connection reset
- rate limit with retry-after
- transient lock conflict

Usually not retryable:

- validation error
- permission denied
- unsupported operation
- not found
- idempotency conflict
- deterministic business rule failure

---

## Backoff and Jitter

Backoff increases delay between retries.

Jitter adds randomness to prevent many clients retrying at the same time.

Without jitter, retries can create traffic waves.

---

## Retry Budget

A retry budget limits extra load caused by retries.

Example:

```text
Retry traffic should not exceed 10% of normal request volume.
```

---

## Circuit Breaker

A circuit breaker stops calls to a failing dependency temporarily.

States:

- closed
- open
- half-open

Use when repeated dependency failures should fail fast and protect resources.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No timeout | Threads hang and queues fill. |
| Retry everything | Failure amplified. |
| Infinite retry | Resource exhaustion. |
| No jitter | Retry storm. |
| Retry non-idempotent writes | Duplicate side effects. |
| Same timeout for all dependencies | Poor tuning. |
| Circuit breaker without fallback | User impact not defined. |

---

## Review Checklist

- Are timeouts defined?
- Are retries bounded?
- Is backoff used?
- Is jitter used?
- Are retryable errors explicit?
- Are non-retryable errors explicit?
- Is idempotency guaranteed for retries?
- Is retry traffic measured?
- Is circuit breaker needed?
- Is fallback/degraded behavior defined?

---

## Summary

Retries are medicine in small doses and poison in excess.

Use timeouts, backoff, jitter, retry budgets, and idempotency to make them safe.
