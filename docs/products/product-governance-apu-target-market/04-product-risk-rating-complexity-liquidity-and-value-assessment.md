# 04. Product Risk Rating, Complexity, Liquidity and Value Assessment

## 1. Product risk rating

Product risk rating converts product characteristics into a controlled risk category used by advisory, suitability, mandates, reporting and client communication.

It is not the same as asset-class classification.

Example:

| Product | Asset class | Possible product risk rating |
|---|---|---|
| Singapore Treasury bill | fixed income / cash equivalent | low |
| investment-grade corporate bond | fixed income | low to medium |
| high-yield bond | fixed income | medium to high |
| perpetual subordinated bond | fixed income | high |
| global equity ETF | equity | medium to high |
| single small-cap stock | equity | high |
| principal-protected note | structured product | medium, depending on issuer and payoff |
| worst-of autocallable | structured product | high |
| private equity fund | alternatives | high |
| leveraged option strategy | derivative | very high |

## 2. Product risk-rating dimensions

A good product risk rating should consider:

| Dimension | Example factor |
|---|---|
| market risk | equity beta, duration, FX, commodity exposure |
| credit risk | issuer rating, counterparty risk, reference entity risk |
| liquidity risk | market depth, lock-up, redemption frequency |
| complexity risk | embedded derivatives, path dependency, non-linear payoff |
| leverage risk | margin, borrowing, derivative notional, embedded leverage |
| volatility | historical / expected volatility |
| loss severity | maximum loss, loss beyond investment, contingent liability |
| concentration | single issuer, sector, country, manager, strategy |
| valuation transparency | listed price vs model price |
| operational risk | manual lifecycle, corporate actions, capital calls |
| product term | maturity, lock-up, extension risk |
| distribution risk | complexity of explanation and client understanding |

## 3. Complexity classification

Complexity classification determines whether additional knowledge, disclosure, appropriateness or advisory controls are required.

| Complexity level | Product examples | Key reason |
|---|---|---|
| Non-complex | cash deposit, listed large-cap equity, simple bond, plain ETF | straightforward payoff and liquidity |
| Moderately complex | callable bond, high-yield bond, REIT, multi-asset fund | additional risk drivers |
| Complex | structured note, hedge fund, private fund, derivative, DCI | nonlinear or illiquid / hard to value |
| Highly complex | leveraged derivative, path-dependent autocallable, accumulator, exotic option | high loss potential and difficult payoff |

Complexity should be based on product behaviour, not product label.

A bond can be complex if it is perpetual, subordinated, callable, convertible, contingent capital, inflation-linked, structured or illiquid.

A fund can be complex if it uses leverage, derivatives, illiquid assets, gates, side pockets or performance fees.

## 4. Liquidity classification

Liquidity classification should include both normal and stressed conditions.

| Liquidity class | Typical meaning | Examples |
|---|---|---|
| Daily liquid | usually trade/redeem daily | listed equity, ETF, money market fund |
| Periodic liquid | monthly/quarterly dealing | many funds |
| Limited liquidity | issuer bid or thin market only | structured note, small bond issue |
| Illiquid | long lock-up or no active market | private equity, private real estate |
| Contingently illiquid | liquid normally, but can gate/suspend | open-ended property fund, stressed credit fund |
| Event-driven liquidity | liquidation depends on maturity/call/capital event | private credit, insurance policy |

Liquidity should affect:

- mandate eligibility,
- concentration limits,
- rebalancing assumptions,
- risk reporting,
- client disclosure,
- stress tests,
- collateral eligibility,
- buying power,
- suitability.

## 5. Liquidity fields to model

| Field | Example |
|---|---|
| liquidity_category | DAILY / WEEKLY / MONTHLY / LIMITED / ILLIQUID |
| dealing_frequency | daily, weekly, monthly, quarterly |
| notice_period_days | 30, 60, 90 |
| lockup_end_date | 2028-12-31 |
| gate_possible | true |
| redemption_fee_schedule | 2% if redeemed before 1 year |
| secondary_market_available | true/false |
| issuer_market_making | yes / best efforts / none |
| stale_price_threshold_days | 5 / 30 / 90 |
| liquidation_horizon_days | estimated days to liquidate |
| stressed_liquidity_category | worse classification under stress |

## 6. Loss-capacity classification

Product governance should classify potential loss clearly.

| Loss profile | Product examples |
|---|---|
| capital protected by deposit scheme | eligible bank deposit within covered limit |
| principal expected but issuer-risk exposed | plain note / bond held to maturity |
| principal protected at maturity but issuer-risk exposed | principal-protected note |
| full capital loss possible | equity, fund, high-yield bond, structured note |
| loss beyond initial investment possible | futures, written options, margin FX, some derivatives |
| contingent liability | capital call commitment, margin call, policy premium obligation |

This should feed client suitability and mandate rules.

## 7. Leverage and embedded leverage

Leverage may be explicit or embedded.

| Type | Examples |
|---|---|
| Explicit borrowing | margin loan, lombard loan |
| Derivative leverage | futures, options, swaps, CFDs |
| Embedded product leverage | leveraged ETF, structured product, inverse product |
| Fund-level leverage | hedge fund, private credit fund, REIT |
| Economic leverage | high duration bond, option delta/gamma exposure |

A product marked as "unleveraged" because the client paid full cash may still have embedded leverage at product level.

## 8. Fair value and value-for-money assessment

Product approval should assess whether the product provides fair value relative to alternatives and target market.

Areas to review:

- product cost versus expected benefit,
- margin embedded in structured products,
- share-class fees,
- trail commission/rebate conflicts,
- performance fee structure,
- early exit penalties,
- surrender charges,
- trading spreads,
- ongoing custody/platform fees,
- expected liquidity versus client need,
- payoff versus simpler alternatives,
- scenario outcomes under stress.

## 9. Product cost taxonomy

| Cost type | Examples |
|---|---|
| upfront explicit | subscription fee, sales charge, brokerage |
| ongoing explicit | management fee, custody fee, advisory fee |
| embedded | structuring margin, fund TER, bid/ask spread |
| contingent | performance fee, carry, redemption fee |
| financing | margin interest, loan spread, funding cost |
| exit | surrender charge, early redemption fee, secondary-market spread |
| tax-related | withholding, stamp duty, transaction tax |

## 10. Product rating changes

Product risk rating should be re-evaluated when:

- issuer rating changes,
- manager changes,
- fund liquidity changes,
- volatility changes materially,
- market dislocation occurs,
- structured-note barrier is breached,
- fund gates redemptions,
- leverage increases,
- regulatory classification changes,
- product documents are amended,
- valuation source changes,
- product is suspended or delisted.

## 11. Risk-rating model design

A platform can support both committee-approved rating and algorithmic suggested rating.

```text
Risk Rating = max(
    asset_class_base_score,
    credit_score,
    liquidity_score,
    complexity_score,
    leverage_score,
    volatility_score,
    loss_severity_score,
    valuation_transparency_score
)
```

Do not allow algorithmic rating to silently override committee rating. Use algorithmic triggers to require review.

## 12. Product-risk rating example

```json
{
  "productId": "BOND123",
  "riskRating": "MEDIUM_HIGH",
  "riskRatingVersion": "2026-06-01",
  "ratingDrivers": [
    "SUBORDINATED_DEBT",
    "CALLABLE",
    "DURATION_8Y",
    "BBB_MINUS",
    "LIMITED_LIQUIDITY"
  ],
  "complexityLevel": "MODERATELY_COMPLEX",
  "liquidityCategory": "LIMITED_LIQUIDITY",
  "maxLossProfile": "FULL_CAPITAL_LOSS_POSSIBLE",
  "reviewTriggers": ["RATING_DOWNGRADE", "SPREAD_WIDENING", "CALL_EVENT"]
}
```

## 13. Common anti-patterns

| Anti-pattern | Problem |
|---|---|
| one risk rating per asset class | hides differences within bonds/funds/notes |
| complexity = product type only | misses complex bonds/funds/ETPs |
| liquidity = listed/unlisted only | listed products can be illiquid |
| product rating never expires | stale governance |
| no reason codes | no explainability |
| no versioning | cannot reconstruct historical decisions |
| advisor can manually override product risk | weak control |
| risk rating not used by reporting | client sees inconsistent view |

## 14. Client-reporting implication

Risk, complexity and liquidity classifications should appear consistently in:

- portfolio holdings reports,
- product factsheets,
- advisory proposals,
- suitability result explanations,
- risk dashboards,
- concentration reports,
- DPM mandate monitoring,
- post-sale review alerts,
- client review packs.

If a product is classified as illiquid for suitability but reported as liquid in portfolio review, the platform creates conduct and reputational risk.
