# 01 — Cross-Product Canonical Position and Transaction Model

## 1. Purpose

A wealth platform should support multiple product families without creating completely separate accounting models for each product. Notes, bonds, funds and equities have different economics, but a common architecture can support them if the model separates instrument master, product-specific terms, transactions, transaction legs, positions, lots, valuations, lifecycle events and analytical exposures.

## 2. Canonical instrument hierarchy

```text
Issuer / Manager / Fund House / Reference Entity
  -> Instrument
      -> Product-Specific Contract / Terms
      -> Listing / Share Class / Quote Line
      -> Lifecycle Events
      -> Classifications
```

| Product | Core instrument | Product-specific terms |
|---|---|---|
| Equity | Share/security | Share class, listing, voting rights, corporate actions |
| Bond | Debt security | Coupon, maturity, call/put, seniority, day count |
| Note | Structured debt security | Payoff, underlyings, barriers, observations, settlement |
| Fund | Fund share/unit class | NAV cycle, fees, dealing rules, distribution policy |

## 3. Canonical position model

| Field | Equity | Bond | Note | Fund |
|---|---|---|---|---|
| quantity | Shares | Units or nominal units | Units | Units |
| nominal_amount | Usually no | Yes | Yes | Usually no |
| market_price | Price/share | % of par | % of par or model price | NAV/unit |
| accrued_income | Dividend receivable optional | Accrued interest | Depends on coupon type | Distribution receivable optional |
| cost_basis | Yes | Yes | Yes | Yes |
| lifecycle_status | Active/delisted/suspended | Active/matured/defaulted | Active/autocalled/knocked-in/matured | Active/suspended/gated |
| lookthrough | Direct/ADR/fund exposure | Credit/duration exposure | Underlying exposure | Underlying fund holdings if available |

## 4. Canonical transaction structure

Use a transaction header plus explicit legs.

| Field | Meaning |
|---|---|
| transaction_id | Unique transaction |
| transaction_group_id | Groups related legs/events |
| account_id | Portfolio/account |
| instrument_id | Security/fund/note/bond |
| transaction_type | Business transaction type |
| trade_date | Economic trade date |
| settlement_date | Value date |
| booking_date | Posting date |
| quantity_delta | Unit/share movement |
| nominal_delta | Nominal movement if relevant |
| gross_amount | Before fees/tax |
| fees | Explicit fees |
| tax | Tax/withholding |
| net_amount | Cash impact |
| currency | Transaction currency |
| fx_rate | FX to reporting/base currency |
| source_event_id | Order/corporate action/lifecycle event |
| performance_classification | External flow/income/internal/fee/etc. |

Leg types:

| Leg type | Example |
|---|---|
| SECURITY | Shares, bonds, notes, fund units |
| CASH | Subscription, redemption, sale proceeds |
| INCOME | Dividend, coupon, distribution |
| FEE | Brokerage, management fee, platform fee |
| TAX | Withholding tax, stamp duty |
| FX | Currency conversion |
| ACCRUAL | Interest/dividend/distribution receivable |
| WRITEDOWN | Default, impairment, credit event |

## 5. Lifecycle events vs transactions

Important rule:

> Lifecycle events explain what should happen. Transactions record what actually changed economically.

| Lifecycle event | Transaction generated |
|---|---|
| Equity split announcement | Split transaction on effective date |
| Equity dividend event | Dividend receivable/cash/tax postings |
| Bond coupon date | Coupon payment |
| Bond call notice | Call redemption on payment date |
| Note autocall observation | Autocall redemption on settlement date |
| Fund NAV published | Valuation update, not transaction |
| Fund gate applied | Order status change, not settlement transaction until paid |

## 6. Analytics exposure model

Actual position and analytical exposure differ.

| Product | Actual position | Analytical exposure |
|---|---|---|
| Equity | Shares/ADR | Issuer, sector, country, currency |
| Bond | Bond security | Issuer, credit rating, duration, spread, currency |
| Note | Note security | Issuer + underlying equity/FX/rate/credit exposure |
| Fund | Fund units | Look-through asset classes/sectors/countries/managers |

Exposure table should support direct exposure, look-through exposure, delta-adjusted exposure, issuer-group exposure, currency exposure, mandate classification exposure and collateral eligibility exposure.

## 7. Performance classification model

| Classification | Meaning |
|---|---|
| EXTERNAL_CONTRIBUTION | New money/security entering portfolio |
| EXTERNAL_WITHDRAWAL | Money/security leaving portfolio |
| INTERNAL_INVESTMENT_ACTIVITY | Buy/sell/rebalance funded inside portfolio |
| INVESTMENT_INCOME | Dividend/coupon/distribution |
| FEE_EXPENSE | Fees paid from portfolio |
| TAX_EXPENSE | Withholding/transaction tax |
| CORPORATE_ACTION_NEUTRAL | Split/conversion that should not create return |
| INVESTMENT_GAIN_LOSS | Market movement, default/writeoff, realized/unrealized P&L |
| FX_GAIN_LOSS | Currency movement |

## 8. Design principle

Do not let product-specific complexity leak into every downstream service. Encapsulate product-specific lifecycle rules in product engines, but publish normalized positions, transactions, valuations, exposures and lifecycle events to analytics/reporting services.
