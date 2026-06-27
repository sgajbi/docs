# Knowledge Maintenance, Lifecycle, Ownership, and Maturity Roadmap

## Purpose

This file explains how to maintain a knowledge base over time.

Knowledge bases decay without ownership, review cadence, and lifecycle management.

---

## Document Lifecycle

| State | Meaning |
|---|---|
| Draft | Work in progress. |
| Current | Reviewed and accurate. |
| Needs review | Possibly stale. |
| Deprecated | Being replaced. |
| Archived | Historical only. |
| Removed | Deleted because no longer useful. |

---

## Ownership

Each durable document should have:

- owner
- reviewing team
- last reviewed date
- review cadence
- classification
- related system
- source of truth

---

## Review Cadence

Suggested cadence:

| Doc Type | Cadence |
|---|---|
| README | Every major change or quarterly. |
| Runbook | Every operational change or after incident. |
| Architecture doc | After major design change or quarterly. |
| ADR | When decision changes or is superseded. |
| API docs | With contract changes. |
| Supported-feature docs | With feature/release changes. |
| KT guide | Every onboarding cycle. |

---

## Maturity Levels

| Level | Description |
|---|---|
| L0 | Scattered notes. |
| L1 | Basic docs exist. |
| L2 | Docs organized and indexed. |
| L3 | Docs source-controlled and reviewed. |
| L4 | Docs validated with CI and evidence-linked. |
| L5 | Knowledge system supports KT, RAG, operations, governance, and continuous improvement. |

---

## Maintenance Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| No owner | Docs decay. |
| No review cadence | Staleness accumulates. |
| Deprecated docs not labelled | Wrong guidance reused. |
| Obsolete docs kept forever | Search quality drops. |
| Every team uses different structure | Knowledge fragmentation. |
| No feedback loop from onboarding | Same questions repeat. |

---

## Review Checklist

- Does document have owner?
- Does it have lifecycle status?
- Does it have last reviewed date?
- Is review cadence defined?
- Is classification clear?
- Is it still useful?
- Should it be merged, archived, or removed?
- Are links current?
- Are users giving feedback?
- Is maturity improving?

---

## Summary

Knowledge management is continuous.

A good knowledge base is maintained like a product, not dumped like an archive.
