# 04 — Data Model, Valuation, Risk and Scenario Analytics

## 1. Data model objectives

A structured product data model must support:

- legal holding and accounting;
- payoff calculation;
- lifecycle processing;
- valuation and risk;
- suitability and mandate checks;
- performance and reporting;
- audit, reconciliation and explainability.

The model should avoid both extremes:

- too simple: one free-text product description and one price;
- too complex: modelling every embedded derivative as a separate client-held position.

## 2. Recommended product model

```text
Instrument
  └── StructuredProductContract
        ├── WrapperTerms
        ├── UnderlyingBasket[]
        ├── PayoffLeg[]
        ├── CouponSchedule[]
        ├── ObservationSchedule[]
        ├── BarrierTerms[]
        ├── CallPutTerms[]
        ├── SettlementTerms
        ├── ProtectionTerms
        ├── CreditTerms?          optional
        ├── FxTerms?              optional
        ├── RateTerms?            optional
        ├── LeverageTerms?        optional
        ├── LiquidityTerms
        ├── ValuationPolicy
        └── LifecycleEvents[]
```

## 3. Core instrument fields

| Field | Description |
|---|---|
| instrument_id | Unique product identifier |
| instrument_type | STRUCTURED_PRODUCT |
| wrapper_type | NOTE / DEPOSIT / CERTIFICATE / WARRANT / FUND / OTC |
| product_family | Yield enhancement, participation, protected, leveraged, etc. |
| product_subtype | Autocallable, DCI, accumulator, protected certificate, warrant, etc. |
| issuer_id | Issuer/arranger/counterparty |
| guarantor_id | Guarantor if any |
| issue_currency | Product currency |
| settlement_currency | Default settlement currency |
| denomination | Minimum unit/face value |
| issue_price | Initial price or principal amount |
| issue_date | Product issue date |
| trade_start_date | Start date for payoff/fixings |
| maturity_date | Final maturity/expiry |
| tenor | Derived tenor |
| listing_status | Listed/unlisted/OTC/private placement |
| exchange_mic | Exchange MIC if listed |
| complexity_classification | Internal/regulatory complexity category |
| sip_flag | Specified Investment Product flag where applicable |
| product_document_link | Termsheet/prospectus/KID/PHS/etc. |
| liquidity_classification | Daily liquidity, issuer bid, restricted, illiquid |
| tax_classification | Jurisdiction/product tax treatment if available |

## 4. Wrapper terms

| Wrapper type | Key fields |
|---|---|
| Note | nominal, issuer, ranking, seniority, ISIN, price % par |
| Deposit | principal amount, bank, early withdrawal rules, deposit protection treatment |
| Certificate | units, ratio, exchange, market maker, issuer call/termination |
| Warrant | units, strike, expiry, exercise style, entitlement ratio, issuer |
| Fund | fund ID, share class, NAV source, dealing frequency, TER |
| OTC contract | notional, counterparty, collateral, confirmation, CSA, settlement method |

## 5. Underlying model

| Field | Description |
|---|---|
| underlying_id | Instrument/index/FX/rate/credit reference |
| underlying_type | Equity, index, FX, rate, credit, commodity, fund, multi-asset |
| reference_currency | Currency of underlying |
| initial_fixing | Initial price/level/rate/spread |
| strike | Strike/conversion/reference level |
| weight | Basket weight where applicable |
| basket_role | Single, weighted, worst-of, best-of, rainbow |
| price_source | Market data source |
| fixing_source | Official fixing source |
| observation_method | Close, intraday, average, continuous, official fixing |
| corporate_action_adjustment_rule | How strikes/barriers adjust for equity events |

## 6. Payoff leg model

| Field | Description |
|---|---|
| payoff_leg_id | Unique payoff leg |
| leg_type | Principal, coupon, option, barrier, credit, FX conversion, leverage |
| direction | Long/short/economic exposure direction |
| formula_type | Fixed, floating, digital, participation, max/min, barrier, worst-of |
| notional | Relevant notional |
| participation_rate | Upside/downside participation |
| cap | Maximum return/coupon |
| floor | Minimum return/coupon |
| buffer_pct | Loss buffer |
| leverage_factor | Multiplier |
| coupon_rate | Fixed or formula coupon |
| coupon_condition | Condition for coupon payment |
| memory_flag | Whether missed coupon can be recovered |
| payoff_currency | Currency of payoff |
| settlement_method | Cash, physical, alternate currency |

## 7. Barrier model

| Field | Description |
|---|---|
| barrier_type | Knock-in, knock-out, autocall, coupon barrier, stop-loss |
| level_type | Percentage of initial, absolute level, dynamic level |
| level_value | Barrier value |
| observation_type | Continuous, daily close, scheduled, final-only |
| observation_window | Start/end dates |
| applies_to | Underlying, worst-of basket, average, portfolio |
| breach_effect | Activate downside, terminate, coupon no-pay, redemption |
| status | Active, breached, expired, not applicable |

## 8. Valuation source hierarchy

Structured product valuation should use a clear hierarchy.

| Level | Source | Use case |
|---|---|---|
| 1 | Exchange traded price | Listed warrant/certificate/ETN with reliable liquidity |
| 2 | Issuer bid/indicative price | Private banking OTC/unlisted products |
| 3 | Independent valuation vendor | Control valuation or client reporting source |
| 4 | Internal model | Fallback, risk analytics, scenario, IPV |
| 5 | Stale/last known price | Exception state, must be flagged |

Client reporting should clearly distinguish:

- executable bid;
- indicative value;
- theoretical model value;
- stale price;
- NAV-like value;
- settlement estimate.

## 9. Valuation approaches by product family

| Product | Valuation approach |
|---|---|
| Principal-protected product | Discounted principal component + option value |
| Yield enhancement / reverse convertible | Debt/deposit value + short put/barrier option economics |
| Autocallable | Monte Carlo/lattice due to path dependency |
| Worst-of basket product | Basket option model with correlation |
| DCI / FX-linked product | Deposit/funding leg + FX option value |
| Rate-linked product | Forward curve discounting + cap/floor/range accrual model |
| Credit-linked product | Risky bond plus credit derivative / default probability model |
| Certificate/warrant | Exchange price if liquid; option model for control |
| Accumulator/decumulator | Forward/option strip model with knock-out and leverage rules |
| Structured fund | NAV, with look-through and embedded strategy analytics if available |

## 10. Common valuation inputs

| Input | Relevance |
|---|---|
| Underlying spot | Moneyness and payoff |
| Initial fixing/strike | Reference level |
| Barrier levels | Trigger risk |
| Volatility | Option value |
| Correlation | Basket/worst-of value |
| Interest-rate curve | Discounting and forwards |
| Dividend yield | Equity option valuation |
| FX spot/forward | FX-linked valuation |
| Credit spread | Issuer/reference credit risk |
| Recovery rate | Credit-linked product loss modelling |
| Time to maturity | Option time value |
| Liquidity spread | Secondary exit valuation |
| Funding spread | Issuer funding economics |
| Fees/margins | Difference between issue price and fair value |
| Corporate action adjustments | Strike/barrier/ratio updates |
| Observation history | Path-dependent valuation |

## 11. Clean, dirty, NAV and price conventions

| Convention | Used for |
|---|---|
| Price % of par | Notes, bonds-like structures |
| Unit price | Certificates/warrants/funds |
| NAV | Funds/structured funds |
| Clean price | Excludes accrued income |
| Dirty price | Includes accrued income |
| Indicative value | Non-executable theoretical/issuer value |
| Bid price | Price at which issuer/market maker may buy back |
| Ask price | Price at which client may buy |

For structured products with conditional coupons, daily accrual for reporting must be carefully controlled. A conditional coupon should normally not be reported as earned income until the condition is met and payment is confirmed, unless the accounting policy explicitly requires estimated accrual.

## 12. Greeks and sensitivities

Structured products can have nonlinear sensitivity. Useful analytics include:

| Measure | Meaning |
|---|---|
| Delta | Sensitivity to underlying price |
| Gamma | Sensitivity of delta to underlying price |
| Vega | Sensitivity to volatility |
| Theta | Time decay |
| Rho | Sensitivity to interest rates |
| Credit DV01 / CS01 | Sensitivity to credit spread |
| FX delta | Currency sensitivity |
| Correlation sensitivity | Sensitivity to basket correlation |
| Barrier distance | Distance to trigger level |
| Autocall probability | Probability of early redemption under model |
| Expected maturity | Weighted expected life |
| Downside exposure | Loss if adverse scenario occurs |

For advisory, client-facing reporting should not be overloaded with Greeks, but advisory dashboards and risk systems should expose them where relevant.

## 13. Scenario analytics

A good structured product platform should support scenario tables.

Example scenarios:

| Scenario type | Example |
|---|---|
| Underlying shock | -10%, -20%, -30%, +10% |
| Volatility shock | Volatility +5 points |
| Rate shock | Parallel curve +100 bps |
| Credit spread shock | Issuer spread +100 bps |
| FX shock | Base currency depreciation/appreciation |
| Barrier scenario | Underlying approaches or breaches barrier |
| Autocall scenario | Early redemption next observation |
| Worst-of scenario | One basket name falls sharply |
| Liquidity scenario | Exit bid widened or unavailable |
| Issuer default scenario | Recovery assumption applied |

Scenario outputs should include:

- estimated product value;
- income impact;
- principal repayment estimate;
- realized/unrealized P&L;
- mandate breach flags;
- concentration changes;
- base-currency impact;
- worst-case loss narrative.

## 14. Risk classification model

Structured products should have a risk classification separate from wrapper type.

| Attribute | Example values |
|---|---|
| principal_at_risk | Yes/no/conditional |
| max_loss | Limited to premium / full notional / leveraged / uncapped |
| leverage_flag | Yes/no |
| complexity_score | 1–5 or Low/Medium/High/Very High |
| liquidity_score | Daily/weekly/issuer-only/illiquid |
| issuer_credit_risk | Low/medium/high based on issuer rating/spread |
| underlying_risk | Equity/FX/rate/credit/commodity/multi-asset |
| path_dependency | None/low/high |
| barrier_flag | Yes/no |
| physical_settlement_flag | Yes/no |
| alternate_currency_flag | Yes/no |
| retail_eligible | Yes/no/only with assessment |
| advisory_required | Execution only / advisory / professional only |

## 15. Look-through exposure model

| Exposure dimension | Example |
|---|---|
| Wrapper exposure | Structured note issued by Bank X |
| Issuer exposure | Bank X credit exposure |
| Underlying exposure | Apple, Microsoft, Nvidia |
| Asset-class exposure | Equity, FX, credit, rates |
| Directional exposure | Long, short, conditional, inverse |
| Delta-adjusted exposure | Model-based equivalent exposure |
| Notional exposure | Contractual notional |
| Worst-case exposure | Potential loss under terms |
| Currency exposure | Product currency, underlying currency, settlement currency |
| Liquidity exposure | Locked/issuer bid/exchange-traded |

For portfolio analytics, report both:

```text
Legal allocation by wrapper
Economic allocation by look-through risk
```

Example:

- Legal allocation: 8% structured products.
- Economic look-through: 5% equity downside exposure, 2% FX conversion risk, 1% issuer credit risk.

## 16. Valuation controls

Minimum controls:

- price source hierarchy configured by product/wrapper;
- stale price detection;
- bid/ask spread monitoring;
- issuer price versus independent price tolerance;
- model input completeness;
- barrier status validation;
- observation price source validation;
- corporate action adjustment validation;
- maturity/redemption cash reconciliation;
- FX conversion rate validation;
- income/redemption reconciliation to custodian cash;
- exception workflow and sign-off.

## 17. Example valuation record

```json
{
  "valuationId": "VAL-001",
  "instrumentId": "SP-456",
  "accountId": "ACC-123",
  "valuationDate": "2026-06-27",
  "valuationCurrency": "USD",
  "marketValue": "98250.00",
  "price": "98.25",
  "priceType": "PCT_OF_PAR",
  "source": "ISSUER_BID",
  "sourceTimestamp": "2026-06-27T18:00:00+08:00",
  "accruedIncome": "0.00",
  "cleanMarketValue": "98250.00",
  "dirtyMarketValue": "98250.00",
  "modelValue": "98600.00",
  "independentValue": "98450.00",
  "priceQuality": "CURRENT",
  "exceptionFlag": false
}
```

## 18. Example risk analytics record

```json
{
  "instrumentId": "SP-456",
  "analyticsDate": "2026-06-27",
  "deltaEquivalentExposure": "65000.00",
  "vega": "1200.00",
  "issuerExposure": "100000.00",
  "barrierDistancePct": "18.50",
  "autocallProbabilityNextObservation": "0.42",
  "expectedMaturityYears": "1.35",
  "worstCaseLossPct": "100.00",
  "liquidityClass": "ISSUER_BID_ONLY",
  "complexityScore": "HIGH"
}
```
