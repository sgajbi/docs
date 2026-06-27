# 09 - Platform Implementation, Observability, Audit and Test Scenarios

## 1. Implementation architecture

A data governance and quality capability should be platform-native.

Recommended components:

| Component | Purpose |
|---|---|
| Data catalogue | Business/technical metadata, CDEs, owners, glossary. |
| Data contract registry | Producer-consumer schemas, SLAs, quality rules. |
| Rule engine | Executes validation, reconciliation and quality checks. |
| Exception service | Stores and routes data-quality breaks. |
| Lineage service | Tracks source-to-report dependencies. |
| Snapshot service | Creates immutable certified data snapshots. |
| Override service | Manages controlled manual corrections. |
| Reconciliation service | Compares sources and calculated outputs. |
| Certification workflow | Captures sign-off and publication readiness. |
| Observability dashboard | Monitors feed, quality, break, run and consumer health. |
| Audit repository | Stores evidence, approvals, restatements and run outputs. |

## 2. Canonical data-quality result model

```json
{
  "ruleId": "DQ-PRICE-001",
  "domain": "PRICING",
  "entityType": "INSTRUMENT_PRICE",
  "entityId": "BOND123|2026-06-30|EOD",
  "severity": "HIGH",
  "status": "FAILED",
  "businessDate": "2026-06-30",
  "detectedAt": "2026-07-01T06:05:00+08:00",
  "owner": "pricing-steward",
  "sourceSystem": "MARKET_DATA_VENDOR",
  "consumerImpact": ["VALUATION", "PERFORMANCE", "CLIENT_REPORT"],
  "message": "EOD price is stale by 3 business days",
  "lineageId": "LIN-123",
  "runId": "RUN-456"
}
```

## 3. Exception entity model

| Field | Purpose |
|---|---|
| exception_id | Unique break reference. |
| rule_id | Rule that detected issue. |
| severity | Business priority. |
| status | Open, assigned, resolved, closed, waived. |
| owner | Resolver. |
| consumer_impact | Affected reports/APIs/calculations. |
| materiality_amount | Financial impact where measurable. |
| affected_clients | Client/portfolio impact count. |
| root_cause | Categorised cause. |
| remediation_action | Fix, override, reprocess, accept, disclose. |
| approval | Required if waived/overridden. |
| closure_evidence | Proof of resolution. |

## 4. Audit and evidence requirements

Audit evidence should answer:

- what data was used;
- which rules ran;
- which rules failed;
- who approved exceptions;
- which reports/calculations consumed the data;
- what was restated;
- what changed between versions;
- why an override was applied;
- whether a control was bypassed;
- whether regression tests were added.

## 5. Observability design

### 5.1 Technical observability

- application health;
- job duration;
- API latency;
- queue lag;
- event replay rate;
- error rate;
- dead-letter count;
- database write failures;
- file arrival latency.

### 5.2 Data observability

- record counts;
- field completeness;
- stale data count;
- duplicate event count;
- schema drift;
- reconciliation breaks;
- quality score trend;
- upstream source delay;
- downstream impacted reports.

### 5.3 Business observability

- portfolios blocked from reporting;
- advisor proposals blocked;
- mandate checks unable to evaluate;
- market value affected by missing prices;
- high-net-worth/UHNW clients affected;
- open breaks by booking centre;
- critical open breaks by product.

## 6. Control APIs

Useful platform APIs:

| API | Purpose |
|---|---|
| `GET /data-quality/status?portfolioId=&date=` | Quality status by portfolio/date. |
| `GET /lineage/{lineageId}` | Trace source-to-report. |
| `GET /exceptions?severity=&owner=&status=` | Break management. |
| `POST /overrides` | Controlled override with approval. |
| `GET /snapshots/{snapshotId}` | Retrieve certified snapshot metadata. |
| `POST /reprocess` | Trigger controlled recalculation/reprocessing. |
| `GET /certifications?date=&domain=` | Certification evidence. |
| `GET /rules/{ruleId}` | Rule definition and owner. |

## 7. Test scenario categories

| Category | Examples |
|---|---|
| Schema validation | Missing mandatory fields, invalid enum, precision issue. |
| Duplicate handling | Same transaction/event sent twice. |
| Out-of-order events | Correction before original, corporate action after position. |
| Stale data | Price/NAV/FX older than threshold. |
| Reconciliation | Cash/position/value mismatches. |
| Product lifecycle | Coupon, dividend, split, autocall, maturity, capital call. |
| Backdated changes | Recalculate performance and reports. |
| Degraded states | Report with missing low-impact classification vs critical price. |
| Overrides | Approved override expires and is replaced by source correction. |
| Migration | Legacy and target outputs compared with expected differences. |

## 8. Detailed QA scenarios

### Scenario 1 - Missing price on material holding

| Step | Expected result |
|---|---|
| Portfolio has 40% market value in a bond. | Bond is material. |
| EOD price missing. | DQ-PRICE rule fails critical. |
| Report generation requested. | Report blocked or generated with approved fallback depending policy. |
| Override entered. | Maker-checker approval and expiry required. |
| Report generated. | Exception line and lineage recorded. |

### Scenario 2 - Duplicate transaction replay

| Step | Expected result |
|---|---|
| Same trade event received twice with same source ID/version. | Second event ignored as duplicate. |
| Same trade received with higher correction version. | Correction processed and old version superseded. |
| Position replay performed. | Deterministic position output. |

### Scenario 3 - Corporate action split

| Step | Expected result |
|---|---|
| Equity 100 shares, cost 10,000. | Initial cost per share 100. |
| 2-for-1 split effective. | Quantity 200, cost remains 10,000, cost per share 50. |
| Market value before/after split adjusted. | No artificial gain/loss due solely to split. |
| Report shows adjusted holdings. | Lineage references CA event. |

### Scenario 4 - Backdated cashflow impacts performance

| Step | Expected result |
|---|---|
| Month-end performance already calculated. | Snapshot exists. |
| Backdated external contribution arrives for mid-month. | Reprocessing dependency identifies affected periods. |
| TWR recalculated. | Old vs new delta produced. |
| Report previously published. | Restatement decision workflow triggered. |

### Scenario 5 - Mandate unable to evaluate

| Step | Expected result |
|---|---|
| DPM mandate caps high-yield bonds at 10%. | Rule exists. |
| Bond rating missing for new holding. | Mandate engine returns unable-to-evaluate/degraded, not pass. |
| Advisor/PM views portfolio. | Clear exception and owner shown. |

## 9. Runbook structure

Every critical control should have a runbook:

1. Symptom.
2. Detection rule/dashboard.
3. Impacted consumers.
4. Immediate containment.
5. Owner and escalation.
6. Diagnosis steps.
7. Remediation options.
8. Reprocessing requirements.
9. Certification requirements.
10. Client/advisor communication rules.
11. Regression test to add.

## 10. Metrics for management reporting

| Metric | Why it matters |
|---|---|
| Critical CDE completeness | Core data readiness. |
| Certification timeliness | Operational readiness. |
| High-severity break ageing | Control effectiveness. |
| Recurring root causes | Structural issues. |
| Manual override count | Process weakness or source instability. |
| Report blockers | Client impact. |
| Reprocessing volume | Data volatility and source quality. |
| Migration defect leakage | Release quality. |
| Regression coverage | Prevention maturity. |

## 11. Practitioner takeaway

The strongest data-governance platforms do not rely on people remembering what happened. They produce evidence automatically: run IDs, lineage IDs, rule results, exception records, snapshots, approvals, reprocessing deltas and certification status.
