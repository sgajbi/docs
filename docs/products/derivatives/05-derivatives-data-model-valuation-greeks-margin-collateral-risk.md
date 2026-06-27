# 05 — Derivatives Data Model, Valuation, Greeks, Margin, Collateral and Risk

## 1. Canonical derivative data model

A robust wealth platform should separate **instrument definition**, **contract terms**, **position**, **transactions**, **lifecycle events**, **valuation**, **risk measures** and **collateral**.

```text
Instrument / Contract Master
  ├── Underlying / Reference Data
  ├── Contract Terms
  ├── Schedules and Fixings
  ├── Lifecycle Events
  ├── Transactions and Cash Legs
  ├── Position / Exposure
  ├── Valuation
  ├── Risk Measures
  └── Margin / Collateral
```

## 2. Core contract table

| Field | Meaning |
|---|---|
| contract_id | Unique derivative contract |
| product_family | OPTION / FUTURE / FORWARD / SWAP / CDS |
| product_subtype | CALL, PUT, IRS, FX_FORWARD, TRS etc. |
| account_id | Client portfolio/account |
| trade_date | Trade date |
| effective_date | Start date |
| expiry_or_maturity_date | Expiry/maturity |
| counterparty_id | Dealer/broker/clearing member |
| clearing_status | EXCHANGE / CLEARED_OTC / BILATERAL_OTC |
| legal_agreement_id | ISDA/CSA or broker agreement |
| settlement_type | CASH / PHYSICAL / NET_CASH |
| base_currency | Reporting or contract currency |
| lifecycle_status | ACTIVE / CLOSED / EXPIRED / EXERCISED / MATURED / DEFAULTED |
| purpose_tag | HEDGE / OVERLAY / INCOME / TACTICAL / STRUCTURED_PRODUCT_HEDGE |
| advisory_classification | Advisory, execution-only, DPM, professional-client etc. |

## 3. Underlying/reference table

| Field | Meaning |
|---|---|
| contract_id | Derivative contract |
| underlying_id | Stock, index, currency pair, rate, commodity, credit entity |
| underlying_type | EQUITY / INDEX / FX / RATE / CREDIT / COMMODITY |
| weight | Basket weight if applicable |
| initial_level | Initial reference level |
| current_level | Latest level |
| price_source | Exchange/vendor/fixing source |
| currency | Underlying currency |
| multiplier | Contract multiplier |
| reference_role | Primary, basket constituent, reference entity |

## 4. Schedule and fixing model

| Table | Purpose |
|---|---|
| derivative_schedule | Payment, reset, fixing, exercise, expiry dates |
| derivative_fixing | Rate/FX/index fixings used in payoff |
| derivative_lifecycle_event | Exercise, assignment, expiry, reset, credit event, barrier event |
| derivative_cashflow_projection | Projected future cashflows |
| derivative_cashflow_actual | Confirmed actual cashflows |

This is essential for swaps, FX forwards, NDFs, OTC options, autocallable hedges and structured products.

## 5. Valuation hierarchy

| Level | Source | Use |
|---|---|---|
| Market price | Exchange settlement/last/bid/ask | Listed options, futures, ETNs |
| Broker/dealer valuation | Counterparty valuation | OTC derivatives |
| Independent vendor | Evaluated price/Greeks | Controls and reporting |
| Internal model | Model-based valuation | Risk, shadow valuation, controls |
| Fallback | Last good price/model/manual | Exception handling only |

Valuation records should store source, timestamp, curve/volatility surface versions and quality flags.

## 6. Valuation by product

| Product | Valuation approach |
|---|---|
| Listed option | Exchange price, option model for Greeks |
| OTC vanilla option | Black-Scholes / local market model / dealer quote |
| Barrier option | Barrier option model / Monte Carlo |
| Future | Exchange settlement price and daily VM |
| FX forward | Present value of forward rate difference |
| NDF | PV of expected cash settlement using fixing curve |
| IRS | PV fixed leg - PV floating leg |
| CCS | PV currency legs with FX and basis curves |
| TRS | PV reference return leg - financing leg |
| CDS | PV premium leg - PV protection leg |
| Swaption | Interest-rate option model |

## 7. Greeks and sensitivities

For options and many structured derivatives, the platform should store Greeks.

| Greek | Meaning | Why useful |
|---|---|---|
| Delta | Price sensitivity to underlying | Directional exposure |
| Gamma | Sensitivity of delta to underlying | Convexity / acceleration risk |
| Vega | Sensitivity to implied volatility | Volatility exposure |
| Theta | Time decay | Carry / option erosion |
| Rho | Sensitivity to interest rates | Rate exposure |
| Vanna/Volga | Advanced FX/option sensitivities | Exotic/FX option risk |

For rates and credit:

| Measure | Meaning |
|---|---|
| DV01 / PV01 | Value change for 1 bp rate move |
| Key-rate duration | Sensitivity by curve tenor |
| CS01 | Value change for 1 bp credit spread move |
| Spread duration | Credit spread sensitivity |
| Basis sensitivity | Cross-currency or bond/futures basis exposure |

## 8. Exposure measures

Do not rely on one exposure number. Use multiple views.

| Exposure measure | Purpose |
|---|---|
| Market value | Accounting value today |
| Gross notional | Contract face exposure |
| Net notional | Long and short net by risk bucket |
| Delta-equivalent exposure | Equity/FX/commodity directional exposure |
| DV01 | Interest-rate exposure |
| CS01 | Credit spread exposure |
| Vega exposure | Volatility exposure |
| Potential future exposure | Counterparty risk estimate |
| Margin requirement | Liquidity/collateral requirement |
| Stress loss | Scenario downside estimate |

Example: options exposure

```text
Delta Equivalent Exposure = Underlying Price × Contract Size × Number of Contracts × Delta
```

Example: futures exposure

```text
Futures Notional = Futures Price × Contract Multiplier × Number of Contracts
```

## 9. Margin and collateral

Derivatives often require margin/collateral.

| Type | Meaning |
|---|---|
| Initial margin | Collateral posted to cover potential future exposure |
| Variation margin | Daily/periodic settlement of mark-to-market changes |
| Independent amount | Extra collateral required by agreement |
| Threshold | Exposure amount before collateral is required |
| Minimum transfer amount | Minimum amount for collateral call |
| Haircut | Reduction applied to collateral value |
| Eligible collateral | Cash/bonds/securities accepted as collateral |

Platform must distinguish:

```text
Collateral movement ≠ investment P&L
Variation margin may be P&L settlement depending product/accounting model
```

## 10. Margin transaction model

| Transaction type | Use |
|---|---|
| DERIVATIVE_INITIAL_MARGIN_POSTED | Initial margin cash/security posted |
| DERIVATIVE_INITIAL_MARGIN_RETURNED | Margin returned |
| DERIVATIVE_VARIATION_MARGIN_GAIN | MTM gain settled in cash |
| DERIVATIVE_VARIATION_MARGIN_LOSS | MTM loss settled in cash |
| DERIVATIVE_COLLATERAL_POSTED | OTC collateral posted |
| DERIVATIVE_COLLATERAL_RETURNED | OTC collateral returned |
| DERIVATIVE_COLLATERAL_SUBSTITUTION | Collateral replaced |
| DERIVATIVE_MARGIN_INTEREST | Interest on cash collateral |
| DERIVATIVE_MARGIN_FEE | Broker/clearing/custody fee |

## 11. P&L decomposition

For analytics, derivative P&L can be decomposed.

| Component | Meaning |
|---|---|
| Delta P&L | Underlying price movement effect |
| Gamma P&L | Convexity effect |
| Vega P&L | Implied volatility movement |
| Theta / carry | Time decay / carry |
| Rates P&L | Curve movement |
| Credit P&L | Spread/default probability movement |
| FX P&L | Currency movement |
| Basis P&L | Difference between hedge and underlying |
| Residual | Model, interaction, data or unexplained component |

This is useful for DPM reviews, performance attribution and advisor explanation.

## 12. Risk analytics

| Risk metric | Derivative-specific considerations |
|---|---|
| Volatility | Non-linear exposure changes over time |
| VaR | Needs full revaluation or delta-gamma approximation |
| CVaR / Expected shortfall | Better for tail risk than simple VaR |
| Stress testing | Must include jumps, volatility spikes, rates shocks, FX devaluation |
| Drawdown | Derivative losses can be sudden |
| Liquidity-adjusted risk | Exit spreads may widen in stress |
| Counterparty exposure | OTC replacement cost and PFE |
| Wrong-way risk | Counterparty exposure increases when counterparty credit weakens |

## 13. Data quality controls

| Control | Purpose |
|---|---|
| Contract completeness | Required terms populated by product type |
| Underlying validity | Underlying exists and has price/fixing data |
| Expiry/maturity sanity | No active expired contract without closure event |
| Price source validation | Exchange/OTC source appropriate |
| Greek freshness | Greeks same date as valuation or flagged stale |
| Margin reconciliation | Broker margin equals internal margin records |
| Collateral eligibility | Posted collateral allowed by CSA/broker |
| P&L explain tolerance | Daily P&L explained within tolerance |
| Notional/exposure limits | Mandate and risk limit control |
| Settlement date monitoring | Exercise/settlement events do not miss value dates |

## 14. Reporting fields

A derivative position report should include:

- product family and subtype;
- underlying/reference;
- long/short direction;
- expiry/maturity;
- strike/rate/fixed leg/forward rate where relevant;
- market value;
- notional exposure;
- delta-equivalent/DV01/CS01/vega exposure;
- margin/collateral used;
- realized/unrealized P&L;
- purpose tag;
- counterparty/clearing broker;
- next lifecycle date;
- valuation source and quality flag.
