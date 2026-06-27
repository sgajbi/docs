# 01 — Derivatives Fundamentals and Product Taxonomy

## 1. What is a derivative?

A **derivative** is a financial contract whose value is derived from something else. That “something else” is called the **underlying** or **reference**.

The underlying can be:

| Underlying type | Examples |
|---|---|
| Equity | Apple shares, DBS shares, S&P 500 index |
| Interest rate | SOFR, SORA, Euribor, swap rate, bond yield |
| FX | USD/SGD, EUR/USD, USD/JPY |
| Commodity | Gold, oil, copper, wheat |
| Credit | Corporate default, sovereign default, credit index |
| Inflation | CPI index, inflation swap index |
| Volatility | VIX, implied volatility index |
| Fund / basket / custom index | Fund NAV, model basket, private-bank index |

The derivative itself may have little upfront cost, but can create large economic exposure.

Example:

| Product | Upfront cash | Economic exposure |
|---|---:|---:|
| Buy 100 Apple shares at USD 200 | USD 20,000 | USD 20,000 equity exposure |
| Buy 1 call option controlling 100 Apple shares | Option premium only | Exposure to Apple upside |
| Equity futures contract | Initial margin only | Large index/equity exposure |
| Interest-rate swap | Often zero upfront | Exposure to rate movements |

## 2. Why derivatives exist

Derivatives are used for four main purposes.

| Purpose | Example | Comment |
|---|---|---|
| Hedging | Buy put option to protect equity portfolio | Reduces risk but has cost |
| Exposure / efficient implementation | Use futures to gain index exposure quickly | Useful for DPM cash equitization |
| Income generation | Sell covered calls | Generates premium but caps upside |
| Speculation / leverage | Buy options/futures for directional view | Can magnify losses |

In wealth management, the same derivative can be suitable or unsuitable depending on purpose, size, client knowledge, risk profile and mandate.

## 3. Exchange-traded vs OTC derivatives

| Dimension | Exchange-traded derivative | OTC derivative |
|---|---|---|
| Contract terms | Standardized | Customized |
| Trading venue | Exchange | Bilateral / dealer platform |
| Clearing | Usually central clearing | Bilateral or cleared depending product/regime |
| Counterparty risk | Mainly clearing house / broker chain | Dealer/counterparty risk |
| Liquidity | Often better for standard contracts | Depends on dealer and product |
| Valuation | Exchange prices available | Model / dealer valuation |
| Documentation | Exchange rules | ISDA / CSA / confirmation |
| Examples | Listed options, futures | FX forwards, swaps, bespoke options, CDS |

Important platform implication:

```text
Exchange-traded derivative processing is often trade + margin + exchange price driven.
OTC derivative processing is contract + lifecycle + valuation + collateral + counterparty driven.
```

## 4. Core product taxonomy

| Family | Products | Main risk driver |
|---|---|---|
| Options | Calls, puts, barriers, digitals, swaptions, FX options | Underlying price, volatility, time |
| Futures | Equity index futures, bond futures, commodity futures | Underlying price, daily margin |
| Forwards | FX forwards, NDFs, equity forwards | Forward price, counterparty risk |
| Swaps | IRS, CCS, TRS, equity swap, inflation swap | Cashflow exchange formula |
| Credit derivatives | CDS, credit index swaps, CLN economics | Credit event/default risk |
| Structured derivative overlays | Collars, covered calls, protective puts, accumulators | Combination of option legs |

## 5. Derivatives vs securities

| Feature | Security | Derivative |
|---|---|---|
| Ownership | Investor owns security or fund units | Investor owns contractual rights/obligations |
| Upfront cash | Usually full purchase amount | Often premium/margin only |
| Valuation | Price × quantity | Model/market value of contract payoff |
| Exposure | Often close to market value | Can be much larger than market value |
| Lifecycle | Trade, income, corporate actions, maturity | Fixings, expiry, exercise, margin, resets, collateral |
| Risk | Market, issuer, liquidity | Market, leverage, counterparty, liquidity, model, collateral |

A platform must therefore support **notional exposure** and **risk-equivalent exposure**, not only book value.

## 6. How derivatives combine with other products

| Product | Derivative component |
|---|---|
| Equity-linked note | Short put option / barrier option |
| Principal-protected note | Zero-coupon bond + long call option |
| Autocallable note | Bond + autocall option + barrier option + coupon option |
| Dual currency note | Deposit-like note + short FX option |
| Credit-linked note | Note + credit derivative / CDS-like exposure |
| Convertible bond | Bond + embedded equity call option |
| Callable bond | Bond + issuer call option |
| Puttable bond | Bond + investor put option |
| Fund hedged share class | FX forwards/FX swaps |
| DPM overlay | Futures, options, FX forwards, swaps |
| Structured fund | Derivative strategy embedded in fund NAV |

## 7. Derivative lifecycle themes

Derivatives often have lifecycle events not present in simple securities.

| Event type | Examples |
|---|---|
| Trade event | Open, close, novation, compression |
| Premium event | Option premium paid/received |
| Margin event | Initial margin, variation margin, margin call |
| Fixing event | FX fixing, rate fixing, index fixing |
| Reset event | Swap coupon reset, floating-rate reset |
| Exercise event | Option exercise, early exercise |
| Assignment event | Option writer assigned |
| Expiry event | Option expires worthless or in the money |
| Settlement event | Cash settlement, physical delivery |
| Credit event | Default, failure to pay, restructuring |
| Collateral event | Collateral posted/returned/substituted |

## 8. Accounting position vs analytical exposure

For derivatives, the accounting position and analytical exposure can be very different.

Example: S&P 500 futures

| Measure | Meaning |
|---|---|
| Market value | May be close to zero after daily variation margin |
| Contract notional | Index level × multiplier × contracts |
| Delta-equivalent exposure | Usually close to notional for futures |
| Margin posted | Cash/collateral required to hold position |
| P&L | Daily mark-to-market through variation margin |

Therefore, a portfolio report should not say “derivatives exposure is zero” just because market value is close to zero.

## 9. Risks specific to derivatives

| Risk | Explanation |
|---|---|
| Market risk | Underlying price/rate/FX/credit movement |
| Leverage risk | Small cash outlay can create large exposure |
| Volatility risk | Option value changes with implied volatility |
| Time decay | Option value may erode as expiry approaches |
| Counterparty risk | OTC counterparty may fail |
| Liquidity risk | Difficult or expensive to close |
| Model risk | Valuation depends on assumptions |
| Basis risk | Hedge may not perfectly match portfolio exposure |
| Margin risk | Margin calls may require liquidity quickly |
| Settlement risk | Physical delivery, FX settlement, cash timing risk |
| Operational risk | Lifecycle events, fixings, resets and collateral can fail |

## 10. Wealth-management framing

In private banking and wealth platforms, derivatives should be assessed through five lenses:

1. **Product understanding** — does the client understand rights, obligations and downside?
2. **Purpose** — hedge, income, overlay, exposure or speculation?
3. **Magnitude** — notional, delta-equivalent, DV01, CS01, margin usage.
4. **Mandate fit** — allowed product, allowed exposure, allowed leverage, allowed counterparty.
5. **Reporting transparency** — can client, advisor and platform explain exposure, P&L and worst-case scenario?

A derivative is not automatically bad. The problem is using it without clear purpose, correct sizing, risk visibility, liquidity planning and lifecycle control.
