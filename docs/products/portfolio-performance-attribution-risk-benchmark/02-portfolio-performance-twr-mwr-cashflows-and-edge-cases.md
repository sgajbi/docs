# 02 - Portfolio Performance: TWR, MWR, Cashflows and Edge Cases

## 1. Why performance measurement is difficult

Performance appears simple: compare beginning value and ending value. In real portfolios, it is difficult because clients add and withdraw money, securities pay income, positions are traded, prices arrive late, FX changes daily, corporate actions restate quantities, private-market NAVs lag, and fees/taxes may be included or excluded.

A proper performance engine must answer:

- What period is being measured?
- What is the beginning market value?
- What is the ending market value?
- Which cashflows are external versus internal?
- Which income is included?
- Which fees and taxes are included?
- Which FX rates are used?
- Are returns daily, monthly or event-based?
- Are values trade-date or settlement-date?
- Are there backdated changes?
- What is the benchmark for comparison?

## 2. Basic return formula

For a simple period with no external cashflows:

```text
Return = (Ending Market Value - Beginning Market Value) / Beginning Market Value
```

Example:

| Item | Amount |
|---|---:|
| Beginning value | 1,000,000 |
| Ending value | 1,050,000 |
| Return | 5.00% |

But if the client added 200,000 during the period, the same formula becomes misleading.

## 3. External versus internal cashflows

The most important performance classification is whether a cashflow is external to the portfolio.

| Cashflow | Usually external? | Explanation |
|---|---:|---|
| Client cash deposit | Yes | New capital enters portfolio |
| Client withdrawal | Yes | Capital leaves portfolio |
| Transfer-in of securities | Yes | New assets enter portfolio |
| Transfer-out of securities | Yes | Assets leave portfolio |
| Buy security using portfolio cash | No | Internal asset mix change |
| Sell security and keep cash in portfolio | No | Internal asset mix change |
| Dividend/coupon received into portfolio cash | No | Investment income, not external capital |
| Coupon withdrawn immediately to client | Income first, then external outflow | Need two-step treatment |
| Advisory fee paid from portfolio | Usually internal expense for net return | Policy-dependent |
| Tax withheld at source | Usually investment drag | Tax basis-dependent |
| Loan drawdown into portfolio cash | Usually liability transaction, not performance inflow | Depends on reporting basis |
| Capital call funded from portfolio cash | Internal investment activity | Unless funded by external contribution |

This classification must be configurable by reporting policy and jurisdiction.

## 4. Time-weighted return (TWR)

TWR measures the performance of the portfolio strategy independent of the timing and amount of external cashflows.

TWR is suitable for:

- Manager performance
- DPM mandate performance
- Strategy comparison
- Benchmark comparison
- GIPS-style performance presentation
- Portfolio return where client cashflow timing should not distort results

## 5. Daily TWR formula

For daily performance, where external cashflows are assumed to occur at beginning or end of day based on policy:

```text
Daily Return = (Ending Value - Beginning Value - External Cashflow) / Adjusted Beginning Value
```

A common end-of-day cashflow convention:

```text
Daily Return = (EMV - BMV - CF) / BMV
```

A common beginning-of-day cashflow convention:

```text
Daily Return = (EMV - BMV - CF) / (BMV + CF)
```

Where:

| Symbol | Meaning |
|---|---|
| BMV | Beginning market value |
| EMV | Ending market value |
| CF | Net external cashflow |

The convention must be documented and consistently applied.

## 6. Geometric linking

Daily returns are linked geometrically:

```text
Cumulative Return = (1 + r1) x (1 + r2) x ... x (1 + rn) - 1
```

Example:

| Day | Return |
|---|---:|
| Day 1 | 1.00% |
| Day 2 | -0.50% |
| Day 3 | 0.80% |

```text
Cumulative = 1.01 x 0.995 x 1.008 - 1 = 1.29996%
```

Do not add daily returns for official cumulative performance.

## 7. Annualization

For periods longer than one year:

```text
Annualized Return = (1 + Cumulative Return)^(365 / Days) - 1
```

Or using trading days:

```text
Annualized Return = (1 + Cumulative Return)^(252 / Trading Days) - 1
```

Use calendar-day or trading-day convention consistently.

## 8. Money-weighted return (MWR / IRR / XIRR)

MWR measures the return experienced by the investor, considering the timing and size of cashflows.

MWR is suitable for:

- Client experience
- Private markets
- Capital-call structures
- Irregular cashflows
- Investor-specific reporting
- Comparing actual cash invested versus value returned

MWR is the discount rate that makes the net present value of all cashflows and ending value equal zero.

```text
0 = sum CF_t / (1 + r)^(days_t / 365)
```

A typical XIRR sequence:

| Date | Cashflow |
|---|---:|
| 2024-01-01 | -1,000,000 |
| 2024-06-01 | -200,000 |
| 2024-12-31 | +1,350,000 |

The solution `r` is the money-weighted return.

## 9. TWR versus MWR

| Topic | TWR | MWR |
|---|---|---|
| Main purpose | Manager/strategy performance | Investor experience |
| Cashflow timing impact | Neutralized | Included |
| Good for benchmark comparison | Yes | Less ideal |
| Good for private markets | Sometimes, but MWR often better | Yes |
| Sensitive to client deposits/withdrawals | No | Yes |
| Calculation method | Sub-period/geometric linking | IRR/XIRR |
| Interpretation | How assets performed | How client's money performed |

Example:

A client adds large capital just before a strong rally. TWR and MWR may both be positive, but MWR may be much higher because most capital was invested during the strong period.

## 10. Modified Dietz

Modified Dietz approximates money-weighted return when exact daily valuation is not available.

```text
Modified Dietz Return = Gain / (BMV + sum Weighted Cashflows)
```

Where:

```text
Gain = EMV - BMV - sum Cashflows
```

Weighted cashflow:

```text
Weighted CF = CF x (Days remaining in period / Total days in period)
```

Modified Dietz is useful when portfolio valuations are available monthly but cashflows happen inside the month. For daily-valued portfolios, daily TWR is usually preferred.

## 11. Performance periods

Common periods:

| Period | Definition |
|---|---|
| 1D | One day return |
| MTD | Month-to-date |
| QTD | Quarter-to-date |
| YTD | Year-to-date |
| 1Y | Trailing one year |
| 3Y | Trailing three years, usually annualized |
| 5Y | Trailing five years, usually annualized |
| Since inception | From portfolio start or mandate start |
| Custom | User-selected start/end |

Key point: inception date may differ by portfolio, mandate, account, asset class or composite.

## 12. Beginning and ending value conventions

A portfolio return engine must define:

| Concept | Options |
|---|---|
| Opening value | Prior EOD market value / BOD valuation |
| Closing value | Current EOD market value |
| Price basis | Close price / bid / mid / custodian valuation / NAV |
| Accrual basis | Include accrued income or not |
| FX basis | Spot close / official rate / custodian rate |
| Trade inclusion | Trade date / settlement date |
| Cashflow timing | BOD / EOD / actual timestamp |

## 13. Income treatment

| Income type | Performance treatment |
|---|---|
| Dividend | Investment income; contributes to return |
| Coupon | Investment income; contributes to return |
| Accrued interest | Included if dirty/accrual-based performance |
| Fund distribution | Income or capital depending classification |
| REIT distribution | Income / return of capital split if available |
| Structured note coupon | Income if confirmed/payable; conditional coupons not recognized before entitlement |
| Money market interest | Income/accrual |
| Loan interest expense | Liability expense / financing drag |
| Insurance premium | Contribution/expense depending reporting purpose |

## 14. Fees and taxes

Return basis must specify fee and tax treatment.

| Item | Gross return | Net return | After-tax return |
|---|---:|---:|---:|
| Advisory fee | Excluded | Included | Included |
| Management fee inside fund NAV | Already included in NAV | Already included | Already included |
| Custody fee | Usually excluded | Policy-dependent | Policy-dependent |
| Transaction commission | Often included in cost/proceeds | Included | Included |
| Withholding tax | Policy-dependent | Policy-dependent | Included |
| Stamp duty | Usually included in transaction cost | Included | Included |
| Client income tax | Excluded | Excluded | Included if supported |

## 15. Cashflow classification matrix

| Transaction | External flow? | Income? | P&L? | Notes |
|---|---:|---:|---:|---|
| Cash deposit | Yes | No | No | External contribution |
| Cash withdrawal | Yes | No | No | External withdrawal |
| Buy equity | No | No | No | Asset exchange cash to security |
| Sell equity | No | No | Yes | Realized P&L |
| Dividend | No | Yes | No | Income return |
| Coupon | No | Yes | No | Income return |
| Bond maturity | No | No | Yes | Principal return plus realized P&L |
| Fund distribution | No | Yes/maybe capital | No/Maybe | Depends classification |
| Structured note autocall | No | Possibly | Yes | Coupon + principal/redemption |
| Option premium paid | No | No | P&L via derivative value | Derivative cost |
| Futures variation margin | No | No | Yes | Realized/settled P&L |
| Private capital call | No if funded from portfolio cash | No | No | Increases investment position |
| Private distribution | No | Income/ROC/gain | Maybe | Needs waterfall split |
| Loan drawdown | No for investment performance; liability event | No | No | Increases cash and liability |
| Fee debit | No | No | Expense | Net performance drag |

## 16. Edge cases

### Zero or negative beginning market value

Return calculation can fail when BMV is zero or negative.

Examples:

- New portfolio funded during day
- Portfolio only has short positions
- Derivative portfolio with negative value
- Liability-only account

Policy options:

| Case | Treatment |
|---|---|
| BMV = 0 and EMV created only by external flow | Return not meaningful / 0 by policy |
| BMV = 0 with investment gain | Use first invested date as start |
| Negative BMV | Use specialized return method / not report TWR |
| Liability-only | Report liability cost metrics, not standard asset return |

### Large external cashflows

Large cashflows can distort approximation methods. A daily valuation system should break performance sub-periods around large cashflows or calculate daily TWR.

### Backdated transactions

Backdated transactions require recalculation from earliest impacted date.

Impacted outputs:

- Daily returns
- Cumulative returns
- Contribution
- Attribution
- Asset allocation history
- Risk based on historical returns
- Benchmark-relative return
- Client reports already generated

### Late prices and FX rates

Late prices can change historical valuations and returns. The system must decide whether to restate client reports or keep prior reports frozen with a correction event.

### Corporate actions

Splits, spin-offs, mergers and rights issues must not create artificial performance. They should preserve economic value except where real consideration or market movement occurs.

### Private assets with stale NAV

Private equity and hedge fund NAVs may lag. Performance should disclose stale valuation and may require estimated/interim values.

### Multi-currency portfolios

Base-currency performance includes both local asset return and FX return. A platform should allow decomposition:

```text
Base-currency return approx. Local return + FX return + Interaction
```

### Transfers between accounts in same portfolio group

A transfer from one account to another may be external at account level but internal at relationship/household level.

## 17. Recalculation strategy

A production-grade performance engine should support:

| Feature | Purpose |
|---|---|
| Impact date detection | Recalculate only affected periods |
| Dependency graph | Know whether price, FX, transaction or classification changed |
| Idempotent jobs | Safe retries |
| Versioned results | Audit and report reproduction |
| Frozen reporting snapshots | Recreate what client saw |
| Restatement policy | Controlled correction of past returns |
| Explainable deltas | Show why return changed |

## 18. TWR implementation checklist

- [ ] Determine valuation calendar.
- [ ] Determine portfolio inception and report start date.
- [ ] Calculate BMV and EMV per day.
- [ ] Classify external cashflows.
- [ ] Apply cashflow timing convention.
- [ ] Include/exclude accrued income according to policy.
- [ ] Translate all values into base currency.
- [ ] Calculate daily return.
- [ ] Link daily returns geometrically.
- [ ] Annualize where required.
- [ ] Store inputs, outputs and calculation version.
- [ ] Explain residuals and missing-data days.

## 19. MWR implementation checklist

- [ ] Define cashflow signs consistently.
- [ ] Include initial investment as negative cashflow.
- [ ] Include capital contributions as negative cashflows.
- [ ] Include withdrawals/distributions as positive cashflows.
- [ ] Include ending market value as positive terminal cashflow.
- [ ] Use exact dates for XIRR.
- [ ] Handle multiple IRR/no IRR cases.
- [ ] Define fallback policy.
- [ ] Annualize consistently.
- [ ] Separate fund-level MWR from client-account MWR.

## 20. Expert summary

TWR answers how the portfolio strategy performed. MWR answers how the investor's money performed. The calculation is only as good as the classification of cashflows, the quality of valuations, the timing convention and the treatment of fees, taxes, FX and income. A professional platform must make these policies explicit and auditable.
