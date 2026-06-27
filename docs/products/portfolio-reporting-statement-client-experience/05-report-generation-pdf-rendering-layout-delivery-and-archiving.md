# 05 - Report Generation, PDF Rendering, Layout, Delivery and Archiving

## 1. Report production lifecycle

A robust report-production process should follow a controlled lifecycle.

```text
Request / Schedule
  -> Scope resolution
  -> Data cut selection
  -> Calculation run
  -> Reporting dataset build
  -> Data quality validation
  -> Template rendering
  -> Business/control approval
  -> Client/advisor delivery
  -> Archive and audit evidence
```

## 2. Batch versus on-demand reports

| Mode | Use case | Design considerations |
|---|---|---|
| Batch | Monthly/quarterly statements, DPM reporting. | Scalability, retries, scheduling, cut-off, production dashboard. |
| On-demand | Advisor portfolio review, client portal request. | Latency, latest available data, clear provisional flags. |
| Event-driven | Maturity report, corporate-action notice, margin call. | Trigger source, approval workflow, notification controls. |
| Regulatory/tax batch | Annual tax statements, CRS/FATCA, fee disclosure. | Immutable evidence, jurisdiction, submission deadlines. |

## 3. Report scope resolution

A report must define the scope before data extraction.

Scope dimensions:

- client;
- household/family group;
- legal entity;
- trust/foundation;
- account;
- sub-account;
- booking centre;
- portfolio/mandate;
- advisor/RM;
- reporting currency;
- reporting period;
- asset inclusion/exclusion policy.

Incorrect scope is one of the most serious reporting defects.

## 4. Data cut selection

A data cut defines what inputs are used.

Examples:

| Cut type | Description |
|---|---|
| EOD final | Final end-of-day curated data. |
| EOD preliminary | Early estimate before all prices/NAVs arrive. |
| Intraday | Current but incomplete data. |
| Month-end official | Controlled month-end reporting cut. |
| Restatement cut | Corrected historical cut. |

Reports should not silently mix cut types.

## 5. Rendering architecture

A modern reporting architecture separates content and rendering.

```text
Report Dataset JSON
  -> Layout Template
  -> Render Engine
  -> PDF/HTML/Excel/Portal View
```

Benefits:

- easier QA;
- same data across UI and PDF;
- controlled template versioning;
- better multilingual support;
- easier archive and replay;
- smaller blast radius for layout changes.

## 6. PDF design principles

| Principle | Explanation |
|---|---|
| Consistent hierarchy | Use predictable section order and headings. |
| Client-friendly labels | Avoid internal system codes. |
| Numbers align | Amounts, dates and percentages should be easy to scan. |
| Unit clarity | Always show currency, %, units, nominal, valuation date. |
| Footnote discipline | Use footnotes for important but secondary information. |
| Exception visibility | Show warnings where data quality impacts interpretation. |
| Mobile/portal parity | PDF and digital view should not contradict each other. |

## 7. Layout patterns

Recommended report layout:

1. Cover page / header.
2. Executive summary.
3. Portfolio valuation and allocation.
4. Performance and benchmark review.
5. Risk and exposure review.
6. Income and cashflow review.
7. Holdings detail.
8. Transactions/activity detail.
9. Product-specific pages.
10. Exceptions and notes.
11. Methodology and disclosures.
12. Appendix.

## 8. Pagination and large holdings

Large portfolios require careful design.

Controls:

- stable sort order;
- grouping by asset class/product/currency;
- subtotal consistency;
- continuation labels;
- page-level totals where useful;
- no orphaned section headers;
- deterministic row splitting;
- export option for full detail;
- suppression rules for small/zero balances.

## 9. Multilingual and localization support

Reporting platforms should separate:

- metric code;
- business definition;
- display label;
- language translation;
- formatting convention;
- jurisdiction-specific disclosure;
- client segment variant.

Avoid hardcoding labels inside calculation services.

## 10. Delivery channels

| Channel | Controls |
|---|---|
| Client portal | Authentication, entitlement, download audit. |
| Email | Encryption, approval, attachment controls, bounce handling. |
| RM/advisor portal | Advisor entitlement and client-view simulation. |
| Print/mail | Address validation and production controls. |
| API/export | Data entitlement, masking, rate limits. |
| Archive retrieval | Immutable copy and access audit. |

## 11. Archive requirements

The archive should store:

- rendered PDF/HTML;
- structured dataset;
- report run metadata;
- approval metadata;
- template version;
- disclosure version;
- data cut ID;
- exception summary;
- delivery evidence;
- supersession/correction links.

## 12. Report production controls

Examples:

| Control | Purpose |
|---|---|
| Completeness control | All expected clients/accounts processed. |
| Data quality control | Missing/stale/estimated data threshold. |
| Reconciliation control | Totals tie to book of record. |
| Performance tolerance | Return calculations within expected tolerance. |
| Disclosure control | Required disclosures included. |
| Template control | Correct template used for client segment/jurisdiction. |
| Approval control | High-risk reports reviewed before release. |
| Delivery control | Report delivered only to entitled recipient. |
| Archive control | Published report stored immutably. |

## 13. Failure and retry design

Report generation should support:

- idempotent generation requests;
- resumable section generation;
- retry for transient dependencies;
- partial failure status;
- exception dashboard;
- manual re-run with reason code;
- no duplicate delivery on retry;
- audit of all attempts.

## 14. Template governance

Every template should have:

- owner;
- version;
- effective date;
- applicable client segment;
- applicable jurisdiction/booking centre;
- supported languages;
- required sections;
- optional sections;
- disclosure mapping;
- approval history;
- regression-test evidence.

## 15. Rendering QA checklist

Before releasing a reporting template:

- validate with small, medium and very large portfolios;
- validate all supported products;
- validate negative values and missing values;
- validate multi-currency portfolios;
- validate long product names;
- validate non-English labels if supported;
- validate page breaks;
- validate footnotes/disclosures;
- validate print and digital outputs;
- validate archive retrieval and replay.
