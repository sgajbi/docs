# Data Quality, Supportability, Warnings, and Degraded Reports

## Purpose

This file explains how reports should handle data-quality issues.

Reports must not hide stale, partial, degraded, permission-blocked, or unsupported states.

---

## Report Supportability States

| State | Meaning |
|---|---|
| READY | Report can be generated normally. |
| PARTIAL | Some sections missing or incomplete. |
| STALE | Data freshness outside threshold. |
| DEGRADED | Report generated with reduced quality. |
| UNAVAILABLE | Required data or service unavailable. |
| PERMISSION_BLOCKED | Caller not entitled. |
| UNSUPPORTED | Report not supported for requested scope. |
| RECONCILIATION_FAILED | Source or report data mismatch. |
| UNKNOWN | Supportability cannot be proven. |

---

## Warning Types

Warnings may include:

- stale price
- missing FX
- missing benchmark
- partial holdings
- unavailable risk section
- performance calculation warning
- entitlement-filtered section
- reconciliation pending
- unsupported instrument
- late transaction not included

---

## Report Decision Matrix

| Condition | Possible Decision |
|---|---|
| Optional section stale | Include with warning. |
| Required valuation missing | Block final report. |
| User lacks entitlement | Block or omit section with permission state. |
| Benchmark missing | Generate without benchmark section if allowed. |
| Reconciliation failed | Block official report unless approved override. |
| Unknown source state | Block or mark not certified. |

---

## Client-Facing Warnings

Warnings should be:

- clear
- safe
- not overly technical
- not leaking internal source details
- approved for client visibility where needed
- stored in evidence

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Missing section rendered as empty table | Misleading. |
| Stale data hidden | Client trust issue. |
| Internal error shown to client | Poor UX/security risk. |
| Unknown state treated as ready | False report confidence. |
| Warning not stored in archive evidence | Later explanation gap. |
| Permission-blocked data omitted silently | Confusing and weak audit. |

---

## Review Checklist

- Are supportability states defined?
- Are warnings structured?
- Are client-visible warnings approved?
- Are blocking conditions defined?
- Are optional sections identified?
- Are stale/partial states tested?
- Are warnings archived in evidence?
- Are permission-blocked states safe?
- Are reconciliation failures handled?

---

## Summary

Report quality is not binary.

A report can be ready, partial, stale, degraded, blocked, or unsupported, and the system must represent that truth.
