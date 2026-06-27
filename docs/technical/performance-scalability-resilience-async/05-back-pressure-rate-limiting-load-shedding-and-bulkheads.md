# Back-Pressure, Rate Limiting, Load Shedding, and Bulkheads

## Purpose

This file explains how systems protect themselves under overload.

A resilient system should reject or slow work safely instead of collapsing unpredictably.

---

## Back-Pressure

Back-pressure tells callers or upstream processes to slow down.

Examples:

- queue full response
- 429 rate limit
- bounded worker queue
- semaphore/concurrency limit
- database connection pool limit
- batch size limit

---

## Rate Limiting

Rate limiting controls request volume.

Dimensions:

- per user
- per tenant
- per service
- per API key
- per route
- per region
- global

Responses should be clear and safe:

```text
429 Too Many Requests
Retry-After: 30
```

---

## Load Shedding

Load shedding rejects lower-priority work to protect critical capability.

Example:

- reject optional analytics request
- pause batch processing
- disable expensive detail panel
- return degraded response instead of timing out

---

## Bulkheads

Bulkheads isolate resources so one failure does not take everything down.

Examples:

- separate worker pools
- separate DB connection pools
- separate queues
- isolate heavy report generation from user-facing API
- separate critical and non-critical dependencies

---

## Queue Bounds

Unbounded queues create hidden overload.

Define:

- max queue depth
- rejection behavior
- priority behavior
- retry behavior
- dead-letter threshold
- alert threshold

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Unbounded request queue | Memory growth and collapse. |
| No rate limiting on expensive route | Abuse or accidental overload. |
| Batch consumes all resources | User-facing outage. |
| One shared worker pool for all work | Low-priority work blocks critical work. |
| Back-pressure not communicated | Callers retry harder. |
| Load shedding silent | Users misled. |

---

## Review Checklist

- What happens under overload?
- Are queues bounded?
- Are concurrency limits defined?
- Is rate limiting needed?
- Are critical flows protected?
- Are batch and API resources isolated?
- Is rejection response safe?
- Are overload metrics visible?
- Are alerts defined?
- Are tests present?

---

## Summary

Back-pressure is a sign of mature design.

Rejecting safely is better than failing unpredictably.
