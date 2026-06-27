# 02 - Equity Trade Lifecycle, Transactions and Position Modelling

## 1. Why equity lifecycle matters

An equity trade looks simple:

```text
Buy 100 shares at USD 50
```

But a production wealth platform must process much more:

- order capture and execution;
- trade allocation;
- fees, commissions, tax and stamp duty;
- cash reservation and settlement;
- unsettled positions;
- custody movement;
- position lots and cost basis;
- realized/unrealized P&L;
- corporate action entitlement;
- performance classification;
- reconciliation with custodian and broker;
- reversal, correction and cancel/rebook flows.

For portfolio analytics and client reporting, the system must preserve both **economic meaning** and **accounting traceability**.

## 2. Trade lifecycle overview

| Stage | Business event | System object | Transaction? |
|---|---|---|---|
| Order intent | Advisor/client wants to trade | Order / proposal / instruction | No accounting transaction yet. |
| Pre-trade checks | Suitability, concentration, buying power, restricted list, market hours | Check results | No. |
| Execution | Order fills on market or OTC | Execution / fill | Usually not final accounting until trade booking. |
| Trade booking | Executed trade is captured | Trade transaction | Yes, creates pending security and cash effects. |
| Confirmation | Broker/custodian confirms trade economics | Confirmation status | No new transaction unless amendment. |
| Settlement | Securities and cash settle | Settlement update / cash security movement | May update status or generate settlement postings depending on system design. |
| Position keeping | Holding becomes settled position | Position / lot update | Derived from transactions. |
| Valuation | Price and FX update | Valuation record | No transaction. |
| Sale/disposal | Shares sold | Sell transaction | Yes. |
| Corporate actions | Issuer event affects holder | CA event + election + postings | Transactions only when economic postings occur. |

## 3. Dates used in equity processing

| Date | Meaning | Why it matters |
|---|---|---|
| Order date/time | When order was placed | Advisory audit and best execution. |
| Execution date/time | When order filled | Market price and trade evidence. |
| Trade date | Official trade date | Start of settlement cycle and performance effective date. |
| Settlement date | Cash/security settlement date | Cash availability, ownership settlement, failed-trade control. |
| Booking date | When system records transaction | Audit and operational reporting. |
| Value date | Effective date of cash/security movement | Cash ledger and interest. |
| Ex-date | Date from which buyer is not entitled to next distribution | Dividend/corporate-action entitlement. |
| Record date | Date issuer/custodian checks holders | Entitlement proof. |
| Pay date | Distribution payment date | Cash/security postings. |

## 4. Core transaction model

A robust transaction model should support security, cash, tax and fee legs.

### 4.1 Transaction header

| Field | Purpose |
|---|---|
| transaction_id | Unique accounting/event transaction identifier. |
| transaction_group_id | Groups linked legs, e.g., corporate-action redemption plus new shares. |
| account_id / portfolio_id | Client portfolio. |
| instrument_id | Equity/security instrument. |
| transaction_type | BUY, SELL, DIVIDEND, SPLIT, etc. |
| trade_date | Economic market date. |
| settlement_date | Expected/actual settlement date. |
| booking_date | Operational booking date. |
| value_date | Cash/security value date. |
| source_system | OMS, custodian, broker, corporate-action feed, manual adjustment. |
| lifecycle_event_id | Link to corporate action or other event, if applicable. |
| status | Pending, confirmed, settled, cancelled, reversed, failed. |
| reversal_of_transaction_id | Link to original transaction for corrections. |
| external_reference | Broker/custodian reference. |
| performance_classification | Investment activity, income, fee, tax, external cashflow, internal transfer. |

### 4.2 Security leg

| Field | Purpose |
|---|---|
| instrument_id | Security being moved. |
| quantity_delta | Positive for buy/receive, negative for sell/deliver. |
| settled_quantity_delta | Settled portion. |
| pending_quantity_delta | Unsettled portion. |
| price | Execution price. |
| price_currency | Listing/trade currency. |
| gross_trade_amount | Quantity x price. |
| lot_id | Position lot created or consumed. |
| cost_basis_amount | Cost basis impact. |
| realized_pnl | Realized gain/loss on disposal. |

### 4.3 Cash leg

| Field | Purpose |
|---|---|
| cash_currency | Cash movement currency. |
| gross_amount | Gross cash before fees/tax. |
| commission | Broker commission. |
| stamp_duty | Market tax/levy. |
| withholding_tax | Tax withheld from dividend/distribution. |
| exchange_fee | Exchange/clearing fee. |
| custody_fee | Custody/depositary fee. |
| fx_rate | FX rate to portfolio base currency. |
| net_amount | Final cash movement. |

## 5. Recommended equity transaction types

| Transaction type | Use |
|---|---|
| EQUITY_BUY | Purchase of shares. |
| EQUITY_SELL | Sale of shares. |
| EQUITY_TRANSFER_IN | Securities transferred into portfolio. |
| EQUITY_TRANSFER_OUT | Securities transferred out of portfolio. |
| EQUITY_SHORT_SELL | Short sale creating negative security position. |
| EQUITY_BUY_TO_COVER | Buy transaction that closes/reduces short position. |
| EQUITY_DIVIDEND_CASH | Cash dividend received. |
| EQUITY_DIVIDEND_STOCK | Stock dividend / bonus shares received. |
| EQUITY_DIVIDEND_REINVESTMENT | Dividend reinvested into additional shares. |
| EQUITY_WITHHOLDING_TAX | Tax withheld from dividend or corporate action. |
| EQUITY_FEE | Commission, custody, ADR, exchange or other fee. |
| EQUITY_STOCK_SPLIT | Share split increasing/decreasing quantity with cost adjustment. |
| EQUITY_REVERSE_SPLIT | Reverse split reducing quantity with cost adjustment. |
| EQUITY_RIGHTS_ENTITLEMENT | Rights received from corporate action. |
| EQUITY_RIGHTS_SUBSCRIPTION | Exercise rights to buy new shares. |
| EQUITY_RIGHTS_SALE | Sale of rights. |
| EQUITY_RIGHTS_EXPIRY | Rights expired unexercised. |
| EQUITY_SPINOFF_OUT | Allocation/reduction from original security, if required. |
| EQUITY_SPINOFF_IN | New security received from spin-off. |
| EQUITY_MERGER_OUT | Old security removed in merger/acquisition. |
| EQUITY_MERGER_IN | New security received in stock-for-stock merger. |
| EQUITY_TENDER_OUT | Shares tendered. |
| EQUITY_TENDER_CASH | Cash proceeds from tender. |
| EQUITY_RETURN_OF_CAPITAL | Capital returned to shareholder. |
| EQUITY_LIQUIDATION_PAYMENT | Cash/security received during liquidation. |
| EQUITY_WRITE_OFF | Position written off due to delisting, bankruptcy or custody confirmation. |
| EQUITY_ADR_FEE | ADR/depositary fee. |
| EQUITY_PROXY_VOTE | Non-accounting event; usually not a transaction unless fees apply. |
| EQUITY_CORRECTION_REVERSAL | Reversal/correction of prior transaction. |

Do not create transaction types for every data point. For example, a price update is a valuation record, not a transaction. A corporate action announcement is an event, not a transaction. Transactions should represent actual economic postings.

## 6. Buy transaction example

Client buys 1,000 shares at USD 50.00. Commission USD 20. Settlement T+1 or T+2 depending on market.

| Field | Value |
|---|---:|
| Quantity | +1,000 shares |
| Price | USD 50.00 |
| Gross amount | USD 50,000 |
| Commission | USD 20 |
| Net cash | -USD 50,020 |
| Cost basis | USD 50,020, depending on policy |

Transaction group:

| Leg | Instrument/cash | Amount |
|---|---|---:|
| Security leg | Equity ABC | +1,000 shares |
| Cash leg | USD cash | -50,000 |
| Fee leg | USD cash | -20 |

Position result:

| Position field | Value |
|---|---:|
| Quantity | 1,000 |
| Average cost | USD 50.02 |
| Cost amount | USD 50,020 |
| Unsettled quantity | 1,000 until settlement |
| Settled quantity | 1,000 after settlement |

## 7. Sell transaction example

Client sells 400 shares at USD 60.00. Commission USD 15. Original average cost USD 50.02.

| Field | Value |
|---|---:|
| Quantity sold | -400 shares |
| Gross proceeds | USD 24,000 |
| Commission | USD 15 |
| Net proceeds | USD 23,985 |
| Cost consumed | 400 x 50.02 = USD 20,008 |
| Realized P&L | 23,985 - 20,008 = USD 3,977 |

Position result:

| Field | Value |
|---|---:|
| Remaining quantity | 600 |
| Remaining cost | USD 30,012 |
| Realized P&L | USD 3,977 |

Cost basis may differ under FIFO, average cost, specific identification, tax-lot or local tax rules.

## 8. Position model

### 8.1 Core position fields

| Field | Meaning |
|---|---|
| account_id / portfolio_id | Client account or portfolio. |
| instrument_id | Equity instrument. |
| listing_id | Exchange/listing used for valuation/trading. |
| quantity | Total current quantity. |
| settled_quantity | Quantity that has settled. |
| unsettled_quantity | Quantity from pending trades. |
| blocked_quantity | Quantity blocked for settlement, collateral, pledge or corporate action election. |
| available_quantity | Quantity available for sale/transfer. |
| average_cost | Average book cost per share. |
| cost_amount | Total cost basis. |
| market_price | Latest valuation price. |
| market_value | Quantity x price x FX. |
| unrealized_pnl | Market value - remaining cost. |
| realized_pnl_ytd / period | Realized P&L over period. |
| income_ytd / period | Dividends and distributions. |
| currency | Trading/valuation currency. |
| base_currency_value | Value in portfolio base currency. |
| last_price_date | Price timestamp. |
| lifecycle_status | Active, suspended, delisted, restricted, bankrupt, written off. |

### 8.2 Position lot fields

Lot-level modelling is essential for tax, P&L and corporate actions.

| Field | Meaning |
|---|---|
| lot_id | Unique lot identifier. |
| acquisition_transaction_id | Buy/transfer/corporate-action source. |
| acquisition_date | Trade or settlement date based on policy. |
| original_quantity | Quantity originally acquired. |
| remaining_quantity | Quantity still held. |
| cost_amount | Cost assigned to lot. |
| cost_currency | Currency of cost basis. |
| adjusted_cost_amount | Cost after splits, returns of capital, spin-offs. |
| holding_period_start | Used for tax or reporting. |
| source_type | Trade, transfer, spin-off, dividend reinvestment, merger. |
| tax_attributes | Jurisdiction-specific tax metadata. |

## 9. Long and short positions

### 9.1 Long position

A long position means the client owns shares.

```text
Quantity > 0
Market value = Quantity x Price
Risk = price falls
```

### 9.2 Short position

A short position means the client has sold borrowed shares and must buy them back later.

```text
Quantity < 0
Market value often shown as negative exposure or liability
Risk = price rises
```

Short-sale platform needs:

| Capability | Reason |
|---|---|
| Borrow locate | Confirm shares can be borrowed. |
| Borrow fee | Ongoing cost. |
| Margin requirement | Short sales require collateral/margin. |
| Recall risk | Lender can recall shares. |
| Dividend-in-lieu | Short seller may owe equivalent dividend. |
| Regulatory constraints | Some markets restrict short selling. |

Many wealth platforms do not allow retail/private-bank clients to hold uncovered short equity positions directly, but they may appear in margin, hedge fund, DPM or professional client contexts.

## 10. Settled versus unsettled positions

A platform should distinguish:

| Quantity type | Meaning |
|---|---|
| Trade-date quantity | Position including executed but unsettled trades. |
| Settlement-date quantity | Position only after settlement. |
| Available-to-sell quantity | Settled quantity less blocked/pledged/pending-delivery quantity. |
| Custody quantity | Quantity confirmed by custodian/depository. |

Example:

Client holds 1,000 shares. Sells 300 shares today. Settlement is T+1.

| View | Quantity |
|---|---:|
| Trade-date position | 700 |
| Settlement-date/custody position until settlement | 1,000 |
| Quantity blocked for settlement | 300 |
| Available-to-sell | Depends on market and broker rules |

For portfolio performance, most wealth platforms use trade-date accounting for investment exposure, but custody reconciliation may be settlement-date based. This must be explicit.

## 11. Cost basis methods

| Method | Description | Common use |
|---|---|---|
| Average cost | One blended cost per holding. | Many portfolio reports and some tax regimes. |
| FIFO | First lots purchased are sold first. | Common accounting/tax method. |
| LIFO | Last lots purchased are sold first. | Less common, jurisdiction-dependent. |
| Specific identification | Client selects which lots are sold. | Tax optimization. |
| Weighted average by currency | Average cost maintained in local/security currency. | Multi-currency portfolios. |
| Tax-lot accounting | Lots maintain tax-specific attributes. | Tax reporting. |

For global wealth systems, the platform may need both:

```text
Book cost basis = used for portfolio reporting
Tax cost basis = jurisdiction-specific tax reporting
```

Do not assume they are the same.

## 12. Performance treatment

| Event | Performance treatment |
|---|---|
| Buy funded from existing portfolio cash | Internal investment transaction, not external flow. |
| Buy funded by new client cash contribution | External cash inflow followed by investment transaction. |
| Sell proceeds retained in portfolio cash | Internal investment transaction. |
| Sell proceeds withdrawn by client | Internal sale plus external cash outflow. |
| Cash dividend retained in portfolio | Investment income. |
| Dividend withdrawn by client | Income recognition plus external cash outflow. |
| Stock split | No economic return by itself; quantity and cost per share adjust. |
| Bonus issue / stock dividend | Usually no external flow; treatment depends on local accounting/tax. |
| Rights received | Entitlement value may be investment result; exercise creates new cost. |
| Spin-off | Internal transformation of value from old security to new security. |
| Merger cash consideration | Disposal; realized P&L may be recognized. |
| Merger stock consideration | Security conversion; no external cash if fully stock-for-stock. |
| Withholding tax | Tax expense reducing net return. |

For TWR, external cashflows are client-driven contributions/withdrawals. Equity trading, dividends, corporate actions and valuation changes are investment activity unless cash or assets enter/leave the portfolio from outside.

## 13. Transaction correction and reversal

Production systems must support:

| Scenario | Recommended approach |
|---|---|
| Wrong price | Reverse and rebook, or amend with audit trail. |
| Wrong quantity | Reverse and rebook. |
| Wrong settlement date | Amendment if non-economic; audit required. |
| Wrong instrument | Reverse and rebook. |
| Late broker correction | Preserve original and correction; do not silently overwrite. |
| Corporate action revised by issuer | Version event and generate correction postings. |
| Cancelled trade | Cancel/reverse trade and associated cash/fees/tax. |

Good practice:

```text
Never delete economic history. Reverse, correct, version and audit.
```

## 14. Booking examples

### 14.1 Buy from existing cash

| Posting | Security | Cash | Performance |
|---|---:|---:|---|
| Buy equity | +shares | -cash | Internal investment. |
| Fee | 0 | -fee | Expense. |

### 14.2 Dividend received

| Posting | Security | Cash | Performance |
|---|---:|---:|---|
| Gross dividend | 0 | +gross dividend | Income. |
| Withholding tax | 0 | -tax | Tax expense. |
| Net dividend | 0 | +net dividend | Net income shown in client reports. |

### 14.3 Sell and withdraw proceeds

| Posting | Security | Cash | Performance |
|---|---:|---:|---|
| Sell equity | -shares | +sale proceeds | Internal investment disposal. |
| Fee/tax | 0 | -fee/tax | Expense/tax. |
| Cash withdrawal | 0 | -cash | External outflow. |

## 15. Common implementation mistakes

| Mistake | Consequence |
|---|---|
| Treating trade date and settlement date as same | Incorrect cash availability and settlement reporting. |
| Not modelling unsettled quantity | Over-selling or incorrect availability. |
| No lot model | Incorrect realized P&L, tax and cost-basis adjustments. |
| Booking corporate action announcements as transactions | False positions/cash before entitlement is confirmed. |
| Ignoring ex-date | Wrong dividend entitlement. |
| Ignoring FX | Wrong base-currency value and P&L. |
| Using stale prices without flags | Misleading valuations. |
| Not versioning corrections | Audit and reconciliation failures. |
| Treating ADR as same as local share | Wrong price, currency, dividend, fee and corporate-action treatment. |

## 16. Practitioner explanation

> For equities, I would distinguish order, trade, settlement, position and corporate-action layers. The accounting transaction records economic postings such as buy, sell, dividend, tax, fee, split, rights exercise, spin-off or merger. The position is derived from transactions and should maintain trade-date quantity, settled quantity, available quantity, cost basis, market value and P&L. For robust wealth platforms, lot-level cost basis and corporate-action event links are essential, because dividends, splits, rights, spin-offs and mergers can change quantity, cash, cost and performance classification without being simple buy/sell trades.
