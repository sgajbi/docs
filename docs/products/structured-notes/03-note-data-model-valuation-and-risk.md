# 03 — Note Data Model, Valuation, and Risk

## 1. Common Data Model Strategy

A structured note model should avoid one massive nullable table and also avoid one completely different model per note subtype.

Recommended structure:

```text
Instrument
  └── NoteContract
        ├── NoteUnderlying[]
        ├── CouponSchedule[]
        ├── ObservationSchedule[]
        ├── BarrierTerms[]
        ├── CallPutTerms[]
        ├── SettlementTerms
        ├── CreditLinkedTerms?       optional
        ├── FxLinkedTerms?           optional
        ├── RateLinkedTerms?         optional
        ├── LifecycleEvents[]
        ├── ValuationRecords[]
        └── LookthroughExposure[]
```

This creates a common model with optional extensions, without making every field nullable in a single wide table.

---

## 2. Core Instrument Model

| Field | Example | Nullable? | Notes |
|---|---|---:|---|
| instrument_id | NOTE123 | No | Internal unique ID |
| security_type | NOTE | No | Broad security type |
| note_subtype | FCN, ELN, CLN, PPN, DCI, ETN, AUTOCALLABLE | No | Product subtype |
| isin / cusip / sedol | XS123... | Sometimes | May be absent for private/internal issues |
| issuer_id | BANK_X | No | Legal issuer |
| guarantor_id | PARENT_BANK_X | Yes | If guarantee exists |
| issue_currency | USD | No | Currency of notional |
| denomination | 1,000 / 10,000 / 100,000 | No | Face value per unit |
| issue_price_pct | 100.00 | No | Usually percent of par |
| face_value_per_unit | 1,000 | No | Used for nominal calculation |
| issue_date | 2026-01-10 | No | Issue date |
| maturity_date | 2027-01-10 | Usually no | May be open-ended for some listed products |
| tenor | 1Y | Derived | Derived from dates |
| settlement_type | CASH, PHYSICAL, CASH_OR_PHYSICAL | No | Determines maturity/conversion flow |
| listing_status | LISTED, UNLISTED, OTC | No | Liquidity and pricing relevance |
| exchange_mic | XSES | Yes | Required if listed |
| price_quote_type | PCT_OF_PAR, PRICE_PER_UNIT | No | Price interpretation |
| day_count | ACT/360, ACT/365, 30/360 | Yes | Coupon/accrual support |
| complexity_class | Simple, Complex, SIP, EIP | No | Jurisdiction/product-governance classification |
| principal_protection_type | NONE, CONDITIONAL, FULL_AT_MATURITY | No | Protection classification |
| principal_protection_pct | 0, 90, 100 | Yes | Meaningful for protected notes |
| min_subscription | 100,000 | Yes | Advisory/order validation |
| lot_size | 1,000 | Yes | Trading/settlement validation |

---

## 3. Note Contract Model

| Field | Meaning |
|---|---|
| note_contract_id | Unique contract record |
| instrument_id | Parent instrument |
| payoff_type | Fixed, linked, reverse convertible, autocallable, credit-linked, FX-linked, rate-linked |
| payoff_formula_id | Reference to formula/rule engine |
| principal_protection_type | None, conditional, full at maturity, partial |
| protection_barrier_type | Final-only, continuous, daily close, observation-date |
| settlement_type | Cash, physical, cash or physical, alternate currency |
| observation_calendar | Dates and business-day convention |
| coupon_calendar | Coupon payment schedule |
| business_day_convention | Following, modified following, preceding |
| calculation_agent | Issuer or third-party calculation agent |
| valuation_source_priority | Market, issuer, vendor, model |
| product_document_ref | Termsheet/prospectus link/reference |
| complexity_flags | Worst-of, barrier, leverage, memory coupon, physical delivery, credit event |

---

## 4. Underlying Model

A note may reference one or many underlyings.

| Field | Example |
|---|---|
| note_id | NOTE123 |
| underlying_id | AAPL_US, SPX_INDEX, USDJPY, COMPANY_ABC |
| underlying_type | EQUITY, INDEX, FX, RATE, CREDIT, COMMODITY |
| basket_role | SINGLE, BASKET, WORST_OF, BEST_OF, WEIGHTED |
| initial_fixing | 200.00 |
| strike | 200.00 |
| barrier_reference_level | 140.00 |
| weight | 33.3333% |
| currency | USD |
| price_source | Exchange, Bloomberg, Reuters, issuer, internal market data |
| dividend_treatment | Included, excluded, price return, total return |
| adjustment_factor | Corporate action adjustment factor |
| current_observed_value | Latest value for monitoring |
| current_performance_pct | Current value / initial fixing - 1 |

For worst-of notes, each underlying should have performance tracking, and the current worst-performing underlying should be derivable.

---

## 5. Coupon Model

| Field | Example |
|---|---|
| coupon_type | FIXED, FLOATING, CONDITIONAL, MEMORY, RANGE_ACCRUAL, DIGITAL |
| coupon_rate | 8% p.a. |
| coupon_frequency | Monthly, quarterly, semi-annual, annual |
| coupon_barrier_pct | 70% |
| memory_coupon_flag | Yes/no |
| memory_coupon_balance | Accumulated unpaid coupon, if applicable |
| floating_index | SOFR, SORA, Euribor |
| spread | +1.50% |
| cap | 10% |
| floor | 0% |
| day_count | ACT/360 |
| accrual_start_date | Start of coupon period |
| accrual_end_date | End of coupon period |
| payment_date | Coupon payment date |
| entitlement_status | Pending, confirmed, not payable, paid |

Coupon modelling rules:

| Coupon Type | Accounting Treatment |
|---|---|
| Fixed unconditional | Can accrue daily if accounting policy supports it. |
| Floating unconditional | Accrue after rate fixing or estimate using policy. |
| Conditional | Do not book as income until condition is met and entitlement confirmed. |
| Memory coupon | Track unpaid amount separately; book income only when payable. |
| Range accrual | Accrue based on confirmed eligible days or model estimate for valuation only. |

---

## 6. Barrier and Observation Model

| Field | Example |
|---|---|
| barrier_type | KNOCK_IN, KNOCK_OUT, AUTOCALL, COUPON_BARRIER |
| barrier_level_pct | 70% |
| barrier_level_value | 140.00 |
| observation_type | CONTINUOUS, DAILY_CLOSE, MONTHLY, QUARTERLY, FINAL_ONLY |
| observation_date | 2026-04-10 |
| condition_operator | >=, <=, >, < |
| condition_value | 100% of initial level |
| event_effect | COUPON_PAYABLE, AUTOCALL, KNOCK_IN_ACTIVE, KNOCK_OUT |
| status | Scheduled, observed, triggered, failed, cancelled |

Important distinction:

```text
Barrier terms define what should be checked.
Observation events record what was actually checked and what happened.
```

---

## 7. Optional Subtype Extensions

### 7.1 Credit-Linked Terms

| Field | Example |
|---|---|
| reference_entity | Company ABC |
| reference_obligation | Senior unsecured bond |
| credit_event_types | Bankruptcy, failure to pay, restructuring |
| recovery_method | Cash settlement, physical settlement, auction settlement |
| assumed_recovery_rate | 40% |
| protection_seller | Investor economically sells protection |
| protection_buyer | Issuer/arranger |
| credit_event_status | None, pending, confirmed, settled |

### 7.2 FX-Linked Terms

| Field | Example |
|---|---|
| investment_currency | USD |
| alternate_currency | SGD |
| fx_pair | USD/SGD |
| strike_or_conversion_rate | 1.3500 |
| fixing_source | WM/Reuters, Bloomberg, central bank, issuer |
| fixing_date | 2026-07-25 |
| redemption_currency_rule | Original if favorable, alternate if triggered |

### 7.3 Rate-Linked Terms

| Field | Example |
|---|---|
| rate_index | SOFR, SORA, Euribor, CMS10Y |
| reset_frequency | Monthly, quarterly |
| spread | +1.50% |
| cap | 8% |
| floor | 0% |
| range_lower | 2% |
| range_upper | 5% |
| fixing_source | Official fixing source |

---

## 8. Position Valuation Model

Basic market value formula:

```text
Market Value = Current Nominal Amount × Price % of Par + Accrued Income
```

Example:

| Item | Value |
|---|---:|
| Current nominal | USD 100,000 |
| Market price | 97.50% |
| Accrued income | USD 500 |
| Market value | USD 98,000 |

For price-per-unit products:

```text
Market Value = Quantity Units × Price Per Unit + Accrued Income
```

Valuation records should store:

| Field | Meaning |
|---|---|
| instrument_id | Note being valued |
| valuation_date_time | Date/time of price |
| price | Market/evaluated/model price |
| price_type | Clean, dirty, bid, mid, ask, indicative |
| quote_type | % of par or price per unit |
| accrued_income | Accrued income, if separately available |
| market_value | Position-level value |
| valuation_currency | Currency of valuation |
| source | Exchange, issuer, custodian, vendor, model |
| source_priority | Which source was used under hierarchy |
| stale_flag | Whether price is stale |
| confidence_level | High, medium, low |
| model_version | If model-derived |
| inputs_snapshot_id | Reference to valuation inputs |

---

## 9. Valuation Hierarchy

A platform should define a source hierarchy.

| Level | Source | Typical Use |
|---|---|---|
| Level 1 | Active exchange market price | ETNs, listed notes with reliable liquidity |
| Level 2 | Issuer quote or independent evaluated price | Private-bank structured notes |
| Level 3 | Internal model valuation | Illiquid or no external price available |

Recommended source priority:

```text
1. Executable market price, if reliable
2. Exchange close / official price, if listed and liquid
3. Issuer bid/indicative quote
4. Independent valuation vendor
5. Custodian evaluated price
6. Internal model
7. Last known price with stale warning
```

Do not mix bid, mid, ask, clean, dirty, and indicative prices without clear labelling.

---

## 10. Valuation by Note Type

| Note Type | Simplified Valuation Approach | Key Inputs |
|---|---|---|
| Plain note | Discounted cashflow using issuer curve | Coupon, maturity, discount curve, issuer spread |
| Floating-rate note | Forward rates + discounting | Rate curve, spread, reset dates, issuer spread |
| Principal-protected note | Zero-coupon issuer bond + long option | Rates, issuer spread, underlying spot, volatility, dividends, time |
| Equity-linked / reverse convertible | Debt component + short put / barrier option | Spot, strike, barrier, volatility, dividends, rates, issuer spread |
| Worst-of note | Basket option model | Spots, volatilities, correlations, barriers, rates, dividends |
| Autocallable / Phoenix | Monte Carlo or lattice/path-dependent model | Observation schedule, barriers, autocall levels, volatility, correlation, rates |
| Dual currency note | Deposit/bond + short FX option | FX spot, FX forward, rates, FX volatility, strike, tenor |
| Credit-linked note | Risky bond + CDS-style credit leg | Issuer spread, reference CDS spread, recovery rate, default probability |
| Range accrual note | Rate model / simulation | Yield curves, volatility, range limits, eligible days |
| ETN | Exchange price + indicative value check | Exchange price, intraday indicative value, fees, issuer credit |

Generic decomposition:

```text
Structured Note Value
= Debt Component Value
+ Embedded Derivative Value
- Fees / funding / hedging / liquidity adjustments
```

---

## 11. Key Valuation Inputs

| Input | Why It Matters |
|---|---|
| Underlying spot price | Determines current moneyness and barrier distance. |
| Initial fixing / strike | Reference level for payoff. |
| Barrier level | Determines protection, knock-in, knock-out, or coupon trigger. |
| Volatility | Major driver of option value. |
| Correlation | Critical for basket and worst-of notes. |
| Interest-rate curve | Discounting and forwards. |
| Dividend yield | Equity option pricing and forward levels. |
| FX spot/forward | Currency-linked notes and multi-currency underlyings. |
| Issuer credit spread | Discounts issuer obligation and affects note value. |
| Reference credit spread | CLN risk and valuation. |
| Recovery assumption | CLN loss estimate. |
| Time to maturity | Option time value and discounting. |
| Liquidity spread | Secondary sale discount or valuation haircut. |
| Fees/structuring margin | Explains difference between issue price and estimated fair value. |

---

## 12. Clean vs Dirty Price

| Price Type | Meaning |
|---|---|
| Clean price | Excludes accrued interest. |
| Dirty price | Includes accrued interest. |
| Bid price | Price at which investor may sell. |
| Ask price | Price at which investor may buy. |
| Mid price | Theoretical midpoint. |
| Indicative value | Model or issuer estimate; may not be executable. |

Treatment by coupon type:

| Coupon Type | Accrual Recommendation |
|---|---|
| Fixed unconditional coupon | Accrue daily if policy supports fixed-income accrual. |
| Floating unconditional coupon | Accrue after fixing or estimate per policy. |
| Conditional coupon | Do not book accounting income until condition is confirmed. |
| Memory coupon | Track memory balance; do not book income until payable. |
| Autocall-linked coupon | Book when entitlement is confirmed and payment is due. |
| Range accrual | Accrue only based on confirmed eligible days, or use model for valuation only. |

---

## 13. Look-Through Exposure Model

The accounting position is the note, but risk systems often need look-through exposure.

| Exposure Type | Example |
|---|---|
| Issuer exposure | Bank X unsecured note exposure |
| Equity underlying exposure | Apple, Microsoft, Nvidia |
| Index exposure | S&P 500, Euro Stoxx 50 |
| FX exposure | USD/SGD, EUR/USD |
| Rate exposure | SOFR, SORA, CMS10Y |
| Credit reference exposure | Company ABC default risk |
| Sector/country/currency exposure | Derived from underlying and issuer |
| Worst-of concentration | Exposure to weakest underlying and basket concentration |

Look-through exposure should be stored separately from accounting position.

Possible fields:

| Field | Meaning |
|---|---|
| account_id | Portfolio/account |
| instrument_id | Note |
| exposure_underlying_id | AAPL_US |
| exposure_type | Equity delta, credit, FX, rate, issuer |
| exposure_amount | Notional or delta-adjusted amount |
| exposure_currency | Exposure currency |
| delta | Option delta if available |
| gamma | Optional Greeks |
| vega | Optional Greeks |
| issuer_exposure_amount | Full unsecured issuer exposure |
| worst_of_flag | Whether exposure is part of worst-of structure |
| confidence_level | Based on model/source quality |

---

## 14. Risk Analytics Treatment

| Risk Metric | Treatment Consideration |
|---|---|
| Market value | Use approved valuation source. |
| Unrealized P&L | Market value less cost basis. |
| Income | Coupons recognized when paid or entitled based on accounting policy. |
| Volatility | Use historical note prices if available; otherwise proxy/model. |
| VaR | Use note price history or look-through Greeks/model. |
| Stress testing | Shock underlyings, volatility, correlation, rates, FX, issuer spread. |
| Concentration | Measure issuer, underlying, sector, currency, product type, worst-of concentration. |
| Liquidity risk | Flag unlisted, stale price, issuer-only bid, long tenor. |
| Suitability risk | Complex product flags and client eligibility. |

---

## 15. Example JSON Contract

```json
{
  "instrumentId": "NOTE123",
  "securityType": "NOTE",
  "noteSubtype": "AUTOCALLABLE_FCN",
  "issuerId": "BANK_X",
  "issueCurrency": "USD",
  "denomination": "1000",
  "issuePricePct": "100.00",
  "maturityDate": "2027-06-25",
  "principalProtectionType": "CONDITIONAL",
  "principalProtectionPct": "100",
  "settlementType": "CASH_OR_PHYSICAL",
  "listingStatus": "UNLISTED",
  "priceQuoteType": "PCT_OF_PAR",
  "underlyings": [
    {
      "underlyingId": "AAPL_US",
      "underlyingType": "EQUITY",
      "basketRole": "WORST_OF",
      "initialFixing": "200.00",
      "strike": "200.00",
      "weight": "0.333333"
    },
    {
      "underlyingId": "MSFT_US",
      "underlyingType": "EQUITY",
      "basketRole": "WORST_OF",
      "initialFixing": "400.00",
      "strike": "400.00",
      "weight": "0.333333"
    }
  ],
  "coupon": {
    "couponType": "CONDITIONAL_MEMORY",
    "couponRatePa": "0.12",
    "couponFrequency": "MONTHLY",
    "couponBarrierPct": "0.70",
    "memoryCoupon": true
  },
  "barriers": [
    {
      "barrierType": "KNOCK_IN",
      "levelPct": "0.70",
      "observationType": "CONTINUOUS"
    },
    {
      "barrierType": "AUTOCALL",
      "levelPct": "1.00",
      "observationType": "MONTHLY"
    }
  ],
  "valuation": {
    "preferredSource": "ISSUER_BID",
    "fallbackSource": "INDEPENDENT_VENDOR",
    "modelType": "MONTE_CARLO"
  }
}
```

---

## 16. Data Feeds Required

| Feed | Purpose |
|---|---|
| Instrument master | Note identity, issuer, terms, identifiers |
| Termsheet/prospectus | Legal payoff and lifecycle rules |
| Underlying market data | Equity/index/FX/rate/commodity/credit prices |
| Initial fixing feed | Strike and initial reference levels |
| Observation/fixing feed | Barrier, coupon, autocall, maturity checks |
| Coupon/redemption corporate action feed | Cash and security events |
| Issuer quote feed | OTC valuation |
| Independent valuation feed | Vendor price validation |
| FX rates | Portfolio reporting and multi-currency valuation |
| Ratings/credit spreads | Issuer and reference credit risk |
| Corporate action feed for underlyings | Adjust strikes/barriers/conversion ratio |
| Tax/withholding feed | Income and redemption tax treatment |

---

## 17. Valuation Controls

| Control | Reason |
|---|---|
| Price source hierarchy | Prevent inconsistent valuation. |
| Stale price detection | Structured notes may not price daily. |
| Bid/mid/ask labelling | Avoid overstating exit value. |
| Clean/dirty price consistency | Avoid double-counting accrued income. |
| Issuer quote timestamp | OTC quotes can become stale quickly. |
| Independent price variance check | Detect issuer/model anomalies. |
| Model input snapshot | Support audit and reproducibility. |
| Manual override approval | Ensure governance and audit trail. |
| Lifecycle event reconciliation | Price should reflect barrier/autocall/credit status. |
| Corporate action adjustment control | Ensure underlying changes are reflected in barriers/strike. |
