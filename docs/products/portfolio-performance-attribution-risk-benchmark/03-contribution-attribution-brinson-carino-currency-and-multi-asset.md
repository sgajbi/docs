# 03 - Contribution, Attribution, Brinson, Carino, Currency and Multi-Asset Analytics

## 1. Contribution versus attribution

Contribution and attribution are often confused.

| Concept | Question answered | Example |
|---|---|---|
| Contribution | What added to or detracted from portfolio return? | Apple contributed +0.80% to portfolio return |
| Attribution | Why did portfolio outperform or underperform benchmark? | Stock selection in US equities added +0.40% active return |

Contribution is usually absolute. Attribution is usually benchmark-relative.

## 2. Return contribution

For a single period, contribution is usually:

```text
Contribution_i = Beginning Weight_i x Return_i
```

Example:

| Asset class | Beginning weight | Return | Contribution |
|---|---:|---:|---:|
| Equity | 60% | 5% | 3.00% |
| Bonds | 30% | 1% | 0.30% |
| Cash | 10% | 0.2% | 0.02% |
| Total | 100% | | 3.32% |

If there are cashflows and trades, the system may use average weights, daily-linked contribution or transaction-based contribution.

## 3. Contribution dimensions

Contribution can be shown by:

- Security
- Asset class
- Sector
- Country
- Currency
- Region
- Issuer
- Rating
- Duration bucket
- Strategy/sleeve
- Advisor model
- Mandate bucket
- Product type
- Income versus price
- Local return versus FX return

A good client report usually starts high-level and allows drilldown.

## 4. Single-period Brinson attribution

Brinson attribution explains active return versus benchmark using weights and returns.

Definitions:

| Symbol | Meaning |
|---|---|
| Wp_i | Portfolio weight in segment i |
| Wb_i | Benchmark weight in segment i |
| Rp_i | Portfolio return in segment i |
| Rb_i | Benchmark return in segment i |
| Rb_total | Total benchmark return |

Common Brinson-Fachler formulation:

```text
Allocation Effect_i = (Wp_i - Wb_i) x (Rb_i - Rb_total)
Selection Effect_i = Wb_i x (Rp_i - Rb_i)
Interaction Effect_i = (Wp_i - Wb_i) x (Rp_i - Rb_i)
Active Effect_i = Allocation + Selection + Interaction
```

## 5. Interpretation

| Effect | Meaning |
|---|---|
| Allocation effect | Did overweight/underweight decisions help? |
| Selection effect | Did holdings within the segment beat segment benchmark? |
| Interaction effect | Combined effect of allocation and selection |
| Active return | Portfolio return minus benchmark return |

Example interpretation:

- Positive allocation: overweighted a segment that beat benchmark overall or underweighted a weak segment.
- Positive selection: selected better securities/funds within a segment.
- Negative interaction: overweighted a segment but selected poor holdings within it.

## 6. Contribution versus Brinson example

Portfolio:

| Segment | Portfolio weight | Portfolio return |
|---|---:|---:|
| Equity | 70% | 8% |
| Bonds | 25% | 2% |
| Cash | 5% | 0.5% |

Benchmark:

| Segment | Benchmark weight | Benchmark return |
|---|---:|---:|
| Equity | 60% | 7% |
| Bonds | 35% | 3% |
| Cash | 5% | 0.5% |

Portfolio return:

```text
70%x8% + 25%x2% + 5%x0.5% = 6.125%
```

Benchmark return:

```text
60%x7% + 35%x3% + 5%x0.5% = 5.275%
```

Active return:

```text
0.850%
```

Attribution explains this 0.850% difference.

## 7. Multi-period attribution problem

Single-period attribution does not automatically add up across time because returns compound geometrically.

If monthly active effects are added arithmetically, they may not exactly reconcile to linked active return.

This creates residuals.

## 8. Carino smoothing / linking

Carino smoothing is one method for linking arithmetic attribution effects across multiple periods so that they reconcile to geometrically linked active return.

The practical purpose:

```text
Sum of adjusted multi-period attribution effects = Total linked active return
```

Without smoothing, attribution reports often show unexplained residuals.

## 9. Residual / non-allocable effect

Residual is the part of performance that cannot be cleanly allocated to chosen dimensions under the calculation method.

Common causes:

| Cause | Example |
|---|---|
| Geometric linking | Multi-period returns do not add arithmetically |
| Cashflow timing | Trades and flows inside period |
| Rounding | Security-level contributions rounded |
| FX interaction | Local and currency effects interact |
| Classification gaps | Holding not mapped to sector/asset class |
| Benchmark mismatch | Portfolio holding not in benchmark |
| Pricing differences | Portfolio price source differs from benchmark |
| Derivative exposure | Notional exposure differs from market value |
| Private asset stale NAV | Return not synchronized with benchmark |
| Fees and taxes | Benchmark usually gross while portfolio may be net |

Client-friendly explanation:

> The residual represents small timing, compounding, currency, classification or rounding effects that cannot be attributed cleanly to a single category. It is separately shown to keep the report transparent and to ensure total contribution reconciles to total portfolio return.

## 10. Currency attribution

Currency matters when portfolio base currency differs from investment currency.

Simplified decomposition:

```text
Base Return = (1 + Local Return) x (1 + FX Return) - 1
```

Expanded:

```text
Base Return = Local Return + FX Return + LocalxFX Interaction
```

Example:

| Item | Value |
|---|---:|
| Local asset return | 5.00% |
| FX return versus base | 3.00% |
| Interaction | 0.15% |
| Base-currency return | 8.15% |

Currency attribution can show:

- Local market effect
- Currency effect
- Hedging effect
- Forward carry effect
- FX transaction effect
- Currency overlay effect

## 11. Fixed income attribution

Fixed income attribution is different from equity attribution because bond returns are driven by yield curve, spread, carry, roll-down, credit and optionality.

Common components:

| Component | Meaning |
|---|---|
| Carry | Income earned from holding bond |
| Roll-down | Price gain/loss from moving down yield curve |
| Yield curve effect | Changes in rates by maturity |
| Spread effect | Credit spread tightening/widening |
| Selection effect | Bond-specific performance |
| Currency effect | FX movement |
| Duration positioning | Benefit/loss from duration overweight/underweight |
| Credit quality allocation | Rating/sector exposure effect |
| Optionality effect | Callable/putable/structured bond behavior |

## 12. Equity attribution

Equity attribution can be done by:

- Sector
- Country
- Region
- Factor
- Style
- Market cap
- Security selection
- Currency

Common equity factors:

| Factor | Meaning |
|---|---|
| Value | Cheap valuation characteristics |
| Growth | High growth expectations |
| Momentum | Recent price trend |
| Quality | Profitability/balance-sheet strength |
| Size | Small versus large cap |
| Low volatility | Lower volatility stocks |
| Dividend yield | Income orientation |

## 13. Fund and ETF attribution

For funds, attribution can be:

| Method | Description |
|---|---|
| Holdings-based look-through | Use underlying holdings if available |
| Return-based style analysis | Infer exposures from returns |
| Classification-based | Use fund category/asset allocation |
| Fund-manager attribution | Use manager-provided attribution |
| Proxy benchmark attribution | Compare fund to assigned benchmark |

Look-through is best but may be unavailable, stale or licensed.

## 14. Structured product attribution

Structured products are hard to attribute because return depends on path, barriers, options and issuer pricing.

Possible components:

| Component | Meaning |
|---|---|
| Underlying effect | Movement of linked equity/index/FX/credit |
| Volatility effect | Change in implied volatility |
| Time decay | Option theta / time passage |
| Coupon accrual/payment | Income earned |
| Barrier proximity | Change in value due to barrier risk |
| Issuer credit spread | Issuer risk movement |
| Liquidity spread | Exit bid/offer adjustment |
| Residual/model effect | Unexplained pricing movement |

For client reporting, structured product attribution should be simple and avoid false precision.

## 15. Derivative attribution

Derivatives may be attributed using notional or risk exposure, not only market value.

| Derivative | Attribution driver |
|---|---|
| Equity option | Delta, gamma, vega, theta, underlying movement |
| FX forward | Spot FX, forward points, carry |
| Futures | Underlying price movement, roll, collateral return |
| Interest-rate swap | Curve movement, carry, spread |
| CDS | Credit spread, carry, default event |
| Total return swap | Reference asset return, financing spread |

## 16. Private markets attribution

Private markets often use IRR and multiples rather than daily TWR attribution.

Components:

| Component | Meaning |
|---|---|
| Contributions/capital calls | Capital invested |
| Distributions | Cash returned |
| NAV change | Unrealized value movement |
| FX | Currency translation |
| Fees/carry | Manager economics |
| Vintage effect | Market timing by vintage year |
| Sector/geography effect | Portfolio company exposure |
| Valuation lag | NAV update timing |

## 17. Contribution by asset class with residual

When explaining contribution by asset class, use this structure:

| Component | Explanation |
|---|---|
| Equity contribution | Return generated by equity holdings, including dividends and price movement |
| Fixed income contribution | Coupons, accruals and bond price movement |
| Fund contribution | NAV movement and distributions |
| Alternatives contribution | NAV changes, distributions and valuation updates |
| Cash contribution | Interest and currency effects |
| FX contribution | Currency translation impact |
| Fees/taxes | Explicit drag if included |
| Residual | Timing, rounding, classification and compounding effects |

## 18. Data requirements for attribution

| Data | Needed for |
|---|---|
| Portfolio holdings by date | Weights and return calculation |
| Security returns | Security contribution |
| Classification mappings | Asset class/sector/country attribution |
| Benchmark weights/returns | Active attribution |
| FX rates | Currency attribution |
| Transactions | Trade timing, cashflows, realized effects |
| Income events | Coupon/dividend/distribution contribution |
| Derivative Greeks/exposure | Derivative attribution |
| Bond analytics | Duration/spread attribution |
| Fund look-through | Fund attribution |
| Mandate target weights | Allocation drift and mandate attribution |

## 19. Attribution reporting principles

- Start with high-level active return.
- Explain allocation and selection in business language.
- Show only meaningful effects to clients.
- Keep detailed effects for advisor/analyst drilldown.
- Make benchmark and classification clear.
- Avoid over-attributing residuals.
- Do not pretend illiquid or model-priced assets have daily precision.
- State whether fees and taxes are included.
- Reconcile attribution to reported return.

## 20. Expert summary

Contribution tells what drove total return. Attribution tells why portfolio return differed from benchmark or mandate. A professional system must support multiple attribution methods because equities, fixed income, funds, derivatives, structured products and private markets do not behave the same way. The key is not to force one model everywhere, but to use a common reporting framework with product-appropriate attribution engines.
