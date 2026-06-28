# Cross-Product Transaction And Position Data Model

## Purpose

This guide defines a reusable transaction and position model for wealth-management platforms across cash, deposits, FX, bonds, equities, funds, derivatives, structured products, private markets, real assets, insurance, lending, collateral, tax and reporting workflows.

The model is intentionally not product-prefixed. A transaction type such as `BUY`, `SELL`, `INCOME`, `REDEMPTION`, `COLLATERAL_PLEDGE` or `LOAN_DRAWDOWN` should work across product families. Product-specific meaning is carried by the instrument, account, lifecycle event, transaction legs, cash movements, valuation basis, cost-lot treatment and source evidence.

Use this guide when designing:

1. transaction APIs,
2. position APIs,
3. ledger and cash posting models,
4. event schemas,
5. migration mappings,
6. reporting datasets,
7. reconciliation rules,
8. QA regression scenarios,
9. data-product contracts.

## Modelling Principles

| Principle | Meaning |
|---|---|
| Position is state | A position is the current or historical state of a holding, liability, obligation or exposure as of a date and source basis. |
| Transaction is an economic posting | A transaction changes a position, cash balance, lot, ledger account, income classification, liability or obligation. |
| Lifecycle event is evidence | A lifecycle event is the source event that explains what happened: order accepted, trade executed, coupon announced, NAV confirmed, option exercised, margin call issued, settlement failed or correction received. |
| Order is intent | An order, instruction or election is not a transaction until it is accepted, executed, booked or otherwise economically effective. |
| Cash movement is a leg | Cash movement is one effect of a transaction, not the whole transaction. Many transactions have multiple cash, security, fee, tax, collateral or income legs. |
| Ledger is accounting view | Ledger entries should balance and explain book impact, but they should not replace business transaction and lifecycle semantics. |
| Exposure is analytical view | Exposure can be notional, delta-adjusted, look-through, issuer, counterparty, currency, sector, duration, liquidity or collateral view. It is not always the legal position. |
| Corrections are explicit | Reversals, cancellations, restatements and rebooks should be linked to the original record, not hidden by destructive overwrite. |

## Canonical Entity Model

| Entity | Description | Typical Owner |
|---|---|---|
| `party` | Client, beneficial owner, advisor, counterparty, issuer or service provider. | Client master, CRM, counterparty master |
| `account` | Legal booking account where positions, cash and transactions are held. | Core banking, custody, accounting |
| `portfolio` | Analytical or reporting grouping of one or more accounts and sleeves. | Portfolio master, reporting platform |
| `instrument` | Security, fund, derivative, policy, loan, deposit, structured note, real asset, currency or contract definition. | Instrument master, product master, market data |
| `position` | As-of holding, liability, obligation, policy value, cash balance or collateral state. | Position book, custody, accounting, lending, policy admin |
| `position_lot` | Acquisition or accounting lot used for cost basis, tax, P&L and disposal matching. | Investment accounting, tax-lot engine |
| `transaction` | Business posting that changes economic state. | Trading, accounting, custody, lending, policy admin |
| `transaction_leg` | Atomic effect of a transaction: cash, security, income, fee, tax, margin, collateral, accrual or liability. | Accounting, custody, settlement |
| `cash_movement` | Currency-specific cash receivable, payable, debit, credit, sweep or settlement movement. | Cash ledger, payments, treasury |
| `ledger_entry` | Debit/credit posting for accounting, audit and financial control. | General ledger, investment accounting |
| `lifecycle_event` | Source event or state change that explains why a transaction, valuation, obligation or correction exists. | Event source, operations, issuer/custodian feed |
| `valuation_snapshot` | Point-in-time price, NAV, market value, policy value, collateral value or fair value. | Pricing, valuation, market data |
| `exposure_snapshot` | Analytical exposure view derived from positions, reference data and risk models. | Risk, portfolio analytics |
| `source_evidence` | Original feed, notice, confirmation, statement, contract, election, batch or manual approval reference. | Source system, document archive, workflow |
| `reconciliation_break` | Difference between source, book, ledger, custody, reporting or analytics records. | Operations, data quality |

## Position Model

A position should answer: what is held or owed, where it is booked, what it is worth, what state it is in, what source supports it and how it should be used.

### Position Types

| Position Type | Description | Examples |
|---|---|---|
| `CASH_BALANCE` | Currency cash balance with settled, pending, restricted and available components. | USD account cash, EUR pending receivable, restricted escrow balance |
| `SECURITY_POSITION` | Listed or custody-held security position. | Equity shares, bond nominal, ETF units, listed REIT units |
| `FUND_POSITION` | Fund or pooled investment holding often valued by NAV. | Mutual fund units, hedge fund shares, private fund units |
| `DERIVATIVE_CONTRACT` | Open derivative contract with quantity, notional, MTM, margin and lifecycle state. | Option, future, forward, swap, CDS |
| `STRUCTURED_PRODUCT_POSITION` | Note or structured wrapper with payoff terms, observation schedule and issuer exposure. | Autocall note, capital-protected note, participation note |
| `DEPOSIT_POSITION` | Term, notice or callable deposit position. | 3-month time deposit, dual-currency deposit, certificate of deposit |
| `LOAN_LIABILITY` | Borrowing, overdraft, Lombard, margin or policy loan liability. | Drawn credit line, margin loan, policy loan |
| `INSURANCE_POLICY` | Policy state with premium, cash value, surrender value, benefit and beneficiary attributes. | Insurance-linked policy, annuity, protection policy |
| `PRIVATE_MARKET_INTEREST` | Commitment-backed or NAV-lagged private investment interest. | Private equity fund, private credit fund, co-investment |
| `REAL_ASSET_HOLDING` | Direct or indirect real asset position. | Direct property, art, precious metal account, infrastructure asset |
| `COMMITMENT_OR_OBLIGATION` | Undrawn commitment, capital-call obligation, unfunded liability or pledged obligation. | Private fund commitment, unpaid call, future funding obligation |
| `COLLATERAL_PLEDGE` | Asset pledge or collateral pool relationship attached to a facility or exposure. | Securities pledged to loan, cash margin, metal collateral |

### Position Header Fields

| Field | Type | Description | Example |
|---|---|---|---|
| `position_id` | string | Stable position identifier for the as-of position record or position thread. | `POS-2026-000124` |
| `position_type` | enum | Canonical position type. | `SECURITY_POSITION` |
| `position_status` | enum | Operational state. | `ACTIVE`, `PENDING`, `RESTRICTED`, `MATURED`, `CLOSED`, `DISPUTED` |
| `as_of_date` | date | Date the position is valid for. | `2026-06-30` |
| `as_of_timestamp` | datetime | Timestamp for intraday or source-specific freshness. | `2026-06-30T18:00:00Z` |
| `account_id` | string | Legal booking account. | `ACC-001` |
| `portfolio_id` | string | Reporting or analytical portfolio. | `PORT-GLOBAL-BAL` |
| `instrument_id` | string | Instrument, product, currency, policy, facility or contract identifier. | `ISIN-US0378331005` |
| `product_family` | enum | Broad product family. | `EQUITY`, `BOND`, `FUND`, `DERIVATIVE`, `LOAN`, `CASH` |
| `product_subtype` | string | More granular subtype. | `LISTED_COMMON_STOCK`, `TERM_DEPOSIT`, `AUTOCALL_NOTE` |
| `wrapper_type` | enum | Legal or wrapper form when relevant. | `DIRECT`, `FUND`, `NOTE`, `POLICY`, `ACCOUNT`, `FACILITY` |
| `legal_holding_type` | enum | How the position is legally held or owed. | `OWNED_ASSET`, `LIABILITY`, `CONTRACT_RIGHT`, `PLEDGE`, `COMMITMENT` |
| `supportability_state` | enum | Whether the platform fully supports the position. | `SUPPORTED`, `SOURCE_LIMITED`, `MANUAL`, `UNSUPPORTED`, `ESTIMATED` |
| `source_system` | string | System of record or source feed. | `custody-feed` |
| `source_position_id` | string | Native source identifier. | `CUST-POS-77812` |
| `lineage_id` | string | Lineage or data-product trace identifier. | `LIN-20260630-01` |

### Position Quantity And Value Fields

| Field | Type | Description | Applies To |
|---|---|---|---|
| `quantity` | decimal | Units, shares, contracts or policy units. | Securities, funds, derivatives |
| `quantity_type` | enum | Basis for quantity. | `UNITS`, `SHARES`, `CONTRACTS`, `OUNCES`, `POLICY_UNITS` |
| `nominal_amount` | decimal | Par, face or principal amount. | Bonds, notes, loans, deposits |
| `notional_amount` | decimal | Contract notional used for exposure, payoff or derivative economics. | Derivatives, FX, structured products |
| `commitment_amount` | decimal | Committed capital or funding obligation. | Private markets, facilities |
| `unfunded_amount` | decimal | Remaining unpaid commitment. | Private markets, credit lines |
| `drawn_amount` | decimal | Amount currently borrowed or funded. | Loans, facilities |
| `market_price` | decimal | Price, NAV, fair value input or quote. | Valued instruments |
| `price_basis` | enum | Price interpretation. | `PER_UNIT`, `PERCENT_OF_PAR`, `NAV`, `FAIR_VALUE`, `MODEL_VALUE` |
| `market_value` | decimal | Local currency market value. | All valued positions |
| `base_market_value` | decimal | Portfolio base currency value. | Reporting and analytics |
| `valuation_currency` | ISO currency | Currency of market value. | All valued positions |
| `reporting_currency` | ISO currency | Currency used for portfolio reporting. | Multi-currency reporting |
| `fx_rate_to_reporting` | decimal | FX rate used for translation. | Multi-currency reporting |
| `valuation_basis` | enum | Valuation source and method basis. | `EXCHANGE_PRICE`, `NAV`, `CUSTODIAN_VALUE`, `MODEL_VALUE`, `MANUAL_VALUE`, `ESTIMATED` |
| `valuation_date` | date | Date of price or valuation. | All valued positions |
| `accrued_income` | decimal | Income accrued but not yet paid. | Bonds, deposits, loans, funds |
| `cost_basis` | decimal | Book or tax cost basis. | Investments with P&L/tax lots |
| `unrealized_pnl` | decimal | Unrealized gain or loss under selected basis. | Investment positions |
| `collateral_value` | decimal | Eligible value after collateral policy may differ from market value. | Pledged assets |
| `haircut_rate` | decimal | Collateral haircut rate. | Lending/collateral |
| `restriction_status` | enum | Transfer, trading, pledge or reporting restriction. | `NONE`, `LOCKED`, `PLEDGED`, `BLOCKED`, `SANCTIONED`, `DISPUTED` |

### Position State Rules

1. `quantity`, `nominal_amount`, `notional_amount` and `market_value` must not be used interchangeably.
2. Cash positions must separate settled, pending, restricted and available balances.
3. Derivative positions must separate legal contract state from exposure state and margin state.
4. Loans and collateral should model the liability and pledged asset separately, then link them through a pledge relationship.
5. Private-market positions should separate commitment, paid-in capital, unfunded commitment, distributions and NAV.
6. Insurance positions should separate policy status, premium flows, surrender value, cash value, benefit value and loan value.
7. A position can be reportable even when not tradeable, transferable or fully supported.
8. A closed position should generally have zero open quantity or nominal unless residual cash, unsettled lots, pending claims or corrections remain.

## Transaction Model

A transaction should answer: what economic action happened, what source event caused it, which accounts and positions were affected, which legs were created, what dates govern it and how it should reverse or correct.

### Transaction Header Fields

| Field | Type | Description | Example |
|---|---|---|---|
| `transaction_id` | string | Stable transaction identifier. | `TXN-2026-000901` |
| `transaction_group_id` | string | Groups related transactions and legs. | `TXG-2026-000144` |
| `transaction_type` | enum | Canonical generic type. | `BUY`, `INCOME`, `REDEMPTION`, `LOAN_REPAYMENT` |
| `transaction_subtype` | string | Optional detail without changing canonical type. | `CASH_DIVIDEND`, `FINAL_COUPON`, `PARTIAL_REDEMPTION` |
| `transaction_status` | enum | Booking state. | `PENDING`, `BOOKED`, `SETTLED`, `FAILED`, `CANCELLED`, `REVERSED`, `CORRECTED` |
| `lifecycle_event_id` | string | Source lifecycle event that caused the transaction. | `EVT-2026-004812` |
| `order_id` | string | Related order, instruction or election when applicable. | `ORD-2026-000501` |
| `account_id` | string | Legal booking account. | `ACC-001` |
| `portfolio_id` | string | Reporting or analytical portfolio. | `PORT-GLOBAL-BAL` |
| `instrument_id` | string | Instrument, currency, contract, policy, facility or product identifier. | `ISIN-XS1234567890` |
| `trade_date` | date | Execution date or economic trade date. | `2026-06-27` |
| `effective_date` | date | Date the event becomes economically effective. | `2026-06-27` |
| `value_date` | date | Cash value date. | `2026-07-01` |
| `settlement_date` | date | Expected or actual settlement date. | `2026-07-01` |
| `posting_date` | date | Book posting date. | `2026-06-28` |
| `gross_amount` | decimal | Gross transaction amount before fees and taxes. | `100000.00` |
| `net_amount` | decimal | Net amount after fees, taxes, accrued interest or adjustments. | `99875.25` |
| `transaction_currency` | ISO currency | Currency of transaction economics. | `USD` |
| `price` | decimal | Price, NAV, strike, execution price or quote used. | `101.25` |
| `price_basis` | enum | Price interpretation. | `PER_UNIT`, `PERCENT_OF_PAR`, `NAV`, `FX_RATE`, `PREMIUM`, `MODEL_VALUE` |
| `fx_rate_to_reporting` | decimal | Translation rate to reporting currency. | `1.3521` |
| `settlement_status` | enum | Settlement state. | `PENDING`, `PARTIAL`, `SETTLED`, `FAILED`, `CANCELLED` |
| `position_effect` | enum | Effect on position state. | `INCREASE`, `DECREASE`, `CLOSE`, `NO_POSITION_CHANGE`, `RECLASSIFY` |
| `cash_effect` | enum | Effect on cash state. | `DEBIT`, `CREDIT`, `TWO_SIDED`, `NO_CASH_CHANGE`, `PENDING_ONLY` |
| `ledger_effect` | enum | Accounting effect. | `BALANCED_POSTING`, `MEMO_ONLY`, `OFF_LEDGER`, `PENDING_POSTING` |
| `reversal_of_transaction_id` | string | Original transaction reversed by this transaction. | `TXN-2026-000850` |
| `correction_group_id` | string | Group linking original, reversal and replacement records. | `CORR-2026-000030` |
| `idempotency_key` | string | Deterministic key to prevent duplicate booking. | `SRC-A:2026-06-27:12345` |
| `source_system` | string | Source of record. | `custody-feed` |
| `source_transaction_id` | string | Native source transaction identifier. | `CUST-TXN-998877` |
| `source_timestamp` | datetime | Timestamp of source event or message. | `2026-06-28T02:10:00Z` |
| `supportability_state` | enum | Processing quality state. | `SUPPORTED`, `SOURCE_LIMITED`, `MANUAL`, `ESTIMATED`, `DISPUTED` |

### Transaction Leg Fields

| Field | Type | Description | Example |
|---|---|---|---|
| `leg_id` | string | Stable leg identifier. | `LEG-001` |
| `transaction_id` | string | Parent transaction. | `TXN-2026-000901` |
| `leg_type` | enum | Leg category. | `SECURITY`, `CASH`, `FEE`, `TAX`, `INCOME`, `ACCRUAL`, `MARGIN`, `COLLATERAL`, `LIABILITY`, `LOT` |
| `instrument_id` | string | Instrument affected by this leg. | `ISIN-US0378331005` |
| `currency` | ISO currency | Leg currency. | `USD` |
| `quantity_delta` | decimal | Change in units, shares or contracts. | `100.00` |
| `nominal_delta` | decimal | Change in par, face, principal or drawn amount. | `100000.00` |
| `notional_delta` | decimal | Change in notional exposure. | `1000000.00` |
| `cash_delta` | decimal | Cash debit or credit. Positive/negative convention must be defined. | `-18500.00` |
| `amount` | decimal | Leg amount under selected amount type. | `125.00` |
| `amount_type` | enum | Meaning of amount. | `PRINCIPAL`, `INCOME`, `FEE`, `TAX`, `ACCRUED`, `PNL`, `MARGIN`, `COLLATERAL`, `LIABILITY` |
| `cost_basis_delta` | decimal | Change in lot or cost basis. | `18500.00` |
| `realized_pnl` | decimal | Realized gain or loss created by this leg. | `250.00` |
| `income_classification` | enum | Income classification where relevant. | `INTEREST`, `DIVIDEND`, `DISTRIBUTION`, `COUPON`, `RETURN_OF_CAPITAL` |
| `tax_classification` | enum | Tax treatment where relevant. | `WITHHOLDING`, `STAMP_DUTY`, `CAPITAL_GAINS`, `TAX_RECLAIM` |
| `ledger_account` | string | Accounting ledger account if posted. | `cash`, `investment`, `income`, `expense`, `payable` |
| `debit_credit` | enum | Accounting sign. | `DEBIT`, `CREDIT` |

## Canonical Transaction Types

Canonical transaction types should be generic. Product context comes from `instrument_id`, `product_family`, `transaction_subtype`, legs and lifecycle event.

### Investment And Holding Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `BUY` | Acquire a held asset or contract. | Increase holding, create lot, debit cash. | Buy listed equity, bond, ETF, gold certificate or option premium. |
| `SELL` | Dispose of a held asset or contract. | Decrease holding, close lots, credit cash, realize P&L. | Sell equity shares, fund units, bond nominal or listed REIT units. |
| `SUBSCRIBE` | Enter a fund, note, deposit, private vehicle or insurance wrapper through subscription. | Create or increase position, often after acceptance/NAV confirmation. | Subscribe to a fund share class or structured note. |
| `REDEEM` | Exit a fund, note, deposit, policy or investment through issuer/platform redemption. | Decrease or close position, create receivable or cash credit. | Redeem mutual fund units or structured note principal. |
| `TRANSFER_IN` | Receive an in-kind position or asset transfer. | Increase position, may create lot with transferred cost basis. | Transfer equities from another custodian. |
| `TRANSFER_OUT` | Send an in-kind position or asset transfer out. | Decrease position, no sale proceeds unless cash leg applies. | Transfer fund units to another account. |
| `IN_KIND_DISTRIBUTION` | Receive non-cash assets as a distribution. | Increase received asset and reduce source entitlement. | Private fund distributes listed shares. |
| `MIGRATION_OPENING_BALANCE` | Seed initial position during migration or book inception. | Establish opening state with explicit source basis. | Opening bond nominal and cost lot during platform migration. |
| `LOT_ADJUSTMENT` | Adjust cost lot or quantity basis without a normal trade. | Update lot, cost basis or quantity basis. | Corporate-action cost reallocation or tax-lot correction. |

### Cash, FX And Liquidity Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `CASH_DEPOSIT` | External cash inflow. | Increase cash balance. | Client wires USD into account. |
| `CASH_WITHDRAWAL` | External cash outflow. | Decrease cash balance. | Client withdraws SGD to bank account. |
| `CASH_SWEEP` | Automated movement between cash, deposit, sweep or money-market account. | Move cash between balances or create deposit/MMF position. | Overnight sweep from current account into liquidity fund. |
| `FX_CONVERSION` | Exchange one currency for another. | Debit one cash currency and credit another. | Convert USD cash to SGD cash. |
| `FX_SETTLEMENT` | Settle FX forward, swap, NDF or spot cash legs. | Update cash balances and realized FX where applicable. | Settle EUR/USD forward at value date. |
| `DEPOSIT_PLACEMENT` | Place cash into a deposit or money-market instrument. | Reduce cash, create deposit position. | Place 90-day term deposit. |
| `DEPOSIT_MATURITY` | Deposit matures and returns principal and interest. | Close deposit, credit cash and income. | Term deposit maturity at principal plus interest. |

### Income, Accrual, Fee And Tax Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `INCOME` | Generic income receipt where subtype carries detail. | Usually no quantity change, credit cash/receivable. | Fund distribution classified by source feed. |
| `INTEREST` | Interest income or expense. | Cash/receivable/payable change; accrual may reverse. | Deposit interest, loan interest, bond accrued interest settlement. |
| `COUPON` | Bond or note coupon. | No nominal change unless final redemption includes principal. | Semi-annual bond coupon. |
| `DIVIDEND` | Equity, ETF or fund dividend. | Cash credit or reinvestment. | Cash dividend from listed equity. |
| `DISTRIBUTION` | Fund, private-market, trust or real-asset distribution. | Cash/in-kind credit; may affect income, return of capital or NAV. | Private equity distribution. |
| `RETURN_OF_CAPITAL` | Distribution that reduces cost basis rather than income. | Reduce cost basis and credit cash. | Fund capital return. |
| `FEE` | Expense charged to account, product or service. | Debit cash or accrue payable. | Advisory fee, custody fee, fund platform fee. |
| `TAX` | Tax withholding, stamp duty, transaction tax or reporting tax posting. | Debit cash, reduce income, create tax lot or receivable. | Dividend withholding tax. |
| `TAX_RECLAIM` | Refund or reclaim of previously withheld tax. | Credit cash or receivable. | Reclaim foreign withholding tax. |
| `ACCRUAL` | Recognize earned or incurred amount before cash settlement. | Update accrued income/expense. | Daily bond coupon accrual. |
| `ACCRUAL_REVERSAL` | Reverse prior accrual when cash event or correction occurs. | Reduce accrued income/expense. | Reverse accrued interest on coupon payment. |

### Maturity, Corporate Action And Servicing Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `MATURITY` | Contract reaches maturity and settles principal/obligation. | Close or reduce position, settle cash. | Bond maturity, deposit maturity, note maturity. |
| `REDEMPTION` | Issuer, fund, note or product redeems all or part of position. | Decrease or close position, credit cash or in-kind asset. | Partial bond redemption or fund redemption. |
| `CORPORATE_ACTION` | Generic asset-servicing transaction where subtype carries action. | May adjust quantity, cost, cash, entitlement or election. | Tender offer, merger consideration, rights event. |
| `SPLIT_OR_CONSOLIDATION` | Security split, reverse split or unit consolidation. | Adjust quantity and price basis, usually no cash. | 2-for-1 equity split. |
| `STOCK_DIVIDEND` | Distribution paid in additional shares or units. | Increase quantity, adjust cost basis. | Scrip dividend. |
| `RIGHTS_EXERCISE` | Exercise subscription rights or warrants. | Debit cash, create or increase underlying position. | Exercise rights issue entitlement. |
| `ENTITLEMENT_RECEIPT` | Record receivable entitlement before final settlement. | Create receivable or pending asset/cash. | Coupon receivable after record date. |

### Derivative, Margin And Collateral Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `DERIVATIVE_PREMIUM` | Premium paid or received for option-like contract. | Create derivative position and cash leg. | Buy call option premium. |
| `DERIVATIVE_MARGIN` | Initial, variation or maintenance margin movement. | Update margin cash/collateral, may affect P&L. | Futures variation margin. |
| `DERIVATIVE_SETTLEMENT` | Cash or physical settlement of derivative. | Close contract, settle cash or deliver/receive underlying. | Cash settle option or NDF. |
| `OPTION_EXERCISE` | Exercise option into underlying or cash settlement. | Close option; create underlying/cash effect. | Exercise call option into shares. |
| `OPTION_EXPIRY` | Option expires without exercise or with automatic settlement. | Close option, recognize P&L if required. | Out-of-the-money option expiry. |
| `COLLATERAL_PLEDGE` | Pledge asset or cash as collateral. | Mark asset as pledged or create collateral record. | Pledge bond portfolio against credit line. |
| `COLLATERAL_RELEASE` | Release previously pledged collateral. | Remove pledge restriction. | Facility repaid and securities released. |
| `MARGIN_CALL` | Funding or collateral movement required by margin/collateral policy. | Create payable/receivable or collateral obligation. | Margin call for derivative exposure. |

### Lending And Liability Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `LOAN_DRAWDOWN` | Borrow against facility or loan. | Increase liability and credit cash. | Draw Lombard facility. |
| `LOAN_REPAYMENT` | Repay principal on loan or facility. | Decrease liability and debit cash. | Repay margin loan principal. |
| `LOAN_INTEREST` | Loan interest charge or payment. | Expense/accrual/cash movement. | Monthly loan interest debit. |
| `CREDIT_LIMIT_CHANGE` | Change approved facility or overdraft limit. | Usually changes limit, not booked cash. | Increase credit line from 1m to 1.5m. |
| `COLLATERAL_REVALUATION` | Revalue eligible collateral for lending control. | Update collateral value, usually not transaction unless ledger impact. | Haircut-adjusted collateral revaluation. |

### Private Market, Insurance And Wealth-Structuring Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `COMMITMENT` | Establish or change committed capital or obligation. | Create commitment and unfunded amount. | Commit to private equity fund. |
| `CAPITAL_CALL` | Fund committed capital. | Debit cash, increase paid-in capital, reduce unfunded commitment. | Private fund call for 10% of commitment. |
| `RECALLABLE_DISTRIBUTION` | Distribution that may be called again. | Credit cash and increase recallable commitment. | Private equity recallable distribution. |
| `NAV_ADJUSTMENT` | Update lagged or revised NAV where treated as position valuation. | Update valuation, not always a transaction. | Private fund quarterly NAV revised. |
| `INSURANCE_PREMIUM` | Premium payment into policy. | Debit cash, update policy value or coverage state. | Annual premium for insurance policy. |
| `INSURANCE_SURRENDER` | Full or partial policy surrender. | Reduce/close policy, credit cash net of fees/tax. | Partial surrender of investment-linked policy. |
| `INSURANCE_CLAIM` | Claim payment or benefit event. | Credit beneficiary/client or close benefit obligation. | Death benefit claim. |
| `POLICY_LOAN` | Borrowing against policy value. | Increase policy loan liability and credit cash. | Policy loan drawdown. |
| `TRUST_DISTRIBUTION` | Distribution from trust, estate or holding structure. | Cash or in-kind transfer with beneficiary/account context. | Trust income distribution to beneficiary. |

### Correction And Control Transactions

| Type | Description | Typical Position Effect | Example |
|---|---|---|---|
| `REVERSAL` | Equal and opposite record that reverses a transaction. | Neutralizes original effects. | Reverse incorrectly booked dividend. |
| `CORRECTION` | Replacement or adjustment after error or source restatement. | Depends on corrected economics. | Correct trade price and cash amount. |
| `CANCEL` | Cancel pending or erroneous instruction before economic finality. | Remove pending state, often no settled effect. | Cancel unexecuted order. |
| `RESTATEMENT_ADJUSTMENT` | Adjust records due to NAV, price, tax, statement or source restatement. | Update value, lot, income, tax or performance basis. | Restated fund NAV changes historical valuation. |
| `WRITE_DOWN` | Reduce carrying value due to impairment or valuation event. | Reduce market/book value, may realize loss. | Private debt impairment. |
| `WRITE_OFF` | Remove asset or receivable judged unrecoverable. | Close value/receivable and recognize loss. | Defaulted receivable written off. |
| `COMPENSATION` | Manual or controlled compensation to client/account. | Cash credit or fee reversal, with evidence. | Compensation for operational error. |

## Lifecycle Events That Are Not Always Transactions

These events should often exist in an event store, workflow system, source-evidence layer or operational case record. They should become transactions only when they create economic, cash, position, ledger, cost, tax, liability or obligation impact.

| Lifecycle Event | Why It Matters | When It Becomes A Transaction |
|---|---|---|
| `ORDER_PLACED` | Captures client/advisor instruction intent. | When executed, accepted or booked depending on product. |
| `ORDER_REJECTED` | Explains why no transaction should exist. | Usually never. |
| `TRADE_EXECUTED` | Execution event for market instruments. | Creates buy/sell/derivative transaction. |
| `SETTLEMENT_CONFIRMED` | Confirms cash/security settlement. | Updates settlement status; may create failed-settlement corrections. |
| `SETTLEMENT_FAILED` | Creates operational exception. | May create reversal, fail fee, receivable/payable or compensation. |
| `CORPORATE_ACTION_ANNOUNCED` | Source notice. | When entitlement, election, cash or security outcome is booked. |
| `ELECTION_SUBMITTED` | Client/advisor election. | When accepted and economically processed. |
| `NAV_RECEIVED` | Valuation input. | Usually valuation snapshot; transaction only for restatement adjustment or fee/NAV equalization. |
| `BARRIER_OBSERVED` | Structured product observation. | When coupon, autocall, redemption or settlement is triggered. |
| `MARGIN_CALL_NOTICE` | Collateral/funding requirement. | When cash/collateral movement or payable is booked. |
| `CAPITAL_CALL_NOTICE` | Funding obligation notice. | When obligation and funded cash transaction are booked. |
| `POLICY_STATUS_CHANGED` | Insurance policy lifecycle. | When premium, surrender, claim or policy-loan economics are booked. |
| `REPORT_DELIVERED` | Evidence of statement/report delivery. | Usually never; belongs to reporting archive. |
| `TAX_DOCUMENT_AMENDED` | Revised tax evidence. | When tax posting, reclaim or restatement adjustment is booked. |

## Transaction Effect Matrix

| Transaction Family | Position Change | Cash Change | Lot/Cost Change | Income/Expense | Liability/Obligation | Example |
|---|---|---|---|---|---|---|
| Acquisition/disposal | Yes | Yes | Yes | Realized P&L possible | No | `BUY`, `SELL`, `SUBSCRIBE`, `REDEEM` |
| Cash/liquidity | Cash position only | Yes | No | Interest possible | No | `CASH_DEPOSIT`, `FX_CONVERSION`, `CASH_SWEEP` |
| Income | Usually no quantity change | Yes or receivable | Sometimes | Yes | No | `COUPON`, `DIVIDEND`, `DISTRIBUTION` |
| Accrual | Usually no cash | No immediate cash | No | Yes | Receivable/payable possible | `ACCRUAL`, `ACCRUAL_REVERSAL` |
| Servicing/corporate action | Sometimes | Sometimes | Often | Sometimes | Entitlement possible | `SPLIT_OR_CONSOLIDATION`, `RIGHTS_EXERCISE` |
| Derivative/margin | Yes or MTM/margin state | Often | Product-specific | P&L possible | Collateral possible | `DERIVATIVE_MARGIN`, `OPTION_EXERCISE` |
| Lending/collateral | Liability or pledge state | Yes or restricted cash | No | Interest/fees possible | Yes | `LOAN_DRAWDOWN`, `COLLATERAL_PLEDGE` |
| Private markets | Commitment/NAV state | Yes | Yes | Distribution classification | Yes | `COMMITMENT`, `CAPITAL_CALL` |
| Insurance | Policy state | Yes | Product-specific | Fees/surrender gains possible | Policy loan possible | `INSURANCE_PREMIUM`, `INSURANCE_SURRENDER` |
| Correction/control | Depends on correction | Depends | Depends | Depends | Depends | `REVERSAL`, `CORRECTION`, `RESTATEMENT_ADJUSTMENT` |

## Lifecycle State Model

Use separate state fields instead of forcing all lifecycle into one status.

| State Dimension | Typical Values | Why Separate |
|---|---|---|
| Instruction state | `DRAFT`, `SUBMITTED`, `APPROVED`, `REJECTED`, `CANCELLED` | Captures client/advisor intent and control workflow. |
| Execution state | `NOT_EXECUTED`, `PARTIAL`, `EXECUTED`, `FAILED` | Separates market execution from booking and settlement. |
| Booking state | `PENDING`, `BOOKED`, `CORRECTED`, `REVERSED` | Explains book-of-record state. |
| Settlement state | `PENDING`, `PARTIAL`, `SETTLED`, `FAILED`, `CANCELLED` | Explains cash/security finality. |
| Reconciliation state | `MATCHED`, `UNMATCHED`, `TOLERANCE_MATCHED`, `BREAK_OPEN`, `BREAK_CLOSED` | Supports operations and data quality. |
| Reporting state | `REPORTABLE`, `SUPPRESSED`, `ESTIMATED`, `DISCLOSED_EXCEPTION` | Avoids silent reporting of poor-quality data. |
| Supportability state | `SUPPORTED`, `SOURCE_LIMITED`, `MANUAL`, `ESTIMATED`, `UNSUPPORTED` | Separates technical support from source truth. |

## Worked Examples

### 1. Equity Buy

| Step | Record | Details |
|---|---|---|
| Instruction | `ORDER_PLACED` | Advisor places buy order for 100 shares. |
| Execution | `TRADE_EXECUTED` | Execution at USD 185.00. |
| Transaction | `BUY` | Transaction header uses instrument, trade date, settlement date and settlement status. |
| Legs | `SECURITY`, `CASH`, `FEE`, `TAX` | Security leg `+100`; cash leg `-18,500`; fee and tax legs as applicable. |
| Position | `SECURITY_POSITION` | Quantity increases by 100; market value updates from price feed. |
| Lot | `position_lot` | New lot created with acquisition date, quantity and cost basis. |
| QA | Reconcile | Security quantity, cash debit, fees, taxes, settlement status and lot cost reconcile to confirmation. |

### 2. Bond Coupon With Accrued Interest

| Step | Record | Details |
|---|---|---|
| Lifecycle | `ENTITLEMENT_RECEIPT` | Coupon entitlement identified from record date and holding. |
| Transaction | `COUPON` | Coupon booked when payable or paid, depending on accounting policy. |
| Legs | `INCOME`, `CASH`, `TAX`, `ACCRUAL_REVERSAL` | Coupon income credited; withholding tax debited; accrued income reversed. |
| Position | `SECURITY_POSITION` | Bond nominal usually unchanged. |
| Reporting | Income | Coupon appears as income, not capital gain or sale proceeds. |
| QA | Accrual bridge | Accrued income before payment plus cash coupon and tax should reconcile. |

### 3. Fund Subscription

| Step | Record | Details |
|---|---|---|
| Instruction | `ORDER_PLACED` | Subscription instruction sent before dealing cutoff. |
| Lifecycle | `ORDER_ACCEPTED`, `NAV_RECEIVED` | Fund accepts order and confirms NAV/unit allocation. |
| Transaction | `SUBSCRIBE` | Use generic transaction type with fund instrument. |
| Legs | `CASH`, `SECURITY`, `FEE` | Cash debit first may be pending; units confirmed after NAV. |
| Position | `FUND_POSITION` | Units and cost basis created after confirmation. |
| QA | Pending handling | Pending cash and pending units must not be reported as fully settled holdings without disclosure. |

### 4. FX Spot Conversion

| Step | Record | Details |
|---|---|---|
| Lifecycle | `TRADE_EXECUTED` | FX spot trade agreed. |
| Transaction | `FX_CONVERSION` | One generic transaction with two cash legs. |
| Legs | `CASH`, `CASH` | Debit sold currency and credit bought currency on value date. |
| Position | `CASH_BALANCE` | Two cash balances change; no security position is created. |
| Reporting | FX | Realized FX may be created if closing an existing currency exposure under accounting policy. |
| QA | Currency signs | Both legs require currency, amount, value date and FX rate basis. |

### 5. Private Market Capital Call

| Step | Record | Details |
|---|---|---|
| Lifecycle | `CAPITAL_CALL_NOTICE` | Notice specifies due date, percentage, purpose and fund. |
| Transaction | `CAPITAL_CALL` | Funded amount is booked when payable or paid. |
| Legs | `CASH`, `INVESTMENT`, `COMMITMENT` | Cash debit; paid-in capital increases; unfunded commitment decreases. |
| Position | `PRIVATE_MARKET_INTEREST` and `COMMITMENT_OR_OBLIGATION` | NAV may remain unchanged until next valuation; commitment state changes immediately. |
| Reporting | Liquidity | Show funded, unfunded, recallable and NAV separately. |
| QA | Commitment roll-forward | Opening commitment - calls + recallable distributions + changes = closing unfunded commitment. |

### 6. Option Exercise

| Step | Record | Details |
|---|---|---|
| Lifecycle | `OPTION_EXERCISE` | Exercise instruction accepted or automatic exercise triggered. |
| Transaction | `OPTION_EXERCISE` | Closes option and creates cash or underlying effects. |
| Legs | `DERIVATIVE`, `SECURITY`, `CASH` | Option position closes; underlying position or cash settlement posts. |
| Position | `DERIVATIVE_CONTRACT` and possibly `SECURITY_POSITION` | Option closed; underlying shares or cash settlement recorded. |
| QA | Premium treatment | Premium, strike, quantity multiplier and fees must reconcile to final economics. |

### 7. Loan Drawdown And Collateral Pledge

| Step | Record | Details |
|---|---|---|
| Lifecycle | `FACILITY_DRAWDOWN_APPROVED` | Credit line drawdown approved. |
| Transaction | `LOAN_DRAWDOWN` | Liability increases and cash is credited. |
| Legs | `LIABILITY`, `CASH` | Loan principal leg and cash credit leg. |
| Related transaction | `COLLATERAL_PLEDGE` | Securities or cash marked as pledged against facility. |
| Position | `LOAN_LIABILITY`, `CASH_BALANCE`, `COLLATERAL_PLEDGE` | Borrowing and collateral state are separate but linked. |
| QA | LTV | Collateral value, haircuts, pledged restrictions and loan balance drive LTV and margin monitoring. |

### 8. Insurance Premium And Surrender

| Step | Record | Details |
|---|---|---|
| Premium | `INSURANCE_PREMIUM` | Cash debit updates policy value, coverage state or expense treatment. |
| Valuation | `valuation_snapshot` | Policy value or surrender value sourced from insurer. |
| Surrender | `INSURANCE_SURRENDER` | Full or partial policy surrender creates cash credit and policy reduction. |
| Position | `INSURANCE_POLICY` | Policy remains active, lapsed, surrendered or matured depending on source state. |
| QA | Value basis | Cash value, surrender value, benefit value and loan value must not be mixed. |

### 9. Structured Product Autocall

| Step | Record | Details |
|---|---|---|
| Lifecycle | `BARRIER_OBSERVED` | Observation determines whether coupon or autocall condition is met. |
| Transaction | `COUPON` and `REDEMPTION` | Coupon may pay and note principal may redeem early. |
| Legs | `INCOME`, `CASH`, `SECURITY` | Coupon credit; principal redemption; note position closed. |
| Position | `STRUCTURED_PRODUCT_POSITION` | Status changes from active to redeemed/autocalled. |
| QA | Observation evidence | Underlying level, observation date, barrier, issuer confirmation and payoff formula must be traceable. |

### 10. Correction, Reversal And Rebook

| Step | Record | Details |
|---|---|---|
| Original | `DIVIDEND` | Dividend booked at incorrect tax rate. |
| Reversal | `REVERSAL` | Equal and opposite legs reverse original transaction. |
| Replacement | `CORRECTION` or corrected `DIVIDEND` | Correct dividend and withholding tax booked with new transaction ID. |
| Links | `reversal_of_transaction_id`, `correction_group_id` | All records remain traceable. |
| QA | Audit trail | Net economic effect equals corrected source evidence; original error remains explainable. |

## API And Event Design Guidance

### Position API

Typical query dimensions:

1. `portfolio_id`,
2. `account_id`,
3. `as_of_date`,
4. `position_type`,
5. `product_family`,
6. `currency`,
7. `supportability_state`,
8. `include_pending`,
9. `include_lots`,
10. `include_exposures`.

Position responses should include valuation basis, source, freshness, restriction status and supportability state when any value can affect trading, reporting, lending or advice.

### Transaction API

Typical query dimensions:

1. `portfolio_id`,
2. `account_id`,
3. `instrument_id`,
4. `transaction_type`,
5. `transaction_status`,
6. `trade_date_from`,
7. `trade_date_to`,
8. `settlement_date_from`,
9. `settlement_date_to`,
10. `include_legs`,
11. `include_reversals`,
12. `include_source_evidence`.

Transaction APIs should default to returning canonical transaction types and should avoid product-prefixed enums. Use filters and response attributes for product context.

### Event Schema

A lifecycle event should include:

| Field | Description |
|---|---|
| `event_id` | Stable event identifier. |
| `event_type` | Lifecycle event type, separate from `transaction_type`. |
| `event_timestamp` | When event occurred or was received. |
| `source_system` | Source of event. |
| `source_event_id` | Native source identifier. |
| `account_id` | Account context when applicable. |
| `portfolio_id` | Portfolio context when applicable. |
| `instrument_id` | Instrument or contract context when applicable. |
| `payload_version` | Schema version. |
| `idempotency_key` | Deterministic duplicate-prevention key. |
| `source_evidence_ref` | Link to confirmation, notice, document, message or workflow evidence. |
| `derived_transaction_ids` | Transactions created from the event. |

## Data Quality And Control Rules

1. Every transaction must have a source identifier, idempotency key or controlled manual evidence.
2. Every cash leg must have currency, value date, amount and sign convention.
3. Every position movement must identify an instrument, policy, facility, currency or contract.
4. Every lot-affecting transaction must define cost basis, acquisition date and cost method impact.
5. Every reversal must link to the original transaction.
6. Every correction should group original, reversal and replacement records.
7. Pending and settled amounts must remain separate.
8. Legal holding and analytical exposure must remain separate.
9. Market value must carry valuation date, valuation basis, price source and currency.
10. Collateral values must distinguish market value, eligible value and haircut-adjusted value.
11. Private-market records must separate commitment, funded amount, unfunded amount, recallable amount and NAV.
12. Insurance records must separate premium, cash value, surrender value, benefit value and policy loan.
13. Manual or estimated records must be visible in APIs and reports.
14. Ledger entries must balance by transaction group under the selected accounting policy.
15. Position roll-forward must reconcile opening position, transactions, corporate actions, transfers, corrections and closing position.

## QA Regression Checklist

Use these scenarios before accepting transaction and position models:

1. Buy/sell with fees, taxes, accrued interest and multi-currency settlement.
2. Same-day reversal and rebook with audit trail intact.
3. Partial settlement and failed settlement.
4. Corporate action with no cash but quantity and cost-basis change.
5. Income event with withholding tax and accrual reversal.
6. FX conversion with two cash legs and value-date handling.
7. Private-market capital call and subsequent distribution.
8. Derivative margin movement and option exercise.
9. Loan drawdown with collateral pledge and LTV recalculation.
10. Policy premium, policy loan and surrender value update.
11. Migration opening balance followed by first live transaction.
12. Restated NAV or corrected price affecting valuation and reporting.
13. Unsupported or source-limited product state shown with clear supportability flags.
14. Position API and transaction API reconcile to reporting output.
15. Ledger entries balance for each transaction group.

## Implementation Anti-Patterns

Avoid these patterns:

1. product-prefixed transaction types for every product family when a generic type plus instrument context is sufficient,
2. using `amount` without amount type, currency, date and sign convention,
3. mixing trade date, settlement date, value date and posting date,
4. treating lifecycle notices as settled transactions,
5. hiding reversals by overwriting original records,
6. reporting pending holdings as settled holdings without disclosure,
7. treating notional exposure as market value,
8. mixing cash value, surrender value and benefit value for insurance policies,
9. mixing commitment, NAV and market value for private markets,
10. mixing collateral market value and haircut-adjusted lendable value,
11. treating valuations as transactions when no economic posting exists,
12. building report-specific transaction categories that cannot reconcile back to book records.

## Related Guides

Use this model with:

1. [Product Lifecycle, Cashflow And Event Guide](product-lifecycle-cashflow-and-event-guide.md)
2. [Product Taxonomy And Vocabulary Guide](product-taxonomy-and-vocabulary-guide.md)
3. [Investment Accounting, Ledger, Fees, P&L, Accruals And Book-Of-Record Design](investment-accounting-ledger/README.md)
4. [Trade Lifecycle, Order Management, Settlement, Custody And Operations](trade-lifecycle-operations/README.md)
5. [Market Data, Reference Data, Pricing And Instrument Master Design](market-data-reference-data-pricing-instrument-master/README.md)
6. [Data Governance, Data Quality, Lineage, Reconciliation And Operating Controls](data-governance-quality-lineage-controls/README.md)
7. [Source Ownership, Calculation And Reporting Matrix](source-ownership-calculation-reporting-matrix.md)

## Disclaimer

This is product, data-model and implementation guidance for engineering and knowledge-management use. It is not legal, tax, accounting, regulatory or investment advice.
