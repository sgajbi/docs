# 06 - Investment Accounting, P&L, Cost Basis, Accruals and Fees Worked Examples

## 1. Average cost basis equity sale

### Business scenario

Buy 100 shares at USD 10. Buy another 100 at USD 14. Sell 50 at USD 16.

### Average cost

```text
Total cost = 100x10 + 100x14 = 2,400
Total shares = 200
Average cost = 12
Sold cost = 50 x 12 = 600
Sale proceeds = 50 x 16 = 800
Realized P&L = 200
Remaining shares = 150
Remaining cost = 1,800
```

### QA assertions

- Realized P&L uses selected cost method.
- Remaining cost basis is reduced correctly.
- Realized and unrealized P&L do not overlap.

## 2. FIFO cost basis

### Business scenario

Same buys as above. Sell 150 at USD 16 using FIFO.

### FIFO cost

```text
First 100 cost = 100 x 10 = 1,000
Next 50 cost = 50 x 14 = 700
Total sold cost = 1,700
Sale proceeds = 150 x 16 = 2,400
Realized P&L = 700
```

### QA assertions

- Sold lots selected in FIFO order.
- Remaining lot is 50 shares at cost 14.
- Lot-level history preserved.

## 3. Management fee charged quarterly

### Business scenario

Portfolio average AUM USD 2,000,000. Annual management fee 1%. Quarterly charge.

```text
Quarterly fee = 2,000,000 x 1% / 4 = 5,000
```

### Transaction

| Transaction | Cash | Expense |
|---|---:|---:|
| MANAGEMENT_FEE | -5,000 | 5,000 |

### Performance treatment

Usually expense within portfolio. Reporting may show gross-of-fee and net-of-fee performance separately.

### QA assertions

- Fee period matches billing period.
- Fee is not treated as external withdrawal unless policy says so.
- Net performance includes fee impact.

## 4. Withholding tax on dividend

### Business scenario

Gross dividend USD 1,000. Withholding tax 30%. Net cash received USD 700.

### Transactions

| Transaction | Amount |
|---|---:|
| DIVIDEND_GROSS_INCOME | +1,000 |
| WITHHOLDING_TAX | -300 |
| NET_CASH_RECEIVED | +700 |

### Reporting

Show gross income, tax withheld and net received if report supports tax detail.

### QA assertions

- Net cash equals gross minus tax.
- Gross and net views are consistent.
- Tax is not treated as investment loss.

## 5. Accrued bond interest

### Business scenario

Bond coupon USD 6,000 annually. 60 days accrued, ACT/360.

```text
Accrued interest = 6,000 x 60 / 360 = 1,000
```

### Valuation

```text
Dirty value = clean market value + accrued interest
```

### QA assertions

- Accrued interest uses correct day count.
- Dirty/clean views are labelled.
- Accrued interest is not double counted with coupon payment.

## 6. FX unrealized P&L

### Business scenario

Portfolio base currency USD. Holds EUR 100,000 cash.

- Initial EUR/USD = 1.08
- Current EUR/USD = 1.10

```text
Initial value = 108,000 USD
Current value = 110,000 USD
FX unrealized gain = 2,000 USD
```

### QA assertions

- FX gain due to currency movement is separated if reporting supports it.
- FX direction is correct.
- Local currency value remains EUR 100,000.

## 7. Fee rebate

### Business scenario

Client receives fee rebate USD 500.

### Treatment

| Transaction | Cash | Income/expense |
|---|---:|---:|
| FEE_REBATE | +500 | negative expense or other income by policy |

### QA assertions

- Rebate links to original fee if possible.
- Reporting label is clear.
- Performance treatment follows policy.

## 8. Key takeaway

Accounting examples must preserve the distinction between cash, income, expense, cost basis, realized P&L, unrealized P&L and external cashflows.
