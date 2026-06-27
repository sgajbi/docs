# 04 - Data Model, Reporting Snapshot, Lineage and Versioning

## 1. Why reporting needs snapshots

A report must be reproducible. Reproducibility requires more than recalculating today from current source data because historical source data may change.

Examples:

- a fund NAV is corrected after statement production;
- a bond price is restated by the vendor;
- a corporate action is booked late;
- a trade is cancelled and rebooked;
- FX rates are corrected;
- a client hierarchy changes;
- a product classification changes;
- a performance engine version is upgraded.

Therefore reporting should store a reporting snapshot or sufficient lineage to replay the report exactly.

## 2. Core reporting dataset pattern

Recommended logical layers:

```text
Source Systems
  -> Curated Data Store
  -> Calculation Engines
  -> Reporting Dataset
  -> Rendered Outputs
  -> Archive and Evidence Store
```

The reporting dataset should be independent of layout. PDF, UI and API should consume the same controlled data model.

## 3. Report run model

| Field | Meaning |
|---|---|
| report_run_id | Unique report production run. |
| report_type | Statement, review, DPM, tax, risk, etc. |
| client_id | Client or household/entity. |
| account_scope | Account, relationship, mandate, family group. |
| period_start | Reporting period start. |
| period_end | Reporting period end. |
| valuation_datetime | Valuation cut-off timestamp. |
| report_currency | Base/report currency. |
| basis | Trade-date, settlement-date, accounting, performance, tax. |
| data_cut_id | Source data cut used. |
| calculation_version | Version of analytics/calculation engine. |
| template_version | Layout/template version. |
| disclosure_version | Disclosure pack version. |
| status | Draft, validated, approved, published, archived, cancelled. |
| produced_by | System/user/batch. |
| approved_by | Optional approval actor. |
| archive_uri | Link to immutable archive. |

## 4. Report section model

| Field | Meaning |
|---|---|
| report_section_id | Unique section instance. |
| report_run_id | Parent run. |
| section_type | Holdings, activity, performance, risk, etc. |
| section_status | Complete, partial, suppressed, failed. |
| data_quality_status | Clean, warning, exception. |
| exception_count | Number of material exceptions. |
| generated_at | Generation timestamp. |
| renderer_component | Template/render component used. |

## 5. Report cell lineage

For high-control reporting, important report values should have lineage.

| Field | Meaning |
|---|---|
| cell_id | Logical report-cell identifier. |
| value | Displayed value. |
| value_type | Amount, percent, date, text, flag. |
| source_system | Price source, custody, ledger, performance engine, etc. |
| source_record_id | Original source identifier where available. |
| calculation_id | Calculation job/run ID. |
| formula_version | Formula version. |
| input_snapshot_ids | Inputs used. |
| quality_flag | Good, estimated, stale, missing, overridden. |
| override_id | User/system override if applicable. |

This is especially important for total wealth, performance, fees, tax, NAV, valuation and mandate breach values.

## 6. Snapshot versus reference-by-lineage

Two patterns exist.

| Pattern | Description | Pros | Cons |
|---|---|---|---|
| Full snapshot | Store all report data as produced. | Strong reproducibility. | Storage and privacy footprint. |
| Reference lineage | Store source IDs and calculation versions. | Lower storage and easier correction. | Reproduction can fail if sources mutate. |
| Hybrid | Store report-critical fields and lineage to inputs. | Practical for wealth platforms. | Requires careful design. |

For client statements, a hybrid approach is usually best: archive the rendered output and store the structured report dataset for major sections.

## 7. Correction and restatement model

A report can be corrected without silently overwriting the original.

Recommended states:

| State | Meaning |
|---|---|
| DRAFT | Generated but not published. |
| VALIDATED | Passed controls, pending approval. |
| PUBLISHED | Delivered to client/advisor. |
| ARCHIVED | Immutable evidence stored. |
| SUPERSEDED | Replaced by a corrected report. |
| CANCELLED | Produced in error and not valid. |

If a published report is corrected, store:

- original report ID;
- corrected report ID;
- reason code;
- materiality assessment;
- approval actor;
- client notification status;
- changed sections;
- changed values summary.

## 8. Versioning dimensions

A report value may change because of:

| Version type | Example |
|---|---|
| Data version | Corrected transaction or price. |
| Formula version | Performance methodology changed. |
| Template version | Layout or label changed. |
| Disclosure version | Updated risk disclaimer. |
| Product taxonomy version | Product category changed. |
| Client hierarchy version | Account moved under family group. |
| FX policy version | Valuation FX source changed. |
| Benchmark version | Benchmark composition changed. |

Each dimension should be captured when relevant.

## 9. Data lineage for key sections

| Section | Required lineage |
|---|---|
| Holdings | Position source, price source, FX source, instrument master. |
| Transactions | Ledger/custody source, booking status, reversal/correction links. |
| Income | Corporate action/source event, tax withholding, payment date. |
| Performance | Valuation source, cashflow classification, calculation version. |
| Attribution | Segment classification, benchmark, residual method. |
| Risk | Risk model version, exposure model, market data inputs. |
| Mandate | Rule version, target allocation version, breach evaluation timestamp. |
| Disclosures | Disclosure template version, product risk labels. |

## 10. Data quality flags

Use standardized quality flags.

| Flag | Meaning |
|---|---|
| FINAL | Final approved value. |
| PROVISIONAL | Expected to be finalized later. |
| ESTIMATED | Modelled or estimated value. |
| STALE | Source value older than policy threshold. |
| MISSING | Required value unavailable. |
| OVERRIDDEN | Manual or system override applied. |
| NOT_APPLICABLE | Metric does not apply. |
| SUPPRESSED | Value intentionally hidden due to policy. |

Do not use blank values where a quality flag is needed.

## 11. Canonical reporting data objects

Recommended objects:

- `ReportRun`
- `ReportScope`
- `ReportSection`
- `ReportMetric`
- `ReportHoldingLine`
- `ReportTransactionLine`
- `ReportCashBalanceLine`
- `ReportIncomeLine`
- `ReportPerformanceLine`
- `ReportRiskLine`
- `ReportMandateLine`
- `ReportException`
- `ReportDisclosure`
- `ReportApproval`
- `ReportArchiveRecord`

## 12. API design principle

Avoid APIs that return only PDF bytes. Prefer:

- structured report dataset API;
- section-level APIs;
- render-to-PDF API;
- archive retrieval API;
- report status API;
- exception API;
- lineage API.

This enables UI, PDF, mobile, data export and QA to use the same data.
