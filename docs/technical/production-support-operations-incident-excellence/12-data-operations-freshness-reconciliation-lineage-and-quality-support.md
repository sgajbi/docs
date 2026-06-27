# Data Operations, Freshness, Reconciliation, Lineage, and Quality Support

## Purpose

This file explains production support for data-driven systems.

Many incidents in wealth and banking platforms are data incidents: stale data, failed ingestion, reconciliation breaks, or lineage gaps.

---

## Data Operations Signals

Monitor:

- source feed arrival
- ingestion success
- record count
- data freshness
- reconciliation result
- quality-rule failures
- late records
- duplicate records
- backdated corrections
- curation failures
- downstream publication
- consumer lag

---

## Freshness States

| State | Meaning |
|---|---|
| CURRENT | Data is within expected freshness threshold. |
| STALE | Data is older than threshold. |
| DELAYED | Expected source has not arrived. |
| PARTIAL | Some expected data missing. |
| FAILED | Data processing failed. |
| UNKNOWN | Freshness cannot be proven. |

---

## Reconciliation Support

Support should know:

- reconciliation rule
- expected tolerance
- break severity
- break owner
- downstream impact
- rerun/replay option
- accepted exception process
- escalation path

---

## Lineage Support

Lineage helps answer:

- which source produced this value?
- when was it ingested?
- which transformation ran?
- which batch id?
- which records failed?
- which report/API consumed it?

---

## Data Operations Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Data freshness hidden from users/support | Misleading output. |
| Reconciliation break only in logs | Support blind. |
| Manual rerun without audit | Control gap. |
| Failed records dropped silently | Data loss. |
| Unknown state treated as current | False confidence. |
| Source owner unclear | Slow resolution. |
| Lineage not captured | Hard to explain issue. |

---

## Review Checklist

- Are freshness states defined?
- Are data-quality rules monitored?
- Are reconciliation breaks visible?
- Are break owners defined?
- Is rerun/replay safe?
- Is lineage captured?
- Are downstream impacts known?
- Are data runbooks available?
- Are stale/partial states shown?
- Are accepted exceptions governed?

---

## Summary

Data operations are production operations.

Freshness, reconciliation, lineage, and quality states must be visible and supportable.
