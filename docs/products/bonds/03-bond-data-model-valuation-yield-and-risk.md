# 03 - Bond Data Model, Valuation, Yield, and Risk

## 1. Recommended Data Model Overview

A robust bond model should separate:

```text
Instrument identity
Bond contract terms
Issuer / obligor / guarantor
Coupon schedule
Cashflow schedule
Call / put / amortisation schedules
Rating and credit data
Lifecycle events
Transactions
Positions
Valuations
Risk analytics
Performance classifications
```

This allows one common model to support simple government bonds and more complex instruments such as floating-rate notes, callable bonds, convertibles, perpetuals, amortising bonds, and subordinated bank capital.

---

## 2. Core Instrument Table

| Field | Example | Nullable? |
|---|---|---|
| instrument_id | BOND123 | No |
| security_type | BOND | No |
| bond_subtype | FIXED_RATE / FRN / ZERO / CALLABLE / CONVERTIBLE / PERPETUAL / ABS | No |
| isin / cusip / sedol | XS1234567890 | Usually no, but may be null for private securities |
| issuer_id | Company ABC | No |
| obligor_id | Operating company / sovereign | Sometimes |
| guarantor_id | Parent company / government guarantee | Yes |
| country_of_risk | US / SG / CN | No |
| issue_currency | USD | No |
| denomination | 1,000 / 10,000 / 100,000 | No |
| minimum_denomination | 200,000 | Yes |
| issue_date | 2026-01-10 | No |
| maturity_date | 2031-01-10 | Nullable for perpetuals |
| original_maturity_years | 5 | Derived |
| issue_price_pct | 99.50 | Yes |
| redemption_price_pct | 100.00 | No |
| day_count_convention | ACT/ACT / 30/360 / ACT/365 | No |
| business_day_convention | Following / Modified Following | No |
| coupon_frequency | Annual / Semi-annual / Quarterly | No for coupon bonds |
| coupon_type | FIXED / FLOATING / ZERO / STEP_UP / INFLATION_LINKED | No |
| listing_status | LISTED / OTC / UNLISTED | No |
| exchange_mic | XSES / XLON | Nullable |
| price_quote_type | PCT_OF_PAR | No |
| settlement_cycle | T+1 / T+2 | Market dependent |
| seniority | SENIOR_SECURED / SENIOR_UNSECURED / SUBORDINATED / AT1 | No |
| secured_flag | true/false | No |
| collateral_type | Covered pool / mortgage pool / none | Yes |
| governing_law | English law / NY law / local | Yes |
| tax_status | Taxable / tax-exempt / withholding applies | Yes |
| complexity_class | Simple / Complex / SIP / EIP | Policy-driven |

---

## 3. Bond Contract Extension Tables

Use extensions rather than one huge nullable table.

```text
Instrument
  +-- BondContract
        +-- CouponSchedule[]
        +-- CashflowSchedule[]
        +-- FloatingRateTerms?
        +-- InflationLinkedTerms?
        +-- CallSchedule?
        +-- PutSchedule?
        +-- AmortisationSchedule?
        +-- SinkingFundSchedule?
        +-- ConvertibleTerms?
        +-- PerpetualHybridTerms?
        +-- ABS_MBS_Terms?
        +-- RatingHistory[]
        +-- LifecycleEvents[]
        +-- Valuations[]
```

This keeps the common model clean and makes subtype-specific attributes explicit.

---

## 4. Coupon Schedule Model

| Field | Meaning |
|---|---|
| schedule_id | Unique schedule ID. |
| instrument_id | Bond. |
| coupon_period_start | Start date of coupon accrual period. |
| coupon_period_end | End date of coupon accrual period. |
| coupon_payment_date | Date coupon is paid. |
| coupon_rate | Fixed rate or determined rate. |
| coupon_amount_per_100 | Coupon amount per 100 par. |
| day_count_fraction | Year fraction for period. |
| ex_coupon_date | Date after which buyer no longer receives next coupon. |
| record_date | Holder record date if applicable. |
| status | PROJECTED / FIXED / PAID / CANCELLED / DEFAULTED. |

For floating-rate bonds, the coupon is not fully known until the rate is fixed.

---

## 5. Floating-Rate Terms

| Field | Example |
|---|---|
| reference_rate | SOFR / SORA / Euribor / SONIA |
| spread_bps | 125 |
| reset_frequency | Quarterly |
| fixing_lag_days | 2 |
| lookback_days | 5, if applicable |
| observation_shift | true/false |
| cap_rate | Optional |
| floor_rate | Optional |
| compounding_method | Simple / compounded in arrears |
| fallback_rate_rule | Replacement benchmark fallback |

Coupon formula example:

```text
Coupon rate = reference rate + spread
Coupon amount = nominal x coupon rate x day-count fraction
```

---

## 6. Call, Put, and Redemption Schedule

### Call schedule

| Field | Meaning |
|---|---|
| call_date | Date issuer may call. |
| call_price_pct | Redemption price as % of par. |
| call_type | Optional / make-whole / regulatory / tax / clean-up. |
| notice_period_days | Required notice period. |
| call_status | Available / announced / exercised / expired. |

### Put schedule

| Field | Meaning |
|---|---|
| put_date | Date investor may put bond back. |
| put_price_pct | Price investor receives. |
| election_deadline | Deadline to instruct. |
| put_status | Available / elected / expired / settled. |

### Amortisation schedule

| Field | Meaning |
|---|---|
| principal_payment_date | Date principal is partially repaid. |
| repayment_pct | % of original/current face repaid. |
| remaining_factor | Factor after repayment. |
| status | Projected / confirmed / paid. |

---

## 7. Rating and Credit Data

Ratings are not static. Store history.

| Field | Meaning |
|---|---|
| instrument_id | Bond. |
| issuer_id | Issuer. |
| agency | S&P / Moody's / Fitch / internal. |
| rating | BBB+ / Baa1 / etc. |
| outlook | Positive / stable / negative. |
| watch_status | Watch positive / watch negative. |
| effective_date | Rating effective date. |
| end_date | When superseded. |
| rating_scope | Issuer / instrument / senior unsecured / subordinated. |

Important:

```text
Issuer rating, instrument rating, seniority, collateral, and guarantee can differ.
A senior secured bond and subordinated bond from the same issuer may have different ratings and recovery expectations.
```

---

## 8. Valuation Record Model

| Field | Meaning |
|---|---|
| valuation_id | Unique valuation ID. |
| instrument_id | Bond. |
| valuation_date_time | Timestamp. |
| valuation_source | Exchange / dealer / issuer / custodian / vendor / model. |
| price_type | BID / ASK / MID / LAST / EVALUATED / MODEL. |
| clean_price_pct | Price excluding accrued interest. |
| accrued_interest_pct | Accrued interest per 100 par. |
| dirty_price_pct | Clean + accrued. |
| yield_to_maturity | YTM. |
| yield_to_call | YTC, if callable. |
| yield_to_worst | Lowest relevant yield. |
| spread_to_benchmark | Spread over government/swap curve. |
| z_spread | Constant spread to discount curve. |
| oas | Option-adjusted spread for callable/MBS/etc. |
| duration | Macaulay/modified/effective duration. |
| convexity | Price curvature measure. |
| dv01 | Price value of one basis point. |
| price_quality | Observed / stale / interpolated / modelled / manual. |
| stale_price_flag | true/false. |
| source_priority_rank | For price hierarchy. |

---

## 9. Clean Price, Dirty Price, and Accrued Interest

Bonds usually quote **clean price**, excluding accrued interest.

The buyer pays:

```text
Dirty price = Clean price + accrued interest
```

Market value:

```text
Clean market value = nominal x clean price / 100
Dirty market value = clean market value + accrued interest amount
```

Example:

| Item | Value |
|---|---:|
| Nominal | USD 100,000 |
| Clean price | 98.00 |
| Accrued interest | USD 1,200 |
| Clean market value | USD 98,000 |
| Dirty market value | USD 99,200 |

Why this matters:

| Use Case | Usually Uses |
|---|---|
| Quoted price | Clean price |
| Settlement cash | Dirty price |
| Portfolio market value | Often dirty value for total wealth; clean + accrued shown separately |
| P&L analytics | Clean price movement and accrued income may be separated |
| Performance | Accrued income treatment must be consistent with income recognition policy |

---

## 10. Accrued Interest Calculation

General formula:

```text
Accrued interest = nominal x coupon rate x accrued day-count fraction
```

Example:

| Item | Value |
|---|---:|
| Nominal | USD 100,000 |
| Coupon | 6% p.a. |
| Coupon period | Semi-annual |
| Days accrued | 60 |
| Day-count base | 180 |

```text
Accrued interest = 100,000 x 6% x 60/360 = 1,000
```

Actual calculation depends on day-count convention, coupon calendar, ex-coupon convention, business-day adjustments, default status, and market practice.

---

## 11. Bond Valuation Hierarchy

Use a valuation hierarchy rather than one hard-coded price source.

| Level | Source | Use Case |
|---|---|---|
| Level 1 | Exchange/observable last trade where reliable | Listed or highly liquid bonds. |
| Level 2 | Dealer/broker quotes, evaluated prices, TRACE-like market data, custodian price | Most actively priced OTC bonds. |
| Level 3 | Internal model | Illiquid, distressed, complex, private, stale price cases. |

Practical price source priority:

```text
1. Trusted market/exchange price if liquid and fresh
2. Independent evaluated price
3. Custodian price
4. Dealer/broker executable bid/ask
5. Issuer/arranger indication
6. Internal model
7. Last known price with stale flag
```

Always store price quality and timestamp.

---

## 12. Are Bonds Listed?

Bonds can be listed, but many bonds trade primarily **over-the-counter** through dealers rather than on a centralized exchange.

| Case | Listing / Trading | Platform Treatment |
|---|---|---|
| Government benchmark bonds | Often highly liquid OTC/electronic market | Market price / curve. |
| Corporate bonds | Often OTC; some exchange-listed | Dealer/evaluated price. |
| Retail bonds | May be exchange-listed in some markets | Exchange price if liquid; still validate liquidity. |
| Private placements | Usually unlisted/illiquid | Custodian/vendor/model price. |
| Bond ETFs/funds | Exchange-traded/fund NAV | Model as fund/ETF, not individual bond. |

Important:

```text
Listing does not guarantee liquidity.
Unlisted does not mean unpriceable.
For wealth platforms, price source, freshness, bid/ask, and liquidity score matter more than listing flag alone.
```

---

## 13. Valuation by Bond Type

| Bond Type | Valuation Approach |
|---|---|
| Fixed-rate plain bond | Discount future coupon and principal cashflows using yield curve + credit spread. |
| Floating-rate bond | Project floating coupons using forward rates; discount using appropriate curve/spread. |
| Zero-coupon bond | Discount single maturity payment. |
| Inflation-linked bond | Adjust principal/cashflows by inflation index, then discount real/nominal cashflows. |
| Callable bond | Value as straight bond minus issuer call option; use yield-to-worst/effective duration/OAS. |
| Putable bond | Straight bond plus investor put option. |
| Convertible bond | Bond floor plus equity conversion option; depends on equity price, volatility, credit, rates. |
| Perpetual bond | Discount perpetual or call-adjusted cashflows; model extension and coupon deferral risk. |
| Amortising bond | Discount scheduled principal and coupon cashflows using current face/factor. |
| ABS/MBS | Model collateral cashflows, prepayment, default, recovery, extension, tranche priority. |
| Distressed/defaulted bond | Recovery-based valuation and scenario analysis. |

---

## 14. Core Bond Pricing Formula

For a plain fixed-rate bond:

```text
Bond price = Present value of coupons + Present value of principal
```

Expanded:

```text
Price = sum of discounted coupons + discounted principal
```

The price is quoted per 100 par.

If coupon > market yield, the bond may trade above par.
If coupon < market yield, the bond may trade below par.

---

## 15. Yield Concepts

| Yield Metric | Meaning | Use |
|---|---|---|
| Coupon yield | Contractual coupon rate. | Tells cash income rate on par, not market return. |
| Current yield | Annual coupon / current price. | Simple income measure, ignores maturity gain/loss. |
| Yield to maturity | Discount rate if held to maturity and all cashflows paid. | Standard comparison for non-callable bonds. |
| Yield to call | Yield assuming redemption on call date. | Important for callable bonds. |
| Yield to put | Yield assuming investor put is exercised. | Important for putable bonds. |
| Yield to worst | Worst yield among maturity/call/put scenarios. | Critical for callable/hybrid bonds. |
| Running yield | Coupon income relative to current price. | Income-focused reporting. |
| Spread yield | Yield relative to benchmark curve. | Credit/spread analysis. |
| Tax-equivalent yield | Tax-adjusted yield comparison. | Municipal/tax-exempt analysis. |

Important:

```text
Coupon is not the same as yield.
Yield depends on price, coupon, maturity, redemption features, and expected cashflows.
```

---

## 16. Yield to Maturity: Practical Example

Bond terms:

| Item | Value |
|---|---:|
| Coupon | 5% p.a. |
| Maturity | 5 years |
| Price | 95 |
| Redemption | 100 |

Since the bond is bought below par, the investor receives both coupon income and potential pull-to-par gain if held to maturity and issuer pays.

Therefore:

```text
Yield to maturity > coupon rate
```

If the same bond trades at 105:

```text
Yield to maturity < coupon rate
```

because part of future coupons compensates for the price falling back toward par at maturity.

---

## 17. Duration, Modified Duration, and DV01

### Duration

Duration measures sensitivity to interest-rate/yield changes.

High-level intuition:

```text
Longer maturity + lower coupon = higher duration.
Higher duration = more price sensitivity to yield changes.
```

### Modified duration approximation

```text
Approximate price change % = -Modified duration x yield change
```

Example:

| Item | Value |
|---|---:|
| Modified duration | 6 |
| Yield rises | 1.00% |
| Approximate price impact | -6% |

### DV01 / PV01

DV01 is the approximate currency value change for a 1 basis point move.

```text
DV01 approx. Market value x modified duration x 0.0001
```

Example:

| Item | Value |
|---|---:|
| Market value | USD 1,000,000 |
| Modified duration | 5 |
| DV01 | USD 500 per 1 bp |

Meaning: if yields rise by 1 bp, position value falls by about USD 500.

---

## 18. Convexity

Duration is a linear approximation. Convexity adjusts for curvature.

| Bond Type | Convexity Comment |
|---|---|
| Plain government/corporate bond | Usually positive convexity. |
| Callable bond | Can have negative convexity when rates fall because call risk increases. |
| MBS | Often has negative convexity due to prepayment behavior. |
| Putable bond | Investor put may improve downside behavior. |

For large yield moves, duration alone is not enough.

---

## 19. Spread Concepts

| Spread | Meaning |
|---|---|
| Government spread | Yield over similar maturity government bond. |
| Swap spread | Yield over swap curve. |
| Credit spread | Compensation for issuer credit/liquidity risk. |
| Z-spread | Constant spread added to the discount curve to match price. |
| OAS | Option-adjusted spread; removes value of embedded options. |
| CDS basis | Difference between bond spread and CDS spread. |

Spread movement matters:

```text
Even if government rates do not move, a corporate bond price can fall if credit spreads widen.
```

---

## 20. Credit Risk and Recovery Model

Credit risk has several layers.

| Layer | Meaning |
|---|---|
| Issuer probability of default | Likelihood issuer fails to pay. |
| Loss given default | Expected loss after recovery. |
| Seniority | Priority in repayment. |
| Collateral | Assets backing the bond. |
| Guarantee | Third-party support. |
| Covenant protection | Contract terms limiting issuer actions. |
| Structural subordination | Holding company debt may rank behind operating company debt. |
| Jurisdiction/legal regime | Affects enforcement and recovery. |

Expected loss framework:

```text
Expected loss = Probability of default x Loss given default
Loss given default = 1 - recovery rate
```

Example:

| Item | Value |
|---|---:|
| Probability of default | 5% |
| Recovery rate | 40% |
| Loss given default | 60% |
| Expected loss | 3% |

This is simplified. Actual credit valuation depends on timing, discounting, spread curves, market liquidity, and scenario assumptions.

---

## 21. Currency and FX Treatment

For foreign-currency bonds, maintain both local and base values.

| Field | Meaning |
|---|---|
| bond_currency | Currency of coupons/principal. |
| settlement_currency | Currency used to settle trade. |
| portfolio_base_currency | Client reporting currency. |
| local_market_value | Value in bond currency. |
| base_market_value | Value converted to portfolio base currency. |
| fx_rate | Rate used for valuation. |
| local_pnl | Price/income P&L in bond currency. |
| fx_pnl | P&L from currency movement. |

Performance attribution should separate:

```text
Bond local return
Currency return
Interaction / residual, depending methodology
```

---

## 22. Look-Through and Risk Exposure

For most plain bonds, the accounting position and issuer exposure are aligned.

But some bonds require deeper exposure modelling:

| Product | Look-Through Need |
|---|---|
| Corporate bond | Issuer, sector, country, rating, seniority. |
| Sovereign bond | Sovereign/country exposure, currency, duration. |
| Covered bond | Issuer + cover pool exposure. |
| ABS/MBS | Collateral pool, tranche, prepayment/default exposure. |
| Convertible bond | Issuer credit + equity delta exposure. |
| Inflation-linked bond | Real rate + inflation exposure. |
| AT1 / subordinated bank debt | Issuer + regulatory capital + loss absorption. |
| Green bond | Issuer exposure remains; green label is use-of-proceeds attribute. |

Do not confuse bond issuer exposure with collateral or underlying pool exposure.

---

## 23. Valuation Controls

Recommended controls:

| Control | Purpose |
|---|---|
| Price source hierarchy | Avoid inconsistent values. |
| Stale price detection | Flag old prices. |
| Outlier detection | Identify abnormal price/yield moves. |
| Bid/ask spread monitoring | Liquidity indicator. |
| Rating change feed | Update credit risk. |
| Call/maturity calendar | Avoid missing lifecycle events. |
| Accrued interest validation | Ensure day-count/coupon accuracy. |
| Yield reasonability check | Price/yield consistency. |
| Negative yield handling | Support correctly, not as error. |
| Default status override | Stop normal accrual if appropriate. |
| Manual price audit | Track overrides and approvals. |
| Corporate-action reconciliation | Match custodian/issuer events. |

---

## 24. Example Common Bond Contract Object

```json
{
  "instrumentId": "BOND123",
  "securityType": "BOND",
  "bondSubtype": "CALLABLE_FIXED_RATE",
  "isin": "XS1234567890",
  "issuerId": "COMPANY_ABC",
  "issueCurrency": "USD",
  "denomination": "1000",
  "issueDate": "2026-01-10",
  "maturityDate": "2031-01-10",
  "couponType": "FIXED",
  "couponRatePa": "0.0500",
  "couponFrequency": "SEMI_ANNUAL",
  "dayCountConvention": "30/360",
  "businessDayConvention": "MODIFIED_FOLLOWING",
  "redemptionPricePct": "100.00",
  "seniority": "SENIOR_UNSECURED",
  "securedFlag": false,
  "ratingCurrent": {
    "agency": "S&P",
    "rating": "BBB+",
    "outlook": "STABLE"
  },
  "callSchedule": [
    {
      "callDate": "2029-01-10",
      "callPricePct": "102.00",
      "noticePeriodDays": 30
    },
    {
      "callDate": "2030-01-10",
      "callPricePct": "101.00",
      "noticePeriodDays": 30
    }
  ],
  "valuationPolicy": {
    "preferredSource": "INDEPENDENT_EVALUATED_PRICE",
    "fallbackSource": "CUSTODIAN_PRICE",
    "priceQuoteType": "PCT_OF_PAR",
    "marketValueBasis": "DIRTY"
  }
}
```

---

## 25. Practitioner Summary

A bond platform needs a common model that separates instrument terms, coupon/cashflow schedules, lifecycle events, transactions, positions, valuations, and risk analytics. Bonds are usually held by nominal/current face, while prices are quoted as percentage of par. Clean price excludes accrued interest; dirty price includes accrued interest and is closer to settlement or full market value. Valuation depends on price source availability: liquid bonds can use market prices, while many corporate/OTC/private bonds use evaluated prices, dealer quotes, custodian prices, or model valuation. For analytics, the key measures are yield to maturity, yield to call, yield to worst, duration, DV01, convexity, credit spread, rating, liquidity, and currency exposure. Callable, convertible, perpetual, amortising, and ABS/MBS bonds need additional modelling because cashflows are uncertain or option-driven.
