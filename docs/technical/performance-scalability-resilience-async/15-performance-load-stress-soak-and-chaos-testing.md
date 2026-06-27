# Performance, Load, Stress, Soak, and Chaos Testing

## Purpose

This file explains testing approaches for performance and resilience.

Different tests answer different questions.

---

## Test Types

| Test Type | Question |
|---|---|
| Benchmark | How fast is one operation under controlled conditions? |
| Load test | How does system behave under expected load? |
| Stress test | Where does system break? |
| Soak test | Does system degrade over long duration? |
| Spike test | How does system handle sudden traffic? |
| Capacity test | What is maximum sustainable throughput? |
| Resilience test | What happens when dependency fails? |
| Chaos test | Can system tolerate controlled failure injection? |

---

## Load Test Design

Define:

- target environment
- test data
- user/request profile
- duration
- ramp-up
- expected throughput
- latency target
- pass/fail criteria
- monitoring dashboard
- artifact output

---

## Stress Test

Stress tests identify limits.

Measure:

- failure threshold
- bottleneck
- degradation behavior
- recovery after stress
- alerts triggered
- resource saturation

---

## Soak Test

Soak tests catch:

- memory leaks
- connection leaks
- slow queue buildup
- log growth
- cache growth
- file handle leaks
- performance drift

---

## Chaos Testing

Controlled failure examples:

- stop dependency
- inject latency
- kill worker
- drop messages
- fail database connection
- simulate queue backlog
- return malformed upstream response

Only run in controlled environments with approval.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Load test with unrealistic data | False confidence. |
| No pass/fail criteria | Results subjective. |
| Testing only average latency | Tail risk hidden. |
| No monitoring during test | Root cause unclear. |
| Chaos in unsafe environment | Business impact. |
| Results not retained | No trend analysis. |
| No follow-up from findings | Test wasted. |

---

## Review Checklist

- What question does the test answer?
- Is data realistic and synthetic?
- Is environment appropriate?
- Are pass/fail criteria defined?
- Are dashboards ready?
- Are dependencies monitored?
- Are results retained?
- Are findings converted to backlog?
- Is test safe to run?
- Is repeat cadence defined?

---

## Summary

Performance and resilience tests should produce decisions.

If a test does not change confidence or backlog, redesign the test.
