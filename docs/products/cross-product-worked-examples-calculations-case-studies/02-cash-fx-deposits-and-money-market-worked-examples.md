# 02 - Cash, FX, Deposits and Money Market Worked Examples

## 1. Cash deposit into portfolio

### Business scenario

Client deposits USD 100,000 into portfolio P-100.

### Expected transaction

| Field | Value |
|---|---:|
| transaction_type | CASH_DEPOSIT |
| cash_currency | USD |
| cash_delta | +100,000 |
| performance_classification | External inflow |

### Position / balance impact

| Balance | Before | Delta | After |
|---|---:|---:|---:|
| USD cash | 0 | +100,000 | 100,000 |

### Performance treatment

External inflow. It should not itself create investment return.

### QA assertions

- Portfolio cash increases by USD 100,000.
- External cashflow table records inflow.
- TWR does not treat deposit as performance.
- Client statement shows deposit in activity section.

## 2. Internal FX conversion

### Business scenario

Portfolio converts USD 10,000 to SGD at USD/SGD 1.35.

### Calculation

```text
SGD received = 10,000 x 1.35 = 13,500
```

### Expected transactions

| Transaction | Currency | Cash delta |
|---|---|---:|
| FX_SELL | USD | -10,000 |
| FX_BUY | SGD | +13,500 |

### Performance treatment

Internal portfolio activity, not external cashflow.

### Reporting

Show in transaction activity as FX conversion. Cash allocation changes from USD to SGD.

### QA assertions

- USD cash decreases.
- SGD cash increases.
- Base currency value remains approximately unchanged, excluding spread/fees.
- FX rate direction is not inverted.

## 3. Fixed deposit placement and maturity

### Business scenario

Client places USD 100,000 fixed deposit for 90 days at 4% p.a., ACT/360.

### Interest calculation

```text
Interest = 100,000 x 4% x 90 / 360 = 1,000
```

### Lifecycle

| Event | Transaction |
|---|---|
| Placement | DEPOSIT_PLACEMENT |
| Interest accrual | DEPOSIT_INTEREST_ACCRUAL |
| Maturity | DEPOSIT_MATURITY |
| Interest payment | DEPOSIT_INTEREST_PAYMENT |

### Position impact

| Item | Placement | Maturity |
|---|---:|---:|
| Cash | -100,000 | +101,000 |
| Fixed deposit position | +100,000 | -100,000 |
| Interest income | 0 | +1,000 |

### Performance treatment

- Placement is internal investment activity if funded by portfolio cash.
- Interest is investment income.

### QA assertions

- Maturity closes deposit position.
- Interest is not double-counted as both income and market gain.
- Day-count convention is applied correctly.

## 4. Money market fund subscription and redemption

### Business scenario

Portfolio subscribes USD 50,000 into a money market fund at NAV 1.0000, later redeems at NAV 1.0020.

### Subscription

```text
Units = 50,000 / 1.0000 = 50,000 units
```

### Redemption

```text
Proceeds = 50,000 x 1.0020 = 50,100
Gain = 100
```

### Transactions

| Transaction | Units | Cash |
|---|---:|---:|
| FUND_SUBSCRIPTION | +50,000 | -50,000 |
| FUND_REDEMPTION | -50,000 | +50,100 |

### Reporting

- Show as money market fund holding, not bank deposit.
- Risk label should not imply deposit insurance unless applicable and true.
- Gain may appear as price return or income depending on fund/reporting policy.

### QA assertions

- NAV date is correct.
- Redemption proceeds use confirmed NAV.
- Settlement date cash availability is respected.

## 5. FX forward settlement

### Business scenario

Portfolio enters FX forward to buy EUR 100,000 and sell USD 108,000 at maturity.

### At trade date

No immediate cash exchange for standard forward. Create derivative/contract record.

### At maturity

| Currency | Cash delta |
|---|---:|
| EUR | +100,000 |
| USD | -108,000 |

### Performance treatment

Internal FX derivative settlement. Gain/loss comes from comparison to market FX rate.

### QA assertions

- No spot cash movement at trade date unless premium/collateral exists.
- Maturity cash movement uses contracted rate.
- Unrealized MTM before maturity is captured if reporting requires.

## 6. Common edge cases

| Edge case | Expected handling |
|---|---|
| Negative cash | allow if overdraft/credit line, otherwise flag |
| Same-day FX reversal | maintain both transactions with reversal link |
| Settlement holiday | adjust value date |
| Missing FX rate | use fallback hierarchy or flag report |
| Deposit broken early | calculate penalty and revised interest |
| Money market fund gated | redemption pending and liquidity disclosure |
