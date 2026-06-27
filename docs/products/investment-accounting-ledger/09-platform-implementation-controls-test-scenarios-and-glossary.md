# 09 - Platform Implementation, Controls, Test Scenarios and Glossary

## 1. Reference architecture

A scalable investment-accounting architecture should separate ingestion, normalization, accounting rules, position building, valuation and reporting.

```text
Source feeds
  -> event intake and validation
  -> transaction normalization
  -> accounting posting engine
  -> cash/position/lot/accrual sub-ledgers
  -> valuation engine
  -> reconciliation engine
  -> analytics/performance/reporting services
  -> audit and lineage store
```

## 2. Service responsibilities

| Service/module | Responsibility |
|---|---|
| Source intake | Receive trades, cash, positions, income, CA, fees, prices and FX |
| Event normalizer | Map source events to canonical events |
| Posting engine | Generate transaction legs and ledger postings |
| Position builder | Maintain position time series by basis |
| Cash ledger | Maintain multi-currency cash and value-date balances |
| Lot engine | Maintain cost lots and realized P&L |
| Accrual engine | Calculate income/expense accruals |
| Valuation engine | Apply prices, FX and valuation rules |
| Reconciliation engine | Compare source/platform outputs |
| Reporting mart | Curate client/advisor/reporting views |
| Lineage service | Store source, rule, run and output traceability |

## 3. Idempotency and replay

Accounting ingestion must be idempotent.

Recommended keys:

```text
source_system + source_event_id + account_id + instrument_id + event_type + effective_date + source_version
```

Replay rules:

- replay should produce identical results for same input/rule versions;
- changed inputs should create a new event version;
- reversals should not delete previous bookings;
- recalculation should be deterministic;
- period locks should be respected.

## 4. Control framework

| Control | Purpose |
|---|---|
| Schema validation | Reject malformed source events |
| Reference data validation | Ensure instrument/account/currency exists |
| Sign validation | Prevent buy/sell/cash sign errors |
| Currency validation | Ensure trade, settlement, instrument and account currency rules are valid |
| Date validation | Trade/settlement/value/accounting date consistency |
| Price/FX validation | Missing/stale/outlier checks |
| Balance reconciliation | Opening + movements = closing |
| Source reconciliation | Compare with custodian/core/OMS |
| Maker-checker | Control manual overrides/corrections |
| Period lock | Prevent unauthorized closed-period changes |
| Lineage completeness | Ensure report numbers trace to sources |

## 5. Data-quality flags

| Flag | Meaning |
|---|---|
| SOURCE_ESTIMATED | Source value is estimated |
| PLATFORM_ESTIMATED | Platform derived estimate |
| STALE_PRICE | Price is older than threshold |
| MISSING_FX | FX unavailable |
| MANUAL_OVERRIDE | Manual price/transaction override used |
| COST_BASIS_LIMITED | Lot/cost history incomplete |
| NAV_LAGGED | NAV is lagged |
| UNSETTLED | Trade/cash not settled |
| RECON_BREAK | Reconciliation mismatch exists |
| REPORTING_LIMITED | Output should carry disclosure/caveat |

## 6. Regression test scenarios

| Scenario | Expected assertion |
|---|---|
| Equity buy and sell with fee | Position closes, cash reconciles, realized P&L correct |
| Bond buy with accrued interest | Accrued paid separated from cost; coupon income correct |
| Fund subscription with NAV lag | Units based on confirmed NAV; pending state until confirmed |
| Dividend with withholding | Gross income, tax and net cash reconcile |
| Stock split | Quantity doubles, total cost unchanged, no false P&L |
| Spin-off | Cost allocated between parent and child |
| Structured note autocall | Note closes, coupon/principal cash posted, lifecycle linked |
| Option expiry | Position closes, premium P&L recognized |
| Futures variation margin | Cash movement and realized P&L daily posting correct |
| Private market capital call | Commitment/unfunded/funded/NAV updated correctly |
| Loan interest accrual | Interest expense accrues then reverses on payment |
| Backdated trade | Historical positions/performance recalculated with lineage |
| Price correction after statement | Delta identified and period-lock policy applied |
| Multi-currency sale | Local and FX P&L split by policy |
| Transfer between accounts | External/internal classification depends consolidation boundary |

## 7. Migration checklist

Before migrating accounting data, profile:

- product taxonomy;
- open positions;
- cash balances by currency and value date;
- unsettled trades;
- open lots and cost basis;
- accrued income/expenses;
- pending corporate actions;
- fee accruals and billing cycles;
- prices and FX rates used in legacy statements;
- historical realized P&L if required;
- tax classifications;
- pledge/collateral state;
- reconciliation breaks and known limitations.

## 8. Release-gate evidence

A release should provide evidence for:

- transaction count reconciliation;
- cash and position reconciliation;
- market value reconciliation;
- income and fee reconciliation;
- P&L reconciliation;
- performance regression;
- cost basis regression;
- product lifecycle scenarios;
- negative tests and duplicate ingestion tests;
- lineage and audit trail tests;
- degraded-state reporting tests.

## 9. Glossary

| Term | Meaning |
|---|---|
| Book of record | Authoritative record for a specific business purpose |
| Sub-ledger | Detailed ledger supporting a domain such as investments, cash or fees |
| Transaction leg | Component of a transaction with a specific economic effect |
| Lot | Acquisition layer used for cost basis and realized P&L |
| Accrual | Earned/incurred amount not yet paid |
| Clean price | Bond price excluding accrued interest |
| Dirty price | Bond price including accrued interest |
| Realized P&L | Gain/loss from closed/disposed positions |
| Unrealized P&L | Gain/loss on open positions based on current valuation |
| Amortized cost | Cost adjusted over time for premium/discount and repayments |
| Fair value | Market-participant exit-value style measurement under applicable standards |
| Stale price | Price older than acceptable threshold |
| Reversal | Transaction that negates a previous booking |
| Restatement | Re-issuing or recalculating a prior period because inputs changed |
| Idempotency | Ability to process same input again without duplicate effect |

## 10. Interview-ready explanation

A strong investment-accounting platform models every event as explicit transaction legs that update cash, positions, lots, accruals, income, expenses and ledger records with full lineage. It must support trade-date and settlement-date views, multi-currency valuation, cost basis, realized/unrealized P&L, fees, taxes, accruals, corporate actions and product-specific lifecycle events. The platform should separate source events from accounting postings and derived balances, preserve corrections through reversals, reconcile to custody/core sources, and provide explainable outputs for performance, risk, advisory, DPM mandates and client reporting.
