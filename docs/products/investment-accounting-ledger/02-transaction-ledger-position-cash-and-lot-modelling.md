# 02 - Transaction, Ledger, Position, Cash and Lot Modelling

## 1. Canonical accounting objects

A wealth platform should separate the objects below even if some source systems provide them in a single feed.

| Object | Purpose |
|---|---|
| Event | Something happened: trade executed, coupon paid, dividend announced, fee charged |
| Transaction | Economic booking that affects holdings, cash, income, expense or P&L |
| Transaction leg | A component of the transaction: security leg, cash leg, fee leg, tax leg, accrued leg |
| Ledger entry | Debit/credit-style accounting posting or sub-ledger movement |
| Position | Current quantity/nominal/units held for an instrument |
| Cash balance | Currency-level cash amount by account and value date |
| Lot | Acquisition layer used for cost basis and realized P&L |
| Accrual | Earned but not yet paid income or incurred but not yet paid expense |
| Valuation | Market/fair/NAV value at a time point |
| Reconciliation item | Difference between source and platform state |

## 2. Transaction header model

| Field | Meaning |
|---|---|
| transaction_id | Platform unique transaction ID |
| source_transaction_id | ID from custodian/core/OMS/corporate-action source |
| transaction_group_id | Groups related legs and related transactions |
| event_id | Link to business event or lifecycle event |
| account_id | Portfolio/custody/account identifier |
| booking_entity | Legal entity / booking centre |
| transaction_type | BUY, SELL, COUPON, DIVIDEND, FEE, REDEMPTION, TRANSFER, etc. |
| product_family | Equity, bond, fund, note, derivative, loan, cash, private market, etc. |
| trade_date | Execution/economic date |
| settlement_date | Contractual settlement date |
| value_date | Cash value date |
| booking_date | Date/time booked in platform |
| accounting_date | Accounting period date |
| status | Pending, booked, settled, failed, reversed, corrected |
| reversal_of_transaction_id | Link if reversal/cancel/correct |
| idempotency_key | Prevent duplicate ingestion |
| source_system | Custodian, OMS, core banking, CA feed, fee engine, valuation vendor |
| rule_version | Classification / posting rule version |
| narrative | Client/reporting-friendly explanation |

## 3. Transaction leg model

Use transaction legs to avoid overloaded transaction types.

| Field | Meaning |
|---|---|
| leg_id | Unique leg ID |
| transaction_id | Parent transaction |
| leg_type | SECURITY, CASH, FEE, TAX, ACCRUED_INCOME, FX, PNL, COLLATERAL |
| instrument_id | Security/instrument if applicable |
| currency | Cash or valuation currency |
| quantity_delta | Unit movement |
| nominal_delta | Face/nominal movement |
| amount | Leg amount in leg currency |
| amount_sign | Debit/credit or positive/negative convention |
| price | Execution or valuation price |
| fx_rate_to_base | FX rate used for base conversion |
| base_amount | Amount in portfolio base currency |
| reporting_amount | Amount in reporting currency |
| cost_basis_impact | Increase, decrease, no impact, allocate |
| pnl_impact | Realized, unrealized, none, income, expense |
| performance_classification | External cashflow, income, fee, internal trade, transfer, tax, correction |
| tax_classification | Dividend, interest, capital gain, withholding, non-taxable, unknown |
| settlement_status | Pending, settled, failed, partially settled |

## 4. Example: equity buy transaction

Client buys 100 shares at USD 50, commission USD 10.

| Leg | Type | Quantity | Cash | Meaning |
|---|---|---:|---:|---|
| 1 | SECURITY | +100 shares | 0 | Increase equity position |
| 2 | CASH | 0 | -5,000 | Gross purchase cash |
| 3 | FEE | 0 | -10 | Commission/expense |

Depending on policy, cost basis may be USD 5,000 or USD 5,010. The platform should store the configured policy explicitly.

## 5. Example: bond buy with accrued interest

Client buys bond nominal USD 100,000 at 98.50 clean price and pays accrued interest USD 800.

| Leg | Type | Amount | Meaning |
|---|---:|---:|---|
| Security leg | SECURITY | +100,000 nominal | Position increases |
| Principal cash leg | CASH | -98,500 | Clean price consideration |
| Accrued interest leg | ACCRUED_INCOME | -800 | Buyer compensates seller for accrued coupon |
| Fee leg | FEE | -X | Commission if applicable |

This distinction matters because the next coupon payment should not be treated as entirely earned during the holding period if the buyer paid accrued interest on purchase.

## 6. Position model

| Field | Meaning |
|---|---|
| account_id | Portfolio/account |
| instrument_id | Security/instrument |
| quantity | Shares/units/contracts |
| nominal | Face amount for bonds/notes/loans |
| current_notional | Derivative or loan exposure |
| average_cost | Cost per unit / per nominal |
| total_cost | Cost basis in local and base currency |
| market_price | Latest price/NAV/fair value input |
| accrued_income | Accrued coupon/interest/distribution where applicable |
| market_value_clean | Excluding accrued where applicable |
| market_value_dirty | Including accrued where applicable |
| unrealized_pnl | Market value minus cost basis |
| position_status | Active, pending settlement, closed, suspended, impaired |
| pledge_status | Free, pledged, blocked, collateral, restricted |
| valuation_quality | Official, estimated, stale, missing, overridden |

## 7. Cash balance model

Cash is not just one balance.

| Balance type | Meaning |
|---|---|
| Ledger cash | Posted cash balance |
| Available cash | Cash usable for trading/withdrawal after holds and settlement rules |
| Settled cash | Cash settled by value date |
| Unsettled receivable | Expected cash from unsettled sales/redemptions |
| Unsettled payable | Expected cash needed for unsettled buys/fees/taxes |
| Accrued interest receivable | Earned but unpaid interest |
| Blocked cash | Pledged, reserved, restricted or pending |
| Overdraft / loan balance | Negative cash supported by credit line or margin |

Buying power should use the correct cash basis, not necessarily statement cash.

## 8. Tax-lot / cost-lot model

Lots support realized P&L, tax reporting, cost-basis display and performance explanations.

| Field | Meaning |
|---|---|
| lot_id | Unique lot |
| opening_transaction_id | Acquisition event |
| account_id | Account |
| instrument_id | Security |
| acquisition_date | Trade/effective date |
| settlement_date | Settlement date |
| original_quantity | Quantity acquired |
| remaining_quantity | Quantity remaining |
| original_cost_local | Original cost in instrument currency |
| adjusted_cost_local | Cost after corporate actions, amortization, return of capital |
| cost_base_currency | Base cost |
| fx_rate_at_acquisition | FX basis |
| cost_method | FIFO, LIFO, average cost, specific ID, tax jurisdiction method |
| wash_sale_or_tax_adjustment | Jurisdiction-specific if applicable |
| source_quality | Custodian-provided, platform-calculated, converted, estimated |

## 9. Ledger entry model

Ledger entries can be explicit double-entry postings or simplified sub-ledger movements. In either case, they should be auditable.

| Field | Meaning |
|---|---|
| ledger_entry_id | Unique ledger posting |
| transaction_id | Parent transaction |
| account_code | Cash, investments, income, expense, receivable, payable, realized P&L |
| debit_credit | Debit or credit |
| amount | Amount in ledger currency |
| currency | Ledger currency |
| base_amount | Reporting/base equivalent |
| posting_date | Accounting posting date |
| reversal_flag | True/false |
| period_id | Accounting/reporting period |
| source_rule | Posting rule that produced entry |

## 10. Derived balance rule

Where possible:

```text
Opening balance + sum(transaction legs) = Closing balance
```

If the platform receives source-provided balances, it should reconcile rather than blindly overwrite. Exceptions should identify whether the difference is due to late trade, missing corporate action, missing FX, stale valuation, booking-date mismatch, rounding or source correction.
