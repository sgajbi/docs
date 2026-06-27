# 04 - FX, Interest Rate Curves, Yield Curves and Benchmarks

## 1. Why FX and curves are platform-critical

FX and curve data affect nearly every product family:

- portfolio base-currency valuation,
- cash and deposits,
- FX spot/forwards/swaps/NDFs,
- bond yield and accrued interest,
- loan interest and benchmark resets,
- derivatives valuation,
- structured note pricing,
- fund hedged share classes,
- performance in base currency,
- client reporting across booking centres.

## 2. FX rate types

| Rate type | Use |
|---|---|
| Spot rate | trade pricing, valuation, FX exposure |
| Closing rate | end-of-day reporting and performance |
| Official fixing | benchmark calculation, derivative settlement, reports |
| Trade rate | actual execution rate |
| Average rate | period reporting or accounting policy |
| Forward rate | FX forwards, swaps, DCI and hedged funds |
| Cross rate | derived rate between two currencies |
| Collateral FX rate | conservative valuation for buying power |

## 3. FX quote conventions

FX must store quote direction explicitly.

| Field | Example |
|---|---|
| base_currency | USD |
| terms_currency | SGD |
| pair | USD/SGD |
| quote | 1.3500 |
| interpretation | 1 USD = 1.35 SGD |

Never rely on string naming alone. Maintain explicit base/terms fields, inversion logic and precision rules.

## 4. FX conversion formula

```text
Value in target currency = Value in source currency x FX(source -> target)
```

For cross rates:

```text
FX(A -> C) = FX(A -> B) x FX(B -> C)
```

Store whether the rate was direct, inverted or triangulated.

## 5. Interest-rate data

| Data type | Example | Consumer |
|---|---|---|
| Overnight rate | SOFR, SORA, EUR short-term rate | loans, floating notes, swaps |
| Term rate | 1M/3M/6M term benchmark | coupons and reset schedules |
| Deposit curve | money market deposits | short-end pricing |
| Government yield curve | Treasury/SGS curve | discounting, relative value |
| Swap curve | USD SOFR swap curve | derivatives valuation |
| Credit curve | issuer or rating spread curve | bond/structured note valuation |
| Inflation curve | inflation swaps/breakevens | inflation-linked products |

## 6. Curve construction inputs

A curve is not just a table of yields. It has construction rules.

| Attribute | Example |
|---|---|
| curve_id | USD_SOFR_OIS |
| currency | USD |
| curve_type | discount / forward / credit / government |
| collateral_basis | OIS / unsecured |
| day_count | ACT/360 |
| business_day_convention | modified following |
| interpolation_method | linear zero / cubic / log discount |
| bootstrapping_inputs | deposits, futures, swaps |
| source | vendor / internal market data |
| valuation_date | 2026-06-26 |
| quality_status | valid / stale / incomplete |

## 7. Benchmark and index data

Benchmarks are used by performance, attribution, advisory and mandate analytics.

| Benchmark data | Example |
|---|---|
| index level | S&P 500 close level |
| total return level | S&P 500 total return index |
| price return level | price-only index |
| constituent weights | MSCI ACWI components |
| rebalance dates | monthly/quarterly index rebalance |
| benchmark currency | USD / SGD / EUR |
| holiday calendar | index calendar |
| methodology version | provider methodology |

## 8. Price-return vs total-return benchmarks

| Type | Includes dividends/coupons? | Use |
|---|---|---|
| Price return | generally excludes income | price-only comparison |
| Total return | includes reinvested income | preferred for performance comparison |
| Net total return | income net of withholding assumptions | common for cross-border indices |
| Gross total return | income gross of withholding | benchmark policy dependent |

Using a price-return benchmark for an income portfolio can make the portfolio look artificially strong. Benchmark selection must be documented.

## 9. Curve and benchmark governance

| Control | Purpose |
|---|---|
| missing fixing check | detect missing daily rates |
| holiday calendar validation | avoid false missing data |
| benchmark level continuity check | detect bad jumps |
| benchmark return recomputation | validate provider return |
| component weight sum check | ensure weights sum to 100% |
| curve monotonicity/tolerance check | detect bad curve points |
| stale curve check | prevent old data in valuation |
| benchmark methodology change log | preserve comparability |

## 10. Platform implementation guidance

Store FX, rates, curves and benchmarks as first-class domain data, not utility tables hidden inside valuation code. They should have source lineage, business date, timestamp, quality status and consumer-specific usage policy.
