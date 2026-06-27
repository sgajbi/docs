# 06 - Data Pipeline, Event, Batch, API and Snapshot Control Design

## 1. Why pipeline controls matter

A wealth platform usually combines batch files, APIs, event streams, database extracts, manual operations and calculation jobs. Data defects can enter at any point. Strong controls must therefore cover the full movement of data from source to report.

## 2. Ingestion patterns

| Pattern | Examples | Key controls |
|---|---|---|
| Batch file | EOD positions, prices, transactions, fund NAVs. | File arrival, checksum, row count, control totals, schema validation. |
| API pull | Reference data, prices, client profile, mandate rules. | Response validation, retry, pagination, freshness, idempotency. |
| Event stream | Trades, transactions, position events, corporate actions. | Ordering, deduplication, offset tracking, replay safety. |
| Database replication | Source tables replicated to platform. | CDC completeness, lag monitoring, schema drift detection. |
| Manual input | Price override, correction, product setup. | Maker-checker, approval, expiry, audit trail. |
| Vendor portal | Private-market notices, fund statements. | Upload validation, document capture, extraction QA. |

## 3. Batch control design

Batch ingestion should validate:

- file name and expected calendar;
- source identity and environment;
- checksum/hash;
- row count;
- control totals by amount/currency/product;
- schema version;
- mandatory fields;
- duplicate records;
- rejected rows;
- late arrival;
- partial delivery;
- downstream publication status.

Batch run metadata:

| Field | Purpose |
|---|---|
| batch_run_id | Unique run identifier. |
| source_system | Origin. |
| file_name | Source file. |
| business_date | EOD or report date. |
| received_at | Operational timestamp. |
| processed_at | Completion timestamp. |
| status | Received, validated, rejected, loaded, published. |
| record_count | Basic completeness. |
| rejected_count | Quality issue count. |
| control_total | Amount/quantity totals. |
| lineage_id | Link to downstream calculations. |

## 4. Event-stream control design

Event-driven pipelines need controls different from batch.

| Risk | Control |
|---|---|
| Duplicate event | Idempotency key and event ledger. |
| Missing event | Sequence number/offset monitoring. |
| Out-of-order event | Versioning, effective timestamp and reorder buffer. |
| Replay | Replay mode with deterministic output and no duplicate postings. |
| Poison message | Quarantine and dead-letter queue. |
| Schema evolution | Backward-compatible contract and schema registry. |
| Partial processing | Transactional outbox/inbox or checkpoint control. |
| Event correction | Correction event references original event. |

## 5. API control design

APIs must expose quality and lineage, not only data.

Recommended API metadata:

```json
{
  "data": {},
  "asOfDate": "2026-06-30",
  "sourceSystem": "PRICE_SERVICE",
  "lineageId": "LIN-12345",
  "qualityStatus": "DEGRADED",
  "exceptions": [
    {
      "code": "STALE_PRICE",
      "severity": "HIGH",
      "affectedInstrumentId": "BOND123",
      "message": "Price is older than freshness threshold"
    }
  ],
  "calculationRunId": "CALC-789",
  "snapshotId": "SNAP-456"
}
```

API quality controls:

- pagination correctness;
- deterministic sorting;
- request idempotency for mutations;
- correlation ID;
- freshness metadata;
- source and calculation version;
- null semantics;
- degraded-state representation;
- backward-compatible schema changes;
- rate limiting and resiliency.

## 6. Snapshot design

A snapshot is a controlled, point-in-time representation of data.

Use snapshots for:

- client reports;
- performance calculations;
- risk reports;
- advisory proposals;
- DPM mandate review;
- migration parallel run;
- audit reproduction.

Snapshot fields:

| Field | Purpose |
|---|---|
| snapshot_id | Unique identifier. |
| snapshot_type | Valuation, report, performance, risk, proposal. |
| as_of_date | Business date. |
| created_at | System timestamp. |
| source_versions | Input dataset versions. |
| calculation_versions | Formula/model versions. |
| quality_status | Complete/degraded/blocked/preliminary/certified. |
| exception_summary | Known issues. |
| locked_flag | Prevents mutation after publication. |
| supersedes_snapshot_id | Restatement lineage. |

## 7. Idempotency and deduplication

Idempotency means the same input can be processed more than once without creating duplicate effects.

Examples:

| Event | Idempotency key |
|---|---|
| Trade booking | Source system + trade ID + version. |
| Coupon payment | Source event ID + account + instrument + pay date. |
| Fund subscription confirmation | Transfer agent confirmation ID. |
| Corporate action entitlement | CA event ID + account + instrument + entitlement type. |
| Price load | Instrument + price source + price type + valuation date + version. |
| FX rate | Currency pair + rate type + source + valuation date + version. |

Do not rely only on payload equality. Corrections often share business keys but differ by version.

## 8. Reprocessing and recalculation controls

Reprocessing should be deterministic.

| Control | Description |
|---|---|
| Run reason | Backdated transaction, price correction, corporate action, migration, bug fix. |
| Scope | Account, instrument, date range, report, benchmark. |
| Dependency graph | Identify downstream calculations to refresh. |
| Old vs new comparison | Delta report before publication. |
| Approval | Required for client-impacting restatement. |
| Audit | Store before/after snapshot and lineage. |
| Notification | Consumers alerted if published numbers change. |

## 9. Late-arriving and backdated data

Late data is common in wealth platforms.

Examples:

- backdated transaction;
- fund NAV received late;
- private-market capital call notice processed after date;
- corporate action corrected after ex-date;
- price restatement;
- FX correction;
- settlement failure after trade-date booking.

Required behaviours:

1. Store original arrival time and effective date.
2. Determine impacted date range.
3. Recalculate dependent positions, cash, performance and reports.
4. Compare old vs new outputs.
5. Decide whether restatement is needed.
6. Preserve audit trail.

## 10. Degraded pipeline states

| State | Meaning |
|---|---|
| Source late | Expected feed not received by SLA. |
| Source partial | Feed received but control total/record count incomplete. |
| Source rejected | Schema or mandatory validation failed. |
| Loaded with warnings | Non-critical rows rejected or degraded. |
| Published preliminary | Data available but not final certified. |
| Published certified | Passed controls and certified. |
| Downstream blocked | Critical dependency failed. |

## 11. Observability metrics

| Metric | Purpose |
|---|---|
| Feed arrival time | SLA monitoring. |
| Processing duration | Performance and bottleneck control. |
| Records processed/rejected | Completeness and quality. |
| Duplicate event count | Idempotency issue detection. |
| Dead-letter queue size | Event processing health. |
| Stale data count | Freshness. |
| Recalculation jobs open/failed | Analytics stability. |
| Snapshot certification delay | Reporting readiness. |
| Break ageing | Operational risk. |

## 12. Practitioner takeaway

Data pipelines should be designed like financial processes: controlled, auditable and reversible. A successful batch, API or event stream is not just one that technically runs; it is one that proves what it received, what it rejected, what it transformed, what it published and what downstream outputs it affected.
