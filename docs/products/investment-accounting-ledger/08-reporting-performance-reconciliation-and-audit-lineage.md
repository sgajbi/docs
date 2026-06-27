# 08 - Reporting, Performance, Reconciliation and Audit Lineage

## 1. Accounting as reporting foundation

Client reporting is only as reliable as the accounting foundation. Every report number should trace back to accounting objects.

| Report item | Required foundation |
|---|---|
| Holdings | Position book, valuation, FX |
| Cash | Cash ledger, unsettled cash, blocked cash |
| Income | Dividend/coupon/interest/distribution transactions and accruals |
| Fees | Fee transactions/accruals and billing policy |
| Realized P&L | Disposal transactions and cost lots |
| Unrealized P&L | Cost basis and valuation |
| Performance | Valuation time series and classified cashflows |
| Asset allocation | Instrument taxonomy and look-through |
| Mandate breaches | Holdings, exposures, limits, rule versions |
| Tax summary | Tax classification, withholding, lots, jurisdiction rules |

## 2. Accounting-to-performance mapping

Performance needs classified cashflows, not raw accounting transactions.

| Accounting event | Performance treatment |
|---|---|
| Client deposit | External inflow |
| Client withdrawal | External outflow |
| Security buy funded by portfolio cash | Internal investment activity |
| Security sale with proceeds retained | Internal investment activity |
| Dividend/coupon retained in portfolio | Investment income |
| Advisory/DPM fee paid from portfolio | Expense for net return; may be excluded for gross return |
| Tax withholding | Tax drag / income reduction depending on report view |
| Transfer between portfolios | External or internal depending consolidation boundary |
| Physical settlement into new security | Internal transformation |
| Corporate action split | Non-cash position adjustment |
| Write-off/default | Investment loss |

## 3. Income reporting

Income should be reported by both source and classification.

| Dimension | Examples |
|---|---|
| Product source | Equity dividend, bond coupon, fund distribution, deposit interest, loan interest expense |
| Tax view | Gross income, withholding tax, net income |
| Cash status | Accrued, receivable, paid, reversed |
| Recurrence | Recurring, special, one-off, liquidation distribution |
| Currency | Local, base, reporting |
| Performance inclusion | Income return, tax drag, excluded from gross view |

## 4. P&L reporting

P&L should be decomposed when possible.

| Component | Meaning |
|---|---|
| Realized price P&L | Gain/loss on disposals |
| Unrealized price P&L | Mark-to-market change on open positions |
| Income | Coupon/dividend/interest/distribution |
| FX P&L | Currency translation impact |
| Fees/taxes | Explicit costs and tax drag |
| Accrual/amortization | Fixed-income and fee accrual effects |
| Valuation adjustment | Model/override/stale estimate impact |

## 5. Reconciliation domains

| Reconciliation | Compares |
|---|---|
| Trade reconciliation | OMS/execution versus accounting trade book |
| Settlement reconciliation | Expected settlement versus custodian/core settlement |
| Cash reconciliation | Platform cash versus custodian/core cash |
| Position reconciliation | Platform holdings versus custodian/core holdings |
| Price reconciliation | Platform price versus vendor/custodian/source price |
| FX reconciliation | FX used by platform versus approved source |
| Income reconciliation | Expected entitlement versus paid income |
| Corporate action reconciliation | Expected CA effect versus custodian booking |
| Fee reconciliation | Fee engine versus cash ledger/invoice |
| Performance reconciliation | Return movement versus market value and cashflows |

## 6. Break classification

| Break type | Common root cause |
|---|---|
| Quantity break | Missing trade, settlement fail, corporate action, transfer |
| Cash break | Missing fee/tax/income, value-date mismatch, FX booking difference |
| Market value break | Price/FX/source mismatch, stale price, accrued-income treatment |
| Cost break | Lot history missing, cost method mismatch, corporate-action adjustment |
| Income break | Entitlement mismatch, withholding difference, payment reversal |
| P&L break | Price/cost/FX/accrual mismatch |
| Performance break | Cashflow classification or timing issue |
| Collateral break | Pledge/LTV/haircut source mismatch |

## 7. Audit lineage

Every report number should support drill-down.

Example lineage for unrealized P&L:

```text
Unrealized P&L
 -> position quantity
 -> open lots / cost basis
 -> current price and price source
 -> FX rate and source
 -> accrued income policy
 -> valuation rule version
 -> report calculation run
```

Example lineage for income:

```text
Income amount
 -> entitlement event
 -> security position on entitlement date
 -> rate/amount per share or coupon schedule
 -> tax withholding rule/source
 -> payment transaction
 -> cash ledger posting
 -> reporting classification
```

## 8. Restatement and backdated changes

Backdated transactions, corrected prices, late corporate actions and restated NAVs can change historical reports.

Recommended controls:

- store immutable original report run;
- store recalculated run with version;
- identify changed inputs;
- classify impact as material/non-material by policy;
- provide delta report for support/business sign-off;
- do not silently overwrite client-visible history if statements were issued;
- support period locks and authorized adjustments.

## 9. Production-support triage questions

When a client statement is wrong, ask:

1. Is the position quantity correct?
2. Is cash correct on trade-date or settlement-date basis?
3. Are all trades booked and settled correctly?
4. Are corporate actions applied?
5. Is price/FX correct and as-of date correct?
6. Is accrued income included/excluded consistently?
7. Is cost basis correct and lot method correct?
8. Are fees/taxes included in the right section?
9. Is the report using the correct portfolio/base currency/consolidation group?
10. Is this a source issue, mapping issue, calculation issue, or reporting-label issue?

## 10. Client explanation principle

Do not expose internal accounting jargon directly to clients. Translate carefully:

| Internal term | Client-friendly explanation |
|---|---|
| Unsettled receivable | Sale proceeds expected but not yet settled |
| Accrued income | Income earned but not yet paid |
| Dirty price | Bond value including accrued interest |
| Stale price | Last available valuation, not current market quote |
| Reversal | Correction of a previous booking |
| Corporate action adjustment | Change due to company event such as split/merger/dividend |
| Cost-basis adjustment | Update to original cost due to event or tax/accounting rule |
