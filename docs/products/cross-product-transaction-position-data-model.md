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

## Executive Summary

The model has five durable truths:

1. Instrument static defines what the product is and how calculations work.
2. Position state defines what the client holds, owes, has pledged, has committed to fund, or can report as of a date.
3. Transactions define economic changes through headers and typed legs.
4. Lifecycle events explain why a transaction, valuation, obligation, correction or operational state exists.
5. Valuation and exposure snapshots derive market value, report value and risk views without replacing legal holdings or booked transactions.

For implementation, the app should be able to answer four questions for every portfolio value:

| Question | Must Trace To |
|---|---|
| What is this value? | Instrument, position type, amount basis, currency, date and supportability state. |
| Why did it change? | Transaction group, transaction legs, lifecycle event and source evidence. |
| Can I trust it? | Source, valuation basis, reconciliation state, stale/manual/estimated flags and owner. |
| How should it be used? | Reporting classification, tradeability, liquidity, collateral eligibility, mandate/risk treatment and disclosure state. |

## How To Use This Model

This guide is long by design. Use it as a working model, not as a document to read only front-to-back.

| Need | Start With | Then Use |
|---|---|---|
| Build instrument master or product terms | Instrument static and calculation data model | Calculation dependency matrix and product-specific instrument attributes. |
| Build holdings or position APIs | Position model | Position architecture, position state dimensions and position relationship model. |
| Build transaction ingestion | Transaction model | Transaction type selection guide, leg sign conventions and source classification rules. |
| Model corporate actions | Corporate action mapping | Connecting instrument lifecycle, product legs and cash legs. |
| Build portfolio app screens | Portfolio management app implementation blueprint | Screen/API design implications and derived views. |
| Build reconciliation controls | Implementation flow and model invariants | Reconciliation model and golden certification scenarios. |
| Build migration mappings | Minimum domain tables | Migration opening balance rules, source classification and model readiness checklist. |
| Build QA fixtures | Golden scenarios | QA regression checklist and portfolio app acceptance criteria. |

## Core Design Choices

| Design Choice | Decision |
|---|---|
| Transaction vocabulary | Use generic transaction types such as `BUY`, `SELL`, `COUPON`, `DIVIDEND`, `REDEMPTION`, `CAPITAL_CALL`, `LOAN_DRAWDOWN` and `REVERSAL`. Avoid product-prefixed top-level types. |
| Product specificity | Put product detail in instrument static, lifecycle event, transaction subtype, legs, valuation basis, exposure snapshot and supportability state. |
| Event connection | Use `lifecycle_thread_id`, `lifecycle_event_id`, `transaction_group_id`, `transaction_id` and `leg_id` to connect the full story. |
| Position truth | Store source-backed snapshots and transaction-driven deltas; use reconciliation to prove they agree. |
| Cash truth | Model cash as currency-specific settled/pending/available/restricted state, not just report-currency value. |
| Calculation truth | Use explicit instrument static and versioned terms, not product names or display labels. |
| Correction truth | Preserve original, reversal and replacement records. Do not overwrite economic history. |
| Reconstruction truth | Use deterministic snapshot identity and restatement version when exposing reconstructed portfolio state. |

## Boundary Tests

Use these tests when a field, table, API or screen design feels ambiguous.

| If You Are Modelling... | It Belongs In | Not In | Reason |
|---|---|---|---|
| Coupon rate, day count, maturity, barrier, strike, multiplier, fee schedule | Instrument static | Transaction or position | These are product terms used repeatedly for calculations. |
| Quantity held today | Position snapshot | Instrument static | Holdings are account/client-specific state. |
| Buy, sell, coupon, dividend, fee, tax, margin, capital call | Transaction and transaction legs | Instrument static | These are economic events. |
| Announcement, election, observation, notice, settlement fail | Lifecycle event | Transaction by default | These explain state; only create transactions when economics post. |
| Price, NAV, FX rate, appraisal, policy value | Valuation snapshot | Transaction by default | Valuations measure value; they do not always change economics. |
| Delta, duration, issuer exposure, look-through sector exposure | Exposure snapshot | Legal position | These are analytical views derived from positions and reference data. |
| Pledged amount, haircut, eligible collateral value | Collateral/pledge state | Plain holding market value | Pledge and eligibility affect availability and lending, not legal ownership alone. |
| Stale NAV, manual price, unsupported product state | Data-quality/supportability state | Hidden UI note | Data confidence must be available to APIs, reports and controls. |
| Report delivery, statement archive, document generation | Report/evidence archive | Transaction | Delivery is operational evidence unless it creates economic posting. |

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

## Instrument Static And Calculation Data Model

Instrument static data should answer: what is the product, how does it behave, what terms drive cashflows, income, valuation, accrued interest, cost treatment, risk, eligibility and reporting. Static data belongs to the instrument/product master and should not be copied into every transaction or position except where a booked event needs the historical terms used at the time.

### Instrument Static Principles

| Principle | Meaning |
|---|---|
| Separate terms from holdings | Instrument terms define the product; positions define what a client holds. |
| Version terms | Callable bonds, structured notes, fund share classes, policy values and product documents can change or be corrected. Keep `terms_version` and effective dates. |
| Preserve calculation inputs | Day-count, coupon frequency, strike, barrier, NAV basis, fee schedule, maturity, multiplier and lot rules are calculation inputs, not display text. |
| Keep product family generic | Use common fields where possible and product-specific extension blocks where needed. |
| Store source and confidence | Instrument static should carry source system, effective date, approval state and data-quality state. |
| Do not infer legal terms from names | Product name is not a calculation source. Calculations should use explicit fields and source evidence. |

### Common Instrument Header

| Field | Type | Description | Example |
|---|---|---|---|
| `instrument_id` | string | Canonical instrument identifier used across positions, transactions, prices and events. | `ISIN-US0378331005` |
| `instrument_type` | enum | Specific instrument type. | `EQUITY`, `BOND`, `FUND`, `OPTION`, `STRUCTURED_NOTE`, `LOAN_FACILITY` |
| `product_family` | enum | Broad product family. | `EQUITY`, `FIXED_INCOME`, `FUND`, `DERIVATIVE`, `CASH`, `LOAN`, `INSURANCE` |
| `product_subtype` | string | Product subtype for implementation and reporting. | `CALLABLE_BOND`, `MUTUAL_FUND_SHARE_CLASS`, `AUTOCALL_NOTE` |
| `name` | string | Display name. | `ABC Corp 5.25% 2029` |
| `issuer_id` | string | Issuer, fund manager, insurer, borrower, bank or counterparty. | `ISSUER-ABC` |
| `counterparty_id` | string | Contract counterparty where relevant. | `BANK-XYZ` |
| `currency` | ISO currency | Primary denomination, pricing or contract currency. | `USD` |
| `country_of_risk` | string | Risk country. | `US` |
| `country_of_issue` | string | Issue or domicile country. | `LU` |
| `exchange` | string | Trading venue when listed. | `NASDAQ` |
| `identifier_set` | object | ISIN, CUSIP, SEDOL, ticker, FIGI, internal ID and vendor IDs. | `ISIN`, `ticker` |
| `issue_date` | date | Issue, inception, policy start or contract start date. | `2026-01-15` |
| `maturity_date` | date | Final maturity, expiry or termination date. | `2029-01-15` |
| `status` | enum | Instrument lifecycle state. | `ACTIVE`, `MATURED`, `SUSPENDED`, `REDEEMED`, `DELISTED` |
| `terms_version` | string | Version of calculation-relevant terms. | `v3` |
| `terms_effective_from` | date | Date terms version becomes effective. | `2026-01-15` |
| `source_system` | string | Golden source for static data. | `instrument-master` |
| `source_document_ref` | string | Prospectus, term sheet, KID, policy document, confirmation or facility agreement. | `DOC-12345` |
| `supportability_state` | enum | Static-data support state. | `SUPPORTED`, `SOURCE_LIMITED`, `MANUAL`, `ESTIMATED`, `UNSUPPORTED` |

### Calculation Attribute Groups

| Attribute Group | Purpose | Typical Fields |
|---|---|---|
| Identity and classification | Drives routing, reporting, tax, eligibility and supportability. | Product family, subtype, identifiers, issuer, domicile, exchange, sector, asset class, complexity, liquidity class. |
| Trading and settlement | Drives order capture, settlement, cash availability and failed-settlement handling. | Trading currency, settlement currency, lot size, minimum trade size, settlement cycle, calendar, cut-off, venue. |
| Cashflow schedule | Drives coupons, dividends, distributions, principal repayment, premiums, fees and maturity cashflows. | Coupon rate, frequency, day count, ex-date, record date, pay date, amortization schedule, capital-call schedule. |
| Income classification | Drives income reporting, tax, withholding, performance income and client statements. | Income type, tax category, withholding eligibility, distribution components, return-of-capital flag. |
| Accrual rules | Drives accrued interest/income/expense. | Accrual start, day-count convention, business-day convention, coupon period, compounding, reset dates. |
| Valuation rules | Drives market value and degraded-state behavior. | Price basis, quote type, NAV basis, valuation frequency, stale threshold, fair-value method, FX source. |
| Cost and lot rules | Drives cost basis, realized P&L, tax lots and corporate-action adjustments. | Cost method, acquisition-date basis, lot matching method, amortization/accretion rule, cost allocation rule. |
| Risk and exposure | Drives concentration, stress, Greeks, duration, issuer/counterparty and look-through analytics. | Underlyings, delta, duration, spread duration, rating, sector, country, leverage, collateral, counterparty. |
| Lifecycle and servicing | Drives events, notifications, elections, redemptions and corporate actions. | Callable/putable flags, call schedule, observation schedule, corporate-action eligibility, election options. |
| Eligibility and governance | Drives advisory, mandate, APU, suitability and restrictions. | Approved status, target market, complexity, product risk rating, liquidity risk, jurisdiction restrictions. |

### Product-Specific Instrument Attributes

| Product Area | Static Attributes Needed For Calculation | Used To Calculate |
|---|---|---|
| Cash currency | Currency code, decimals, holiday calendar, overdraft eligibility, interest basis. | Cash balances, value dates, FX translation, overdraft interest, available cash. |
| Term deposits | Principal currency, rate type, fixed/floating rate, start date, maturity date, day count, compounding, early-break rules, rollover instruction. | Accrued interest, maturity proceeds, break cost, yield, cash availability. |
| Money market instruments | Issue date, maturity, discount/yield basis, day count, issuer, settlement convention. | Discount accretion, yield, accrued income, maturity cashflow. |
| FX spot/forward/swap/NDF | Currency pair, dealt currency, settlement currency, value date, forward rate, fixing source, NDF settlement currency. | Settlement cash, realized FX, forward MTM, NDF fixing cashflow. |
| Equity | Ticker/ISIN, exchange, shares outstanding if needed, sector, country, dividend currency, corporate-action eligibility. | Market value, dividends, corporate actions, exposure, concentration. |
| ETF | Exchange, fund domicile, base currency, distribution policy, replication type, expense ratio, underlying index, creation/redemption notional if relevant. | NAV/price valuation, distributions, look-through exposure, fees, liquidity. |
| Mutual fund share class | Fund ID, share-class ID, NAV currency, dealing frequency, cut-off, settlement cycle, minimums, distribution policy, fee schedule, swing-pricing flag. | Subscription/redemption units, NAV value, fees, distributions, pending dealing state. |
| Hedge fund/alternative fund | NAV frequency, lock-up, notice period, gate terms, side-pocket terms, fee structure, liquidity class. | Stale NAV state, redemption eligibility, fees, liquidity reporting, side-pocket modelling. |
| Bond | Coupon type, coupon rate, coupon frequency, day count, business-day convention, issue date, maturity, call/put schedule, amortization, clean/dirty price basis, rating, seniority. | Accrued interest, coupon cashflows, yield, duration, maturity/redemption proceeds, amortized cost. |
| Floating-rate note | Reference index, spread, reset frequency, fixing calendar, cap/floor, lookback/lockout if any. | Coupon reset, accrued interest, projected cashflows, rate sensitivity. |
| Inflation-linked bond | Inflation index, lag, base index, index ratio convention, deflation floor. | Inflation-adjusted principal, coupon, accrued interest, real yield. |
| Structured note | Issuer, note currency, notional, payoff type, underlyings, strike, barrier, coupon condition, observation dates, autocall schedule, protection level, settlement mode. | Coupon eligibility, autocall/redemption, downside payoff, physical settlement, scenario values. |
| Option/warrant | Underlying, call/put, strike, expiry, exercise style, contract multiplier, premium currency, settlement style. | MTM, exercise/expiry outcome, delta exposure, premium cost, physical/cash settlement. |
| Future | Underlying, contract size, tick size/value, expiry, margin currency, settlement style, exchange. | Variation margin, notional exposure, P&L, expiry/roll handling. |
| Swap | Legs, notional, fixed/floating rates, reset schedule, payment calendar, day count, collateral terms, counterparty. | Accrual, MTM, settlement cashflows, counterparty exposure. |
| Credit derivative/CDS | Reference entity, notional, coupon/spread, maturity, recovery assumption, credit event terms. | Premium accrual, credit event settlement, spread exposure. |
| Private equity/private credit fund | Commitment currency, commitment period, investment period, capital-call mechanics, distribution waterfall, management fee, carry, NAV frequency. | Commitment roll-forward, capital calls, distributions, fees, NAV lag, unfunded exposure. |
| Real estate/direct property | Ownership percentage, appraisal basis, appraisal date, rental terms, expense terms, debt links, currency. | Appraised value, income, expenses, leverage, liquidity and reporting confidence. |
| Precious metals/commodities | Commodity type, unit of measure, purity, storage location, account/certificate form, pricing source. | Unit conversion, market value, storage fees, collateral value. |
| Loan/facility | Facility limit, drawn currency, rate type, margin/spread, reset, maturity, repayment schedule, collateral eligibility, covenants. | Interest accrual, repayment schedule, available credit, LTV, margin call. |
| Insurance policy/annuity | Policy type, premium schedule, policy currency, cash value basis, surrender rules, benefit terms, policy loan terms, beneficiary rules. | Premium due, surrender value, benefit value, policy loan balance, claim cashflow. |
| Trust/holding vehicle | Legal owner, beneficial-owner rules, distribution rules, authority structure, asset ownership map. | Report grouping, entitlement, distribution mapping, access control, tax context. |

### Calculation Dependency Matrix

| Calculation | Required Instrument Static | Required Position/Transaction Data | Output |
|---|---|---|---|
| Market value | Price basis, valuation currency, quote/NAV source, multiplier, FX source | Quantity/nominal/notional, as-of date, FX rate | Local and reporting market value. |
| Accrued interest | Coupon rate, day count, frequency, coupon dates, business-day convention | Nominal, settlement date, holding period, prior coupon state | Accrued income receivable/payable. |
| Coupon cashflow | Coupon schedule, coupon rate, currency, record/pay dates, tax category | Eligible nominal, record-date position, withholding status | Gross coupon, tax, net cash. |
| Dividend/distribution | Distribution policy, ex-date, record date, pay date, income components | Eligible quantity/units, tax profile, election | Income, return of capital, reinvested units or cash. |
| Fund subscription units | NAV basis, dealing date, cut-off, fees, settlement cycle | Subscription cash amount, accepted order, confirmed NAV | Units created, cost basis, pending/settled state. |
| Fund redemption proceeds | NAV basis, dealing date, fees, settlement cycle, redemption gates | Units redeemed, confirmed NAV, fee/tax legs | Cash proceeds, realized P&L, position reduction. |
| Bond yield/duration | Coupon, maturity, call schedule, day count, price basis, redemption value | Clean/dirty price, nominal, settlement date | Yield, duration, convexity, sensitivity. |
| Amortized cost | Coupon, effective yield, amortization/accretion policy | Acquisition cost, nominal, cashflows, lots | Book cost, premium/discount amortization. |
| Realized P&L | Cost method, lot matching rules, price basis, fees/taxes | Disposal transaction, lots, FX rates | Realized gain/loss and tax basis. |
| Corporate-action adjustment | Corporate-action terms, ratio, consideration, cost allocation rule | Record-date holding, lots, election | New quantity, cash-in-lieu, cost-basis allocation. |
| Structured note payoff | Underlyings, strikes, barriers, observation dates, coupon condition, protection level | Holding nominal, observed levels, issuer confirmation | Coupon, autocall, redemption, physical settlement. |
| Option exercise outcome | Strike, multiplier, settlement style, expiry, underlying | Contracts held, exercise event, market/settlement price | Underlying quantity/cash settlement, P&L. |
| Derivative MTM | Contract terms, curve/model inputs, collateral terms | Position notional/contracts, valuation date, market data | Fair value, unrealized P&L, exposure. |
| Margin requirement | Margin method, haircut, eligible collateral rules, thresholds | Derivative MTM, collateral positions, cash | Margin call, excess/deficit, collateral movement. |
| Private-market commitment roll-forward | Commitment terms, recallable rules, fee/carry terms | Calls, distributions, NAV statements, transfers | Paid-in, unfunded, recallable, NAV, DPI/TVPI inputs. |
| Loan interest | Rate type, spread, reset calendar, day count, repayment schedule | Drawn balance, repayment transactions, rate fixings | Interest accrual, payment due, effective borrowing cost. |
| LTV/buying power | Collateral eligibility, haircut rules, facility limits, concentration caps | Pledged positions, market values, loan balance | Eligible collateral, LTV, margin deficit, buying power. |
| Insurance surrender value | Policy terms, surrender charges, premium schedule, policy-loan terms | Policy value, premiums paid, loan balance, surrender event | Net surrender value and cash proceeds. |
| Tax withholding | Tax category, income type, jurisdiction, treaty eligibility | Client/account tax profile, income transaction | Withholding tax, reclaim receivable, net income. |

### Instrument Static Quality Rules

1. Every instrument used in a transaction or position must have a valid `instrument_id` and product classification.
2. Calculation-critical fields must carry source, effective date and quality state.
3. If a calculation field is missing, the app should produce a degraded result with reason code instead of silently guessing.
4. For historical transactions, calculations should use the terms version effective on the transaction or event date.
5. Corporate-action terms should be stored as lifecycle events and linked to instrument static where they permanently alter terms.
6. Fund, private-market and policy values require valuation date and source confidence; stale values must be visible.
7. Structured products and derivatives require explicit underlying identifiers and payoff/contract terms before risk or scenario analytics can be trusted.
8. Product display names should never be parsed to infer coupon rate, maturity, barrier, leverage or payoff.

### Instrument Static Examples

#### Bond Static Example

| Field | Example |
|---|---|
| `instrument_type` | `BOND` |
| `currency` | `USD` |
| `coupon_type` | `FIXED` |
| `coupon_rate` | `5.25%` |
| `coupon_frequency` | `SEMI_ANNUAL` |
| `day_count` | `30/360` |
| `maturity_date` | `2029-01-15` |
| `price_basis` | `PERCENT_OF_PAR` |
| `call_schedule` | Callable at 101 from `2027-01-15` |
| Calculation use | Accrued interest, coupon cashflows, yield, duration, redemption proceeds. |

#### Fund Share-Class Static Example

| Field | Example |
|---|---|
| `instrument_type` | `FUND_SHARE_CLASS` |
| `nav_currency` | `USD` |
| `dealing_frequency` | `DAILY` |
| `cut_off_time` | `13:00 fund domicile time` |
| `settlement_cycle` | `T+3` |
| `distribution_policy` | `ACCUMULATING` |
| `swing_pricing_flag` | `true` |
| Calculation use | Subscription units, redemption proceeds, NAV valuation, liquidity state. |

#### Structured Note Static Example

| Field | Example |
|---|---|
| `instrument_type` | `STRUCTURED_NOTE` |
| `issuer_id` | `ISSUER-XYZ` |
| `notional_currency` | `USD` |
| `payoff_type` | `AUTOCALLABLE_REVERSE_CONVERTIBLE` |
| `underlying_ids` | Basket of equity identifiers |
| `strike_levels` | Per underlying |
| `barrier_level` | `60%` |
| `coupon_condition` | Coupon paid if all underlyings above coupon barrier |
| `observation_schedule` | Monthly observations |
| `settlement_mode` | `CASH_OR_PHYSICAL` |
| Calculation use | Coupon eligibility, autocall, downside payoff, physical settlement, scenario analytics. |

#### Loan Facility Static Example

| Field | Example |
|---|---|
| `instrument_type` | `LOAN_FACILITY` |
| `facility_currency` | `USD` |
| `limit_amount` | `1000000` |
| `rate_type` | `FLOATING` |
| `reference_rate` | `SOFR` |
| `spread` | `1.50%` |
| `interest_day_count` | `ACT/360` |
| `collateral_policy_id` | `COLL-POLICY-001` |
| Calculation use | Interest accrual, available credit, LTV, margin call, repayment schedule. |

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

## Position Architecture For All Product Families

Positions should be modelled as state snapshots derived from transactions, valuations, lifecycle events and source positions. A robust portfolio-management app needs more than one position number. It needs legal position, cash state, cost lots, valuation, exposure, collateral eligibility, restrictions, source confidence and operational state.

### Position Layers

| Layer | What It Answers | Record Type | Example |
|---|---|---|---|
| Legal holding | What does the client legally own or owe? | `position_snapshot` | 10,000 shares, USD 100,000 bond nominal, USD 250,000 loan liability |
| Cash balance | What currency cash is settled, pending, available or restricted? | `cash_balance_snapshot` | USD settled cash, SGD pending receivable |
| Cost lot | What acquisition lots remain open and what is their cost basis? | `position_lot` | Equity lot acquired on trade date with FIFO remaining quantity |
| Valuation | What is the position worth under a defined valuation basis? | `valuation_snapshot` plus position valuation fields | Exchange price, NAV, custodian value, appraisal, model value |
| Accrual | What income or expense has accrued but not paid? | Accrual transaction or accrual balance | Bond coupon accrued income, loan interest payable |
| Commitment/obligation | What future funding or obligation exists? | `COMMITMENT_OR_OBLIGATION` | Private fund unfunded commitment, capital call payable |
| Liability | What is owed? | `LOAN_LIABILITY` or payable position | Drawn credit line, margin loan, policy loan |
| Collateral | What is pledged, eligible or restricted? | `COLLATERAL_PLEDGE` | Bond pledged against Lombard facility |
| Exposure | What market, issuer, currency, duration, delta or look-through exposure exists? | `exposure_snapshot` | Equity delta from option, underlying exposure from structured note |
| Reporting state | Can this position be shown in client reports and with what disclosure? | Reporting classification on position/snapshot | Estimated NAV, stale price, manual valuation |
| Operational state | Is it reconciled, disputed, restricted, pending or unsupported? | Position state and reconciliation break | Custody mismatch, stale NAV, pending transfer |

### Position Component Matrix

| Component | Required For | Must Include | Common Mistake |
|---|---|---|---|
| Quantity | Equities, funds, ETFs, REITs, derivatives, units, metals | Quantity type, unit basis, sign convention, source | Treating units and nominal as the same. |
| Nominal | Bonds, notes, loans, deposits | Principal/face/par basis, currency, amortization state | Using market value as nominal. |
| Notional | Derivatives, FX forwards, swaps, structured products | Notional currency, multiplier, payoff/exposure basis | Treating notional as client market value. |
| Market value | All valued positions | Currency, valuation date, source, basis, FX rate | Showing value without stale/estimated flag. |
| Cost basis | Taxable/reportable investments | Cost method, lot link, currency, adjustments | Recomputing from average price when lot rules require FIFO/specific ID. |
| Accrued income | Bonds, deposits, loans, money market, private debt | Accrual start/end, day-count, currency, tax treatment | Double counting accrued income in both price and separate income. |
| Commitment | Private markets, credit facilities, real assets | Commitment amount, funded amount, unfunded amount, recallable amount | Treating commitment as current market value. |
| Drawn liability | Loans, margin, overdrafts, policy loans | Drawn amount, rate, maturity, collateral link | Netting borrowing against investments without trace. |
| Collateral pledge | Lending, margin, derivatives | Pledged asset, facility link, haircut, eligible value, restriction | Treating pledged assets as unavailable without showing reason. |
| Policy values | Insurance and annuities | Cash value, surrender value, benefit value, policy loan, premium status | Mixing surrender value and death benefit. |
| NAV/appraisal | Funds, private markets, real assets | NAV/appraisal date, source, confidence, lag | Treating old NAV as current without disclosure. |
| Restriction | Locked, pledged, sanctioned, gated, suspended assets | Restriction type, reason, authority, start/end | Hiding restrictions until trade fails. |
| Supportability | Source-limited, manual, estimated, unsupported states | Reason code, owner, evidence, report behavior | Presenting weak data as fully supported. |

### Position State Dimensions

Use multiple state fields because a position can be active, pledged, stale-priced and reportable at the same time.

| State Dimension | Typical Values | Applies To | Why It Matters |
|---|---|---|---|
| Holding state | `ACTIVE`, `PENDING_OPEN`, `PENDING_CLOSE`, `CLOSED`, `MATURED`, `EXPIRED` | All positions | Explains legal/economic existence. |
| Settlement state | `SETTLED`, `PENDING_RECEIVE`, `PENDING_DELIVER`, `PARTIAL`, `FAILED` | Securities, cash, funds, FX, derivatives | Separates trade date from finality. |
| Valuation state | `CURRENT`, `STALE`, `ESTIMATED`, `MODELLED`, `MANUAL`, `MISSING` | All valued positions | Drives reporting confidence and analytics controls. |
| Reconciliation state | `MATCHED`, `UNMATCHED`, `TOLERANCE_MATCHED`, `DISPUTED`, `UNDER_REVIEW` | Book/custody/source positions | Drives operational breaks. |
| Liquidity state | `LIQUID`, `NOTICE_PERIOD`, `LOCKED_UP`, `GATED`, `SUSPENDED`, `ILLIQUID` | Funds, private markets, real assets, structured products | Drives advice, reporting and rebalancing feasibility. |
| Restriction state | `NONE`, `PLEDGED`, `BLOCKED`, `COURT_ORDERED`, `SANCTIONED`, `IN_TRANSFER`, `CLIENT_RESTRICTED` | All positions | Prevents incorrect trading or withdrawal availability. |
| Collateral state | `NOT_PLEDGED`, `PLEDGED`, `ELIGIBLE`, `INELIGIBLE`, `UNDER_MARGIN_CALL`, `RELEASE_PENDING` | Lending, derivatives, margin | Drives borrowing capacity and margin controls. |
| Supportability state | `SUPPORTED`, `SOURCE_LIMITED`, `MANUAL`, `ESTIMATED`, `UNSUPPORTED` | All product families | Tells APIs/UI/reports how much confidence to place in the record. |
| Reporting state | `REPORTABLE`, `REPORTABLE_WITH_DISCLOSURE`, `SUPPRESSED`, `INTERNAL_ONLY` | Client reports/statements | Avoids misleading client-facing output. |

### Product-Specific Position Treatment

| Product Area | Position To Store | Key Fields | Valuation Basis | Special Handling |
|---|---|---|---|---|
| Currency cash | `CASH_BALANCE` per account/currency | Settled, pending receivable, pending payable, available, restricted, overdraft | Par currency amount | Do not net currencies; preserve value date and restriction reason. |
| Term deposits | `DEPOSIT_POSITION` plus cash | Principal, rate, maturity, accrued interest, break cost | Principal plus accrued or fair value depending reporting basis | Placement and maturity are transactions; rate reset is lifecycle/reference data unless value changes. |
| Money market funds | `FUND_POSITION` | Units, NAV, accrued/distributed income, liquidity | NAV | Treat as fund position, not cash, unless platform has explicit cash-equivalent reporting rule. |
| FX spot | Cash balances | Bought/sold currency cash, value date, FX rate | Currency amount plus translated reporting value | No security position; two cash legs. |
| FX forwards/swaps/NDFs | `DERIVATIVE_CONTRACT` | Notional, maturity, forward rate, MTM, settlement currency | Mark-to-market/model value | Separate notional exposure from MTM and settlement cash. |
| Direct equities | `SECURITY_POSITION` | Shares, price, market value, lot cost, restrictions | Exchange price | Corporate actions must update quantity/lots without fake buy/sell. |
| Bonds | `SECURITY_POSITION` | Nominal, clean price, dirty price, accrued income, yield, maturity | Price percent of par plus accrued treatment | Keep nominal, market value and accrued interest separate. |
| Structured notes | `STRUCTURED_PRODUCT_POSITION` | Nominal, issuer, payoff, observations, barrier state, valuation | Issuer/custodian/model value | Observation state is lifecycle; exposure may reference underlying basket. |
| Mutual funds/ETFs | `FUND_POSITION` or `SECURITY_POSITION` | Units, NAV/price, share class, dealing status | NAV or exchange price | Pending subscriptions/redemptions must not be treated as settled units. |
| Hedge funds | `FUND_POSITION` | Units/shares, NAV date, lockup, gates, side pockets | Administrator NAV | Stale NAV and liquidity restrictions must be explicit. |
| Private equity/private credit | `PRIVATE_MARKET_INTEREST` and `COMMITMENT_OR_OBLIGATION` | Commitment, paid-in, unfunded, recallable, NAV, distributions | Manager NAV, often lagged | Commitment roll-forward is as important as NAV. |
| Real estate/direct real assets | `REAL_ASSET_HOLDING` | Ownership share, appraisal value, income, expenses, debt link | Appraisal or manager valuation | Valuation date and confidence must be visible. |
| Commodities/precious metals | `REAL_ASSET_HOLDING`, `SECURITY_POSITION`, or `FUND_POSITION` | Unit type, weight, purity, storage, certificate/account form | Spot, custodian, ETP price | Ounces, grams, bars, certificates and ETP units are not interchangeable. |
| Listed derivatives | `DERIVATIVE_CONTRACT` | Contracts, multiplier, strike, expiry, premium, MTM, margin | Exchange or model MTM | Position quantity and underlying exposure are separate. |
| OTC derivatives/swaps | `DERIVATIVE_CONTRACT` | Notional, reset dates, MTM, collateral, counterparty | Model/counterparty/collateralized value | Counterparty exposure and collateral state are required. |
| Loans/margin/Lombard | `LOAN_LIABILITY` | Drawn amount, limit, rate, maturity, collateral link, LTV | Outstanding liability plus accrued interest | Do not hide liability by netting against collateral. |
| Collateral | `COLLATERAL_PLEDGE` linked to asset and facility | Pledged quantity/value, haircut, eligible value, restriction | Haircut-adjusted eligible value | Asset remains a holding but has restricted/collateral state. |
| Insurance/annuities | `INSURANCE_POLICY` | Premium, cash value, surrender value, benefit value, policy loan, beneficiaries | Insurer value or actuarial basis | Policy value, benefit value and surrender value are different. |
| Trust/estate/family office vehicles | Underlying position plus ownership/authority context | Beneficial owner, legal owner, authority, distribution rules | Depends on underlying asset | Authority changes are governance records, not economic transactions. |
| Tax lots/reporting balances | `position_lot` or receivable/payable | Cost basis, acquisition date, tax status, reclaim receivable | Tax/accounting policy basis | Tax view should reconcile to economic transaction view. |

### Position Relationship Model

Positions should be connected to the records that explain them:

```text
account / portfolio
  -> position_snapshot as of date
       -> instrument / policy / facility / currency
       -> source_position_id and source_system
       -> latest valuation_snapshot
       -> open position_lot records
       -> transaction_leg records that changed the position
       -> lifecycle_thread records for corporate actions and lifecycle changes
       -> restriction / pledge / collateral relationship
       -> exposure_snapshot records for analytics
       -> reconciliation_break records where source/book/report differ
```

For implementation, keep these relationships explicit:

| Relationship | Key Fields | Use |
|---|---|---|
| Position to instrument | `position.instrument_id` | Product terms, valuation basis, identifiers and exposure mapping. |
| Position to account | `position.account_id` | Legal booking, custody, cash and entitlement eligibility. |
| Position to portfolio | `position.portfolio_id` | Reporting and analytics grouping. |
| Position to transaction legs | `position_id`, `instrument_id`, `account_id`, date range, `leg_type` | Roll-forward, audit trail and reconciliation. |
| Position to lots | `position_id`, `lot_id` | Cost basis, realized P&L and tax treatment. |
| Position to valuation | `instrument_id`, `valuation_date`, `valuation_basis`, currency | Market value and stale-price controls. |
| Position to collateral/facility | `pledge_id`, `facility_id`, pledged `position_id` | LTV, margin, eligibility and restrictions. |
| Position to exposure | `position_id`, `exposure_snapshot_id` | Look-through, risk, concentration and stress testing. |
| Position to source evidence | `source_system`, `source_position_id`, `lineage_id` | Data quality, lineage and operating support. |

### Position Snapshot Versus Position Delta

Use both concepts:

| Concept | Purpose | Source |
|---|---|---|
| Position snapshot | Current or as-of state for screens, reports and APIs. | Custody/book snapshot or roll-forward output. |
| Position delta | Change caused by a transaction leg or lifecycle event. | Transaction legs and corporate-action processing. |
| Reconciled position | Position after comparing source, book, custody, ledger and report outputs. | Reconciliation engine. |
| Derived position | View derived for analytics, such as exposure, look-through or risk contribution. | Analytics engine. |

Do not store only snapshots without deltas. Without deltas, the app cannot explain why holdings changed. Do not store only deltas without snapshots. Without snapshots, the app becomes slow and fragile for reporting and user experience.

### Position Sign Conventions

Define sign conventions once and enforce them in validation:

| Position Type | Positive Means | Negative Means | Notes |
|---|---|---|---|
| Asset/security/fund | Client owns asset. | Short position or delivery obligation if allowed. | Shorting should be explicitly supported before negative holdings are allowed. |
| Cash | Client has cash balance. | Overdraft or payable cash balance. | Available cash may be lower than settled cash because of restrictions. |
| Loan liability | Client owes money. | Usually invalid unless representing overpayment/credit. | Keep liability sign convention clear in APIs and UI. |
| Derivative MTM | Positive value to client. | Negative value to client. | Notional sign and MTM sign are different. |
| Commitment | Client has remaining obligation. | Usually invalid; reductions should lower amount. | Recallable distributions may increase unfunded/recallable obligation. |
| Collateral pledge | Amount pledged/restricted. | Usually invalid. | Asset holding remains, but available/unpledged amount decreases. |
| Policy value | Value attributable to policy. | Usually invalid. | Policy loan should be separate liability, not negative policy value. |

### Position Examples

#### Bond Position

| Attribute | Example |
|---|---|
| Position type | `SECURITY_POSITION` |
| Quantity basis | `nominal_amount = 100000`, `quantity_type = NOMINAL` |
| Value | Clean price, accrued income, dirty value and reporting market value separated. |
| Lots | Acquisition lot tracks cost, premium/discount and disposal basis. |
| Lifecycle links | Coupon, call, amortization, maturity, default and rating events. |
| Common failure | Reporting dirty value as price or double-counting accrued income. |

#### Private Market Position

| Attribute | Example |
|---|---|
| Position types | `PRIVATE_MARKET_INTEREST` plus `COMMITMENT_OR_OBLIGATION` |
| Quantity basis | Commitment and ownership percentage, not exchange-traded units. |
| Value | Latest manager NAV with NAV date and confidence state. |
| Cashflows | Capital calls, distributions, recallable distributions, fees and tax. |
| Lifecycle links | Commitment, capital-call notices, distribution notices, NAV statements. |
| Common failure | Showing NAV only and losing unfunded commitment and liquidity obligation. |

#### Derivative Position

| Attribute | Example |
|---|---|
| Position type | `DERIVATIVE_CONTRACT` |
| Quantity basis | Contracts or notional, multiplier, underlying, expiry, strike/rate. |
| Value | MTM/fair value separate from notional exposure. |
| Cashflows | Premium, margin, settlement cash, fees and collateral. |
| Lifecycle links | Trade, margin call, exercise, expiry, settlement, reset/fixing. |
| Common failure | Treating notional as market value or hiding margin/collateral. |

#### Collateralized Loan Position

| Attribute | Example |
|---|---|
| Position types | `LOAN_LIABILITY`, `COLLATERAL_PLEDGE`, underlying asset positions |
| Liability | Drawn amount, limit, interest rate, maturity and facility state. |
| Collateral | Pledged assets, market value, haircut, eligible value and restrictions. |
| Cashflows | Drawdown, repayment, interest, margin call, collateral release. |
| Lifecycle links | Facility approval, drawdown approval, margin-call notice, pledge/release. |
| Common failure | Netting loan against portfolio value without showing borrowing and pledge details. |

#### Insurance Policy Position

| Attribute | Example |
|---|---|
| Position type | `INSURANCE_POLICY` |
| Values | Cash value, surrender value, death/benefit value, premium due, policy loan. |
| Cashflows | Premiums, fees, surrender, claims, policy-loan drawdowns/repayments. |
| Lifecycle links | Policy issue, beneficiary change, premium due, surrender request, claim approval. |
| Common failure | Showing benefit value as liquid portfolio market value. |

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

### Transaction Leg Sign And Amount Conventions

Define sign conventions once and enforce them across ingestion, APIs, ledger posting, reporting and reconciliation. The examples below use client/account perspective.

| Leg Type | Positive Means | Negative Means | Required Basis | Example |
|---|---|---|---|---|
| `SECURITY` | Client holding increases. | Client holding decreases. | Quantity type, instrument, account, trade/effective date. | Buy `+100` shares; sell `-100` shares. |
| `CASH` | Cash balance increases. | Cash balance decreases. | Currency, value date, settlement state. | Dividend cash `+85`; purchase cash `-18,500`. |
| `INCOME` | Income earned or receivable by client. | Income reversed or reclassified away. | Income type, gross/net basis, tax treatment. | Coupon income `+2,500`. |
| `FEE` | Fee rebate or fee receivable. | Fee charged to client. | Fee type, currency, tax treatment. | Custody fee `-25`. |
| `TAX` | Tax refund/reclaim receivable or cash credit. | Tax withheld or tax payable. | Tax type, jurisdiction/profile basis. | Withholding tax `-15`; reclaim `+15`. |
| `ACCRUAL` | Accrued income/receivable increases. | Accrual reverses or accrued payable increases depending amount type. | Accrual period, day count, income/expense basis. | Bond accrued income `+120`; coupon payment reversal `-120`. |
| `MARGIN` | Margin returned or margin receivable. | Margin posted or margin payable. | Margin type, collateral/cash basis, value date. | Futures variation margin `-1,000`. |
| `COLLATERAL` | Collateral availability or release increases. | Collateral pledged/restricted increases. | Pledge ID, facility ID, haircut basis. | Pledge bond collateral `-100,000 eligible value`; release `+100,000`. |
| `LIABILITY` | Liability decreases if using asset-perspective signs, or liability increases if using liability-positive convention. | Opposite of chosen convention. | Convention must be explicit by API and ledger policy. | Prefer separate `liability_delta` when possible. |
| `LOT` | Cost basis/open lot increases. | Cost basis/open lot decreases. | Lot ID, cost method, currency, acquisition/disposal date. | Buy creates cost basis `+18,522.50`; sale closes cost basis `-9,000`. |

Recommended convention:

1. use positive `quantity_delta` for more client-owned asset quantity and negative for less,
2. use positive `cash_delta` for cash credited to the client account and negative for cash debited,
3. use explicit `amount_type` for principal, income, fee, tax, accrued, P&L, margin, collateral and liability,
4. avoid relying on a single generic `amount` for calculations,
5. keep ledger debit/credit separate from business signs because accounting signs depend on ledger policy.

### Leg Pairing Patterns

| Business Event | Product-Side Legs | Cash-Side Legs | Other Legs |
|---|---|---|---|
| Buy security | `SECURITY +quantity`, `LOT +cost_basis` | `CASH -settlement_amount` | `FEE`, `TAX` |
| Sell security | `SECURITY -quantity`, `LOT -cost_basis` | `CASH +proceeds` | `FEE`, `TAX`, `PNL` |
| Dividend | Usually none | `CASH +net_payment` | `INCOME +gross`, `TAX -withholding` |
| Bond coupon | Usually none | `CASH +net_payment` | `INCOME +coupon`, `TAX`, `ACCRUAL -reversal` |
| Bond maturity | `SECURITY -nominal`, `LOT -cost_basis` | `CASH +principal` | Optional `INCOME` final coupon |
| Fund subscription | `SECURITY/FUND +units`, `LOT +cost_basis` | `CASH -subscription_amount` | `FEE` |
| Fund redemption | `SECURITY/FUND -units`, `LOT -cost_basis` | `CASH +redemption_proceeds` | `FEE`, `TAX`, `PNL` |
| FX conversion | None unless derivative contract exists | `CASH -sold_currency`, `CASH +bought_currency` | Realized FX P&L if policy requires |
| Capital call | `COMMITMENT -unfunded`, `INVESTMENT +paid_in` | `CASH -funded_amount` | `FEE` or `TAX` if applicable |
| Loan drawdown | `LIABILITY +drawn_amount` under liability-positive convention | `CASH +drawdown_proceeds` | Fees if charged |
| Collateral pledge | `COLLATERAL -available_or +pledged` depending convention | Optional restricted cash | Link to facility and pledged position |

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
| `INTERNAL_TRANSFER` | Movement between internal accounts, sleeves, portfolios or sub-accounts where ownership/reporting context changes. | Debit one internal cash/asset position and credit another. | Move cash from advisory account to discretionary portfolio cash sleeve. |
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

## Transaction Type Selection Guide

Use this guide when mapping source events, custody feeds, order-management events, accounting postings or migration records into the canonical transaction model.

| Business Case | Use Transaction Type | Product Families | Required Evidence | Required Legs | Do Not Use |
|---|---|---|---|---|---|
| Client buys a traded instrument in the market | `BUY` | Equities, bonds, ETFs, listed funds, listed REITs, listed derivatives, precious-metal certificates | Order/execution/contract note | Security, cash, fee, tax, lot | Product-specific types such as `EQUITY_BUY` or `BOND_BUY` |
| Client sells a traded instrument in the market | `SELL` | Equities, bonds, ETFs, listed funds, listed REITs, listed derivatives, precious-metal certificates | Order/execution/contract note | Security, cash, fee, tax, realized P&L, lot close | `REDEEM`, unless issuer/platform redemption rather than market sale |
| Client enters a product through issuer or fund dealing process | `SUBSCRIBE` | Funds, structured products, private funds, deposits, insurance wrappers, alternatives | Subscription order, acceptance, NAV/terms confirmation | Cash, security/fund/policy/deposit, fee, lot | `BUY`, when there is no market execution |
| Client exits through issuer, fund administrator or product terms | `REDEEM` | Funds, structured products, deposits, insurance, private funds, notes | Redemption request, acceptance, NAV/terms confirmation | Security/fund/policy/deposit, cash, fee, tax, lot close | `SELL`, when exit is not a market sale |
| Asset matures under contractual terms | `MATURITY` | Bonds, deposits, structured products, loans, policies, derivatives | Maturity schedule and source confirmation | Principal/security/liability, cash, income/tax if applicable | `REDEEM`, unless issuer called or redeemed before final contractual maturity |
| Issuer calls or product redeems before final maturity | `REDEMPTION` | Bonds, structured notes, funds, preferreds, deposits | Call/redemption notice and confirmation | Position decrease, cash, income/tax if applicable | `MATURITY`, unless final maturity event |
| Cash is externally funded into account | `CASH_DEPOSIT` | Cash accounts, settlement accounts, savings/current accounts | Payment instruction or bank statement | Cash | `TRANSFER_IN`, unless receiving asset transfer in-kind |
| Cash leaves account externally | `CASH_WITHDRAWAL` | Cash accounts, settlement accounts, savings/current accounts | Payment instruction or bank statement | Cash | `TRANSFER_OUT`, unless sending assets in-kind |
| Cash moves between internal accounts or sleeves | `INTERNAL_TRANSFER` or `CASH_SWEEP` | Cash, money market, treasury, advisory sleeves | Transfer instruction or sweep rule | Cash debit and cash credit | External deposit/withdrawal types |
| Currency is converted | `FX_CONVERSION` | Cash, FX, multi-currency portfolios | FX confirmation, value date and rate | Two cash legs | Separate buy/sell security transactions |
| FX forward, swap or NDF reaches settlement | `FX_SETTLEMENT` or `DERIVATIVE_SETTLEMENT` | FX forwards, swaps, NDFs, structured FX | Trade confirmation and settlement statement | Cash, derivative close/MTM, P&L where applicable | `FX_CONVERSION` when derivative contract lifecycle matters |
| Interest, coupon, dividend or distribution is received | `INTEREST`, `COUPON`, `DIVIDEND`, `DISTRIBUTION` | Cash, deposits, bonds, equities, funds, private markets, REITs, real assets | Entitlement, record date, payment confirmation | Income, cash/receivable, tax, accrual reversal | `INCOME` only if source cannot classify more specifically |
| Amount is earned but not paid | `ACCRUAL` | Bonds, deposits, loans, fees, private debt, money market | Accrual policy and calendar | Accrual receivable/payable | Cash income types before payment or entitlement |
| Accrued amount is paid or corrected | `ACCRUAL_REVERSAL` plus actual income/expense type | Bonds, deposits, loans, fees | Accrual schedule and payment event | Accrual reversal, cash/income | Destructive update to prior accrual |
| Fee is charged | `FEE` | Advisory, custody, platform, fund, insurance, loans, transactions | Fee schedule, invoice, posting | Expense, cash/payable, tax if applicable | `TAX`, unless the economic nature is tax |
| Tax is withheld or charged | `TAX` | Income, trading, reporting, withholding, stamp duty | Tax rule/source posting | Tax, cash/payable, income reduction | `FEE`, unless charged by service provider |
| Tax is refunded or reclaimed | `TAX_RECLAIM` | Cross-border income, withholding, regulatory tax | Reclaim approval/refund statement | Tax receivable/cash | Negative `TAX` without reclaim evidence |
| Stock split or unit consolidation changes quantity | `SPLIT_OR_CONSOLIDATION` | Equities, ETFs, funds, REITs, warrants | Corporate-action notice | Security quantity, lot adjustment | `BUY` or `SELL`, because no market trade occurred |
| Corporate-action election creates cash or securities | `CORPORATE_ACTION`, `RIGHTS_EXERCISE`, `STOCK_DIVIDEND` | Equities, bonds, funds, structured products | Election and processing confirmation | Security, cash, lot, tax | Generic `BUY` unless client truly bought in market |
| Option premium is paid or received | `DERIVATIVE_PREMIUM` | Options, warrants, structured overlays | Trade confirmation | Derivative contract, cash | `BUY`/`SELL` if premium and contract lifecycle need derivative treatment |
| Derivative settles or expires | `DERIVATIVE_SETTLEMENT`, `OPTION_EXERCISE`, `OPTION_EXPIRY` | Options, futures, forwards, swaps, NDFs | Settlement/expiry/exercise confirmation | Derivative close, cash or underlying, P&L | `SELL` if not a market closing sale |
| Margin is posted or returned | `DERIVATIVE_MARGIN` or `MARGIN_CALL` | Futures, OTC derivatives, margin lending, collateralized products | Margin call, statement or collateral notice | Cash/collateral, payable/receivable | Fee or tax types |
| Asset is pledged against a facility | `COLLATERAL_PLEDGE` | Loans, Lombard, margin, derivatives, structured lending | Pledge agreement/collateral instruction | Collateral restriction/pledge leg | `TRANSFER_OUT`, unless asset legally leaves account |
| Collateral is released | `COLLATERAL_RELEASE` | Loans, Lombard, margin, derivatives | Release notice | Pledge release leg | `TRANSFER_IN`, unless asset legally returns from external custody |
| Loan is drawn | `LOAN_DRAWDOWN` | Credit lines, Lombard, margin loans, policy loans | Facility/drawdown confirmation | Liability, cash | `CASH_DEPOSIT`, because cash came with liability |
| Loan principal is repaid | `LOAN_REPAYMENT` | Credit lines, Lombard, margin loans, policy loans | Repayment instruction/statement | Liability, cash | `CASH_WITHDRAWAL`, because cash reduces liability |
| Private-market commitment is created | `COMMITMENT` | Private equity, private credit, real assets, alternatives | Subscription agreement/commitment notice | Commitment/obligation | `SUBSCRIBE` alone, because unfunded obligation must be tracked |
| Private-market capital is called | `CAPITAL_CALL` | Private equity, private credit, real assets, alternatives | Capital-call notice and payment evidence | Cash, paid-in capital, commitment reduction | `BUY`, because investment is funded against commitment |
| Private-market distribution may be recalled | `RECALLABLE_DISTRIBUTION` | Private equity, private credit, alternatives | Distribution notice | Cash/in-kind, recallable commitment | Plain `DISTRIBUTION` if recallable obligation exists |
| Insurance premium is paid | `INSURANCE_PREMIUM` | Insurance, annuities, investment-linked policies | Premium notice/policy statement | Cash, policy value/coverage state | `FEE`, unless premium is explicitly an expense-only charge |
| Insurance policy is surrendered | `INSURANCE_SURRENDER` | Insurance, annuities, investment-linked policies | Surrender confirmation | Policy reduction/close, cash, fee, tax | `REDEEM`, unless platform intentionally treats policy as redeemable wrapper |
| Trust, estate or family-office vehicle distributes value | `TRUST_DISTRIBUTION` | Trusts, estates, foundations, holding companies | Trustee/administrator instruction | Cash/in-kind asset, beneficiary context, tax if applicable | Generic `CASH_WITHDRAWAL` if beneficiary/source context matters |
| Opening records are loaded during migration | `MIGRATION_OPENING_BALANCE` | All product families | Migration source, sign-off and reconciliation evidence | Position, cash, lot, liability or policy opening state | Backdated synthetic buys/sells unless source proves historical transaction detail |
| Source corrects prior economic record | `REVERSAL` plus replacement type or `CORRECTION` | All product families | Source correction, operations approval | Equal/opposite reversal and corrected legs | Overwriting the original transaction |

## Product Family Coverage Matrix

This matrix shows how the same canonical model covers all major product families. It is a design aid for portfolio-management applications: product-specific detail belongs in instrument attributes, lifecycle events, legs, valuation basis, risk/exposure snapshots and supportability state.

| Product Area | Main Position Types | Common Lifecycle Events | Transaction Types To Use | Critical Modelling Notes |
|---|---|---|---|---|
| Cash and current accounts | `CASH_BALANCE` | Payment received, payment sent, transfer approved, overdraft used, balance restricted | `CASH_DEPOSIT`, `CASH_WITHDRAWAL`, `INTERNAL_TRANSFER`, `CASH_SWEEP`, `FEE`, `TAX` | Separate settled, pending, available, restricted and overdraft components by currency and value date. |
| Deposits and money market | `DEPOSIT_POSITION`, `CASH_BALANCE`, `FUND_POSITION` | Placement, rollover, rate reset, interest accrual, maturity, early break | `DEPOSIT_PLACEMENT`, `INTEREST`, `ACCRUAL`, `DEPOSIT_MATURITY`, `FEE`, `TAX`, `CASH_SWEEP` | Deposit principal, accrued interest, break cost and maturity proceeds should reconcile to cash. |
| FX spot and cash conversion | `CASH_BALANCE` | FX order, execution, confirmation, settlement, cancellation | `FX_CONVERSION`, `FX_SETTLEMENT`, `REVERSAL`, `CORRECTION` | Model as two currency cash legs with value date, rate basis and realized FX policy. |
| FX forwards, swaps and NDFs | `DERIVATIVE_CONTRACT`, `CASH_BALANCE` | Trade, fixing, margin, valuation, settlement, expiry | `DERIVATIVE_PREMIUM`, `DERIVATIVE_MARGIN`, `FX_SETTLEMENT`, `DERIVATIVE_SETTLEMENT`, `OPTION_EXPIRY` | Separate derivative contract state from settlement cash and currency exposure. |
| Bonds and fixed income | `SECURITY_POSITION` | Trade, settlement, accrued interest, coupon, call, maturity, default, rating change | `BUY`, `SELL`, `COUPON`, `ACCRUAL`, `ACCRUAL_REVERSAL`, `REDEMPTION`, `MATURITY`, `WRITE_DOWN`, `TAX` | Track nominal, clean/dirty price, accrued interest, yield basis, credit events and lot/cost treatment. |
| Structured notes and certificates | `STRUCTURED_PRODUCT_POSITION` | Subscription, issue, observation, barrier event, coupon, autocall, maturity, physical settlement | `SUBSCRIBE`, `COUPON`, `REDEMPTION`, `MATURITY`, `DERIVATIVE_SETTLEMENT`, `IN_KIND_DISTRIBUTION`, `CORRECTION` | Observation events are lifecycle evidence; transactions occur when coupon, redemption, settlement or delivery is booked. |
| Equities | `SECURITY_POSITION` | Trade, settlement, dividend record/pay date, split, merger, rights, delisting | `BUY`, `SELL`, `DIVIDEND`, `SPLIT_OR_CONSOLIDATION`, `STOCK_DIVIDEND`, `RIGHTS_EXERCISE`, `CORPORATE_ACTION`, `TAX` | Corporate actions must preserve quantity, cost-basis and entitlement lineage. |
| ETFs, mutual funds and pooled funds | `FUND_POSITION`, `SECURITY_POSITION` | Subscription, redemption, NAV received, distribution, fee, gate/suspension, switch | `SUBSCRIBE`, `REDEEM`, `DISTRIBUTION`, `RETURN_OF_CAPITAL`, `FEE`, `TAX`, `RESTATEMENT_ADJUSTMENT` | Pending cash and pending units should stay separate until administrator confirmation. |
| Hedge funds and alternatives | `FUND_POSITION`, `PRIVATE_MARKET_INTEREST`, `COMMITMENT_OR_OBLIGATION` | Subscription, lock-up, NAV lag, redemption notice, gate, distribution | `SUBSCRIBE`, `REDEEM`, `DISTRIBUTION`, `RETURN_OF_CAPITAL`, `NAV_ADJUSTMENT`, `RESTATEMENT_ADJUSTMENT` | Liquidity restrictions, lockups, gates and stale NAVs must be explicit supportability/reporting states. |
| Private equity, private credit and private real assets | `PRIVATE_MARKET_INTEREST`, `COMMITMENT_OR_OBLIGATION` | Commitment, capital call, distribution, recallable distribution, NAV update, write-down | `COMMITMENT`, `CAPITAL_CALL`, `DISTRIBUTION`, `RECALLABLE_DISTRIBUTION`, `RETURN_OF_CAPITAL`, `WRITE_DOWN`, `WRITE_OFF`, `NAV_ADJUSTMENT` | Do not collapse commitment, paid-in capital, unfunded commitment, recallable amount and NAV into one amount. |
| Derivatives and overlays | `DERIVATIVE_CONTRACT`, `CASH_BALANCE`, `COLLATERAL_PLEDGE` | Trade, premium, margin, valuation, exercise, expiry, settlement, collateral call | `DERIVATIVE_PREMIUM`, `DERIVATIVE_MARGIN`, `OPTION_EXERCISE`, `OPTION_EXPIRY`, `DERIVATIVE_SETTLEMENT`, `COLLATERAL_PLEDGE`, `COLLATERAL_RELEASE` | Track contract quantity, multiplier, notional, MTM, margin, collateral and underlying exposure separately. |
| Commodities and precious metals | `SECURITY_POSITION`, `FUND_POSITION`, `REAL_ASSET_HOLDING`, `CASH_BALANCE` | Buy/sell, storage fee, delivery, conversion, valuation, collateral pledge | `BUY`, `SELL`, `FEE`, `TRANSFER_IN`, `TRANSFER_OUT`, `COLLATERAL_PLEDGE`, `COLLATERAL_RELEASE`, `RESTATEMENT_ADJUSTMENT` only when book value is corrected | Unit basis matters: ounces, grams, bars, certificates, ETP units and currency settlement are different views. |
| Real estate, REITs and infrastructure | `SECURITY_POSITION`, `FUND_POSITION`, `REAL_ASSET_HOLDING`, `PRIVATE_MARKET_INTEREST` | Trade, rental income, distribution, appraisal, capital call, property expense, sale | `BUY`, `SELL`, `DISTRIBUTION`, `INCOME`, `FEE`, `CAPITAL_CALL`, `NAV_ADJUSTMENT`, `WRITE_DOWN` | Appraisal/NAV date and valuation confidence are first-class reporting fields. |
| Loans, Lombard and margin | `LOAN_LIABILITY`, `COLLATERAL_PLEDGE`, `CASH_BALANCE` | Facility approval, drawdown, repayment, interest, collateral pledge/release, margin call | `LOAN_DRAWDOWN`, `LOAN_REPAYMENT`, `LOAN_INTEREST`, `COLLATERAL_PLEDGE`, `COLLATERAL_RELEASE`, `MARGIN_CALL` | Borrowing, collateral, available cash, buying power and LTV are related but separate states. |
| Insurance and annuities | `INSURANCE_POLICY`, `CASH_BALANCE`, `LOAN_LIABILITY` | Application, premium, policy value update, beneficiary change, loan, surrender, claim, maturity | `INSURANCE_PREMIUM`, `POLICY_LOAN`, `INSURANCE_SURRENDER`, `INSURANCE_CLAIM`, `MATURITY`, `FEE`, `TAX` | Separate premium, cash value, surrender value, death/benefit value, policy loan and beneficiary authority. |
| Trusts, estates and family office structures | `REAL_ASSET_HOLDING`, `SECURITY_POSITION`, `CASH_BALANCE`, `COMMITMENT_OR_OBLIGATION` | Authority change, contribution, distribution, beneficiary payment, asset transfer, valuation | `TRANSFER_IN`, `TRANSFER_OUT`, `TRUST_DISTRIBUTION`, `IN_KIND_DISTRIBUTION`, `CASH_DEPOSIT`, `CASH_WITHDRAWAL`, `TAX` | Authority and beneficial ownership are not transaction types; they are governance/context records. |
| Tax and regulatory reporting | Tax lot, receivable/payable, report snapshot | Withholding, tax reclaim, cost-basis adjustment, document amendment, report filing | `TAX`, `TAX_RECLAIM`, `LOT_ADJUSTMENT`, `RESTATEMENT_ADJUSTMENT` | Filing/report delivery is not a transaction unless it creates economic posting or receivable/payable. |
| Portfolio reporting and performance | Report snapshot, valuation snapshot, exposure snapshot | Valuation, performance run, benchmark update, restatement, report delivery | `RESTATEMENT_ADJUSTMENT` only when economic/book data changes | Most analytics events are snapshots, not transactions. Preserve source timestamp and calculation version. |

## Lifecycle Event To Transaction Mapping

Use this mapping to decide whether a source event should create a transaction, update a state, or only create operational evidence.

| Lifecycle Phase | Event | Record To Create Or Update | Transaction Type When Economic | Example |
|---|---|---|---|---|
| Product setup | Instrument approved, product terms loaded, policy opened | Instrument/product master, approved-product universe, supportability state | None unless opening premium/subscription posts | Structured note terms loaded before issue date. |
| Instruction | Order placed, subscription requested, redemption requested, election submitted | Order/instruction/election | None until accepted/executed/effective | Fund subscription order before NAV cut-off. |
| Control approval | Suitability passed, mandate check passed, credit approval granted | Workflow/control evidence | None | Loan facility approved but undrawn. |
| Execution | Market trade executed | Lifecycle event and transaction | `BUY`, `SELL`, `DERIVATIVE_PREMIUM`, `FX_CONVERSION` | Equity order fills in market. |
| Acceptance | Fund/issuer accepts subscription or redemption | Lifecycle event and pending transaction/state | `SUBSCRIBE`, `REDEEM` when economically effective | Fund administrator accepts subscription. |
| Booking | Book-of-record posts economic event | Transaction, transaction legs, ledger entries | Selected canonical transaction type | Custody posts coupon payment. |
| Settlement | Cash/security settlement confirmed | Settlement state, cash/security finality | Usually update existing transaction; create correction if settlement differs | Bond trade settles T+2. |
| Accrual | Income or expense accrues over time | Accrual transaction or accrual balance | `ACCRUAL` | Daily bond coupon accrual. |
| Entitlement | Coupon/dividend/distribution entitlement arises | Entitlement/receivable state | `ENTITLEMENT_RECEIPT`, then income type when booked | Dividend record date reached. |
| Payment | Cash income/expense paid | Transaction and cash leg | `COUPON`, `DIVIDEND`, `INTEREST`, `DISTRIBUTION`, `FEE`, `TAX` | Dividend paid net of withholding tax. |
| Asset servicing | Corporate action announced | Lifecycle event/evidence | None until elected/processed | Stock split announced. |
| Asset servicing processing | Corporate action processed | Transaction and position/lot legs | `SPLIT_OR_CONSOLIDATION`, `STOCK_DIVIDEND`, `RIGHTS_EXERCISE`, `CORPORATE_ACTION` | Rights issue exercised. |
| Observation | Barrier, fixing, rate, NAV or appraisal observed | Lifecycle event or valuation snapshot | Only if coupon/redemption/settlement posts | Structured note barrier observed. |
| Valuation | Price, NAV, policy value, collateral value received | Valuation snapshot | Usually none; `RESTATEMENT_ADJUSTMENT` if book/reporting basis changes | Fund NAV received. |
| Margin/collateral | Margin call issued, collateral pledged/released | Obligation/collateral state | `MARGIN_CALL`, `DERIVATIVE_MARGIN`, `COLLATERAL_PLEDGE`, `COLLATERAL_RELEASE` | Futures variation margin posted. |
| Commitment funding | Capital-call notice received | Obligation and payable state | `CAPITAL_CALL` when payable/funded | Private equity capital call. |
| Lending | Drawdown, repayment, interest posting | Loan liability and cash/interest records | `LOAN_DRAWDOWN`, `LOAN_REPAYMENT`, `LOAN_INTEREST` | Lombard drawdown credited to cash. |
| Insurance | Premium due, surrender accepted, claim approved | Policy state, cash, liability/benefit records | `INSURANCE_PREMIUM`, `INSURANCE_SURRENDER`, `INSURANCE_CLAIM`, `POLICY_LOAN` | Policy surrender paid to account. |
| Reporting | Statement generated, report delivered, filing submitted | Report snapshot/archive/evidence | None unless economic adjustment posts | Portfolio statement delivered. |
| Exception | Settlement fail, mismatch, stale price, disputed source | Reconciliation break, data-quality state | `CORRECTION`, `REVERSAL`, fee/compensation if economic | Failed FX settlement corrected. |
| Correction | Source correction, NAV restatement, tax amendment | Correction group, reversal/rebook, restatement snapshot | `REVERSAL`, `CORRECTION`, `RESTATEMENT_ADJUSTMENT`, `TAX_RECLAIM` | Incorrect withholding tax reversed and rebooked. |

## Corporate Action Mapping

Corporate actions should be modelled as lifecycle evidence first and transactions only when they create a confirmed economic, cash, position, lot, tax, receivable, payable or ledger effect.

### Corporate Action Processing Stages

| Stage | Record Type | Transaction? | Example |
|---|---|---|---|
| Announcement | `lifecycle_event` | No | Issuer announces stock split, dividend, tender offer or merger terms. |
| Eligibility / record date | Entitlement or lifecycle state | Sometimes | Client becomes entitled to dividend or rights. Book `ENTITLEMENT_RECEIPT` only if accounting/reporting recognizes a receivable or pending asset. |
| Election window | Election/workflow record | No | Advisor/client elects cash vs stock option, tender participation or rights exercise. |
| Election accepted | Lifecycle event | Usually no | Custodian accepts election, but economic outcome is not yet settled. |
| Processing / effective date | Transaction and legs | Yes when economic | Split quantity adjustment, dividend receivable, merger consideration, redemption, tax withholding. |
| Settlement / payment date | Settlement state and cash/security legs | Usually update transaction; create transaction if source only posts at settlement | Cash dividend paid, new shares delivered, tender proceeds credited. |
| Correction / restatement | Correction group | Yes when book changes | Dividend tax corrected, split factor corrected, merger consideration amended. |

### Corporate Action Transaction Type Matrix

Use the most specific generic type available. Use `CORPORATE_ACTION` only when the event does not fit a more specific type or when the platform needs a temporary umbrella type with subtype detail.

| Corporate Action | Use Transaction Type(s) | Product Families | Position Effect | Cash Effect | Lot/Tax Effect | Example |
|---|---|---|---|---|---|---|
| Cash dividend | `DIVIDEND`, `TAX`, optional `ACCRUAL_REVERSAL` | Equities, ETFs, REITs, listed funds | Usually no quantity change | Cash credit net/gross plus tax debit | Income classification and withholding tax | Equity pays USD 1.00 dividend per share with 15% withholding. |
| Stock dividend / bonus issue | `STOCK_DIVIDEND` | Equities, funds, REITs | Quantity increases | Usually none | Cost basis may be reallocated | 1 bonus share for every 10 shares held. |
| Scrip dividend / dividend reinvestment | `DIVIDEND` plus `SUBSCRIBE` or `STOCK_DIVIDEND` | Equities, funds, REITs | Cash dividend elected into units/shares | May be no cash if reinvested directly | Income plus new lot or adjusted basis | Client elects stock instead of cash dividend. |
| Return of capital | `RETURN_OF_CAPITAL` | Funds, REITs, private markets, equities | Usually no quantity change | Cash credit | Reduces cost basis where policy requires | Fund returns capital rather than income. |
| Stock split | `SPLIT_OR_CONSOLIDATION` | Equities, ETFs, funds, REITs | Quantity increases, price basis adjusts | None | Lot quantity and per-unit cost adjust | 2-for-1 split. |
| Reverse split / consolidation | `SPLIT_OR_CONSOLIDATION` | Equities, ETFs, funds, REITs | Quantity decreases, price basis adjusts | Cash-in-lieu possible | Lot quantity and per-unit cost adjust | 1-for-10 reverse split with fractional cash. |
| Fractional cash-in-lieu | `SELL` or `CORPORATE_ACTION` with subtype `FRACTIONAL_CASH_IN_LIEU` | Equities, ETFs, funds, REITs | Fractional quantity removed | Cash credit | Realized P&L/tax basis may be required | Reverse split creates 0.4 fractional share paid in cash. |
| Rights entitlement | `ENTITLEMENT_RECEIPT` if receivable/right is booked; otherwise lifecycle event | Equities, funds, REITs | Rights position may be created | None | Usually no tax until sale/exercise/lapse depending policy | Shareholder receives tradable rights. |
| Rights exercise | `RIGHTS_EXERCISE` | Equities, funds, REITs | Underlying shares/units increase, rights decrease | Subscription cash debit | New lot/cost basis created | Client exercises rights to buy new shares. |
| Rights sale | `SELL` | Equities, funds, REITs | Rights position decreases | Cash credit | Realized P&L/tax may apply | Client sells listed rights. |
| Rights lapse | `WRITE_OFF` or `CORPORATE_ACTION` subtype `RIGHTS_LAPSE` | Equities, funds, REITs | Rights position closes | None | Cost basis/write-off policy applies | Client does not exercise rights before expiry. |
| Warrant exercise | `RIGHTS_EXERCISE` or `OPTION_EXERCISE` | Equities, warrants | Warrant decreases, underlying increases | Strike cash debit | New lot/cost basis includes warrant basis and strike | Client exercises equity warrant. |
| Spin-off / demerger | `CORPORATE_ACTION` plus optional `STOCK_DIVIDEND` / `LOT_ADJUSTMENT` | Equities, funds, listed holding companies | New security position created; parent may adjust | Sometimes cash-in-lieu | Cost basis allocation required | Parent company distributes shares of new company. |
| Merger - cash consideration | `CORPORATE_ACTION` or `REDEMPTION` plus `SELL`-like lot close | Equities, funds, REITs | Old security closes | Cash credit | Realized P&L/tax basis required | Target company acquired for cash. |
| Merger - stock consideration | `CORPORATE_ACTION`, `TRANSFER_OUT`, `TRANSFER_IN`, `LOT_ADJUSTMENT` | Equities, funds, REITs | Old security closes; new security opens | Cash-in-lieu possible | Cost basis transfers/reallocates | Target shares converted into acquirer shares. |
| Mixed cash and stock merger | `CORPORATE_ACTION` plus cash/security legs | Equities, funds, REITs | Old security closes; new security opens | Cash credit | Realized and deferred basis may both apply | Acquisition pays 0.5 new share plus cash per old share. |
| Tender offer accepted | `SELL` or `REDEMPTION` depending legal form | Equities, bonds, funds | Tendered position decreases/closes | Cash credit or new security | Realized P&L/tax basis | Client tenders shares for cash offer. |
| Share buyback / issuer repurchase | `SELL` or `REDEMPTION` | Equities, listed funds, REITs | Position decreases | Cash credit | Realized P&L/tax basis | Issuer repurchases shares from client. |
| Capital reduction | `RETURN_OF_CAPITAL`, `CORPORATE_ACTION`, optional `LOT_ADJUSTMENT` | Equities, funds, REITs | Quantity may stay same or reduce | Cash credit possible | Cost basis reduction or realized event depending policy | Company returns capital by reducing par value. |
| Name, ticker, ISIN or exchange change | Instrument/reference-data update, lifecycle event | All listed products | No economic change | None | None | Company changes name and ticker. |
| Domicile change / redomiciliation | Instrument/reference-data update; `CORPORATE_ACTION` only if positions are exchanged | Equities, funds, REITs | Usually no economic change; may exchange instrument | Usually none | Tax/reporting classification may change | Fund redomiciles to another jurisdiction. |
| Exchange offer | `CORPORATE_ACTION`, `TRANSFER_OUT`, `TRANSFER_IN`, optional cash leg | Bonds, equities, preferreds, structured products | Old instrument decreases; new instrument increases | Cash sweetener possible | Lot/cost transfer or realization policy | Bond exchanged for new bond series. |
| Bond coupon | `COUPON`, `TAX`, optional `ACCRUAL_REVERSAL` | Bonds, notes, preferreds | Nominal unchanged | Cash credit | Income and withholding tax | Semiannual fixed coupon paid. |
| Bond call / early redemption | `REDEMPTION`, `COUPON` if final coupon paid | Bonds, notes, preferreds | Nominal decreases/closes | Principal and income cash credit | Lot close, realized P&L and tax | Callable bond redeemed at 101. |
| Bond maturity | `MATURITY`, `COUPON` if final coupon paid | Bonds, deposits, notes | Nominal closes | Principal and income cash credit | Lot close, realized P&L if applicable | Bond matures at par plus final coupon. |
| Partial redemption / amortization | `REDEMPTION` | Bonds, ABS/MBS, structured products | Nominal decreases | Principal cash credit | Pro-rata cost-basis reduction | Amortizing bond repays part of principal. |
| Default / credit event write-down | `WRITE_DOWN`, `WRITE_OFF`, optional `CORPORATE_ACTION` | Bonds, credit-linked notes, private credit | Value or nominal decreases | Recovery cash possible | Loss recognition/tax basis | Credit-linked note writes down after reference default. |
| Recovery payment after default | `DISTRIBUTION`, `RETURN_OF_CAPITAL`, or `CORPORATE_ACTION` subtype `RECOVERY_PAYMENT` | Bonds, private credit, distressed assets | Usually no quantity increase | Cash credit | Recovery classification required | Defaulted bond pays recovery proceeds. |
| Consent fee | `FEE` or `INCOME` depending economic treatment | Bonds, structured products, funds | No quantity change | Cash credit/debit | Income/expense classification | Bondholders receive consent fee. |
| Fund distribution | `DISTRIBUTION`, `DIVIDEND`, `RETURN_OF_CAPITAL`, `TAX` | Funds, ETFs, REITs, private funds | Usually no unit change | Cash or reinvested units | Income source and tax classification | Fund pays income and capital distribution. |
| Fund split / unit consolidation | `SPLIT_OR_CONSOLIDATION` | Funds, ETFs, hedge funds | Unit quantity changes | None or cash-in-lieu | Cost basis adjusts | Fund changes unit ratio. |
| Fund merger | `CORPORATE_ACTION`, `TRANSFER_OUT`, `TRANSFER_IN`, `LOT_ADJUSTMENT` | Funds, ETFs, hedge funds | Old fund closes; new fund opens | Cash-in-lieu possible | Cost basis transfer or realization | Fund A merges into Fund B. |
| Fund liquidation | `REDEMPTION` or `CORPORATE_ACTION` subtype `LIQUIDATION` | Funds, ETFs, hedge funds | Position closes | Liquidation proceeds credited | Realized P&L/tax basis | Fund winds up and pays final NAV. |
| Side-pocket creation | `CORPORATE_ACTION`, `TRANSFER_OUT`, `TRANSFER_IN`, `LOT_ADJUSTMENT` | Hedge funds, private funds | Main fund partly reclassified into side-pocket position | Usually none | Cost basis allocation | Illiquid asset moved to side pocket. |
| Structured note autocall | `REDEMPTION`, `COUPON` | Structured products | Note closes | Principal and coupon cash credit | Income/capital classification | Autocall barrier met on observation date. |
| Structured note physical settlement | `DERIVATIVE_SETTLEMENT`, `IN_KIND_DISTRIBUTION`, `REDEMPTION` | Structured products | Note closes; underlying security opens | Cash may include coupon/cash-in-lieu | Cost basis allocation | Reverse convertible delivers shares instead of cash. |
| Option assignment | `OPTION_EXERCISE` or `DERIVATIVE_SETTLEMENT` | Options, warrants | Option closes; underlying/cash changes | Strike/settlement cash | Lot basis and realized P&L | Short option assigned. |
| Futures daily variation | `DERIVATIVE_MARGIN` | Futures | Contract remains open unless closed | Cash margin credit/debit | Realized daily P&L policy | Futures variation margin posted. |
| ETF creation/redemption by AP | Usually not client transaction unless client is AP | ETFs | No client holding change unless AP activity belongs to account | Depends | Depends | Treat as reference/market event for normal wealth client. |

### Corporate Action Examples

#### Cash Dividend

| Record | Example |
|---|---|
| Lifecycle event | `CORPORATE_ACTION_ANNOUNCED` with dividend amount, currency, record date and pay date. |
| Entitlement | Optional `ENTITLEMENT_RECEIPT` if receivable is booked after record date. |
| Transaction | `DIVIDEND` on payment/booking date. |
| Legs | `INCOME` gross dividend, `TAX` withholding, `CASH` net receipt. |
| Position | Equity quantity unchanged. |
| QA | Shares held on record date times dividend rate equals gross dividend, adjusted for tax and FX. |

#### Stock Split

| Record | Example |
|---|---|
| Lifecycle event | Split announcement and effective date. |
| Transaction | `SPLIT_OR_CONSOLIDATION`. |
| Legs | `SECURITY` quantity delta or replacement quantity, optional `LOT` adjustment. |
| Position | Quantity changes; market value should not jump solely because of split. |
| QA | Pre-split quantity times split factor equals post-split quantity, allowing fractional cash handling. |

#### Rights Issue

| Record | Example |
|---|---|
| Lifecycle event | Rights announcement with ratio, subscription price and expiry. |
| Entitlement | `ENTITLEMENT_RECEIPT` only if rights are booked as tradable/valuable position. |
| Election | Exercise, sell or lapse election. |
| Transaction | `RIGHTS_EXERCISE`, `SELL` for rights sale, or `WRITE_OFF`/`CORPORATE_ACTION` for lapse. |
| Legs | Rights decrease, underlying shares increase, cash subscription debit, lot creation. |
| QA | Rights received, exercised, sold and lapsed reconcile to opening rights entitlement. |

#### Merger With Cash And Stock

| Record | Example |
|---|---|
| Lifecycle event | Merger terms, election window, effective date and consideration ratio. |
| Transaction | `CORPORATE_ACTION` with old-security close, new-security open and cash legs. |
| Legs | `SECURITY` old position decrease, `SECURITY` new position increase, `CASH` consideration, optional tax. |
| Lot | Cost basis transferred, allocated or realized according to accounting/tax policy. |
| QA | Old shares times exchange ratio plus cash terms reconcile to new shares and cash-in-lieu. |

#### Bond Early Redemption

| Record | Example |
|---|---|
| Lifecycle event | Call notice with call price, effective date and accrued coupon treatment. |
| Transaction | `REDEMPTION`, plus `COUPON` if final coupon or accrued interest is paid separately. |
| Legs | `SECURITY` nominal decrease, `CASH` principal proceeds, `INCOME` coupon, `TAX` if applicable. |
| Lot | Lot closes or reduces pro rata. |
| QA | Redeemed nominal times call price plus accrued/final coupon equals gross proceeds. |

#### Structured Note Physical Settlement

| Record | Example |
|---|---|
| Lifecycle event | Observation/final fixing confirms physical settlement. |
| Transaction | `REDEMPTION` or `DERIVATIVE_SETTLEMENT` for note close, plus `IN_KIND_DISTRIBUTION` for delivered underlying. |
| Legs | Note nominal closes, underlying shares open, coupon/cash-in-lieu cash legs if applicable. |
| Position | Structured product closes; delivered equity/security position opens. |
| QA | Payoff formula, conversion ratio, barrier/fixing evidence and delivered quantity reconcile. |

## Connecting Instrument Lifecycle, Product Legs And Cash Legs

Transactions from the same instrument lifecycle should be connected through explicit identifiers. Do not rely on date, amount or description matching. A portfolio-management app should be able to start from any record - instrument, lifecycle event, transaction, leg, cash movement, lot, ledger entry or position - and trace the full economic story.

### Relationship Identifiers

| Identifier | Scope | Purpose | Example |
|---|---|---|---|
| `instrument_id` | Product/instrument | Connects all lifecycle events, transactions, positions, valuations and exposures for the same instrument. | Apple equity ISIN, bond ISIN, fund share class, note ID |
| `lifecycle_thread_id` | Instrument event thread | Connects all records for one corporate action, coupon cycle, maturity, observation, option exercise, capital call or correction process. | One dividend event across announcement, record date and payment |
| `lifecycle_event_id` | Source event | Links a specific announcement, election, entitlement, settlement or correction event to derived transactions. | Custodian corporate-action event |
| `transaction_group_id` | Economic transaction group | Connects all transactions and legs that must be understood together. | Cash-and-stock merger group |
| `transaction_id` | Transaction header | Connects a transaction to its legs, ledger entries, lots and source evidence. | Dividend payment transaction |
| `leg_id` | Transaction leg | Connects each atomic product, cash, income, tax, fee, collateral or liability effect. | Cash leg for dividend net payment |
| `cash_movement_id` | Cash ledger | Connects cash leg to settled/pending cash balance and payment rail. | USD dividend cash credit |
| `position_id` | Position thread/snapshot | Connects position changes and as-of holdings. | Equity position before and after split |
| `lot_id` | Cost-lot thread | Connects acquisition, disposal, transfer, corporate action and tax basis. | Lot adjusted by stock split |
| `ledger_entry_id` | Accounting ledger | Connects debit/credit postings back to transaction legs. | Dividend income credit and tax debit |
| `correction_group_id` | Correction chain | Connects original, reversal, replacement and restatement records. | Corrected corporate-action tax |

### Parent-Child Record Shape

Use this pattern for any lifecycle that creates multiple economic effects:

```text
instrument
  -> lifecycle_thread
       -> lifecycle_event announcement / election / entitlement / processing / settlement
            -> transaction_group
                 -> transaction_header
                      -> transaction_leg product side
                      -> transaction_leg cash side
                      -> transaction_leg fee/tax/income/accrual side
                      -> position_lot updates
                      -> cash_movement records
                      -> ledger_entry records
                 -> related transaction_header when the event must be split by type
            -> reconciliation_break when source/book/report do not match
```

The `transaction_group_id` is the practical key for screens, APIs and reconciliation. It lets the app show one business event even when the book has multiple transactions.

### Product Side And Cash Side

| Side | What It Represents | Leg Types | Position Impact | Cash Impact |
|---|---|---|---|---|
| Product side | What happened to the held asset, liability, policy, commitment, derivative or collateral. | `SECURITY`, `DERIVATIVE`, `LIABILITY`, `COMMITMENT`, `COLLATERAL`, `LOT` | Quantity, nominal, notional, policy value, commitment, collateral or lot changes. | Usually none directly. |
| Cash side | What money moved or became receivable/payable. | `CASH`, `INCOME`, `FEE`, `TAX`, `ACCRUAL`, `MARGIN` | Cash balance, receivable, payable or accrual state changes. | Debit, credit, pending, settled, restricted or value-dated cash changes. |
| Accounting side | How the business effect posts to books. | Ledger debit/credit entries mapped from transaction legs. | Book value, income, expense, payable, receivable or capital accounts. | Ledger cash account impact. |
| Reporting side | How the event appears in statements and analytics. | Reporting classifications derived from transaction and legs. | Holdings, income, realized P&L, performance, cashflow and disclosures. | Client-visible cashflow classification. |

### Leg Connection Rules

1. One business event may create one `transaction_group_id` with several `transaction_id` records if the event has different canonical transaction types.
2. Each `transaction_id` must have one or more `transaction_leg` records.
3. Product-side and cash-side legs should share the same `transaction_id` when they are part of one economic posting.
4. Use separate transactions inside the same `transaction_group_id` when the event has distinct economic meanings, such as `DIVIDEND` plus `TAX`, `REDEMPTION` plus `COUPON`, or `REVERSAL` plus corrected replacement.
5. A product-side leg that changes quantity, nominal, notional, liability, policy value, commitment or collateral must identify the affected `instrument_id` or contract/facility identifier.
6. A cash-side leg must identify currency, value date, amount type, sign and settlement status.
7. A lot-affecting leg should reference `lot_id` or create a lot update record.
8. A ledger entry should reference the source `transaction_id` and preferably the source `leg_id`.
9. A lifecycle event can derive many transactions, but a transaction should point back to the lifecycle event that justified it.
10. If the source sends product and cash sides separately, the app should link them through deterministic keys: source event ID, lifecycle thread, transaction group, instrument, account, dates and source-provided references.

### Same Instrument Lifecycle Examples

| Lifecycle | Product-Side Record | Cash-Side Record | Connection | App Display |
|---|---|---|---|---|
| Bond coupon | Bond nominal unchanged; accrual may reverse. | Coupon cash credit, withholding tax debit. | Same `lifecycle_thread_id`; `COUPON` transaction group with income/cash/tax/accrual legs. | One coupon event with gross, tax, net, accrual reversal and payment status. |
| Bond maturity | Bond nominal closes. | Principal cash credit and final coupon cash credit. | Same maturity thread; `MATURITY` transaction and optional `COUPON` transaction in same group. | One maturity event with principal, final income, tax and closed position. |
| Equity cash dividend | Equity quantity unchanged. | Dividend cash credit and tax debit. | Same dividend thread; `DIVIDEND` transaction with income/cash/tax legs. | One dividend row expandable into gross, tax and net cash. |
| Stock split | Old quantity converted to new quantity; lots adjusted. | None unless fractional cash-in-lieu. | Same split thread; `SPLIT_OR_CONSOLIDATION` plus optional cash-in-lieu transaction. | One split event showing before/after quantity and any fractional cash. |
| Rights issue | Rights entitlement created; rights decrease if exercised; underlying shares increase. | Subscription cash debit if exercised; cash credit if rights sold. | Same rights thread; entitlement/election event plus `RIGHTS_EXERCISE`, `SELL` or lapse transaction. | One rights event with entitlement, election, resulting shares/cash/lapse. |
| Cash-and-stock merger | Old instrument closes; new instrument opens. | Cash consideration and cash-in-lieu credit. | Same merger thread and transaction group with old security leg, new security leg, cash leg and lot allocation. | One merger event showing old shares exchanged for new shares plus cash. |
| Structured note autocall | Note nominal closes. | Coupon and principal cash credit. | Same observation/autocall thread; `REDEMPTION` plus `COUPON` in same transaction group. | One autocall event with observation evidence, coupon, principal and note closure. |
| Reverse convertible physical settlement | Note closes; underlying equity opens. | Coupon/cash-in-lieu if applicable. | Same final fixing thread; `REDEMPTION`/`DERIVATIVE_SETTLEMENT` plus `IN_KIND_DISTRIBUTION`. | One settlement event showing note closure and delivered shares. |
| Fund distribution reinvested | Fund income entitlement recognized; new units created. | No external cash if directly reinvested, or transient cash leg if book policy requires it. | Same distribution thread; `DISTRIBUTION` plus `SUBSCRIBE`/`STOCK_DIVIDEND` depending source. | One reinvested distribution event showing income and additional units. |
| Private-market capital call | Paid-in capital increases; unfunded commitment decreases. | Cash debit on funding date. | Same capital-call thread; `CAPITAL_CALL` with investment, commitment and cash legs. | One capital-call event showing due, funded, remaining commitment and payment state. |

### Corporate Action Leg Connection Patterns

| Corporate Action | Transaction Group Contents | Product Legs | Cash Legs | Lot / Ledger Links |
|---|---|---|---|---|
| Cash dividend | One `DIVIDEND` transaction, optional separate `TAX` transaction if tax is booked separately. | No security quantity change. | Gross income, withholding tax, net cash receivable/payment. | Income ledger, tax ledger, cash ledger; source shares used for entitlement. |
| Stock dividend | `STOCK_DIVIDEND` transaction. | Security quantity increase. | Usually none. | Lot basis allocation or zero-cost lot, depending accounting policy. |
| Split/consolidation | `SPLIT_OR_CONSOLIDATION` transaction. | Old quantity adjusted to new quantity; same economic holding. | Optional fractional cash-in-lieu. | Lot quantity and per-unit cost adjusted; market value continuity check. |
| Rights issue exercised | `RIGHTS_EXERCISE` transaction, sometimes preceded by entitlement record. | Rights position decreases; underlying position increases. | Subscription cash debit and fees/taxes if applicable. | New lot created for underlying; rights cost basis transferred if policy requires. |
| Rights sold | `SELL` transaction on rights instrument. | Rights position decreases. | Sale proceeds cash credit, fees/taxes. | Realized P&L on rights lot. |
| Rights lapse | `WRITE_OFF` or `CORPORATE_ACTION` subtype. | Rights position closes. | Usually none. | Cost basis write-off if rights carried value. |
| Spin-off | `CORPORATE_ACTION` with `STOCK_DIVIDEND` or transfer-style security legs. | Parent remains or adjusts; new child instrument opens. | Optional cash-in-lieu. | Cost basis allocated between parent and child lots. |
| Cash merger | `CORPORATE_ACTION` or `REDEMPTION` transaction. | Old instrument closes. | Merger proceeds cash credit and tax if applicable. | Old lots closed; realized P&L calculated. |
| Stock merger | `CORPORATE_ACTION` transaction group. | Old instrument closes; new instrument opens. | Optional cash-in-lieu. | Cost basis transferred or partially realized based on policy. |
| Mixed merger | One transaction group with `CORPORATE_ACTION` plus cash/security legs; sometimes separate tax transaction. | Old close, new open. | Cash consideration, cash-in-lieu, tax. | Lot split between realized and transferred basis. |
| Tender offer | `SELL` or `REDEMPTION` depending legal form. | Tendered quantity decreases. | Cash or new security consideration. | Lot close and realized P&L. |
| Bond call | `REDEMPTION`, optional `COUPON`. | Nominal decreases or closes. | Principal proceeds, final/accrued coupon, tax. | Lot close/reduction; income vs capital classification. |
| Bond default write-down | `WRITE_DOWN` or `WRITE_OFF`. | Nominal/value reduced or closed. | Recovery cash only if paid. | Loss recognition ledger and tax basis treatment. |
| Fund merger | `CORPORATE_ACTION` transaction group. | Old fund closes; new fund opens. | Cash-in-lieu possible. | Cost basis transfer or realization policy. |
| Side-pocket creation | `CORPORATE_ACTION`, `TRANSFER_OUT`, `TRANSFER_IN`, `LOT_ADJUSTMENT`. | Main fund quantity/value split; side-pocket opens. | Usually none. | Cost basis allocation and liquidity restriction update. |

### Example: Cash Dividend Connected Records

```text
instrument_id = EQ-ABC
lifecycle_thread_id = CA-DIV-2026-ABC-Q2

Lifecycle events:
  EVT-ANNOUNCED: dividend announced
  EVT-RECORD: entitlement determined
  EVT-PAY: payment received

Transaction group:
  TXG-ABC-Q2-DIV
    TXN-DIV-001 transaction_type = DIVIDEND
      LEG-INCOME gross dividend +100.00 USD
      LEG-TAX withholding tax -15.00 USD
      LEG-CASH net cash +85.00 USD

Position:
  EQ-ABC quantity unchanged

Cash:
  USD cash +85.00 on pay/value date

Ledger:
  dividend income, withholding tax receivable/expense, cash
```

### Example: Rights Issue Connected Records

```text
instrument_id = EQ-ABC
lifecycle_thread_id = CA-RIGHTS-2026-ABC

Lifecycle events:
  EVT-ANNOUNCED: rights terms announced
  EVT-ENTITLEMENT: rights credited
  EVT-ELECTION: client elects exercise
  EVT-PROCESSED: new shares delivered

Transaction group:
  TXG-ABC-RIGHTS-EXERCISE
    TXN-RIGHTS-001 transaction_type = RIGHTS_EXERCISE
      LEG-RIGHTS security quantity -100 rights
      LEG-SHARES security quantity +25 shares
      LEG-CASH subscription cash -250.00 USD
      LEG-FEE transaction fee -5.00 USD
      LEG-LOT new share lot cost basis +255.00 USD

Positions:
  rights position closes or reduces
  equity position increases

Cash:
  USD cash decreases by subscription plus fee
```

### Example: Mixed Cash And Stock Merger Connected Records

```text
old_instrument_id = EQ-OLD
new_instrument_id = EQ-NEW
lifecycle_thread_id = CA-MERGER-2026-OLD-NEW

Transaction group:
  TXG-OLD-NEW-MERGER
    TXN-CA-001 transaction_type = CORPORATE_ACTION
      LEG-OLD security quantity -100 EQ-OLD
      LEG-NEW security quantity +42 EQ-NEW
      LEG-CASH merger cash +500.00 USD
      LEG-CASH-IN-LIEU fractional cash +12.30 USD
      LEG-TAX withholding / transaction tax if applicable
      LEG-LOT allocate old cost basis across realized cash and new holding

Positions:
  EQ-OLD closed
  EQ-NEW opened

Reporting:
  show as one merger event, not an unrelated sale and purchase
```

### Example: Bond Call With Final Coupon Connected Records

```text
instrument_id = BOND-XYZ-2029
lifecycle_thread_id = CA-CALL-2026-BOND-XYZ

Transaction group:
  TXG-BOND-XYZ-CALL
    TXN-RED-001 transaction_type = REDEMPTION
      LEG-BOND nominal -100000
      LEG-CASH principal proceeds +101000.00 USD
      LEG-LOT close/reduce bond lot
    TXN-CPN-001 transaction_type = COUPON
      LEG-INCOME final coupon +2500.00 USD
      LEG-TAX withholding tax -250.00 USD
      LEG-CASH coupon net +2250.00 USD

Position:
  bond nominal closes

Cash:
  cash increases by principal proceeds plus net coupon

Reporting:
  principal is not income; coupon is income
```

### App Query Patterns

| User Question | Query Pattern |
|---|---|
| Show me everything that happened to this instrument. | Filter by `instrument_id`, then group by `lifecycle_thread_id` and `transaction_group_id`. |
| Explain this cash movement. | Start from `cash_movement_id`, join to `leg_id`, `transaction_id`, `transaction_group_id`, lifecycle event and instrument. |
| Explain why my position quantity changed. | Start from `position_id` and date, join position delta to product-side legs and lifecycle events. |
| Show all legs of this corporate action. | Filter by `lifecycle_thread_id` or `transaction_group_id`, include all transactions and legs. |
| Reconcile corporate-action cash to position impact. | Compare product-side legs, cash-side legs, lot updates and ledger entries under the same transaction group. |
| Show corrected view only but keep audit trail. | Query latest active transactions plus correction group history on demand. |

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

## Portfolio Management App Implementation Blueprint

For a portfolio-management application, this model should become a small set of stable domain modules rather than one large transaction table that tries to answer every question.

### Recommended Build Sequence

Build the model in layers. Each layer should have tests and reconciliation before the next layer depends on it.

| Step | Build | Why First / Next | Minimum Proof |
|---:|---|---|---|
| 1 | Instrument, account and portfolio master | Every later record needs stable identity, classification and ownership context. | A transaction or position cannot load without valid instrument, account and portfolio references. |
| 2 | Transaction header and legs | Economic truth should be captured before derived views. | Buy, sell, dividend, coupon, fee, tax and cash transfer can be represented with legs. |
| 3 | Cash balance snapshots and cash movements | Cash is required for settlement, funding, reports and client trust. | Cash roll-forward reconciles by account, currency and value date. |
| 4 | Position snapshots and position deltas | Holdings views need state, but state must be explainable from transaction legs. | Position roll-forward reconciles for cash, equities, bonds and funds. |
| 5 | Lots and cost basis | Realized P&L, tax, transfers and corporate actions need lot support. | Sell and corporate-action examples reconcile lot quantity and cost. |
| 6 | Lifecycle event store | Corporate actions, notices, elections, observations and corrections need source evidence. | Dividend, split, rights issue and redemption can show full lifecycle thread. |
| 7 | Valuation snapshots and FX rates | Market value, reporting currency, performance and collateral need valuation data. | Holdings show valuation basis, source, date and stale/estimated state. |
| 8 | Corporate-action processor | Multi-leg product/cash/lot events need deterministic processing. | Cash dividend, stock split, rights issue, merger and bond call pass golden scenarios. |
| 9 | Reconciliation and break management | Operations need source/book/report control before scaling product coverage. | Breaks are generated, owned, resolved and auditable. |
| 10 | Exposure, performance and reporting views | Analytics should consume stable positions, transactions, prices and classifications. | Reported values can be reproduced from versioned inputs. |
| 11 | Corrections and restatements | Real systems need controlled repair before production use. | Reversal/rebook and restated NAV scenarios preserve audit trail. |

### Core Modules

| Module | Owns | Should Not Own |
|---|---|---|
| Product and instrument master | Instrument identity, product family, subtype, terms, identifiers, payoff metadata, issuer/counterparty, reference-data links. | Client-specific position quantity or transaction history. |
| Account and portfolio master | Legal account, portfolio grouping, reporting hierarchy, base currency, mandate, booking centre, restrictions. | Instrument economics or market data. |
| Transaction book | Canonical transaction header, transaction legs, source evidence, lifecycle link, correction/reversal links. | Derived analytics that can be recalculated from transactions and positions. |
| Position book | As-of holdings, liabilities, cash balances, policy values, commitment states, pledge state, supportability. | Raw order intent or source notices that have not affected state. |
| Lot and cost-basis engine | Acquisition lots, disposal matching, cost adjustments, tax/reporting basis, realized P&L. | Market-value exposure decomposition. |
| Cash and settlement ledger | Settled/pending cash, receivables, payables, value dates, settlement state, restricted cash. | Security quantity as the primary record. |
| Accounting ledger | Balanced debit/credit entries by policy basis. | Business classification alone; ledger should be traceable to transaction legs. |
| Lifecycle event store | Source events, notices, elections, observations, confirmations, settlement events, corrections. | Economic postings without transaction linkage. |
| Valuation and pricing | Price/NAV/fair-value snapshots, stale-price flags, source confidence, FX rates. | Booked transaction economics unless valuation is corrected into books. |
| Exposure and analytics | Look-through, notional, delta, issuer, sector, country, currency, duration, liquidity and risk exposures. | Legal ownership state. |
| Reconciliation and controls | Breaks, tolerances, stale data, source mismatches, sign-off evidence. | Silent mutation of transactions or positions. |

### Minimum Domain Tables

For a practical first implementation, start with these tables or collections:

| Table / Collection | Required Purpose | Important Keys |
|---|---|---|
| `instrument` | Defines what the portfolio holds or references. | `instrument_id`, `product_family`, `product_subtype`, `currency`, `issuer_id`, `terms_version` |
| `account` | Defines legal booking account and owner context. | `account_id`, `client_id`, `booking_centre`, `account_currency`, `status` |
| `portfolio` | Defines reporting and analytics grouping. | `portfolio_id`, `base_currency`, `mandate_id`, `reporting_scope` |
| `transaction_header` | Stores canonical economic transaction. | `transaction_id`, `transaction_type`, `transaction_status`, `account_id`, `instrument_id`, `trade_date`, `settlement_date` |
| `transaction_leg` | Stores atomic cash/security/income/tax/fee/liability effects. | `leg_id`, `transaction_id`, `leg_type`, `amount_type`, `currency`, `quantity_delta`, `cash_delta` |
| `position_snapshot` | Stores as-of position state. | `position_id`, `as_of_date`, `account_id`, `instrument_id`, `position_type`, `quantity`, `market_value` |
| `cash_balance_snapshot` | Stores settled, pending, available and restricted cash. | `account_id`, `currency`, `as_of_date`, `settled_balance`, `available_balance` |
| `position_lot` | Stores open and closed cost lots. | `lot_id`, `position_id`, `acquisition_transaction_id`, `quantity_open`, `cost_basis` |
| `lifecycle_event` | Stores source events and operational state changes. | `event_id`, `event_type`, `source_system`, `source_event_id`, `event_timestamp` |
| `valuation_snapshot` | Stores prices, NAVs, FX rates, policy values and fair values. | `instrument_id`, `valuation_date`, `valuation_basis`, `price_source`, `price` |
| `reconciliation_break` | Stores source/book/report/ledger differences. | `break_id`, `break_type`, `entity_type`, `entity_id`, `severity`, `status` |

### Transaction Classification Algorithm

Use a deterministic classifier before writing a transaction. This avoids inconsistent mappings across ingestion feeds, UI workflows and migration loads.

1. Identify whether the source record is an instruction, lifecycle event, valuation, economic posting, cash movement, ledger entry or correction.
2. If it is only an instruction, notice, election, report delivery, reference-data update or valuation input, store it as a lifecycle event or snapshot and do not create a transaction.
3. If it changes holdings, cash, lots, ledger, income, tax, liability, commitment, collateral or policy state, classify it as a transaction.
4. Select the generic `transaction_type` from the economic action, not from product family.
5. Put product context in `instrument_id`, `product_family`, `transaction_subtype`, lifecycle event payload and legs.
6. Derive legs from economic effects: security, cash, income, fee, tax, accrual, margin, collateral, liability, lot.
7. Attach source evidence and idempotency key before persistence.
8. Validate expected dates: trade date, effective date, value date, settlement date and posting date.
9. If replacing a prior record, create reversal/correction links rather than overwriting.
10. Recalculate affected positions, cash balances, lots, ledger entries, analytics and reports from the canonical posting.

### Classification Decision Rules

| Question | If Yes | If No |
|---|---|---|
| Does it only describe intent? | Store order/instruction, no transaction yet. | Continue. |
| Does it only describe reference data or product terms? | Update instrument/reference data. | Continue. |
| Does it only provide a price, NAV, policy value or appraisal? | Store valuation snapshot. | Continue. |
| Does it change quantity, nominal, notional, cash, liability, commitment, collateral, lot, income, fee or tax? | Create transaction. | Store lifecycle event/control evidence. |
| Is the action a market execution? | Use `BUY`, `SELL`, `FX_CONVERSION` or derivative trade type. | Continue. |
| Is the action issuer/fund/platform-driven? | Use `SUBSCRIBE`, `REDEEM`, `REDEMPTION`, `MATURITY`, `CORPORATE_ACTION` or specific servicing type. | Continue. |
| Is it income or expense? | Use `COUPON`, `DIVIDEND`, `INTEREST`, `DISTRIBUTION`, `FEE`, `TAX`, `TAX_RECLAIM`. | Continue. |
| Is it a correction? | Use `REVERSAL`, `CORRECTION`, `RESTATEMENT_ADJUSTMENT` with links. | Continue. |
| Is it unclear? | Store as `SOURCE_LIMITED` pending review, not as a misleading final transaction type. | Persist normally. |

### Position Roll-Forward

Every portfolio app should be able to explain a position using a roll-forward:

```text
opening position
+ acquisitions / subscriptions / transfers in
- disposals / redemptions / transfers out
+/- corporate actions and quantity adjustments
+/- income reinvestment or in-kind distributions
+/- corrections and restatements
= closing position
```

For cash:

```text
opening settled cash
+ deposits, sale proceeds, income, maturities, redemptions, loan drawdowns
- withdrawals, purchases, fees, taxes, loan repayments, capital calls
+/- FX conversions, sweeps, corrections
= closing settled cash
```

For private-market commitments:

```text
opening commitment
+ new commitments
- capital calls funded
+ recallable distributions
+/- commitment increases, decreases, transfers and corrections
= closing unfunded commitment
```

For collateral and lending:

```text
eligible collateral value after haircut
- drawn liabilities
- pending withdrawals / settlement debits
+ pending repayments / collateral releases where approved
= available borrowing capacity
```

### Derived Views For A Useful Portfolio App

| View | Derived From | Purpose |
|---|---|---|
| Holdings view | Position snapshots, instrument master, valuation snapshots | Show what the client owns or owes now. |
| Transactions view | Transaction headers and legs | Explain economic history and audit trail. |
| Cash view | Cash balance snapshots and cash legs | Explain available, settled, pending and restricted cash. |
| Income view | Income, tax, accrual and distribution legs | Show cash income, accrued income and income classification. |
| Performance view | Positions, transactions, valuations, FX rates, benchmarks | Calculate returns and contribution. |
| Risk/exposure view | Positions, instrument data, derivatives, look-through, collateral | Show issuer, sector, currency, duration, notional and sensitivity exposures. |
| Operations view | Lifecycle events, settlement state, reconciliation breaks | Show exceptions, fails, stale data and manual intervention. |
| Advisor action view | Portfolio, mandate, restrictions, liquidity, suitability, pending orders | Support advice, rebalancing and next-best action. |
| Statement/reporting view | Signed-off positions, transactions, income, performance, disclosures | Produce client-facing evidence and archive. |

## Implementation Flow And Model Invariants

This section turns the model into an implementation checklist. A portfolio-management app should not simply store transactions and positions; it should process source events through a repeatable pipeline and enforce invariants that make holdings, cash, income, risk, reports and operations explainable.

### End-To-End Processing Flow

```text
source event / source record
  -> classify as reference data, lifecycle event, transaction, valuation, cash movement or correction
  -> validate instrument, account, portfolio, source and idempotency keys
  -> create lifecycle event when source evidence matters
  -> create transaction group when economic effects exist
  -> create transaction headers and legs
  -> update cash, lots, positions, liabilities, commitments and collateral state
  -> post ledger entries where accounting policy requires
  -> update valuation and exposure snapshots where affected
  -> reconcile source, book, custody, cash, ledger and report outputs
  -> expose state through APIs, screens, reports, alerts and audit trail
```

### Non-Negotiable Invariants

| Invariant | Why It Matters | Failure Example |
|---|---|---|
| One economic event must be traceable from source evidence to transaction group, legs, positions, cash and ledger. | Enables audit, support, QA and client explanation. | Cash dividend appears in cash but cannot be tied to the equity holding. |
| Every position-changing transaction must have product-side leg evidence. | Prevents unexplained holdings changes. | Position quantity changes because a batch overwrote snapshot. |
| Every cash-changing transaction must have cash-side leg evidence. | Prevents unexplained cash movements. | Fund redemption closes units but no cash receivable is visible. |
| Every transaction leg must have a clear amount type and sign convention. | Avoids mixing income, principal, tax, fee, margin and collateral. | Coupon principal and income both posted as generic amount. |
| Every transaction must be idempotent by source. | Prevents duplicate booking from replayed feeds or retries. | Same custody dividend loaded twice. |
| Every correction must preserve original, reversal and replacement records. | Maintains auditability and reporting restatement trace. | Incorrect trade price overwritten without evidence. |
| Positions must reconcile by roll-forward. | Explains current holdings from prior holdings and events. | Closing position does not match opening plus transactions. |
| Cash must reconcile separately by currency and value date. | Supports settlement, funding, FX and availability. | USD and SGD cash netted into one reporting amount. |
| Ledger entries must balance by transaction group when ledger posting is in scope. | Supports accounting control. | Fee and tax entries post without offsetting cash/payable entries. |
| Valuation must carry basis, source, date and stale/estimated state. | Prevents misleading reports and analytics. | Private fund NAV shown as current market price. |
| Legal holding and analytical exposure must stay separate. | Avoids incorrect concentration, mandate and risk treatment. | Structured note underlying exposure shown as directly owned equity. |
| Pending, settled, restricted and available states must stay separate. | Prevents incorrect trading, withdrawals and reports. | Pending sale proceeds used as fully available cash. |

### Source Classification Rules

| Source Record Looks Like | Store As | Create Transaction? | Example |
|---|---|---|---|
| Instrument terms, identifier, issuer, coupon, barrier, fee schedule | Instrument static/reference data | No | New structured note termsheet loaded. |
| Price, NAV, appraisal, policy value, FX rate | Valuation snapshot | No, unless correcting book value | Fund NAV received. |
| Order, instruction, election, approval, rejection | Workflow/lifecycle event | No, until economically effective | Client elects scrip dividend. |
| Trade execution, issuer redemption, income payment, fee charge, tax posting | Transaction group and transaction legs | Yes | Equity trade, bond coupon, custody fee. |
| Cash ledger movement with no product context | Cash movement and possibly transaction | Yes if economic; otherwise reconcile/link to existing transaction | Payment received or settlement cash. |
| Custody position snapshot | Position snapshot/source position | No by itself; reconcile to book deltas | Daily custody holdings file. |
| Corrected source event or cancellation | Correction group | Yes when prior economics change | Dividend tax correction. |
| Report generated or delivered | Report archive/evidence | No | Monthly statement delivered. |

### Reconciliation Model

Reconciliation should not be a late manual activity. It should be designed into the model.

| Reconciliation | Compare | Expected Result |
|---|---|---|
| Position roll-forward | Opening position + product-side transaction legs + corporate actions + transfers + corrections vs closing position. | Quantity, nominal and status reconcile by account/instrument. |
| Cash roll-forward | Opening cash + cash-side legs + value-date movements + corrections vs closing cash. | Settled, pending, available and restricted cash reconcile by currency. |
| Lot reconciliation | Open lots + lot-affecting transactions vs position quantity/cost basis. | Lot quantities sum to position quantity and cost basis. |
| Ledger balancing | Debit and credit ledger entries by transaction group. | Balanced accounting entries under policy basis. |
| Income reconciliation | Income/accrual/tax legs vs cash payments and receivables. | Gross, tax, net, accrued and paid income reconcile. |
| Corporate-action reconciliation | Entitled position on record date vs processed product/cash legs. | Entitlement, election, result and settlement reconcile. |
| Source/book reconciliation | Custody/source positions and transactions vs internal book records. | Breaks are explicit with owner, severity and status. |
| Report reconciliation | Reported holdings/cash/income/performance vs signed-off source datasets. | Client-facing report can be reproduced. |

### Screen And API Design Implications

| Surface | Must Show | Avoid |
|---|---|---|
| Holdings screen | Position type, quantity/nominal, market value, valuation date/basis, supportability, restrictions, pending state. | Single value with no source/date/confidence. |
| Transaction screen | Transaction type, subtype, dates, status, source, group, legs, reversal/correction links. | Flat feed where cash, tax, fee and product legs are hidden. |
| Cash screen | Settled, pending, available, restricted, value-dated cash by currency. | Reporting-currency-only cash without original currency. |
| Corporate-action screen | Lifecycle stage, entitlement, election, product legs, cash legs, lots, settlement and corrections. | Treating corporate action as unrelated transactions. |
| Income screen | Gross income, tax, fees, accrual reversal, net cash and income classification. | Showing only net cash as income. |
| Position detail | Roll-forward, lots, valuation, exposure, restrictions, source and breaks. | Current quantity without explanation. |
| Operations screen | Reconciliation breaks, stale data, unsupported states, manual overrides and owner. | Hidden data-quality exceptions. |
| Advisor/rebalancing screen | Tradeable quantity, pledged/restricted amount, liquidity, pending orders, suitability/mandate flags. | Using total holding as tradeable quantity. |

### Golden Scenarios For Model Certification

Use these as minimum test fixtures before implementing portfolio, transaction, position, cash, reporting or reconciliation features.

| Scenario | Must Prove |
|---|---|
| Equity buy with fee and tax | Security quantity, cash debit, fee/tax legs, lot creation and settlement state reconcile. |
| Equity dividend with withholding | Gross income, withholding tax, net cash, record-date entitlement and no quantity change. |
| Stock split with fractional cash | Quantity adjustment, lot adjustment, cash-in-lieu and market-value continuity. |
| Cash-and-stock merger | Old position close, new position open, cash consideration, cash-in-lieu, lot allocation and reporting as one event. |
| Bond purchase with accrued interest | Clean/dirty price, accrued interest, cash settlement, nominal and lot basis are correct. |
| Bond coupon and maturity | Coupon income, tax, accrued reversal, principal redemption and position closure are separated. |
| Fund subscription and redemption | Pending cash/units, confirmed NAV, fees, settlement cycle, units and proceeds reconcile. |
| FX conversion | Two currency cash legs, value date, FX rate and realized FX policy are explicit. |
| Structured note autocall | Observation event, coupon, principal redemption, note closure and payoff evidence connect. |
| Reverse convertible physical settlement | Note close, delivered equity position, cash-in-lieu and cost basis are traceable. |
| Option exercise | Option close, underlying open/cash settlement, strike, multiplier and premium treatment reconcile. |
| Futures variation margin | Daily margin cash movement, P&L and open contract state are separated. |
| Private-market capital call | Commitment, unfunded amount, paid-in capital and cash debit roll forward. |
| Private-market distribution | Income/return-of-capital classification, recallable amount and NAV impact are clear. |
| Loan drawdown with collateral pledge | Liability, cash credit, pledged assets, haircut, LTV and buying power reconcile. |
| Insurance surrender | Policy state, surrender value, cash proceeds, fees, tax and policy loan treatment are separated. |
| Migration opening balance | Opening positions, cash, lots and source sign-off load without fake historical transactions. |
| Correction and rebook | Original, reversal, replacement and corrected net result are audit-traceable. |

### Model Readiness Checklist

Before building or changing a portfolio-management module, confirm:

1. Can every position be traced to source, instrument, account, portfolio, valuation and latest transaction deltas?
2. Can every cash movement be traced to transaction leg, value date, currency and settlement status?
3. Can every corporate-action event be shown as one business event with all product and cash legs?
4. Can every instrument calculation use explicit static fields instead of display names?
5. Can every report value be reproduced from versioned source data and calculation rules?
6. Can stale, manual, estimated, unsupported and disputed states be surfaced in UI/API/reporting?
7. Can operations explain and resolve breaks without directly editing final records?
8. Can corrections preserve original records while showing the corrected business view?
9. Can advisors distinguish held value, tradeable value, available cash, pledged collateral and analytical exposure?
10. Can QA create deterministic golden cases for every supported product family and lifecycle event?

## Implementation-Grounded Hardening Notes

These notes capture practical model refinements that matter in real portfolio platforms. They are not tied to any one application name; they are implementation patterns that prevent source, calculation, reconciliation and reporting drift.

### Reconstruction Scope And Snapshot Identity

Any API that returns reconstructed portfolio state should make the reconstruction scope explicit.

| Scope Field | Why It Matters |
|---|---|
| `portfolio_id` | Identifies the portfolio state being reconstructed. |
| `as_of_date` | Business date represented by holdings, cash and reportable state. |
| `valuation_date` | Date of prices, NAVs, FX rates, policy values or appraisals. |
| `position_epoch` | Maximum position-state generation included. |
| `cashflow_epoch` | Maximum cashflow generation included. |
| `transaction_window_start` / `transaction_window_end` | Transaction history range used in the result. |
| `source_data_products` | Named input products that contributed to the result. |
| `policy_version` | Policy version affecting visibility, classification or calculation. |
| `restatement_version` | Version of truth used when corrected or restated history exists. |

Snapshot identity should be deterministic: the same scope produces the same `snapshot_id`; any scope field that changes economic truth, valuation truth, lineage or policy should produce a different `snapshot_id`.

### Temporal Semantics Guardrail

Transaction and portfolio systems should not collapse all dates into one generic transaction date.

| Temporal Field | Meaning | Guardrail |
|---|---|---|
| `trade_date` | Date the trade or market execution occurred. | Use for execution history, trade reporting and position economics where trade-date accounting applies. |
| `effective_date` | Date the business effect becomes economically effective. | Use for corrections, restatements, policy changes and lifecycle events where effect date differs from booking. |
| `value_date` | Date cash value is applied. | Use for cash availability, FX, settlement interest and funding. |
| `settlement_date` | Date cash/security settlement is expected or completed. | Do not infer from trade date; settlement can fail, delay or complete partially. |
| `posting_date` / `booking_date` | Date the transaction or correction is recorded into the book. | Keep separate from trade/event date so late bookings and corrections remain explainable. |
| `as_of_date` | Business date represented by a position, cash, report or reconstructed state. | Use for holdings and reporting scope. |
| `valuation_date` | Date used for price, NAV, FX, policy value or appraisal. | Use for market value and valuation confidence. |
| `restatement_version` | Version of corrected historical truth. | Use when historical state can be repaired, replayed or restated. |

If an existing system uses a legacy field such as `transaction_date`, define whether it means trade/event date or booking date, then expose the missing concept explicitly before downstream consumers depend on it.

### Runtime Metadata For Source-Backed APIs

Portfolio APIs should carry metadata that helps consumers decide whether a response is safe to use.

| Metadata | Purpose |
|---|---|
| `generated_at` | When the API response or source-data product was generated. |
| `as_of_date` | Business date represented by the response. |
| `restatement_version` | Historical truth version used for this response. |
| `reconciliation_status` | Whether the response scope is complete, partial, stale, unreconciled, broken, blocked or unknown. |
| `data_quality_status` | Whether returned rows are complete, partial, stale, blocked or unknown. |
| `latest_evidence_timestamp` | Most recent linked source, validation, reconciliation or calculation evidence. |
| `source_batch_fingerprint` | Deterministic source-batch identity when the result traces to ingestion. |
| `snapshot_id` | Deterministic reconstructed-state identity. |
| `policy_version` | Policy applied to classification, visibility or calculation. |
| `correlation_id` | Request or processing correlation for supportability. |

### Product-Leg And Cash-Leg Pairing Metadata

Real implementations often receive product-side and cash-side records separately. The model should support both service-generated and upstream-provided cash entries.

| Field | Use |
|---|---|
| `economic_event_id` | Business event identifier linking security/product and cash effects. |
| `linked_transaction_group_id` | Group linking related product, cash, fee, tax and correction transactions. |
| `cash_entry_mode` | Distinguishes generated cash leg from upstream-provided separate cash entry. |
| `external_cash_transaction_id` | Links to upstream cash transaction when cash was supplied separately. |
| `settlement_cash_account_id` | Identifies settlement cash account used for cash leg generation. |
| `settlement_cash_instrument_id` | Identifies cash instrument/currency used for settlement leg. |
| `originating_transaction_id` | Links generated cash leg back to product-side transaction. |
| `originating_transaction_type` | Keeps cash leg explanation visible without reclassifying it. |
| `adjustment_reason` | Bounded reason code such as buy settlement, sell settlement, fee, tax or correction. |
| `link_type` | Relationship label between product and cash legs. |
| `reconciliation_key` | Shared key used to match paired records from different sources. |

### Calculation Policy And Versioning

Cost, FX, realized P&L, accrued income, cashflows and tax-lot results should record the policy used to calculate them.

| Field | Applies To | Why It Matters |
|---|---|---|
| `calculation_policy_id` | Transactions, lots, cashflows, valuation, P&L | Identifies method such as FIFO, average cost, clean/dirty price, FX policy or settlement policy. |
| `calculation_policy_version` | Same as above | Allows historical results to be reproduced after policy changes. |
| `calculation_type` | Cashflows, derived records | Distinguishes transaction-derived, projected, manual, imported or model-generated results. |
| `source_lineage` | Lots, cashflows, positions, valuations | Connects result to source system, source record and policy provenance. |

### Epochs, Watermarks And Reprocessing

Large portfolio systems need repeatable reconstruction after late transactions, corrected prices, source replay or failed jobs.

| Concept | Use |
|---|---|
| `epoch` | Version of generated position, cashflow, valuation or timeseries state. |
| `watermark_date` | Latest business date processed for a portfolio/instrument key. |
| `reprocessing_status` | Indicates current, reprocessing, failed or blocked state. |
| idempotent work key | Prevents duplicate valuation, aggregation or reprocessing jobs for the same portfolio/instrument/date/epoch. |
| `failure_reason` and `attempt_count` | Make operational recovery explainable. |

### Cashflow Classification

Derived cashflows should be richer than raw cash legs.

| Field | Purpose |
|---|---|
| `classification` | Business meaning such as trade settlement, income, fee, tax, transfer, margin or corporate action. |
| `timing` | Settled, projected, payable, receivable or expected timing bucket. |
| `is_position_flow` | Whether the cashflow belongs to a specific instrument/position. |
| `is_portfolio_flow` | Whether the cashflow belongs at portfolio level. |
| `calculation_type` | How the cashflow was produced. |

This distinction matters for liquidity planning, performance cashflows, client reporting and reconciliation.

### Tax-Lot Supportability

Tax lots should be queryable as a source-backed window, not just hidden implementation detail.

| Field | Purpose |
|---|---|
| `lot_id` | Stable lot identity. |
| `open_quantity` and `original_quantity` | Current remaining lot quantity and original acquired quantity. |
| `acquisition_date` | Date used for holding period and disposal treatment. |
| `cost_basis_base` and `cost_basis_local` | Cost in portfolio base and local/trade currency. |
| `tax_lot_status` | Open or closed lot state. |
| `source_transaction_id` | Transaction that created the lot. |
| `source_lineage` | Source system and calculation-policy provenance. |
| `supportability` | Whether requested lot scope is ready, degraded, incomplete or unavailable. |

### Reconciliation Status Vocabulary

Use one shared status vocabulary across holdings, cash, transaction windows, cashflows, valuation and reports.

| Status | Meaning |
|---|---|
| `COMPLETE` | Evidence is complete and no open issue affects the scope. |
| `PARTIAL` | Some coverage exists but warnings, running controls or incomplete inputs remain. |
| `STALE` | Evidence exists but is older than the freshness threshold. |
| `UNRECONCILED` | Required reconciliation or coverage has not run or has no observed data. |
| `BREAK_OPEN` | A visible non-blocking break remains open. |
| `BLOCKED` | A blocking finding or data-quality issue prevents safe use. |
| `UNKNOWN` | Inputs are insufficient to classify truthfully. |

Break records should carry type, severity, owner, resolution state, age, tolerance, scope, expected value, observed value, blocking flag and repair recommendation.

## Canonical Payload Examples

These examples are intentionally compact. Real APIs should include pagination, audit metadata, entitlement scoping, schema version and source lineage.

### Transaction Payload

```json
{
  "transaction_id": "TXN-2026-000901",
  "transaction_group_id": "TXG-2026-000144",
  "transaction_type": "BUY",
  "transaction_subtype": "MARKET_BUY",
  "transaction_status": "SETTLED",
  "account_id": "ACC-001",
  "portfolio_id": "PORT-001",
  "instrument_id": "ISIN-US0378331005",
  "product_family": "EQUITY",
  "trade_date": "2026-06-27",
  "settlement_date": "2026-07-01",
  "transaction_currency": "USD",
  "gross_amount": 18500.00,
  "net_amount": 18522.50,
  "source_system": "custody-feed",
  "source_transaction_id": "CUST-TXN-998877",
  "idempotency_key": "custody-feed:CUST-TXN-998877",
  "legs": [
    {
      "leg_id": "LEG-001",
      "leg_type": "SECURITY",
      "instrument_id": "ISIN-US0378331005",
      "quantity_delta": 100,
      "amount_type": "PRINCIPAL",
      "price": 185.00,
      "price_basis": "PER_UNIT"
    },
    {
      "leg_id": "LEG-002",
      "leg_type": "CASH",
      "currency": "USD",
      "cash_delta": -18500.00,
      "amount_type": "PRINCIPAL"
    },
    {
      "leg_id": "LEG-003",
      "leg_type": "FEE",
      "currency": "USD",
      "cash_delta": -22.50,
      "amount_type": "FEE"
    }
  ]
}
```

### Position Payload

```json
{
  "position_id": "POS-2026-000124",
  "position_type": "SECURITY_POSITION",
  "position_status": "ACTIVE",
  "as_of_date": "2026-06-30",
  "account_id": "ACC-001",
  "portfolio_id": "PORT-001",
  "instrument_id": "ISIN-US0378331005",
  "product_family": "EQUITY",
  "quantity": 100,
  "quantity_type": "SHARES",
  "market_price": 190.25,
  "price_basis": "PER_UNIT",
  "market_value": 19025.00,
  "valuation_currency": "USD",
  "reporting_currency": "USD",
  "valuation_basis": "EXCHANGE_PRICE",
  "valuation_date": "2026-06-30",
  "cost_basis": 18522.50,
  "unrealized_pnl": 502.50,
  "supportability_state": "SUPPORTED",
  "source_system": "position-book"
}
```

### Lifecycle Event Payload

```json
{
  "event_id": "EVT-2026-004812",
  "event_type": "CORPORATE_ACTION_ANNOUNCED",
  "event_timestamp": "2026-06-27T09:00:00Z",
  "source_system": "custodian-ca-feed",
  "source_event_id": "CA-2026-7788",
  "instrument_id": "ISIN-US0000000001",
  "payload_version": "1.0",
  "idempotency_key": "custodian-ca-feed:CA-2026-7788",
  "event_payload": {
    "corporate_action_type": "STOCK_SPLIT",
    "split_ratio": "2:1",
    "record_date": "2026-07-10",
    "effective_date": "2026-07-15"
  },
  "derived_transaction_ids": []
}
```

### Reversal And Correction Payload

```json
{
  "correction_group_id": "CORR-2026-000030",
  "records": [
    {
      "transaction_id": "TXN-OLD",
      "transaction_type": "DIVIDEND",
      "transaction_status": "CORRECTED"
    },
    {
      "transaction_id": "TXN-REV",
      "transaction_type": "REVERSAL",
      "reversal_of_transaction_id": "TXN-OLD"
    },
    {
      "transaction_id": "TXN-NEW",
      "transaction_type": "DIVIDEND",
      "transaction_subtype": "CORRECTED_WITHHOLDING_TAX",
      "source_transaction_id": "CUST-CORR-123"
    }
  ]
}
```

## Portfolio App Acceptance Criteria

A transaction and position model is useful only if it can support real workflows. Use these acceptance criteria before treating the model as production-ready.

| Capability | Acceptance Criteria |
|---|---|
| Holdings | The app can show current and historical holdings by account, portfolio, product family, currency and supportability state. |
| Transactions | The app can show transaction history with type, status, dates, source, legs, reversals, corrections and evidence. |
| Cash | The app separates settled, pending, available, restricted and overdraft cash by currency and value date. |
| Income | The app separates coupon, dividend, interest, distribution, return of capital, tax, fee and accrual treatment. |
| Corporate actions | The app can explain announcement, entitlement, election, processing, settlement and correction stages. |
| Cost lots | The app can reconcile lots to positions and realized P&L for disposals, redemptions, corporate actions and transfers. |
| Derivatives | The app separates contract position, notional exposure, MTM, margin, collateral and settlement cash. |
| Private markets | The app separates commitment, paid-in capital, unfunded commitment, recallable distribution, NAV and liquidity state. |
| Lending | The app separates loan liability, collateral pledge, eligible collateral value, haircut, LTV and buying power. |
| Insurance | The app separates policy value, cash value, surrender value, benefit value, premium, claim and policy loan. |
| Reporting | Reported holdings, transactions, income and performance can be traced back to source records and calculation version. |
| Operations | Failed settlement, stale price, unmatched source, manual override and unsupported product states are visible. |
| Corrections | Reversals and rebooks preserve original records and produce an auditable net result. |
| APIs | API payloads use generic transaction types and structured legs instead of product-prefixed transaction enums. |
| QA | Golden test cases cover every product family, core lifecycle event, correction path and source-limited state. |

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

## Gold Standard Definition Of Done

Treat the model as implemented well only when these statements are true:

| Area | Done Standard |
|---|---|
| Instrument static | Every supported product has explicit calculation-relevant terms, source, effective date, version and supportability state. |
| Transactions | Every economic event is represented by a generic transaction type, source evidence, idempotency key and typed legs. |
| Positions | Every position has legal/account context, amount basis, currency, as-of date, valuation basis, restriction state and supportability state. |
| Cash | Cash is separated by currency, value date, settled/pending/available/restricted state and source. |
| Lots and P&L | Disposals, redemptions, transfers and corporate actions can explain cost basis, realized P&L and lot changes. |
| Corporate actions | Announcement, entitlement, election, processing, settlement and correction can be shown as one connected lifecycle thread. |
| Lifecycle events | Notices, observations, elections, settlements, failures and corrections are stored as evidence and linked to derived transactions. |
| Valuation | Market value, NAV, policy value, appraisal and FX rates carry date, source, basis and stale/manual/estimated state. |
| Exposure | Analytical exposure is derived and labelled separately from legal holdings. |
| Reconciliation | Position, cash, lot, income, ledger, corporate-action and report reconciliation produce explicit breaks with owners. |
| Corrections | Original, reversal and replacement records remain available and the corrected business view is reproducible. |
| APIs | APIs expose source, supportability, lifecycle links, legs and degraded states rather than hiding complexity. |
| UI and reports | Screens and reports distinguish held value, tradeable value, available cash, pledged value, exposure and unsupported/estimated values. |
| QA | Golden scenarios cover each product family, major lifecycle event, corporate-action pattern, correction path and source-limited state. |
| Operations | Support users can answer what changed, why it changed, whether it reconciled, who owns a break and what evidence supports it. |

If any of these are false, the model may still be useful as a partial implementation, but it should not be treated as a complete cross-product portfolio-management model.

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
