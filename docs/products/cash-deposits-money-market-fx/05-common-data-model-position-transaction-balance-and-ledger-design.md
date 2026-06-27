# 05 — Common Data Model, Position, Transaction, Balance and Ledger Design

## 1. Design goals

The goal is to support a common platform model across:

- cash balances;
- deposits;
- money market instruments;
- money market funds;
- FX spot;
- FX forwards/swaps/NDFs;
- settlement cash projections;
- advisory and mandate controls;
- portfolio analytics and reporting.

The model should be:

- accounting-grade;
- value-date aware;
- multi-currency aware;
- lifecycle-aware;
- suitable for buying power and settlement checks;
- aligned with performance and reporting;
- extensible for structured deposits, margin, collateral and DPM overlays.

## 2. Core entities

```text
Party / Client / CIF
  └── Portfolio / Account
        ├── CashAccount
        │     └── CashBalance by currency/date/balance-type
        ├── DepositContract
        ├── InstrumentPosition
        │     ├── MoneyMarketInstrument
        │     └── MoneyMarketFund
        ├── FXContract
        ├── TransactionGroup
        │     └── TransactionLeg[]
        ├── LedgerEntry[]
        ├── BalanceProjection[]
        ├── Valuation[]
        └── AnalyticsSnapshot[]
```

## 3. Instrument master model

| Field | Cash | Deposit | T-bill/CP/CD security | MMF | FX forward/NDF |
|---|---|---|---|---|---|
| instrument_id | Optional cash currency pseudo instrument | Deposit product/contract | Security ID | Fund/share class ID | Contract ID |
| product_type | CASH | DEPOSIT | MONEY_MARKET_INSTRUMENT | MONEY_MARKET_FUND | FX_DERIVATIVE |
| currency | Cash currency | Deposit currency | Issue currency | Share-class currency | Buy/sell currencies |
| issuer/counterparty | Bank/custodian | Bank | Issuer | Fund manager | Counterparty |
| maturity_date | No | Yes | Yes | Usually no | Value date |
| price source | Par/FX rate | Contract/accrual | Market/evaluated | NAV | MTM source |
| income type | Interest | Interest | Discount/coupon | Distribution/NAV | MTM/settlement P&L |

## 4. Cash account model

| Field | Description |
|---|---|
| cash_account_id | Unique cash account |
| portfolio_id | Owning portfolio/account |
| custodian_account_id | External custodian/account reference |
| currency | Currency of balance |
| account_type | Settlement, operating, margin, collateral, income, tax |
| interest_bearing_flag | Yes/no |
| overdraft_allowed | Yes/no |
| overdraft_limit | Limit if allowed |
| debit_block | Whether debits are blocked |
| credit_block | Whether credits are blocked |
| pledge_status | Not pledged / partially pledged / fully pledged |
| status | Active / closed / restricted |

## 5. Balance model

Use a separate balance snapshot table rather than storing one balance in the account row.

| Field | Description |
|---|---|
| balance_id | Unique ID |
| cash_account_id | Cash account |
| portfolio_id | Portfolio/account |
| currency | Balance currency |
| as_of_date | Snapshot date/time |
| balance_type | LEDGER / SETTLED / AVAILABLE / BLOCKED / RESERVED / PROJECTED |
| amount | Amount in currency |
| source | Custodian / internal calculation / rules engine |
| value_date | For projected balances |
| calculation_run_id | Traceability |

## 6. Ledger entry model

Ledger entries represent accounting cash movements.

| Field | Description |
|---|---|
| ledger_entry_id | Unique ID |
| transaction_id | Source transaction |
| cash_account_id | Cash account |
| currency | Cash currency |
| amount | Signed cash amount |
| booking_date | Accounting booking date |
| value_date | Settlement/value date |
| trade_date | Trade date where relevant |
| entry_type | Principal / interest / fee / tax / settlement / FX / collateral |
| reversal_of_entry_id | Reversal link |
| source_system | Custodian, order system, deposit system, FX system |
| reconciliation_status | Matched / unmatched / adjusted |

## 7. Transaction group model

Many cash/FX events have multiple legs. Use a transaction group to keep them together.

| Field | Description |
|---|---|
| transaction_group_id | Groups all related legs |
| event_type | FX_SPOT, DEPOSIT_MATURITY, MMF_SUBSCRIPTION, etc. |
| portfolio_id | Portfolio/account |
| trade_date | Trade date |
| value_date | Primary value date |
| source_reference | External reference |
| status | Pending / booked / settled / cancelled / reversed |

## 8. Transaction leg model

| Field | Description |
|---|---|
| transaction_id | Unique transaction/leg ID |
| transaction_group_id | Grouping ID |
| transaction_type | Specific type |
| instrument_id | Product/security/contract/cash pseudo-instrument |
| quantity_delta | Units if fund/security |
| nominal_delta | Face amount/principal if deposit/security |
| cash_amount | Signed cash amount |
| cash_currency | Currency of cash movement |
| price | NAV, price %, FX rate, deposit rate if relevant |
| fees | Fees/charges |
| tax | Tax withholding |
| accrued_income | Accrued interest/income component |
| realized_pnl | Realised gain/loss if disposal/settlement |
| performance_classification | External cashflow / income / internal investment / fee / FX |

## 9. Product-specific position models

### Cash balance position

Cash position can be represented as balance snapshots and ledger entries. If the platform requires a common position interface, use a currency pseudo-instrument.

| Field | Example |
|---|---|
| instrument_id | CASH-USD |
| quantity | 100,000 |
| market_value | USD 100,000 |
| valuation_currency | USD |

### Deposit position

| Field | Example |
|---|---|
| principal_amount | 100,000 |
| accrued_interest | 1,200 |
| market_value | 101,200 or principal+accrual depending on policy |
| maturity_date | 2026-12-31 |
| rate | 3.00% |

### Money market instrument position

| Field | Example |
|---|---|
| nominal_amount | 100,000 |
| price_pct | 99.25 |
| accrued_or_accreted_income | 500 |
| market_value | 99,250 plus accrual policy |
| maturity_date | 2026-09-30 |

### Money market fund position

| Field | Example |
|---|---|
| units | 98,000.234 |
| nav | 1.0001 |
| market_value | 98,010.03 |
| nav_date | 2026-06-26 |

### FX forward/NDF position

| Field | Example |
|---|---|
| notional_currency | USD |
| notional_amount | 1,000,000 |
| contracted_rate | 1.3500 |
| value_date | 2026-09-30 |
| mtm_value | SGD 12,000 |
| status | Open |

## 10. Canonical transaction taxonomy

| Category | Transaction types |
|---|---|
| Cash | CASH_DEPOSIT, CASH_WITHDRAWAL, CASH_TRANSFER_IN, CASH_TRANSFER_OUT, CASH_INTEREST_PAYMENT, CASH_FEE, CASH_TAX, CASH_BLOCK, CASH_UNBLOCK |
| Deposits | DEPOSIT_PLACEMENT, DEPOSIT_INTEREST_ACCRUAL, DEPOSIT_INTEREST_PAYMENT, DEPOSIT_MATURITY_REDEMPTION, DEPOSIT_EARLY_BREAK_REDEMPTION, DEPOSIT_ROLLOVER_IN, DEPOSIT_ROLLOVER_OUT |
| Money market securities | MM_INSTRUMENT_BUY, MM_INSTRUMENT_SELL, MM_DISCOUNT_ACCRETION, MM_MATURITY_REDEMPTION |
| Money market funds | MMF_SUBSCRIPTION, MMF_REDEMPTION, MMF_DISTRIBUTION, MMF_DISTRIBUTION_REINVESTMENT, MMF_LIQUIDITY_FEE |
| FX spot | FX_SPOT_BUY_LEG, FX_SPOT_SELL_LEG |
| FX forwards/swaps/NDF | FX_FORWARD_OPEN, FX_FORWARD_CLOSE, FX_FORWARD_SETTLEMENT_BUY_LEG, FX_FORWARD_SETTLEMENT_SELL_LEG, FX_SWAP_NEAR_LEG, FX_SWAP_FAR_LEG, FX_NDF_OPEN, FX_NDF_CASH_SETTLEMENT |
| Collateral/margin | COLLATERAL_POST, COLLATERAL_RETURN, MARGIN_CALL, MARGIN_RELEASE |
| Corrections | REVERSAL, CANCEL_REBOOK, ADJUSTMENT |

## 11. Availability/reservation model

Availability is not a ledger entry. It is a rule-based calculation.

| Reservation type | Example |
|---|---|
| Pending order reserve | Buy order awaiting settlement |
| Withdrawal reserve | Client withdrawal instruction |
| Fee reserve | Advisory/custody fee due |
| Tax reserve | Expected withholding/tax debit |
| Margin reserve | Derivative margin requirement |
| Pledge reserve | Cash pledged to credit line |
| Capital-call reserve | Expected private-market capital call |
| Sweep reserve | Minimum liquidity retained before cash sweep |

## 12. Cash projection model

Cash projection should be event-driven.

| Projection source | Example |
|---|---|
| Settled cash | Opening balance |
| Pending securities buys/sells | Settlement cash movements |
| Coupons/dividends | Expected income |
| Deposit maturities | Principal and interest inflows |
| Fund redemptions/subscriptions | Cash settlement by dealing date |
| FX forwards | Future buy/sell currency legs |
| Capital calls/distributions | Private-market expected cashflows |
| Fees/taxes | Expected debits |
| Withdrawals | Client payment instructions |

## 13. Cash projection example

| Date | USD projected movement | Reason | Projected USD balance |
|---|---:|---|---:|
| Today | 500,000 | Opening settled cash | 500,000 |
| T+1 | -150,000 | Equity buy settlement | 350,000 |
| T+2 | +20,000 | Bond coupon | 370,000 |
| T+5 | -100,000 | Fund subscription | 270,000 |
| Month-end | +250,000 | Deposit maturity | 520,000 |

## 14. Idempotency and event sourcing

Cash systems are highly sensitive to duplicate bookings. Use:

- source transaction ID;
- external reference;
- transaction group ID;
- idempotency key;
- version number;
- reversal link;
- booking timestamp;
- event status;
- reconciliation status.

## 15. Integration sources

| Data | Typical source |
|---|---|
| Cash balances | Custodian/core banking |
| Cash transactions | Custodian/core banking/general ledger |
| Deposit contracts | Deposit platform/core banking |
| Deposit rates | Treasury/rate service/product master |
| MM instrument master | Security master/market data vendor |
| MMF NAV | Fund accounting/vendor/TA |
| FX spot rates | Market data vendor |
| FX forward points | Market data/treasury |
| FX trades | Order management/FX platform/custodian |
| Collateral/margin | Margin/collateral engine |
| Buying power | Rules/calculation engine |

## 16. API design considerations

Useful APIs:

| API | Purpose |
|---|---|
| `GET /portfolios/{id}/cash-balances` | Current cash by currency and balance type |
| `GET /portfolios/{id}/cash-projections` | Projected cash by value date |
| `GET /portfolios/{id}/liquidity` | Liquidity buckets and restrictions |
| `GET /portfolios/{id}/fx-exposure` | Gross/net currency exposure |
| `GET /portfolios/{id}/income-projection` | Interest, coupons, dividends, maturities |
| `POST /orders/cash-availability-check` | Pre-trade funding check |
| `GET /deposits/{id}` | Deposit contract and lifecycle details |
| `GET /fx-contracts/{id}` | FX forward/swap/NDF details |

## 17. Common design anti-patterns

| Anti-pattern | Why it fails |
|---|---|
| One cash field on portfolio | Cannot support currencies, value dates, restrictions or projections |
| Treating MMF as cash balance | Hides fund/NAV/liquidity risk |
| Modelling FX as one net amount | Loses two-leg settlement and currency exposure |
| Updating cash without transaction audit | Breaks reconciliation and explainability |
| Ignoring value date | Creates settlement failures |
| No transaction group | Cannot connect FX/deposit/rollover legs |
| No reversal model | Corrections become destructive updates |
| No performance classification | TWR/MWR/income reports become wrong |

## 18. Design principle

```text
Position tells what is held.
Ledger tells what moved.
Balance tells what is available or projected.
Contract tells what should happen.
Valuation tells what it is worth.
Classification tells how it affects performance and reporting.
```
