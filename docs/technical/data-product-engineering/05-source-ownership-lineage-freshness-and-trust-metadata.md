# Source Ownership, Lineage, Freshness, and Trust Metadata

## Purpose

This file explains how data products should communicate where data came from, how current it is, and why consumers should trust it.

Source, lineage, and freshness are not optional metadata in regulated financial platforms. They are part of the product.

---

## Source Ownership

Source ownership answers:

```text
Which system/domain is authoritative for this data?
```

Examples:

- portfolio source service owns holdings
- performance service owns performance results
- risk service owns risk metrics
- reporting service owns report evidence
- archive service owns document metadata and retrieval lifecycle
- platform governance owns generated catalog/trust evidence

Consumers should not infer source ownership from where they received the data.

---

## Lineage

Lineage explains how data was produced.

Lineage can include:

- source system
- source record reference
- source business date
- ingestion batch
- transformation version
- calculation methodology version
- reconciliation status
- evidence artifact
- generated timestamp
- consumer request id
- correlation id where safe

---

## Lineage Bundle

A lineage bundle is a structured group of lineage fields.

Example:

```json
{
  "sourceSystem": "portfolio-source",
  "sourceBusinessDate": "2026-04-10",
  "ingestionBatchId": "synthetic-batch",
  "transformationVersion": "v1",
  "reconciliationStatus": "PASSED",
  "evidenceRef": "synthetic-evidence-ref"
}
```

Avoid sensitive raw identifiers unless policy allows.

---

## Freshness

Freshness describes how current data is.

Freshness fields:

- asOfDate
- businessDate
- generatedAt
- sourceTimestamp
- refreshedAt
- freshnessBucket
- staleReason
- expectedFrequency

---

## Freshness Buckets

| Bucket | Meaning |
|---|---|
| CURRENT | Within expected window. |
| RECENT | Slightly older but acceptable. |
| STALE | Outside expected window. |
| UNKNOWN | Cannot prove freshness. |
| NOT_APPLICABLE | Static or non-time-sensitive data. |

---

## Trust Metadata

Trust metadata summarizes product trust posture.

Example:

```json
{
  "certificationState": "CERTIFIED",
  "supportabilityState": "READY",
  "freshnessBucket": "CURRENT",
  "qualityState": "VALIDATED",
  "lineageAvailable": true,
  "evidenceClass": "RUNTIME_TELEMETRY"
}
```

Trust metadata should be:

- bounded
- versioned
- safe
- machine-readable
- validated
- consumer-visible where useful

---

## Source And Trust Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Data returned without source owner | Consumers cannot resolve disputes. |
| Freshness hidden | Stale data used as current. |
| GeneratedAt mistaken for business date | Incorrect interpretation. |
| Lineage only in logs | Consumers cannot access evidence. |
| Raw source paths exposed | Internal data leakage. |
| Static fixture presented as live trust | False certification. |
| Trust metadata free-form | Consumers cannot automate checks. |

---

## Review Checklist

- Is source owner explicit?
- Is lineage available?
- Is freshness defined?
- Are business date and generated timestamp separate?
- Is trust metadata bounded?
- Is evidence reference safe?
- Are stale/unknown states possible and documented?
- Are lineage fields tested?
- Is trust metadata validated by contract?
- Can consumers explain why the data is trusted?

---

## Summary

Trust requires evidence.

A data product without source, lineage, and freshness is only data. A data product with source, lineage, freshness, and evidence can become trusted.
