# 05 - Data Flow, Source Systems, Records of Truth, Snapshots and Lineage

## 1. Why source ownership matters

Wealth platforms often fail not because they lack data, but because they cannot answer:

- Which system owns this field?
- Which value is official?
- Why does the report show a different number than the screen?
- Is the price stale or final?
- Did this transaction come from custodian, core banking or a correction file?
- Which version was used for the client statement?

Source ownership and lineage must be explicit architecture concerns.

## 2. Source system categories

| Source category | Examples of data |
|---|---|
| Client master | Client identity, CIF/BR, household, KYC, advisor, segment, risk profile. |
| Account master | Account, portfolio, mandate, booking centre, service model, restrictions. |
| Product master | Instrument terms, product type, issuer, identifiers, risk attributes. |
| Market data | Prices, FX, rates, curves, benchmarks, index constituents. |
| Core banking | Cash balances, loans, deposits, credit lines, limits, collateral exposure. |
| OMS/EMS | Orders, executions, allocations, trade state. |
| Custodian | Settled positions, settlement, corporate actions, income, custody statements. |
| Fund administrator | Fund NAV, subscriptions, redemptions, distributions, liquidity notices. |
| Structured product issuer | Terms, observations, barrier events, autocall, coupons, redemption amounts. |
| Private market GP/admin | Capital calls, distributions, NAV, commitments, unfunded amounts. |
| Insurance provider | Policy value, cash value, surrender value, premium, rider, claims. |
| Reporting archive | Official statement snapshots, generated documents, template version. |

## 3. Record of truth vs golden copy

| Concept | Meaning |
|---|---|
| Source of record | System legally or operationally responsible for the data. |
| Golden copy | Curated, normalized, validated copy used across the platform. |
| Derived truth | Result produced from source/golden data, e.g. positions or performance. |
| Reporting snapshot | Frozen version used for a client-facing output. |

A golden copy is not necessarily the legal source of record. It is the platform-approved representation after source prioritization, validation and enrichment.

## 4. Data flow pattern

```text
Source data
  -> Intake
  -> Raw landing / immutable capture
  -> Validation
  -> Normalization
  -> Enrichment
  -> Golden copy / curated domain store
  -> Derived calculations
  -> Snapshots
  -> APIs / events / reports
```

Each step should preserve lineage.

## 5. Raw, curated and derived layers

| Layer | Purpose |
|---|---|
| Raw | Preserve what arrived, when, from whom, with file/event/API metadata. |
| Normalized | Convert format, identifiers, date/currency conventions, field names. |
| Curated | Apply source priority, data-quality rules, enrichment and business validations. |
| Derived | Positions, balances, valuations, performance, risk, reports and dashboards. |
| Snapshot | Frozen, versioned state for official reporting or audit. |

## 6. Snapshot types

| Snapshot | Purpose |
|---|---|
| EOD position snapshot | Holdings as of business close. |
| Trade-date snapshot | Includes pending/unsettled transactions. |
| Settlement-date snapshot | Official settled view. |
| Valuation snapshot | Market values using price/FX source hierarchy. |
| Reporting snapshot | Frozen report input set and outputs. |
| Performance snapshot | Time series and cashflows used for calculation. |
| Risk snapshot | Exposures, sensitivities and risk factors used for risk run. |
| Migration snapshot | Source-vs-target comparison at cutover/parallel run. |

## 7. Lineage model

Every curated and derived record should be traceable to:

- source system
- source file/API/event
- source record ID
- business date
- processing timestamp
- transformation version
- validation result
- enrichment rule set
- manual override, if any
- calculation version, if derived
- report snapshot version, if used in reporting

Example lineage object:

```json
{
  "sourceSystem": "custodian-x",
  "sourceType": "FILE",
  "sourceFileId": "positions-20260630-001",
  "sourceRecordId": "row-88281",
  "businessDate": "2026-06-30",
  "ingestedAt": "2026-07-01T01:05:00Z",
  "normalizationVersion": "instrument-map-4.2",
  "validationRuleSet": "position-dq-1.8",
  "curationDecision": "ACCEPTED",
  "curatedRecordId": "pos-cur-123"
}
```

## 8. Source priority and survivorship

When multiple sources supply the same data, define survivorship rules.

Example price priority:

| Priority | Source | Use |
|---|---|---|
| 1 | Exchange close | Listed liquid instruments |
| 2 | Independent pricing vendor | Bonds, OTC, less-liquid instruments |
| 3 | Issuer quote | Structured products |
| 4 | Fund administrator NAV | Funds/private vehicles |
| 5 | Manual approved override | Controlled exception only |

Survivorship should be configurable by product family, market, booking centre and report type.

## 9. Degraded-data states

Not all missing data should block all outputs. Use controlled states.

| State | Meaning | Example action |
|---|---|---|
| OK | Fit for use | Publish normally. |
| WARNING | Minor issue with explanation | Show data-quality flag. |
| DEGRADED | Usable but incomplete/estimated | Display caveat, restrict official reporting if required. |
| BLOCKED | Not fit for output | Block report/trade/control result. |
| PROVISIONAL | Expected to change | Allow dashboard, block official statement. |
| STALE | Too old for policy | Use fallback or block depending on product/report. |

## 10. Reporting snapshot lineage

A report should store:

- report request ID
- report type
- portfolio/account scope
- as-of date
- valuation date
- data cut-off time
- product taxonomy version
- price source hierarchy version
- FX rate set version
- performance calculation version
- risk calculation version
- template version
- disclosure version
- generation timestamp
- user/system requester
- rendered document checksum

Without this, explaining historical statements becomes difficult.

## 11. Data-quality checkpoints

| Checkpoint | Control examples |
|---|---|
| Intake | File count, record count, schema validation, duplicate detection. |
| Normalization | Identifier mapping, currency code validity, date validity. |
| Curation | Source priority, required fields, stale values, negative quantity checks. |
| Calculation | Input completeness, price/FX availability, cashflow classification. |
| Reporting | Section completeness, exception display, disclosure inclusion. |
| Distribution | Archive checksum, entitlement check, delivery status. |

## 12. Manual overrides

Manual overrides may be necessary, but they must be controlled.

Override record should include:

- original value
- override value
- reason code
- evidence attachment/reference
- requester
- approver
- effective date range
- impacted outputs
- expiry/review date
- audit timestamp

Never allow silent overwrites in source-owned data.

## 13. Source ownership matrix example

| Data item | Owner | Consumers | Critical controls |
|---|---|---|---|
| Client risk profile | Client/KYC/suitability system | Advisory, suitability, reporting | Effective date, approval, expiry. |
| Product risk rating | Product governance | Advisory, APU, reporting | Approval, review cycle, suspension. |
| Trade execution | OMS/EMS | Transactions, positions, reporting | Trade ID, allocation, amendment. |
| Settled position | Custodian/core | Position service, reconciliation | Quantity, settlement date, custodian account. |
| Price | Market data | Valuation, risk, reporting | Stale check, source priority, override. |
| Benchmark level | Benchmark service | Performance, relative reporting | Calendar, currency, restatement. |
| Report output | Reporting service | Client portal, archive | Snapshot version, checksum, entitlement. |

## 14. Architecture design rule

For every data domain, define:

```text
Owner
Source of truth
Inbound contract
Validation rules
Curated schema
Derived outputs
Quality flags
Reconciliation controls
APIs/events exposed
Reporting impact
```

This should be mandatory documentation for every material wealth capability.
