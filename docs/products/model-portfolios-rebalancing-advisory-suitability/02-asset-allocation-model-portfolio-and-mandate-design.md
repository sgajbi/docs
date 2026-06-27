# 02 - Asset Allocation, Model Portfolio and Mandate Design

## 1. Asset allocation hierarchy

A professional platform should not store only one target weight number. Allocation must be hierarchical and explainable.

```text
Portfolio Target
  -> Asset Class
      -> Sub-asset Class
          -> Region / Country
              -> Sector / Industry
                  -> Currency
                      -> Product Type
                          -> Instrument / Fund / Model Component
```

Example:

| Level | Example target |
|---|---:|
| Equity | 45% |
| Developed market equity | 32% |
| US equity | 18% |
| Technology sector | 5% |
| USD exposure | 20% |
| Specific ETF/fund | 8% |

The target model should allow constraints at any level: asset class, region, currency, issuer, product type, liquidity bucket, ESG restriction, rating, duration, income target, mandate rule and client-specific restriction.

## 2. Strategic asset allocation, SAA

Strategic asset allocation is the long-term policy allocation. It is normally based on:

- client risk tolerance;
- return objective;
- investment horizon;
- liquidity need;
- base currency;
- income need;
- wealth objective;
- tax and estate context;
- ability and willingness to take risk;
- investment restrictions.

Example SAA:

| Asset class | Conservative | Balanced | Growth |
|---|---:|---:|---:|
| Cash | 10% | 5% | 3% |
| Fixed income | 65% | 45% | 25% |
| Equity | 20% | 40% | 60% |
| Alternatives | 5% | 10% | 12% |

SAA is the anchor for risk. Rebalancing should generally pull the portfolio back toward SAA unless a tactical overlay or client-specific instruction says otherwise.

## 3. Tactical asset allocation, TAA

TAA reflects shorter-term investment views. It can overweight or underweight SAA.

Example:

| Asset class | SAA | Tactical view | Current target |
|---|---:|---:|---:|
| Equity | 40% | +3% | 43% |
| Fixed income | 45% | -2% | 43% |
| Cash | 5% | +1% | 6% |
| Alternatives | 10% | -2% | 8% |

Important design rule:

```text
Effective Target = SAA + Tactical Overlay + Client / Mandate Adjustments
```

The platform should store each layer separately, not just the final number. This enables lineage and explanation.

## 4. Model portfolio construction approaches

| Approach | Description | Best for |
|---|---|---|
| Asset-class model | Only target allocations by broad asset class | High-level advisory and reporting |
| Fund/ETF implementation model | Target implemented with funds/ETFs | Scalable advisory, mass affluent, discretionary models |
| Direct-security model | Uses individual bonds/equities | HNW/UHNW, bespoke accounts, tax-aware portfolios |
| Core-satellite model | Core low-cost diversified exposure + satellite ideas | Advisory and private banking portfolios |
| Income model | Focused on yield and distributions | Retirees, income-seeking clients |
| Liability-aware model | Matched to spending/liability needs | Family office, trust, retirement planning |
| Tax-aware model | Optimized for after-tax outcomes | Jurisdictions with capital-gains/tax-lot relevance |
| ESG/sustainable model | Uses exclusion, integration or impact rules | Sustainability mandates |
| Multi-currency model | Controls FX exposures | Cross-border and global wealth clients |

## 5. Model portfolio governance

A model portfolio should be governed like a controlled investment object.

Minimum governance attributes:

| Attribute | Purpose |
|---|---|
| model_id | Unique identifier |
| model_name | Human-readable name |
| model_version | Version control |
| owner_team | CIO, investment office, PM team, product team |
| approval_status | Draft, approved, retired |
| effective_from/effective_to | Time validity |
| risk_profile | Suitable risk band |
| target_return | Optional expected return assumption |
| target_volatility | Optional expected risk assumption |
| base_currency | Reporting and optimization currency |
| benchmark_id | Relative-performance reference |
| allowed_products | Product universe |
| excluded_products | Prohibited instruments/sectors/issuers |
| rebalance_policy | Calendar/threshold/hybrid |
| minimum_cash_buffer | Liquidity reserve |
| documentation_url | Investment rationale, factsheet, IC minutes |

## 6. Model versioning

Never overwrite a model silently. Use versions.

Example:

| Version | Effective date | Change |
|---|---|---|
| BALANCED_USD_V1 | 2026-01-01 | Initial approved balanced model |
| BALANCED_USD_V2 | 2026-04-01 | Increase IG bonds, reduce EM equity |
| BALANCED_USD_V3 | 2026-06-01 | Add gold allocation |

Client portfolios should reference the model version used when proposal was generated. Later performance review should be able to explain:

- which model version applied;
- when the change happened;
- which portfolios were affected;
- whether the proposal used old or new model;
- whether client accepted or rejected the change.

## 7. Mandate design

A mandate defines investment authority and boundaries.

| Mandate element | Example |
|---|---|
| Objective | Balanced growth and income |
| Risk profile | Medium |
| Base currency | USD |
| Benchmark | 40% MSCI ACWI + 50% Bloomberg Global Aggregate + 10% Cash |
| Asset-class ranges | Equity 30-50%, Fixed Income 35-60%, Cash 0-15% |
| Product permissions | Funds, ETFs, bonds, equities, structured notes up to 10% |
| Restrictions | No single issuer > 10%, no sub-IG bonds > 15% |
| Liquidity rule | At least 5% cash / T+5 liquid assets |
| FX rule | Non-base currency exposure max 30% unhedged |
| ESG rule | Exclude tobacco and controversial weapons |
| Leverage rule | No borrowing or derivatives leverage unless approved |
| Review frequency | Quarterly |

## 8. Mandate range versus target

A target is the preferred allocation. A range is the allowed boundary.

Example:

| Asset class | Target | Minimum | Maximum |
|---|---:|---:|---:|
| Equity | 40% | 30% | 50% |
| Fixed income | 45% | 35% | 60% |
| Alternatives | 10% | 0% | 15% |
| Cash | 5% | 0% | 15% |

Interpretation:

- Current equity at 44% may be above target but within mandate.
- Current equity at 52% is outside mandate and should trigger breach/remediation workflow.
- Rebalancing policy may act before mandate breach through tolerance bands.

## 9. Portfolio sleeves

Large HNW/UHNW portfolios often use sleeves.

| Sleeve | Purpose | Example manager |
|---|---|---|
| Core portfolio | Main diversified model | DPM team |
| Advisory ideas | Client-approved tactical ideas | Advisor |
| Legacy holdings | Founder stock or restricted stock | Client-specific |
| Income sleeve | Coupon/dividend generation | Income PM |
| Alternatives sleeve | PE/private credit/real estate | Alternatives team |
| Liquidity sleeve | Cash/MMF/short-duration bonds | Treasury/liquidity team |
| Hedging sleeve | FX/options/rates hedge | Overlay manager |

Sleeve-level modelling is important because a single total portfolio target can hide purpose-specific positions.

## 10. Product universe design

A model should not propose instruments from an unconstrained universe. It should operate over an approved universe.

Universe filters:

- booking centre availability;
- client domicile restrictions;
- product approval status;
- risk rating;
- currency;
- minimum investment amount;
- liquidity/dealing frequency;
- cost/fee class;
- ESG classification;
- mandate eligibility;
- advisor licence permissions;
- tax documentation status;
- complex product eligibility;
- credit rating / issuer limits;
- concentration rules.

## 11. Model drift and target drift

There are two forms of drift:

| Drift type | Meaning |
|---|---|
| Portfolio drift | Current portfolio differs from target because of market moves, cashflows and trades |
| Model drift | Model target changes because investment office updates views |

A high-quality platform should support both:

```text
Current Portfolio vs Previous Target
Current Portfolio vs New Target
Previous Target vs New Target
```

This helps advisors explain why a proposal is needed.

## 12. Good design questions

When designing model and mandate capability, ask:

1. Is the model asset-class-only or instrument-level?
2. Does the model produce direct orders or only guidance?
3. Are models versioned and time-effective?
4. Are tactical overlays separately stored?
5. Are model changes approved by investment committee?
6. Can client restrictions override model targets?
7. Can the same client have multiple sleeves with different models?
8. How are private markets and illiquid assets included?
9. Are cash needs and upcoming capital calls reflected?
10. Are tax lots and realized-gain limits considered?
11. Are leverage and collateral constraints considered?
12. Are product eligibility and suitability checked before proposal generation?
13. Is benchmark assignment linked to model/mandate version?

## 13. Summary

Model portfolios are not just target weights. They are governed investment templates. Mandates convert models into client-specific authority and restrictions. A robust platform must store model versions, effective dates, target ranges, product universes, overlays, restrictions, benchmarks, sleeves and lineage.
