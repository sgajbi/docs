# Data Quality, Validation, Reconciliation, and Degraded States

## Purpose

This file explains how data products should define and expose data-quality posture.

Data quality is not only a batch check. It is a contract between producer and consumer.

---

## Data Quality Dimensions

| Dimension | Meaning |
|---|---|
| Completeness | Required fields and records are present. |
| Accuracy | Data matches source or expected calculation. |
| Timeliness | Data is fresh enough. |
| Consistency | Data agrees across related sources. |
| Validity | Data matches schema and allowed values. |
| Uniqueness | Duplicate records are controlled. |
| Reconciliation | Source and target totals or records agree. |
| Entitlement safety | Data is visible only to allowed consumers. |

---

## Validation Types

| Validation | Example |
|---|---|
| Schema validation | Required fields, types, enums. |
| Semantic validation | Dates, units, currency, period compatibility. |
| Referential validation | Portfolio exists, instrument exists. |
| Reconciliation validation | Position totals agree with source. |
| Freshness validation | Source date within expected window. |
| Access validation | Consumer allowed to access product. |
| Evidence validation | Required evidence artifact exists. |

---

## Quality States

| State | Meaning |
|---|---|
| VALIDATED | Checks passed. |
| PARTIAL | Some required inputs missing. |
| STALE | Freshness outside threshold. |
| RECONCILIATION_PENDING | Reconciliation not complete. |
| RECONCILIATION_FAILED | Known mismatch exists. |
| SOURCE_UNAVAILABLE | Source dependency missing. |
| RESTRICTED | Data exists but caller cannot access. |
| DISPUTED | Data under investigation. |
| UNKNOWN | Quality cannot be proven. |

---

## Degraded Data States

A data product should distinguish:

- partial data
- stale data
- restricted data
- unavailable source
- failed validation
- unknown trust posture
- unsupported product variant

Consumers need these distinctions because response behavior differs.

---

## Reconciliation

Reconciliation compares source and produced data.

Examples:

- record count
- market value total
- cash balance total
- transaction count
- report job count
- event count
- hash comparison
- field-level checks
- source-to-target completeness

Reconciliation should produce evidence.

---

## Consumer Impact

Each quality state should define consumer impact.

Example:

| State | Consumer Impact |
|---|---|
| VALIDATED | Product can be used normally. |
| STALE | UI should show stale warning; reporting may block depending on policy. |
| PARTIAL | UI may render partial section; calculations may block. |
| RECONCILIATION_FAILED | Consumer should not use for official report without override. |
| RESTRICTED | Consumer should show permission-blocked state. |
| UNKNOWN | Consumer should treat as not certified. |

---

## Quality Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Quality check only in batch log | Consumers cannot see quality posture. |
| Stale data shown as ready | Wrong decisions. |
| Reconciliation failure hidden | Reports and analytics incorrect. |
| Unknown state treated as success | False trust. |
| Validation only checks schema | Semantically invalid data passes. |
| Restricted data returned as empty | User confusion and audit gap. |
| No evidence for quality state | Certification cannot be trusted. |

---

## Review Checklist

- Are quality dimensions defined?
- Are validation rules documented?
- Are reconciliation checks defined?
- Are quality states exposed?
- Are degraded states explicit?
- Is consumer impact documented?
- Are stale/partial/restricted states tested?
- Is evidence generated?
- Are dashboards/alerts linked?
- Are known gaps documented?

---

## Summary

Data quality is useful only when it is visible, actionable, and tied to consumer behavior.

A data product should not simply succeed or fail. It should explain its quality posture.
