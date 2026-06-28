# Source-Lineage Packet Case Studies

## Purpose

A source-lineage packet is the evidence bundle that explains where a product, transaction, price, cashflow, position, calculation, report value or supportability state came from.

Use this pattern when a portfolio platform must prove that a client-facing number is traceable from source event to transaction legs, position impact, calculation basis, report output and QA evidence.

This is especially important for:

- corporate actions,
- fund NAV restatements,
- bond calls and credit events,
- private-market capital calls and distributions,
- collateral and margin changes,
- report restatements,
- migration reconciliation,
- degraded or stale data states.

## Canonical Packet Shape

| Field | Description | Example |
|---|---|---|
| `packet_id` | Stable identifier for the evidence packet. | `SLP-2026-000184` |
| `business_event_id` | Source event or notice being represented. | `CA-ACME-2026-DIV-001` |
| `source_owner` | Accountable source or function. | Corporate actions operations |
| `source_record_type` | Type of source record. | Dividend election notice |
| `source_received_at` | When the platform received the source. | `2026-06-14T02:15:00Z` |
| `source_effective_date` | Business effective date. | `2026-06-20` |
| `source_version` | Source version or correction sequence. | `v2` |
| `instrument_id` | Canonical instrument identifier. | `EQ-ACME-US` |
| `portfolio_scope` | Portfolio, account, household or report scope. | Portfolio `P-10027` |
| `transaction_group_id` | Identifier linking product and cash legs. | `TG-CA-ACME-2026-DIV-001` |
| `valuation_inputs` | Prices, FX, NAV, curves or model inputs used. | USD cash amount and FX rate |
| `calculation_policy_version` | Policy used for amounts, accruals, tax or reporting. | `income-policy-2026.2` |
| `report_snapshot_id` | Report snapshot affected by the event. | `RPT-SNAP-2026-Q2-10027` |
| `supportability_state` | Current supportability posture. | `READY`, `STALE`, `DEGRADED`, `PARTIAL` |
| `qa_evidence` | Reconciliation, regression or sign-off evidence. | Event-to-ledger reconciliation passed |

## Case Study 1: Dividend Correction With Tax Restatement

### Business Event

A cash dividend for a listed equity is first received with an estimated withholding rate. The tax agent later provides a corrected withholding rate before the quarterly client statement is finalized.

### Source Timeline

| Step | Source Event | Source Version | Platform Treatment |
|---|---|---:|---|
| 1 | Dividend announcement received. | `v1` | Create pending entitlement estimate. |
| 2 | Ex-date position confirmed. | `v1` | Lock entitlement quantity. |
| 3 | Pay-date cash received with estimated tax. | `v1` | Book income, withholding and cash movement. |
| 4 | Corrected tax rate received. | `v2` | Reverse old tax leg, book replacement tax leg and mark report snapshot for review. |
| 5 | Statement snapshot generated. | `v2` | Report corrected net income and retain correction evidence. |

### Transaction Group

| Transaction Type | Product Side | Cash Side | Purpose |
|---|---|---|---|
| `INCOME_ACCRUAL` | Equity position earns dividend entitlement. | No cash movement yet. | Represents expected income before payment. |
| `INCOME_PAYMENT` | Reduces pending entitlement. | Credits gross dividend cash. | Books the actual dividend receipt. |
| `TAX_WITHHOLDING` | No position change. | Debits withholding tax. | Books tax withheld from the income event. |
| `REVERSAL` | Reverses original tax leg. | Credits old withholding amount. | Removes incorrect tax treatment. |
| `ADJUSTMENT` | No position change. | Debits corrected withholding amount. | Books replacement tax treatment. |
| `REPORT_RESTATEMENT_MARKER` | No economic movement. | No cash movement. | Marks affected report snapshot and client explanation. |

### Source-Lineage Packet

| Packet Area | Required Evidence |
|---|---|
| Source ownership | Corporate actions source owns dividend terms; tax operations owns withholding correction. |
| Instrument static | Equity identifier, country, listing currency, income classification and tax eligibility. |
| Position evidence | Settled position on ex-date, entitlement quantity and account eligibility. |
| Cash evidence | Gross dividend receipt, old withholding, corrected withholding and net cash. |
| Calculation evidence | Gross income, withholding percentage, net income and FX translation if reporting currency differs. |
| Reporting evidence | Statement line label, correction note, tax-income classification and restatement marker. |
| QA evidence | Entitlement quantity reconciliation, cash ledger reconciliation, tax delta check and report snapshot regeneration check. |

### Expected Reporting

| Report Section | Expected Treatment |
|---|---|
| Holdings | Equity position unchanged. |
| Transactions | Shows dividend and withholding entries with correction grouping if the report policy exposes corrections. |
| Income summary | Uses corrected net income. |
| Tax summary | Uses corrected withholding amount and retains source version. |
| Performance | Treats corrected withholding as income-related cashflow, not market performance. |
| Supportability | `READY` only after corrected source, transaction group and report snapshot reconcile. |

### QA Assertions

| Assertion | Pass Condition |
|---|---|
| No duplicate income | Gross dividend is recognized once. |
| Correction is linked | Old and replacement tax legs share one transaction group. |
| Cash is balanced | Net cash equals gross dividend minus corrected withholding. |
| Report uses latest source | Statement snapshot references `source_version = v2`. |
| Audit is explainable | Operator can trace from statement amount to source event, transaction legs and calculation policy. |

## Case Study 2: Fund NAV Restatement After Statement Draft

### Business Event

A fund administrator restates a month-end NAV after the client statement draft has already been generated. The platform must update valuation, performance, statement evidence and client explanation without changing transaction history.

### Source Timeline

| Step | Source Event | Source Version | Platform Treatment |
|---|---|---:|---|
| 1 | Month-end NAV received. | `v1` | Value fund position and generate draft statement. |
| 2 | Fund administrator issues corrected NAV. | `v2` | Store replacement price and mark dependent valuations stale. |
| 3 | Portfolio valuation is recalculated. | `v2` | Recompute market value, unrealized P&L and performance. |
| 4 | Statement is regenerated. | `v2` | Replace draft values and retain restatement evidence. |

### Transaction And Valuation Treatment

| Area | Treatment |
|---|---|
| Transactions | No trade transaction is created solely because NAV changes. |
| Position quantity | Units remain unchanged. |
| Market value | Recomputed using corrected NAV. |
| Unrealized P&L | Changes because valuation input changed. |
| Performance | Recomputed for affected periods using corrected valuation. |
| Reporting | Draft statement is superseded; final statement references corrected NAV source. |

### Source-Lineage Packet

| Packet Area | Required Evidence |
|---|---|
| Source ownership | Fund administrator owns NAV; pricing operations owns platform adoption and override approval if needed. |
| Instrument static | Fund identifier, share class, dealing currency, valuation frequency and NAV timing. |
| Position evidence | Units held at valuation date. |
| Price evidence | Old NAV, corrected NAV, received timestamp, effective date and approval status. |
| Calculation evidence | Market value before and after restatement, unrealized P&L delta and performance impact. |
| Reporting evidence | Draft statement supersession, final statement snapshot and client explanation if policy requires. |
| QA evidence | Price lineage check, valuation recalculation check, performance recomputation check and report regeneration check. |

### Supportability States

| State | When Used |
|---|---|
| `STALE` | Corrected NAV is known but dependent valuations have not been recomputed. |
| `PARTIAL` | Some portfolios are recalculated but batch completion is not proven. |
| `DEGRADED` | NAV is accepted under exception or override pending final administrator confirmation. |
| `READY` | Corrected NAV, valuation, performance and final report snapshot all reconcile. |

## Case Study 3: Private-Market Distribution Notice With Lagged Cash

### Business Event

A private equity fund sends a distribution notice before cash is received. The notice separates return of capital, realized gain and income. Cash arrives two days later with a small bank-fee deduction.

### Transaction Group

| Transaction Type | Product Side | Cash Side | Purpose |
|---|---|---|---|
| `DISTRIBUTION_NOTICE` | Reduces unfunded or invested-capital analytics where policy applies. | No cash movement. | Records expected distribution components. |
| `INCOME_PAYMENT` | No unit quantity change unless fund records units. | Credits income cash component. | Books income component. |
| `RETURN_OF_CAPITAL` | Reduces cost basis or invested capital. | Credits return-of-capital cash component. | Books capital return. |
| `REALIZED_GAIN` | Realizes gain component. | Credits realized gain cash component. | Books realized gain distribution. |
| `FEE` | No product position change. | Debits bank fee. | Explains cash difference between notice and receipt. |
| `CASH_SETTLEMENT` | Clears expected cash receivable. | Net cash received. | Links notice to cash settlement. |

### Source-Lineage Packet

| Packet Area | Required Evidence |
|---|---|
| Source ownership | Fund administrator owns distribution components; custody owns cash receipt; operations owns fee classification. |
| Commitment evidence | Original commitment, called capital, remaining commitment and fund ownership scope. |
| Notice evidence | Component amounts, effective date, payment date and currency. |
| Cash evidence | Cash receipt, fee deduction and reconciliation to net amount. |
| Calculation evidence | Cost-basis reduction, realized gain, income classification, IRR impact and reporting-currency conversion. |
| Reporting evidence | Activity classification, capital account statement alignment and client statement grouping. |
| QA evidence | Notice-to-cash reconciliation, component total check, fee reason-code check and performance classification check. |

## Implementation Pattern

Use one source-lineage packet per material business event, not one packet per technical row. A single packet can reference many transaction legs, valuation rows, report snapshots and QA artifacts.

```text
source event
-> source-lineage packet
-> transaction group
-> product legs and cash legs
-> position state
-> valuation and calculation inputs
-> report snapshot
-> supportability state
-> QA evidence
```

## Common Failure Modes

| Failure | Impact | Control |
|---|---|---|
| Source version is overwritten instead of versioned. | Report cannot explain why a value changed. | Store source version, effective date and adoption timestamp. |
| Product leg and cash leg are not linked. | Reconciliation and client explanation become manual. | Require transaction group identifiers. |
| NAV correction is booked as a transaction. | Activity history is polluted. | Treat valuation changes as price/valuation events, not trades. |
| Distribution components are collapsed into one cash receipt. | Tax, cost basis and performance classification become wrong. | Preserve component-level classification. |
| Report snapshot lacks source lineage. | Statement cannot be defended after correction. | Link report lines to source packet and calculation policy. |
| Degraded state is hidden. | Users over-trust incomplete data. | Expose `STALE`, `PARTIAL` or `DEGRADED` supportability where relevant. |

## Checklist

Before a product event is reportable, confirm:

1. source owner and source version are recorded,
2. instrument and portfolio scope are canonical,
3. product and cash legs share a transaction group,
4. position impact is explicit,
5. valuation input and calculation policy are identified,
6. report snapshot references the packet,
7. supportability state is justified,
8. reconciliation and regression evidence exist,
9. correction or restatement behavior is defined,
10. client-facing labels are consistent with product and tax classification.
