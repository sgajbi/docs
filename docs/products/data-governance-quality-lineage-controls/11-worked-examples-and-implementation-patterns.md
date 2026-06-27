# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns data governance, data quality, lineage, reconciliation and operating-control concepts into practical examples for implementation, QA, operations, migration and production support.

## Example 1. Critical data element ownership

### Scenario

A product risk rating is used by advisory suitability, mandate controls, reporting and product governance.

| Dimension | Required control |
|---|---|
| Business definition | What the rating means and how it is used. |
| Source owner | Team accountable for the authoritative value. |
| Technical source | System/table/API/feed that publishes the value. |
| Quality rule | Allowed values, freshness and completeness. |
| Consumers | Suitability, DPM, reporting, order checks and monitoring. |
| Degraded behavior | Warning, block, suppress or fallback when value is missing. |

### QA assertions

| Test | Expected result |
|---|---|
| Product risk rating is blank | Suitability cannot silently pass. |
| Rating value is outside allowed vocabulary | Data-quality rule fails. |
| Source owner changes rating | Downstream lineage captures source, timestamp and version. |
| Historical report is regenerated | Effective-dated rating is used. |

## Example 2. Price quality scorecard

### Scenario

Daily valuation requires price quality controls across equities, bonds, funds and structured products.

| Rule | Example threshold | Severity |
|---|---|---|
| Completeness | 100% of reportable holdings need valuation basis. | Critical |
| Freshness | Listed securities priced as of report date. | High |
| Stale private NAV | Must show NAV date and lag. | Medium |
| Outlier move | Price move above configured threshold. | High |
| Source priority | Approved source used by product family. | High |

### Scorecard view

| Metric | Result |
|---|---:|
| Holdings with valid price | 98.7% |
| Holdings with stale price | 1.1% |
| Holdings with missing price | 0.2% |
| High-severity exceptions open | 4 |

### QA assertions

| Test | Expected result |
|---|---|
| Missing price belongs to material holding | Report publication may block. |
| Outlier coincides with stock split | Exception links to corporate-action workflow. |
| Stale NAV is expected | Degraded-state disclosure is generated. |
| Scorecard improves after manual override | Override lineage and expiry are visible. |

## Example 3. Position reconciliation break

### Scenario

The internal position system shows 10,000 shares; the custodian shows 9,000 shares.

| Possible cause | Evidence |
|---|---|
| Pending settlement | Open trade not yet settled. |
| Corporate action | Split, merger or transfer not processed consistently. |
| Duplicate booking | Same trade ingested twice. |
| Mapping issue | Instrument identifier mismatch. |
| Timing issue | Trade-date basis versus settlement-date basis. |

### Break workflow

```text

detect -> classify -> assign owner -> investigate -> correct or explain -> certify closure -> regression test

```

### QA assertions

| Test | Expected result |
|---|---|
| Break is explained by timing | Break is classified, not blindly corrected. |
| Break is due to duplicate transaction | Duplicate is reversed with lineage. |
| Break remains unresolved past SLA | Escalation is triggered. |
| Same break pattern recurs | Regression test and root-cause action are required. |

## Example 4. Lineage replay for a report number

### Scenario

A client asks why portfolio market value changed after month-end.

### Required lineage

| Layer | Evidence |
|---|---|
| Report cell | Report ID, section, row, value and version. |
| Reporting dataset | Snapshot ID and calculation version. |
| Position | Quantity, basis and source. |
| Price | Source, value, timestamp, stale/override status. |
| FX | Rate, source, pair direction and timestamp. |
| Adjustments | Corporate actions, corrections, restatements. |

### QA assertions

| Test | Expected result |
|---|---|
| Report value cannot trace to source records | Lineage QA fails. |
| Price changed due to correction | Report explains price version change. |
| FX rate changed after report date | Original snapshot still uses original FX version. |
| Manual override was used | Override reason, approver and expiry are visible. |

## Example 5. Degraded data state for advisory

### Scenario

An advisor wants to recommend a product, but product complexity is missing.

| Data state | Decision behavior |
|---|---|
| Complexity present and simple | Normal suitability workflow. |
| Complexity present and complex | Knowledge, experience and disclosure checks required. |
| Complexity missing | Unable to evaluate; block or escalate. |

### QA assertions

| Test | Expected result |
|---|---|
| Complexity is missing | Recommendation cannot silently proceed as simple. |
| Fallback value is used | Fallback source and reason are visible. |
| Data is corrected later | Decision replay can show original unable-to-evaluate result. |
| Advisor override is allowed | Approval and reason are captured. |

## Example 6. Migration parallel-run control

### Scenario

A legacy platform is migrated to a new portfolio platform. Both run in parallel for one month.

| Reconciliation area | Required evidence |
|---|---|
| Positions | Quantity, nominal, lots and custody basis. |
| Cash | Currency balances, pending cash and restrictions. |
| Market value | Price, FX, accrued interest and valuation basis. |
| Transactions | Type mapping, signs, dates, fees and taxes. |
| Performance | TWR/MWR continuity and cashflow classification. |
| Reporting | Statement line items, labels and disclosures. |

### QA assertions

| Test | Expected result |
|---|---|
| Totals match but product classification differs | Migration is not signed off until classification is resolved or accepted. |
| P&L differs due to cost-basis method | Difference is documented with policy decision. |
| Corporate-action history is incomplete | A known limitation and reporting impact are recorded. |
| Cutover occurs | Certification package includes reconciliation, exceptions and approvals. |

## Example 7. Control evidence package

### Scenario

A month-end reporting cycle needs governance evidence.

| Evidence | Why it matters |
|---|---|
| Data-quality scorecard | Shows quality state before publication. |
| Reconciliation certification | Confirms breaks are resolved or accepted. |
| Snapshot manifest | Proves exact data cut used. |
| Override register | Shows manual changes and approvals. |
| Report run metadata | Supports replay and audit. |
| Exception register | Shows material data issues and decisions. |

### QA assertions

| Test | Expected result |
|---|---|
| Report is published without certification | Control QA fails. |
| Override has no expiry | Override control fails. |
| Exception is accepted | Acceptance has owner, reason, expiry and impact. |
| Audit asks for evidence | Package can be produced without manual reconstruction. |
