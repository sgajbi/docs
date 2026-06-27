# Operational Supportability, Runbooks, and Data Quality States

## Purpose

This file explains how to support wealth platforms operationally.

Supportability is critical because many issues are data freshness, source quality, reconciliation, entitlement, or dependency problems.

---

## Common Supportability States

| State | Meaning |
|---|---|
| READY | Data/capability supported and current. |
| PARTIAL | Some sections or sources missing. |
| STALE | Data outside freshness threshold. |
| DEGRADED | Capability available with reduced quality. |
| UNAVAILABLE | Required dependency unavailable. |
| PERMISSION_BLOCKED | Caller not entitled. |
| RECONCILIATION_FAILED | Source/target mismatch. |
| UNSUPPORTED | Request outside supported boundary. |
| UNKNOWN | State cannot be proven. |

---

## Common Operational Issues

- missing price
- stale FX rate
- failed ingestion
- delayed EOD batch
- transaction duplicate
- backdated correction
- reconciliation break
- report generation failure
- archive retrieval failure
- entitlement mismatch
- benchmark data missing
- performance calculation failure

---

## Runbook Topics

Runbooks should exist for:

- data ingestion delay
- stale price/FX
- reconciliation failure
- performance calculation failure
- report job failure
- document archive failure
- API degradation
- entitlement issue
- migration/cutover issue
- batch missed SLA

---

## Dashboards

Dashboards should show:

- source freshness
- ingestion status
- reconciliation status
- calculation failures
- report job status
- queue depth
- stale/partial states
- permission-blocked count
- dependency health
- recent deployments

---

## Support Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Data issue returns generic 500 | Poor diagnosis. |
| Stale data not visible | User over-trust. |
| Runbook says contact developer only | Slow support. |
| No data-quality dashboard | Breaks discovered by users. |
| Reconciliation failure hidden | Reports wrong. |
| Unknown state shown as ready | False confidence. |

---

## Review Checklist

- Are supportability states defined?
- Are data-quality states visible?
- Are dashboards available?
- Are runbooks current?
- Are alerts actionable?
- Are reason codes bounded?
- Are stale/partial cases tested?
- Is support owner clear?
- Is escalation defined?

---

## Summary

Wealth-platform support requires visibility into data quality, freshness, reconciliation, dependencies, and entitlements.

Operational states must be first-class.
