# 04 - Transaction, Position, Cash, Lot and Ledger Data Dictionary

## 1. Transaction header

| Field | Type | Description |
|---|---|---|
| transaction_id | string | unique transaction ID |
| transaction_group_id | string | group for related legs |
| transaction_type | enum | buy, sell, coupon, dividend, fee, tax, redemption, etc. |
| transaction_status | enum | pending, booked, settled, cancelled, reversed |
| account_id | string | account |
| portfolio_id | string | portfolio |
| instrument_id | string | instrument |
| trade_date | date | trade date |
| settlement_date | date | settlement date |
| booking_date | date | booking date |
| effective_date | date | effective date |
| source_system | string | source |
| source_transaction_id | string | source ID |
| reversal_of_transaction_id | string | reversal link |
| lifecycle_event_id | string | lifecycle event link |
| created_at | datetime | created timestamp |
| updated_at | datetime | updated timestamp |

## 2. Transaction leg

| Field | Type | Description |
|---|---|---|
| leg_id | string | leg ID |
| transaction_id | string | parent transaction |
| leg_type | enum | cash, security, fee, tax, income, accrual, collateral |
| instrument_id | string | instrument if security leg |
| currency | ISO currency | currency |
| quantity_delta | decimal | unit/shares/contracts change |
| nominal_delta | decimal | nominal change |
| cash_delta | decimal | cash movement |
| amount | decimal | amount |
| amount_type | enum | principal, income, fee, tax, accrued, pnl |
| price | decimal | transaction price |
| price_basis | enum | per_unit, percent_of_par, nav |
| fx_rate_to_base | decimal | FX to portfolio base |
| base_amount | decimal | base currency amount |

## 3. Cash balance

| Field | Type | Description |
|---|---|---|
| cash_balance_id | string | balance record |
| account_id | string | account |
| portfolio_id | string | portfolio |
| currency | ISO currency | currency |
| settled_balance | decimal | settled cash |
| pending_receivable | decimal | pending incoming |
| pending_payable | decimal | pending outgoing |
| available_balance | decimal | available cash |
| restricted_balance | decimal | blocked/restricted |
| overdraft_limit | decimal | allowed overdraft |
| as_of_date | date | balance date |
| source_system | string | source |

## 4. Position

| Field | Type | Description |
|---|---|---|
| position_id | string | position ID |
| portfolio_id | string | portfolio |
| account_id | string | account |
| instrument_id | string | instrument |
| quantity | decimal | shares/units/contracts |
| nominal_amount | decimal | nominal amount for bonds/notes/loans |
| cost_basis | decimal | cost basis in local currency |
| cost_currency | ISO currency | cost currency |
| average_cost | decimal | average cost |
| accrued_income | decimal | accrued income |
| market_price | decimal | current price |
| market_value | decimal | local MV |
| base_market_value | decimal | base currency MV |
| unrealized_pnl | decimal | unrealized P&L |
| status | enum | active, closed, restricted, matured |
| valuation_date | date | valuation date |

## 5. Lot

| Field | Type | Description |
|---|---|---|
| lot_id | string | tax/accounting lot |
| position_id | string | position |
| transaction_id | string | originating transaction |
| acquisition_date | date | acquisition date |
| quantity_open | decimal | remaining quantity |
| quantity_original | decimal | original quantity |
| cost_amount | decimal | cost |
| cost_currency | ISO currency | cost currency |
| cost_method | enum | FIFO, average, specific_id |
| adjusted_cost | decimal | cost adjusted for corporate actions |
| status | enum | open, closed |

## 6. Ledger entry

| Field | Type | Description |
|---|---|---|
| ledger_entry_id | string | ledger entry |
| transaction_id | string | transaction |
| account_id | string | account |
| portfolio_id | string | portfolio |
| ledger_account | string | cash, investment, income, expense, receivable, payable |
| debit_credit | enum | debit, credit |
| amount | decimal | amount |
| currency | ISO currency | currency |
| base_amount | decimal | base amount |
| accounting_date | date | ledger date |
| posting_status | enum | posted, reversed, pending |
| policy_basis | string | accounting policy basis |

## 7. Transaction type examples

| Product | Transaction types |
|---|---|
| cash | CASH_DEPOSIT, CASH_WITHDRAWAL, FX_BUY, FX_SELL |
| bond | BOND_BUY, BOND_SELL, BOND_COUPON, BOND_MATURITY |
| equity | EQUITY_BUY, EQUITY_SELL, DIVIDEND_CASH, STOCK_SPLIT |
| fund | FUND_SUBSCRIPTION, FUND_REDEMPTION, FUND_DISTRIBUTION |
| note | NOTE_SUBSCRIPTION, NOTE_COUPON, NOTE_AUTOCALL, NOTE_REDEMPTION |
| derivative | OPTION_BUY, OPTION_EXERCISE, FUTURES_VARIATION_MARGIN |
| loan | LOAN_DRAWDOWN, LOAN_REPAYMENT, LOAN_INTEREST |
| private market | COMMITMENT_OPEN, CAPITAL_CALL, DISTRIBUTION |

## 8. Validation rules

- transaction IDs must be unique.
- financial corrections should use reversal/correction, not destructive edit.
- every cash movement needs currency.
- every position movement needs instrument.
- settled and pending cash must be distinguishable.
- closed positions should have zero quantity/nominal unless policy allows residual.
- lots must reconcile to position quantity.
- ledger entries must balance by transaction group.

## 9. Key takeaway

Transaction, position, cash, lot and ledger records are the accounting backbone of the wealth platform.
