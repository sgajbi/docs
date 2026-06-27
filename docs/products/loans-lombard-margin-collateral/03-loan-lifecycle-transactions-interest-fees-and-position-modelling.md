# 03 — Loan Lifecycle, Transactions, Interest, Fees and Position Modelling

## 1. Loan lifecycle overview

A lending product lifecycle typically flows as follows:

```text
Origination
  → Credit approval
  → Facility setup
  → Collateral pledge setup
  → Drawdown / utilization
  → Interest accrual
  → Interest payment / capitalization
  → Principal repayment / redraw
  → Collateral monitoring
  → Margin call / shortfall management if needed
  → Renewal / repricing / restructuring
  → Facility closure and collateral release
```

Each stage should be represented either as product master data, facility state, lifecycle event or accounting transaction.

## 2. Facility event versus transaction

Not every lifecycle event is a transaction.

| Event | Transaction? | Reason |
|---|---|---|
| Credit line approved | No | Contract state, no cash movement |
| Collateral pledged | Usually no | Legal/eligibility state, not economic sale |
| Facility activated | No | Facility state |
| Drawdown | Yes | Client receives loan cash / liability increases |
| Interest accrual | Sometimes | Accounting accrual, may be daily/monthly |
| Interest payment | Yes | Cash movement / liability reduction |
| Margin call issued | No | Notification/control event |
| Forced liquidation sale | Yes | Collateral sold |
| Loan repayment | Yes | Cash out / liability reduces |
| Facility expired | No | Facility status |
| Collateral released | Usually no | Pledge state changes |

## 3. Recommended transaction types

| Transaction type | Purpose |
|---|---|
| LOAN_DRAWDOWN | New borrowing utilization |
| LOAN_PRINCIPAL_REPAYMENT | Repayment of principal |
| LOAN_INTEREST_ACCRUAL | Accrued interest accounting entry |
| LOAN_INTEREST_PAYMENT | Interest paid from cash |
| LOAN_INTEREST_CAPITALIZATION | Interest added to principal |
| LOAN_FEE_CHARGE | Arrangement/commitment/utilization fee |
| LOAN_FEE_PAYMENT | Fee settled in cash |
| LOAN_LIMIT_INCREASE | Usually facility event, not accounting transaction |
| LOAN_LIMIT_DECREASE | Facility event; may require repayment if over limit |
| LOAN_REPRICE | Change rate/spread/reset; event not cash movement |
| LOAN_ROLLOVER | Rollover maturity/term loan; may be event or transaction group |
| LOAN_RESTRUCTURE | Change facility terms; event and possibly transactions |
| LOAN_WRITE_OFF | Bank writes off exposure / client liability treatment |
| LOAN_TRANSFER_IN | Liability transferred into platform/custodian |
| LOAN_TRANSFER_OUT | Liability transferred out |
| COLLATERAL_PLEDGE | Lifecycle/security event, usually not value transaction |
| COLLATERAL_RELEASE | Lifecycle/security event |
| COLLATERAL_SUBSTITUTION | Release one asset and pledge another |
| FORCED_COLLATERAL_SALE | Sale of pledged assets due to shortfall |
| MARGIN_CALL_NOTICE | Event, not transaction |
| MARGIN_CALL_CURE_CASH | Client deposits cash to cure |
| MARGIN_CALL_CURE_SECURITY | Client pledges additional securities |

## 4. Loan position model

A drawn loan should be modelled as a liability position.

| Field | Meaning |
|---|---|
| loan_position_id | Unique loan/drawdown position |
| credit_line_id | Parent facility |
| borrower_id | Client/CIF/booking entity |
| account_id | Account where liability is booked |
| loan_currency | Borrowing currency |
| principal_outstanding | Current unpaid principal |
| accrued_interest | Interest accrued but unpaid |
| fees_outstanding | Fees charged but unpaid |
| total_liability | Principal + accrued interest + fees |
| rate_type | Fixed/floating |
| base_rate | SOFR/SORA/Euribor/etc. |
| spread | Client spread |
| all_in_rate | Base + spread, after floors/caps |
| day_count | ACT/360, ACT/365, 30/360 |
| interest_reset_frequency | Daily/monthly/quarterly |
| next_reset_date | Floating-rate reset |
| next_payment_date | Interest payment date |
| maturity_date | Drawdown maturity |
| repayment_type | Bullet, amortizing, revolving |
| collateral_pool_id | Supporting collateral |
| status | Active, matured, defaulted, repaid |

## 5. Facility position versus drawdown position

Some systems model only drawdowns. Better wealth platforms model both:

| Object | What it represents |
|---|---|
| Credit facility | Approved line, terms, limit, collateral pool, controls |
| Loan drawdown | Actual utilized liability |
| Cash overdraft | Negative cash balance, may be linked to facility |
| Exposure summary | Aggregated liability and contingent exposure |

Example:

```text
Facility: USD 1,000,000 revolving Lombard line
Drawdown 1: USD 300,000, 1M SOFR + 1.25%, matures 2026-08-01
Drawdown 2: EUR 100,000, 1M Euribor + 1.50%, matures 2026-07-15
```

The facility is one object. The drawdowns are separate liability positions.

## 6. Drawdown transaction example

Client draws USD 250,000.

| Leg | Account | Amount | Direction |
|---|---|---:|---|
| Cash leg | Client cash account | +250,000 | Inflow |
| Loan liability leg | Loan position | +250,000 | Liability increase |

Transaction:

```json
{
  "transactionType": "LOAN_DRAWDOWN",
  "creditLineId": "CL-1001",
  "loanPositionId": "LN-5001",
  "tradeDate": "2026-06-27",
  "valueDate": "2026-06-27",
  "currency": "USD",
  "principalDelta": "250000.00",
  "cashDelta": "250000.00"
}
```

## 7. Principal repayment example

Client repays USD 50,000 from portfolio cash.

| Leg | Account | Amount | Direction |
|---|---|---:|---|
| Cash leg | Client cash account | -50,000 | Outflow |
| Loan liability leg | Loan position | -50,000 | Liability reduction |

Transaction type: `LOAN_PRINCIPAL_REPAYMENT`.

## 8. Interest accrual

Daily accrual example:

```text
Daily Interest = Principal Outstanding × Annual Rate × Accrual Days / Day Count Basis
```

Example:

| Item | Value |
|---|---:|
| Principal | USD 500,000 |
| Annual rate | 6.00% |
| Day count | ACT/360 |
| Days | 1 |
| Daily interest | 500,000 × 6% × 1 / 360 = 83.33 |

## 9. Interest payment versus capitalization

Interest can be:

| Treatment | Meaning |
|---|---|
| Paid in cash | Cash decreases, accrued interest decreases |
| Capitalized | Accrued interest becomes additional principal |
| Deducted from collateral/cash | Operationally collected from account |
| Past due | Accrued but unpaid, may trigger default or control event |

Example capitalization:

| Before | Amount |
|---|---:|
| Principal | 500,000 |
| Accrued interest | 5,000 |

After capitalization:

| After | Amount |
|---|---:|
| Principal | 505,000 |
| Accrued interest | 0 |

## 10. Fees

Common lending fees:

| Fee | Meaning |
|---|---|
| Arrangement fee | One-time setup fee |
| Commitment fee | Fee on undrawn committed facility |
| Utilization fee | Fee based on drawn amount |
| Rollover fee | Fee when term is extended |
| Late payment fee | Fee on overdue payment |
| Margin-call fee | Operational/admin fee in some products |
| Valuation fee | Property or collateral valuation cost |
| Legal/documentation fee | Legal pledge/charge setup |

Fee modelling should distinguish:

- fee charged
- fee accrued
- fee paid
- fee waived
- fee capitalized

## 11. Amortizing versus bullet loan

| Loan style | Principal repayment |
|---|---|
| Bullet | Full principal at maturity |
| Amortizing | Principal repaid periodically |
| Revolving | Client can redraw within limit |
| Overdraft | Balance fluctuates with cash activity |

Amortization schedule example:

| Date | Opening principal | Principal repayment | Interest | Closing principal |
|---|---:|---:|---:|---:|
| Month 1 | 500,000 | 10,000 | 2,500 | 490,000 |
| Month 2 | 490,000 | 10,000 | 2,450 | 480,000 |

## 12. Overdraft modelling

An overdraft can be modelled as:

1. negative cash balance; or
2. explicit loan position linked to cash account.

For wealth platforms, explicit liability modelling is usually better for reporting and risk.

Example:

```text
Cash Account Balance = -USD 25,000
Overdraft Exposure = USD 25,000
Interest Accrues on Debit Balance
```

## 13. Multi-currency loan treatment

Clients may borrow in a currency different from portfolio base currency.

For each loan:

```text
Loan Value in Portfolio Base = Loan Principal × FX Rate
```

FX P&L on the liability matters. If the client borrows USD and portfolio base is SGD, a stronger USD increases SGD liability.

## 14. P&L and performance treatment

From portfolio perspective:

| Item | Treatment |
|---|---|
| Loan drawdown cash received | External financing inflow / liability creation, not investment gain |
| Principal repayment | Liability reduction, not investment loss |
| Interest paid | Financing cost / expense |
| Fee paid | Expense |
| FX movement on loan | Liability FX P&L / financing effect |
| Collateral price change | Investment P&L on pledged asset |
| Forced sale | Realized P&L on collateral asset |

For performance reporting, decide whether to show:

- gross asset performance before financing
- net-of-financing performance
- leveraged portfolio performance
- client net worth return including liabilities

## 15. Lifecycle status model

| Status | Meaning |
|---|---|
| Proposed | Facility requested |
| Approved | Credit approved but not active |
| Active | Facility available |
| Suspended | No new drawdowns/trades allowed |
| In margin call | Shortfall exists |
| Defaulted | Breach/default state |
| Expired | Facility maturity passed |
| Closed | Facility and exposures closed |

## 16. Event model

Recommended lifecycle event types:

| Event type | Example |
|---|---|
| FACILITY_APPROVED | Credit line approved |
| FACILITY_ACTIVATED | Facility live |
| COLLATERAL_PLEDGED | Asset pledged |
| DRAWDOWN_REQUESTED | Drawdown initiated |
| DRAWDOWN_SETTLED | Cash disbursed |
| RATE_RESET | Floating rate reset |
| INTEREST_CALCULATED | Interest accrual run |
| INTEREST_DUE | Interest payment due |
| MARGIN_CALL_TRIGGERED | Shortfall threshold breached |
| MARGIN_CALL_CURED | Cash/security added or exposure reduced |
| COLLATERAL_SUBSTITUTED | Collateral swapped |
| FACILITY_RENEWED | Tenor extended |
| FACILITY_CLOSED | Facility closed |

## 17. Transaction grouping

Some lifecycle actions create multiple transactions.

Example: margin call cured by selling equity and repaying loan.

| Transaction | Effect |
|---|---|
| EQUITY_SELL | Asset sold, cash increases |
| LOAN_PRINCIPAL_REPAYMENT | Cash decreases, loan decreases |
| FEE_PAYMENT | Cash decreases |
| COLLATERAL_RELEASE | If excess collateral released |

Use `transaction_group_id` to preserve auditability.
