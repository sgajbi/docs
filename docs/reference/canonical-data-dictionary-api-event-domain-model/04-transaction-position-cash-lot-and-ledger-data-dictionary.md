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

## 7. Generic transaction type examples

Use generic transaction types and carry product context through `instrument_id`, `product_family`, `transaction_subtype`, lifecycle event and transaction legs. Avoid creating a different top-level transaction type for every product family when the economic action is the same.

| Economic action | Generic transaction types | Product context examples |
|---|---|---|
| Acquire or dispose | BUY, SELL, SUBSCRIBE, REDEEM | Bond buy, equity sell, fund subscription, structured note redemption |
| Cash and FX | CASH_DEPOSIT, CASH_WITHDRAWAL, CASH_SWEEP, FX_CONVERSION, FX_SETTLEMENT | Cash transfer, liquidity sweep, FX spot, FX forward settlement |
| Income and expense | INCOME, INTEREST, COUPON, DIVIDEND, DISTRIBUTION, FEE, TAX, TAX_RECLAIM | Bond coupon, equity dividend, fund distribution, withholding tax |
| Servicing and maturity | MATURITY, REDEMPTION, CORPORATE_ACTION, SPLIT_OR_CONSOLIDATION, RIGHTS_EXERCISE | Bond maturity, partial redemption, stock split, rights exercise |
| Derivatives and collateral | DERIVATIVE_PREMIUM, DERIVATIVE_MARGIN, DERIVATIVE_SETTLEMENT, OPTION_EXERCISE, COLLATERAL_PLEDGE, COLLATERAL_RELEASE | Option premium, futures variation margin, NDF settlement, pledged securities |
| Lending and liabilities | LOAN_DRAWDOWN, LOAN_REPAYMENT, LOAN_INTEREST, MARGIN_CALL | Lombard drawdown, margin loan repayment, monthly loan interest |
| Private markets and insurance | COMMITMENT, CAPITAL_CALL, RECALLABLE_DISTRIBUTION, INSURANCE_PREMIUM, INSURANCE_SURRENDER, INSURANCE_CLAIM | Private fund commitment, capital call, policy premium, partial surrender |
| Corrections and controls | REVERSAL, CORRECTION, CANCEL, RESTATEMENT_ADJUSTMENT, WRITE_DOWN, WRITE_OFF | Reversed dividend, corrected trade price, restated NAV, impaired private debt |

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
