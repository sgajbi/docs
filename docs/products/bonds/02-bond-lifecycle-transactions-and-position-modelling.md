# 02 - Bond Lifecycle, Transactions, and Position Modelling

## 1. Core Design Principle

A bond should normally be modelled as **one security position** in the client portfolio.

The position is usually measured by:

```text
Nominal amount held
Current face amount
Clean market price
Accrued interest
Dirty market value
Cost / book value
Realized and unrealized P&L
```

Do not create a separate position for each coupon, rating, call date, or cashflow. Those are schedules, lifecycle events, valuations, or accounting postings.

---

## 2. Bond Lifecycle Overview

Most bonds follow this broad lifecycle:

| Stage | Business Event | System Object | Transaction? |
|---|---|---|---|
| Product setup | Bond terms are created or loaded | Instrument + bond contract | No |
| Primary issue | Issuer offers bond to investors | Offering / order | No until trade booked |
| Subscription / trade | Client buys bond | Trade transaction | Yes |
| Settlement | Cash paid, bond received | Settled position | Usually status update + transaction settlement |
| Holding period | Bond price and accrued interest change | Valuation records | No |
| Coupon accrual | Interest accrues between coupon dates | Accrual record or derived value | Optional accounting transaction |
| Coupon payment | Issuer pays interest | Income transaction | Yes |
| Secondary sale | Client sells before maturity | Sell transaction | Yes |
| Call / put / tender | Bond redeemed or sold under special terms | Lifecycle event + redemption/sale transaction | Yes when economic posting occurs |
| Partial redemption / amortisation | Principal is partly repaid | Principal transaction | Yes |
| Maturity | Principal repaid | Redemption transaction | Yes |
| Default / restructuring | Issuer fails or terms change | Credit event | Yes when write-down/recovery booked |
| Closure | Position is zero | Position status | No separate transaction needed |

---

## 3. Lifecycle Event vs Transaction

This distinction is critical.

| Object | Purpose | Example |
|---|---|---|
| Lifecycle event | Records that something happened or was determined | Coupon rate fixed, bond called, default announced, maturity confirmed |
| Transaction | Records an economic/accounting posting | Cash coupon paid, nominal redeemed, security sold, write-down booked |

Examples:

| Event | Transaction? | Treatment |
|---|---|---|
| Rating downgrade | No | Update rating/risk history. |
| Coupon rate fixing for FRN | No, unless accrual posting required | Store fixing and projected coupon. |
| Ex-coupon date reached | Usually no | Affects entitlement/pricing. |
| Coupon payment date | Yes | Book coupon cash income. |
| Issuer announces call | Lifecycle event first | No transaction until redemption is effective/settled. |
| Call redemption payment | Yes | Reduce nominal and book cash. |
| Default notice | Lifecycle event first | Transaction only when write-down/recovery is booked. |

---

## 4. Recommended Bond Transaction Types

Use transaction types based on economic meaning, not every possible product subtype.

| Transaction Type | Use |
|---|---|
| `BOND_SUBSCRIPTION` | Primary market subscription/allocation. |
| `BOND_SECONDARY_BUY` | Secondary market purchase. |
| `BOND_SECONDARY_SELL` | Secondary market sale. |
| `BOND_TRANSFER_IN` | Transfer from another account/custodian without market trade. |
| `BOND_TRANSFER_OUT` | Transfer out to another account/custodian. |
| `BOND_COUPON_ACCRUAL` | Optional accounting accrual posting. Many portfolio systems derive accrual instead. |
| `BOND_COUPON_PAYMENT` | Cash coupon received. |
| `BOND_ACCRUED_INTEREST_PAID` | Accrued interest paid to seller when buying. Can also be modelled as a leg of buy. |
| `BOND_ACCRUED_INTEREST_RECEIVED` | Accrued interest received from buyer when selling. Can also be modelled as a leg of sell. |
| `BOND_PARTIAL_REDEMPTION` | Partial return of principal. |
| `BOND_AMORTISATION_REDEMPTION` | Scheduled principal amortisation. |
| `BOND_SINKING_FUND_REDEMPTION` | Principal redemption through sinking fund. |
| `BOND_MATURITY_REDEMPTION` | Principal repaid at maturity. |
| `BOND_CALL_REDEMPTION` | Issuer calls/redeems bond before maturity. |
| `BOND_PUT_REDEMPTION` | Investor exercises put option. |
| `BOND_TENDER_ACCEPTED` | Investor sells into tender offer. |
| `BOND_EXCHANGE_OUT` | Old bond surrendered in exchange offer. |
| `BOND_EXCHANGE_IN` | New bond received in exchange offer. |
| `BOND_CONVERSION_OUT` | Convertible bond surrendered. |
| `EQUITY_CONVERSION_IN` | Equity received after conversion. |
| `BOND_DEFAULT_WRITEDOWN` | Position written down after default/impairment. |
| `BOND_RECOVERY_PAYMENT` | Cash or security recovery after default/restructuring. |
| `BOND_RESTRUCTURING_ADJUSTMENT` | Change in nominal, coupon, maturity, or terms due to restructuring. |
| `BOND_FEE` | Custody/brokerage/platform fee. |
| `BOND_TAX_WITHHOLDING` | Tax withheld from coupon or redemption. |
| `BOND_CORRECTION_REVERSAL` | Correction/reversal of prior transaction. |

Practical design choice:

```text
For buy/sell transactions, accrued interest can be a separate transaction type or a transaction leg.
A transaction-leg model is cleaner because the trade is one economic event with multiple postings.
```

---

## 5. Core Transaction Fields

| Field | Meaning |
|---|---|
| transaction_id | Unique transaction ID. |
| transaction_group_id | Groups related legs, e.g., buy + accrued interest + fee + tax. |
| account_id | Portfolio/account. |
| instrument_id | Bond instrument. |
| transaction_type | Economic event type. |
| trade_date | Execution date. |
| settlement_date / value_date | Date cash/security settlement occurs. |
| booking_date | Date transaction is booked in system. |
| ex_date | For coupon entitlement when applicable. |
| nominal_delta | Change in face amount held. |
| current_face_delta | Change after factor/amortisation if applicable. |
| quantity_units | Optional units if system uses denomination-based units. |
| clean_price_pct | Trade price excluding accrued interest. |
| dirty_price_pct | Trade price including accrued interest. |
| accrued_interest_amount | Interest paid/received through settlement. |
| gross_amount | Clean consideration or principal/coupon amount depending on transaction type. |
| fees | Brokerage, custody, platform, execution fees. |
| tax | Withholding tax, stamp duty, other taxes. |
| net_cash_amount | Actual cash movement. |
| cash_currency | Cash currency. |
| settlement_currency | Settlement currency if different. |
| fx_rate | FX rate used for base currency reporting. |
| realized_pnl | Realized gain/loss if disposal/redemption. |
| income_amount | Coupon/income amount. |
| cost_basis_amount | Cost basis consumed or created. |
| lifecycle_event_id | Link to call/maturity/default/conversion event. |
| source_system | Custodian, broker, corporate action feed, manual booking, etc. |
| reversal_of_transaction_id | Link for corrections. |
| performance_classification | External flow, income, internal transaction, fee, tax, corporate action, loss. |

---

## 6. Position Model

A bond position should support nominal, market value, accrued income, and lifecycle status.

| Field | Meaning |
|---|---|
| account_id | Portfolio/account. |
| instrument_id | Bond security. |
| nominal_amount | Face value currently held. |
| original_nominal_amount | Face value originally purchased or transferred in. |
| current_face_amount | Face value after amortisation/factor reductions. |
| quantity_units | Number of units if denomination-based. |
| denomination | Face value per unit. |
| average_clean_cost_pct | Average clean cost as % of par. |
| book_cost_amount | Cost basis, based on bank accounting policy. |
| amortised_cost_amount | Optional, for accounting/reporting. |
| accrued_interest_amount | Interest accrued since last coupon date. |
| clean_market_price_pct | Current clean price. |
| dirty_market_price_pct | Clean price + accrued interest per par. |
| clean_market_value | Current face x clean price %. |
| dirty_market_value | Clean market value + accrued interest. |
| valuation_currency | Usually bond currency. |
| base_market_value | Converted into portfolio base currency. |
| unrealized_pnl | Market value minus cost basis. |
| realized_pnl | Realized through sale/redemption/default. |
| lifecycle_status | ACTIVE / CALLED / PUT / MATURED / DEFAULTED / RESTRUCTURED / CONVERTED. |
| next_coupon_date | Next scheduled coupon payment. |
| next_call_date | Next call date, if callable. |
| next_put_date | Next put date, if putable. |
| maturity_date | Final contractual maturity, if applicable. |
| rating_current | Current external/internal rating. |
| yield_to_maturity | Latest YTM. |
| yield_to_worst | Latest YTW. |
| duration | Latest duration. |
| dv01 | Latest DV01/PV01. |

Important:

```text
Use nominal/current face as the primary position quantity.
Use clean and dirty valuation separately.
Do not confuse market value with nominal amount.
```

---

## 7. Primary Market Subscription

When a client subscribes to a newly issued bond:

| Item | Example |
|---|---:|
| Nominal | USD 100,000 |
| Issue price | 99.50% |
| Fees | USD 100 |
| Settlement cash | USD 99,600 |

Transaction group:

| Transaction / Leg | Instrument | Nominal Delta | Cash Impact |
|---|---|---:|---:|
| `BOND_SUBSCRIPTION` | Bond | +100,000 | -99,500 |
| Fee leg / `BOND_FEE` | Cash | 0 | -100 |

Position after settlement:

| Field | Value |
|---|---:|
| Nominal | 100,000 |
| Average clean cost | 99.50% |
| Book cost | 99,500 plus or excluding fee depending policy |

---

## 8. Secondary Market Buy with Accrued Interest

This is one of the most important modelling scenarios.

Example:

| Item | Value |
|---|---:|
| Nominal bought | USD 100,000 |
| Clean price | 98.00% |
| Accrued interest | USD 1,200 |
| Fee | USD 100 |
| Cash paid | USD 99,300 |

Calculation:

```text
Clean consideration = 100,000 x 98.00% = 98,000
Accrued interest paid = 1,200
Fee = 100
Total cash paid = 98,000 + 1,200 + 100 = 99,300
```

Transaction group:

| Leg | Transaction Type | Bond Nominal | Cash |
|---|---|---:|---:|
| Security leg | `BOND_SECONDARY_BUY` | +100,000 | -98,000 |
| Accrued interest leg | `BOND_ACCRUED_INTEREST_PAID` or leg attribute | 0 | -1,200 |
| Fee leg | `BOND_FEE` | 0 | -100 |

Accounting/performance note:

```text
Accrued interest paid is not usually investment cost in the same way as clean price.
It is compensation paid to the seller for interest earned before settlement.
When the next coupon is received, part of it economically recovers the accrued interest paid.
```

---

## 9. Coupon Payment

Example:

| Item | Value |
|---|---:|
| Nominal | USD 100,000 |
| Coupon | 5% p.a. |
| Frequency | Semi-annual |
| Gross coupon | USD 2,500 |
| Withholding tax | USD 250 |
| Net cash | USD 2,250 |

Transaction group:

| Leg | Transaction Type | Cash |
|---|---|---:|
| Gross coupon income | `BOND_COUPON_PAYMENT` | +2,500 |
| Tax withholding | `BOND_TAX_WITHHOLDING` | -250 |
| Net cash posted | Cash position | +2,250 |

Position impact:

```text
Nominal amount does not change.
Accrued interest resets or reduces after coupon date.
Income is recognized.
```

---

## 10. Secondary Market Sale with Accrued Interest

Example:

| Item | Value |
|---|---:|
| Nominal sold | USD 100,000 |
| Clean sale price | 101.00% |
| Accrued interest received | USD 800 |
| Fee | USD 100 |
| Cash received | USD 101,700 |

Calculation:

```text
Clean sale proceeds = 100,000 x 101.00% = 101,000
Accrued interest received = 800
Fee = 100
Net cash received = 101,000 + 800 - 100 = 101,700
```

Transaction group:

| Leg | Transaction Type | Bond Nominal | Cash |
|---|---|---:|---:|
| Security sale | `BOND_SECONDARY_SELL` | -100,000 | +101,000 |
| Accrued interest | `BOND_ACCRUED_INTEREST_RECEIVED` or leg attribute | 0 | +800 |
| Fee | `BOND_FEE` | 0 | -100 |

Realized P&L should be computed on clean price/cost basis, with accrued interest treated as income/accrual settlement according to accounting policy.

---

## 11. Maturity Redemption

At maturity, the issuer repays principal.

Example:

| Item | Value |
|---|---:|
| Nominal held | USD 100,000 |
| Redemption price | 100% |
| Final coupon | USD 2,500 |

Transaction group:

| Transaction Type | Nominal Delta | Cash Impact |
|---|---:|---:|
| `BOND_MATURITY_REDEMPTION` | -100,000 | +100,000 |
| `BOND_COUPON_PAYMENT` | 0 | +2,500 |

Position closes when nominal reaches zero.

---

## 12. Callable Bond Lifecycle

Callable bonds require call schedule and event handling.

Lifecycle:

| Step | Object |
|---|---|
| Bond terms include call schedule | Bond contract / call schedule |
| Issuer announces call | Lifecycle event: CALL_ANNOUNCED |
| Record date / ex-date | Entitlement event |
| Effective call date | Lifecycle event: CALL_EFFECTIVE |
| Cash redemption paid | `BOND_CALL_REDEMPTION` transaction |

Example:

| Item | Value |
|---|---:|
| Nominal | USD 100,000 |
| Call price | 102% |
| Cash redemption | USD 102,000 |

Transaction:

| Transaction Type | Nominal Delta | Cash Impact |
|---|---:|---:|
| `BOND_CALL_REDEMPTION` | -100,000 | +102,000 |

Advisory issue:

```text
A high coupon callable bond may be called when rates fall, causing reinvestment risk.
Yield to worst is often more relevant than yield to maturity.
```

---

## 13. Putable Bond Lifecycle

Putable bonds give investor the right to redeem/sell back to issuer on specific dates.

Lifecycle:

| Step | Object |
|---|---|
| Put date schedule | Bond contract / put schedule |
| Client elects put | Instruction / lifecycle event |
| Put confirmed | Lifecycle event |
| Cash redemption paid | `BOND_PUT_REDEMPTION` transaction |

Position impact is similar to call redemption, but initiated by investor.

---

## 14. Amortising / Sinking Fund Bond Lifecycle

Some bonds repay principal gradually.

Example amortisation schedule:

| Date | Principal Repayment |
|---|---:|
| Year 1 | 10% |
| Year 2 | 10% |
| Year 3 | 20% |
| Year 4 | 30% |
| Year 5 | 30% |

Transaction type:

| Event | Transaction Type | Position Impact |
|---|---|---|
| Scheduled principal repayment | `BOND_AMORTISATION_REDEMPTION` | Reduces current face amount. |
| Sinking fund draw | `BOND_SINKING_FUND_REDEMPTION` | Reduces nominal/current face depending terms. |

Important modelling requirement:

```text
For amortising bonds, original nominal and current face may differ.
Valuation must use current factor/current face, not original face.
```

---

## 15. Zero-Coupon Bond Lifecycle

A zero-coupon bond has no periodic coupons.

Lifecycle:

| Event | Transaction Type |
|---|---|
| Buy at discount | `BOND_SUBSCRIPTION` or `BOND_SECONDARY_BUY` |
| Accretion of discount | Optional derived/accounting accrual |
| Sale before maturity | `BOND_SECONDARY_SELL` |
| Maturity redemption | `BOND_MATURITY_REDEMPTION` |

Example:

| Item | Value |
|---|---:|
| Buy price | 80% |
| Maturity redemption | 100% |
| Return source | Price accretion to par |

Performance systems should distinguish between market price change and accounting accretion if both are needed.

---

## 16. Floating-Rate Bond Lifecycle

Floating-rate bonds need rate fixing.

Lifecycle:

| Event | System Object | Transaction? |
|---|---|---|
| Coupon reset date | Rate fixing event | No |
| Coupon amount determined | Coupon schedule update | No |
| Coupon payment date | Coupon transaction | Yes |

Example:

```text
Coupon = 3-month SOFR + 1.50%
```

Store:

| Field | Example |
|---|---|
| reference_rate | SOFR |
| reset_frequency | Quarterly |
| spread | 1.50% |
| cap/floor | Optional |
| fixing_lag | 2 business days |
| coupon_period | Start/end date |

---

## 17. Convertible Bond Lifecycle

Convertible bonds are hybrid instruments.

Lifecycle:

| Event | Transaction Type | Position Impact |
|---|---|---|
| Buy convertible | `BOND_SUBSCRIPTION` or `BOND_SECONDARY_BUY` | Creates bond position. |
| Coupon | `BOND_COUPON_PAYMENT` | Income, no nominal change. |
| Conversion election | Lifecycle event | No transaction until conversion settles. |
| Bond surrendered | `BOND_CONVERSION_OUT` | Bond nominal reduced/closed. |
| Equity received | `EQUITY_CONVERSION_IN` | Equity position created. |
| Cash-in-lieu | `BOND_CASH_IN_LIEU` or generic cash adjustment | Fractional amount. |

Example:

| Item | Value |
|---|---:|
| Convertible nominal | USD 100,000 |
| Conversion price | USD 50 |
| Shares delivered | 2,000 |

Transaction group:

| Transaction | Instrument | Quantity / Nominal |
|---|---|---:|
| `BOND_CONVERSION_OUT` | Convertible bond | -100,000 nominal |
| `EQUITY_CONVERSION_IN` | Issuer equity | +2,000 shares |
| Cash-in-lieu | Cash | small amount if fractional |

Cost basis treatment depends on accounting/tax policy.

---

## 18. Default, Restructuring, and Recovery Lifecycle

Default handling must be event-driven and auditable.

Lifecycle:

| Step | Object | Transaction? |
|---|---|---|
| Missed coupon / grace period | Lifecycle event | Usually no immediate transaction unless impairment booked. |
| Default confirmed | Credit event | Maybe write-down depending policy. |
| Bond price collapses | Valuation | No transaction. |
| Impairment/write-down booked | `BOND_DEFAULT_WRITEDOWN` | Yes. |
| Restructuring terms confirmed | Restructuring event | Usually yes when old/new securities/cash booked. |
| Recovery received | `BOND_RECOVERY_PAYMENT` / `BOND_EXCHANGE_IN` | Yes. |

Example restructuring:

| Transaction | Instrument | Nominal / Cash |
|---|---|---:|
| `BOND_EXCHANGE_OUT` | Old defaulted bond | -100,000 nominal |
| `BOND_EXCHANGE_IN` | New restructured bond | +60,000 nominal |
| `BOND_RECOVERY_PAYMENT` | Cash | +5,000 |
| `BOND_DEFAULT_WRITEDOWN` | Old bond | Loss booked if required |

Important distinction:

```text
Market price decline is valuation loss.
Write-down is an accounting transaction/event.
Recovery is a later economic posting.
```

---

## 19. Transfers

Transfers are not buys/sells.

| Transfer Type | Use |
|---|---|
| `BOND_TRANSFER_IN` | Bond moved into portfolio from another account/custodian. |
| `BOND_TRANSFER_OUT` | Bond moved out of portfolio. |

A transfer may or may not be an external performance cashflow depending on portfolio performance policy.

Recommended fields:

| Field | Meaning |
|---|---|
| transfer_value | Market value used for performance opening basis. |
| cost_basis_carried | Whether historical cost is carried in. |
| accrued_interest_carried | Accrued interest on transfer date. |
| tax_lot_id | If tax lots are tracked. |

---

## 20. Performance Treatment

| Event | Performance Classification |
|---|---|
| Buy funded from existing portfolio cash | Internal investment transaction. |
| Subscription funded by new client cash | External contribution plus internal buy. |
| Coupon paid into portfolio cash | Investment income. |
| Coupon withdrawn by client | Income first, then external withdrawal. |
| Accrued interest paid on purchase | Accrual/income adjustment, not usually external flow. |
| Sale proceeds kept in portfolio | Internal disposal. |
| Redemption proceeds kept in portfolio | Internal disposal. |
| Maturity proceeds withdrawn | Internal redemption then external withdrawal. |
| Transfer in | External in-kind contribution or internal transfer depending scope. |
| Transfer out | External in-kind withdrawal or internal transfer depending scope. |
| Default write-down | Investment loss. |
| Recovery payment | Investment recovery/gain. |
| Conversion to equity | Internal security transformation. |
| Fees paid from portfolio | Expense. |
| Tax withheld | Tax expense or withholding, depending reporting policy. |

For TWR:

```text
External client contributions and withdrawals are external cashflows.
Coupons, accrued interest, sales, redemptions, defaults, recoveries, and conversions are investment activity inside the portfolio unless cash/securities enter or leave the measured portfolio from outside.
```

---

## 21. Common Modelling Mistakes

| Mistake | Correct Treatment |
|---|---|
| Modelling bond quantity as market value | Use nominal/current face as quantity; price drives market value. |
| Ignoring accrued interest | Use clean and dirty price correctly. |
| Treating coupon as external cashflow | Coupon is investment income unless withdrawn. |
| Treating buy/sell price as dirty when it is clean | Store clean price, accrued interest, and dirty value separately. |
| Ignoring call schedule | Callable bonds require yield-to-call/yield-to-worst and lifecycle handling. |
| Treating rating as static | Store rating history and effective dates. |
| Ignoring current factor for amortising/ABS/MBS | Use current face/factor. |
| Treating perpetual as normal long maturity | Perpetuals need call/extension/coupon-deferral modelling. |
| Ignoring FX | Foreign-currency bonds require local and base currency values. |
| Mixing issuer and guarantor | Store issuer, guarantor, obligor, collateral, and seniority separately. |

---

## 22. Practitioner Summary

In a wealth platform, a bond position should be modelled primarily by nominal amount/current face, with clean price, accrued interest, dirty market value, cost basis, yield, duration, rating, and lifecycle status. Bond lifecycle includes primary subscription, secondary buy/sell, coupon accrual and payment, maturity redemption, call/put redemption, partial amortisation, conversion, default, restructuring, recovery, and transfers. I would separate lifecycle events from transactions: rating change, call announcement, coupon fixing, and default notice are events; coupon cash, redemption cash, conversion settlement, write-down, and recovery are accounting transactions. Accrued interest should be modelled explicitly because bond trades usually settle on clean price plus accrued interest. For performance, coupons are income, defaults are investment losses, redemptions and sales are internal investment activity unless cash leaves the measured portfolio.
