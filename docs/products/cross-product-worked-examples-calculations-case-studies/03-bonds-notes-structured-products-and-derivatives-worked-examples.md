# 03 - Bonds, Notes, Structured Products and Derivatives Worked Examples

## 1. Bond purchase with accrued interest

### Business scenario

Buy USD 100,000 nominal bond at clean price 98.50 with accrued interest USD 750.

### Cash paid

```text
Principal cost = 100,000 x 98.50% = 98,500
Dirty cash paid = 98,500 + 750 = 99,250
```

### Transaction

| Field | Value |
|---|---:|
| transaction_type | BOND_BUY |
| nominal_delta | +100,000 |
| clean_price | 98.50 |
| principal_cost | 98,500 |
| accrued_interest_paid | 750 |
| cash_delta | -99,250 |

### Position

| Field | Value |
|---|---:|
| nominal | 100,000 |
| cost_basis | 98,500 or 99,250 depending accounting policy |
| accrued_interest | 750 |
| market_value_clean | based on clean price |
| market_value_dirty | clean MV + accrued |

### QA assertions

- Clean and dirty price are not confused.
- Accrued interest is separately available.
- Cash paid equals principal cost plus accrued and fees.

## 2. Bond coupon payment

### Business scenario

Bond pays 5% annual coupon semi-annually on USD 100,000 nominal.

### Coupon

```text
Coupon = 100,000 x 5% / 2 = 2,500
```

### Transaction

| Field | Value |
|---|---:|
| transaction_type | BOND_COUPON |
| income_amount | 2,500 |
| cash_delta | +2,500 |
| nominal_delta | 0 |

### Performance treatment

Investment income.

### QA assertions

- Coupon does not reduce nominal.
- Coupon is included in total return.
- Accrued interest resets or reduces appropriately.

## 3. Bond sale and realized P&L

### Business scenario

Sell USD 100,000 nominal at clean price 101.00. Cost clean basis was 98,500. Accrued interest received is 600.

### Calculation

```text
Sale principal = 100,000 x 101% = 101,000
Realized P&L = 101,000 - 98,500 = 2,500
Cash received = 101,000 + 600 = 101,600
```

### QA assertions

- Realized P&L excludes accrued interest if accrued interest is treated separately.
- Position nominal becomes zero.
- Accrued interest received is not double-counted as capital gain.

## 4. Structured note autocall

### Business scenario

Client holds USD 100,000 autocallable note. Quarterly observation shows underlying at 105% of initial level, above autocall level 100%. Coupon due is 2%.

### Lifecycle event

| Field | Value |
|---|---|
| event_type | AUTOCALL_TRIGGER |
| observed_level | 105% |
| result | triggered |
| payment_date | T+5 |

### Cash on redemption

```text
Coupon = 100,000 x 2% = 2,000
Principal = 100,000
Total cash = 102,000
```

### Transactions on payment date

| Transaction | Nominal | Cash |
|---|---:|---:|
| NOTE_AUTOCALL_REDEMPTION | -100,000 | +100,000 |
| NOTE_COUPON_PAYMENT | 0 | +2,000 |

### QA assertions

- Observation event is not itself cash movement.
- Position closes on redemption/payment date.
- Coupon and principal are separately reportable.

## 5. Reverse convertible physical settlement

### Business scenario

USD 100,000 equity-linked note linked to ABC stock with strike USD 50. Note knocks in and settles physically. Final stock price is USD 35.

### Delivered shares

```text
Shares delivered = 100,000 / 50 = 2,000
Market value of shares = 2,000 x 35 = 70,000
Economic loss = 30,000
```

### Transaction group

| Transaction | Instrument | Quantity / nominal |
|---|---|---:|
| NOTE_PHYSICAL_REDEMPTION_OUT | note | -100,000 nominal |
| EQUITY_DELIVERY_IN | ABC equity | +2,000 shares |

### Reporting

- Note closes.
- Equity position opens.
- Client report should explain conversion/physical delivery.
- Loss should be visible through market value of delivered shares.

### QA assertions

- Delivered share count uses strike, not final price.
- Underlying exposure becomes legal equity holding only after delivery.
- Cash-in-lieu handled for fractional shares if relevant.

## 6. Option premium and exercise

### Business scenario

Buy 10 call option contracts, contract size 100, premium USD 2.50.

### Premium

```text
Premium paid = 10 x 100 x 2.50 = 2,500
```

### Exercise

Strike USD 50. Exercise creates purchase of 1,000 shares at USD 50.

### Transactions

| Stage | Transaction | Cash / Position |
|---|---|---|
| Buy option | OPTION_BUY_PREMIUM | cash -2,500, option +10 contracts |
| Exercise | OPTION_EXERCISE_OUT | option -10 contracts |
| Exercise | EQUITY_BUY_FROM_EXERCISE | shares +1,000, cash -50,000 |

### QA assertions

- Premium is initial cost of option.
- Exercise creates underlying equity position.
- Option position closes.
- Cost basis policy for shares includes premium if applicable.

## 7. Futures daily variation margin

### Business scenario

Long 1 futures contract. Price increases, variation margin USD 1,200 credited.

### Transaction

| Field | Value |
|---|---:|
| transaction_type | FUTURES_VARIATION_MARGIN |
| cash_delta | +1,200 |
| realized_pnl | +1,200 |
| position_quantity | unchanged |

### QA assertions

- Futures P&L is realized daily through variation margin.
- Position remains open until offset/expiry.
- Margin cash movement is not external cashflow.

## 8. Common edge cases

| Edge case | Expected handling |
|---|---|
| callable bond redeemed early | close nominal and book call premium |
| missing bond price | use evaluated price or stale-price flag |
| structured note barrier touched | lifecycle status update, no cash until payoff |
| option expires worthless | close option, realized loss equals remaining cost |
| derivative collateral movement | collateral cash movement, not investment income |
| CLN credit event | write-down and recovery lifecycle |
