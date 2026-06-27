# 01 - Domain Model Catalog and Design Principles

## 1. Why canonical domain models matter

A wealth platform becomes fragile when every application defines its own terms for client, account, portfolio, instrument, transaction, position, valuation, performance and report output.

Canonical domain models create:

- consistent vocabulary
- easier integrations
- reusable APIs
- reliable reporting
- cleaner audit lineage
- better QA automation
- safer AI grounding
- simpler migration and reconciliation
- clearer ownership

## 2. Canonical domains

| Domain | Purpose |
|---|---|
| Party / Client | person, company, trust, family office, beneficial owner |
| Relationship | CIF/BR, household, advisor assignment, relationship hierarchy |
| Account | legal/accounting container at custodian or booking centre |
| Portfolio | investment view or reporting/mandate grouping |
| Instrument | security, product, cash, derivative, loan, policy |
| Product governance | APU, risk rating, target market, complexity, disclosures |
| Market data | prices, FX, curves, benchmarks, reference data |
| Transaction | economic posting or lifecycle-driven activity |
| Position | current holding or exposure derived from transactions |
| Cash balance | currency balance, settled/pending, restrictions |
| Lot | cost-basis and tax/accounting lot |
| Valuation | price, FX, market value, accrued income, fair value level |
| Performance | returns, contribution, attribution, cashflow treatment |
| Risk | exposure, concentration, VaR, stress, liquidity |
| Benchmark | benchmark composition, returns, relative analytics |
| Mandate | DPM/advisory rules, restrictions, target allocation |
| Workflow | tasks, approvals, cases, status transitions |
| Reporting | snapshots, sections, templates, archive, disclosures |
| AI context | prompts, retrieval sources, outputs, evaluations |
| Audit evidence | lineage, approval, calculation version, source version |

## 3. Design principles

| Principle | Meaning |
|---|---|
| Separate legal holding from economic exposure | A structured note is legal note, but risk may be equity/FX/credit |
| Separate lifecycle event from transaction | Observation may trigger future transaction but is not itself cash movement |
| Separate trade date and settlement date | both are needed for accounting, cash, risk and reporting |
| Separate source data and calculated data | preserve lineage |
| Keep product subtype extensible | do not hardcode one table per product |
| Use high precision internally | round only at display/report boundary |
| Version calculations and rules | performance/risk/report values must be reproducible |
| Use explicit currency fields | every amount must carry currency |
| Use status and effective dates | data is time-dependent |
| Use corrections/reversals | avoid destructive changes to financial history |
| Support degraded states | missing/stale data should be visible, not hidden |
| Make AI outputs traceable | every answer should know source and context |

## 4. Entity relationship overview

```text
Party
  `-- Relationship / CIF / BR
        `-- Account
              `-- Portfolio
                    |-- Positions
                    |-- Transactions
                    |-- Cash Balances
                    |-- Mandates
                    |-- Reports
                    `-- Analytics

Instrument
  |-- Product Terms
  |-- Market Data
  |-- Corporate Actions
  |-- Product Governance
  `-- Risk Classification

Transaction
  |-- Cash Legs
  |-- Security Legs
  |-- Fees / Taxes
  |-- Lots
  `-- Lifecycle Event Link
```

## 5. Canonical identifiers

| Identifier | Purpose |
|---|---|
| party_id | stable party/person/company/trust identifier |
| relationship_id | relationship/CIF/BR grouping |
| account_id | custodian/legal account |
| portfolio_id | investment/reporting portfolio |
| instrument_id | internal instrument master ID |
| product_id | product governance/catalog ID |
| transaction_id | economic transaction ID |
| transaction_group_id | group related legs |
| position_id | current position record |
| lot_id | cost/tax/accounting lot |
| valuation_id | valuation snapshot record |
| report_snapshot_id | reproducible report state |
| workflow_id | task/case/approval workflow |
| audit_event_id | evidence/audit event |
| ai_interaction_id | AI prompt/output trace |

## 6. Amount field pattern

Every amount should specify:

| Field | Example |
|---|---|
| amount | 100000.00 |
| currency | USD |
| amount_type | principal / income / fee / tax / market_value |
| fx_rate_to_base | 1.3500 |
| base_currency | SGD |
| base_amount | 135000.00 |
| valuation_date | 2026-06-27 |
| source | price_vendor_x |
| precision_policy | decimal |

## 7. Time field pattern

Use separate time concepts:

| Field | Meaning |
|---|---|
| trade_date | execution date |
| settlement_date | legal/value settlement date |
| booking_date | when recorded in system |
| effective_date | business-effective date |
| valuation_date | date of valuation |
| as_of_timestamp | snapshot timestamp |
| created_at | system creation time |
| updated_at | system update time |
| source_timestamp | timestamp from source system |

## 8. Status pattern

Use explicit status:

| Entity | Example status |
|---|---|
| transaction | pending, booked, settled, cancelled, reversed |
| position | active, closed, suspended, restricted |
| lifecycle event | observed, triggered, confirmed, cancelled |
| workflow | draft, pending approval, approved, rejected, completed |
| report | draft, generated, approved, delivered, archived, restated |
| AI output | draft, reviewed, approved, rejected, expired |

## 9. Key takeaway

Canonical models are the foundation for reliable wealth APIs, events, analytics, reporting, AI and controls.
