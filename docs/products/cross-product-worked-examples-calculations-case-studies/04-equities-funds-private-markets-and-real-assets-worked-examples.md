# 04 - Equities, Funds, Private Markets and Real Assets Worked Examples

## 1. Equity purchase and dividend

### Business scenario

Buy 1,000 shares at USD 20. Later receive USD 0.50 dividend per share.

### Purchase

```text
Cost = 1,000 x 20 = 20,000
```

### Dividend

```text
Dividend = 1,000 x 0.50 = 500
```

### Transactions

| Transaction | Quantity | Cash | Income |
|---|---:|---:|---:|
| EQUITY_BUY | +1,000 | -20,000 | 0 |
| DIVIDEND_CASH | 0 | +500 | +500 |

### QA assertions

- Dividend does not change share quantity.
- Dividend is investment income.
- Dividend withholding tax, if any, is separate.

## 2. Stock split

### Business scenario

Hold 1,000 shares at average cost USD 20. Company announces 2-for-1 split.

### Position impact

```text
New quantity = 1,000 x 2 = 2,000
New average cost = 20 / 2 = 10
Total cost basis unchanged = 20,000
```

### QA assertions

- Quantity doubles.
- Total cost unchanged.
- Average cost halves.
- Market value should remain approximately unchanged before price movement.

## 3. Rights issue

### Business scenario

Hold 1,000 shares. Rights issue: 1 new share for every 5 held at subscription price USD 8.

### Rights entitlement

```text
Rights shares = 1,000 / 5 = 200
Subscription cash = 200 x 8 = 1,600
```

### If exercised

| Transaction | Quantity | Cash |
|---|---:|---:|
| RIGHTS_ENTITLEMENT | +200 rights | 0 |
| RIGHTS_EXERCISE_OUT | -200 rights | 0 |
| EQUITY_SUBSCRIPTION | +200 shares | -1,600 |

### QA assertions

- Rights entitlement is separate from final share subscription.
- Unexercised rights expire or may be sold if tradable.
- Cost basis adjusted by policy.

## 4. Fund subscription with front-end fee

### Business scenario

Client invests USD 100,000 into fund. Front-end fee 2%. NAV USD 10.

### Calculation

```text
Fee = 100,000 x 2% = 2,000
Net invested = 98,000
Units = 98,000 / 10 = 9,800
```

### Transactions

| Transaction | Units | Cash | Fee |
|---|---:|---:|---:|
| FUND_SUBSCRIPTION | +9,800 | -98,000 | 0 |
| FUND_SUBSCRIPTION_FEE | 0 | -2,000 | 2,000 |

### QA assertions

- Fee is not converted into fund units.
- Units use net invested amount.
- Reporting shows fee clearly.

## 5. Fund distribution reinvestment

### Business scenario

Hold 10,000 fund units. Distribution USD 0.20 per unit. Reinvestment NAV USD 10.

### Calculation

```text
Distribution = 10,000 x 0.20 = 2,000
Reinvested units = 2,000 / 10 = 200
```

### Transactions

| Transaction | Units | Cash |
|---|---:|---:|
| FUND_DISTRIBUTION | 0 | +2,000 |
| FUND_REINVESTMENT | +200 | -2,000 |

### QA assertions

- Income is recognized.
- Cash may net to zero if automatic reinvestment.
- Unit count increases.

## 6. Private equity capital call

### Business scenario

Client commits USD 1,000,000 to private equity fund. Capital call is 20%.

### Calculation

```text
Capital called = 1,000,000 x 20% = 200,000
Unfunded commitment = 800,000
```

### Transactions

| Transaction | Commitment | Funded position | Cash |
|---|---:|---:|---:|
| COMMITMENT_OPEN | +1,000,000 | 0 | 0 |
| CAPITAL_CALL | -200,000 unfunded | +200,000 funded | -200,000 |

### Reporting

Show both funded NAV and unfunded commitment.

### QA assertions

- Commitment is not same as market value.
- Unfunded exposure remains visible.
- Capital call funded from portfolio cash is internal investment activity.

## 7. Private market distribution

### Business scenario

Private equity fund distributes USD 50,000, of which USD 30,000 is return of capital and USD 20,000 is gain/income.

### Transactions

| Component | Cash | Accounting |
|---|---:|---|
| Return of capital | +30,000 | reduce cost/funded exposure |
| Gain/income | +20,000 | income or realized gain by policy |

### QA assertions

- Distribution classification matters.
- NAV/cost treatment depends on statement from fund administrator.
- Performance should classify correctly.

## 8. REIT distribution

### Business scenario

Hold 5,000 REIT units. Distribution USD 0.08 per unit.

```text
Distribution = 5,000 x 0.08 = 400
```

### QA assertions

- REIT distribution may include income and return-of-capital components depending source.
- Units unchanged.
- Tax/withholding treatment configurable.

## 9. Common edge cases

| Edge case | Expected handling |
|---|---|
| fund redemption gate | pending redemption and liquidity disclosure |
| stale private-market NAV | stale/estimated value flag |
| ETF split | corporate action handling similar equity |
| spin-off | create new security and allocate cost basis |
| partial capital call | reduce unfunded commitment only by called amount |
| recallable distribution | may increase unfunded recallable amount |
