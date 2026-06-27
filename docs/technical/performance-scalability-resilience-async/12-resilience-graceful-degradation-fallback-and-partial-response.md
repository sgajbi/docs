# Resilience, Graceful Degradation, Fallback, and Partial Response

## Purpose

This file explains how systems should behave when dependencies or data are incomplete, stale, slow, or unavailable.

Graceful degradation is the ability to reduce capability safely instead of failing everything.

---

## Degraded States

Useful states:

| State | Meaning |
|---|---|
| READY | Full behavior available. |
| PARTIAL | Some sections available. |
| STALE | Data is old. |
| DEGRADED | Reduced quality. |
| UNAVAILABLE | Required dependency unavailable. |
| PERMISSION_BLOCKED | Caller not allowed. |
| UNSUPPORTED | Not supported for request. |
| UNKNOWN | Posture cannot be proven. |

---

## Partial Response

Use partial response when:

- optional section fails
- UI can render remaining sections
- user can understand limitation
- no incorrect business decision is created
- supportability state is explicit

Do not use partial response when correctness requires all inputs.

---

## Fallback

Fallback examples:

- cached but marked stale
- previous successful result
- summary without detail
- read-only mode
- degraded analytics section
- queued async execution instead of synchronous
- retry later message

Fallback must be honest.

---

## Dependency Classification

Classify dependencies:

| Dependency Type | Failure Behavior |
|---|---|
| Required | Not ready or request unavailable. |
| Optional | Degraded or partial response. |
| Enhancement | Omit section or warning. |
| Async | Queue/retry/replay. |
| Security-critical | Fail closed. |

---

## Resilience Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Fallback silently uses stale data | Misleading output. |
| Optional dependency blocks whole service | Poor availability. |
| Required dependency failure hidden | Incorrect result. |
| Permission failure shown as empty | Audit gap. |
| Unknown state treated as ready | False confidence. |
| All dependencies checked in liveness | Restart loops. |

---

## Review Checklist

- Which dependencies are required?
- Which are optional?
- What is fallback?
- Is stale data labelled?
- Is partial response safe?
- Does UI show degraded state?
- Are reason codes bounded?
- Are metrics emitted?
- Are degraded states tested?
- Is runbook updated?

---

## Summary

Resilience is not pretending everything is fine.

It is failing, degrading, and recovering truthfully.
