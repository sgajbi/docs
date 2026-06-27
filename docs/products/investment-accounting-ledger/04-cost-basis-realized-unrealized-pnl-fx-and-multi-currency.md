# 04 - Cost Basis, Realized P&L, Unrealized P&L, FX and Multi-Currency Accounting

## 1. Cost basis purpose

Cost basis answers: **what did the client pay for the position, after relevant adjustments?**

It supports:

- realized P&L;
- unrealized P&L;
- tax-lot reporting;
- client statement cost display;
- performance explanation;
- migration reconciliation;
- corporate-action adjustments;
- advisor sell-lot decisions.

Cost basis is not always the same as accounting carrying value, tax basis, book cost or performance capital. The platform should label which basis is used.

## 2. Cost methods

| Method | Meaning | Typical use |
|---|---|---|
| FIFO | Sell earliest acquired lots first | Common default for tax/reporting |
| LIFO | Sell latest lots first | Jurisdiction/policy-specific |
| Average cost | Use pooled average cost | Funds/unit trusts in some markets |
| Specific identification | User selects lots to sell | Tax-aware advisory |
| Weighted average by position | Operational reporting where lots unavailable | Simplified view |
| Source-provided basis | Custodian/tax engine provides basis | Multi-custody reporting |

The chosen method must be explicit by account, product, jurisdiction and reporting purpose.

## 3. Realized P&L

Realized P&L occurs when a position is disposed, redeemed, matured, expired, written off or otherwise closed.

Basic formula:

```text
Realized P&L = Net disposal proceeds - Allocated cost basis
```

For a sale:

```text
Net proceeds = Gross sale proceeds - fees - transaction taxes
```

For a bond redemption:

```text
Realized P&L = Principal redemption + redemption premium/discount - adjusted amortized cost
```

For an option expiry:

```text
Long option realized P&L = 0 - remaining premium cost
Short option realized P&L = premium retained - close/settlement cost
```

## 4. Unrealized P&L

Unrealized P&L is the difference between current valuation and remaining cost basis.

```text
Unrealized P&L = Current market value - Remaining adjusted cost basis
```

For multi-currency portfolios, unrealized P&L should be split when possible:

| Component | Meaning |
|---|---|
| Local price P&L | Gain/loss from instrument price in local currency |
| FX translation P&L | Gain/loss from currency movement versus base/reporting currency |
| Accrued income effect | Accrued coupon/interest/dividend included or excluded depending on dirty/clean view |
| Valuation adjustment | Model, liquidity, stale price or manual override effect |

## 5. Local, base and reporting currency

A robust wealth platform should track at least these currency concepts:

| Currency | Meaning |
|---|---|
| Instrument currency | Currency in which security is quoted or denominated |
| Trade currency | Currency of execution |
| Settlement currency | Currency of cash settlement |
| Account currency | Currency of the custody/cash account |
| Portfolio base currency | Main valuation/performance currency |
| Client reporting currency | Currency used in statement/report |
| Tax currency | Currency required for tax reporting |

Never assume instrument currency equals settlement currency or portfolio base currency.

## 6. FX treatment

FX can affect accounting in several ways:

| Case | FX handling |
|---|---|
| Security bought in foreign currency | Convert cost into base currency at trade/settlement FX based on policy |
| Security valued at period end | Convert market value using valuation-date FX |
| Income received in foreign currency | Convert income at payment-date FX or configured accounting date |
| Realized sale P&L | Split local price gain and FX gain if required |
| FX spot/forward trade | Model currency legs, value dates and realized FX separately |
| Multi-currency cash | Treat each currency as separate cash position |

## 7. Example: foreign equity P&L split

Client buys USD stock for USD 10,000 when USD/SGD is 1.30. Base cost = SGD 13,000.

Later value is USD 11,000 and USD/SGD is 1.35. Base market value = SGD 14,850.

| Component | Calculation | Amount |
|---|---:|---:|
| Local price gain | USD 1,000 x 1.30 | SGD 1,300 |
| FX gain on original cost | USD 10,000 x (1.35 - 1.30) | SGD 500 |
| FX gain on local price gain | USD 1,000 x (1.35 - 1.30) | SGD 50 |
| Total base gain | 14,850 - 13,000 | SGD 1,850 |

Some reports simplify this into price P&L and FX P&L. The decomposition policy should be consistent.

## 8. Corporate action cost adjustments

| Corporate action | Cost-basis treatment |
|---|---|
| Stock split | Quantity changes, total cost unchanged, cost per share adjusted |
| Bonus issue | Quantity increases, total cost usually unchanged |
| Return of capital | Reduces cost basis; excess may become realized gain depending on tax rules |
| Spin-off | Allocate original cost between parent and child using fair-value ratio or source rule |
| Merger for cash | Dispose old security, recognize realized P&L |
| Merger for stock | Close old security, open new security, carry over or fair-value basis by rule |
| Rights issue | New cost added if rights exercised; rights may have separate value if sold/lapsed |
| Structured note physical delivery | Allocate note carrying value to delivered security by policy |

## 9. Amortized cost

For some fixed-income instruments, cost basis may be adjusted over time through premium/discount amortization.

| Item | Meaning |
|---|---|
| Premium | Bond bought above par; premium amortizes down toward par |
| Discount | Bond bought below par; discount accretes up toward par |
| Effective interest rate | Rate that amortizes cost to expected cashflows |
| Amortized cost | Cost adjusted for amortization and impairment/write-downs |

Client reporting may still show market value separately from amortized cost.

## 10. Migration warning

Cost basis is one of the hardest items to migrate. Required evidence:

- current position quantity;
- open lots and remaining quantities;
- original acquisition dates;
- adjusted cost amounts;
- FX rates used for base cost;
- corporate-action history;
- source tax-basis flags;
- realized P&L history if needed;
- reconciliation to custodian statements.

If lot history is missing, clearly flag cost as estimated or source-limited.
