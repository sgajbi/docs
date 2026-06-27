# 10 - Worked Examples and Implementation Patterns

Use these examples when modelling listed REITs, business trusts, private real estate funds, direct property, infrastructure funds, distributions, appraisals, leverage, liquidity, advisory, DPM, reporting, QA, and implementation boundaries.

## 1. Wrapper Versus Economic Exposure

| Wrapper | Operational model | Economic exposure | Main source owner | Platform warning |
|---|---|---|---|---|
| Listed REIT | equity-like trade and settlement | property sectors, leverage, rental income | exchange, issuer, corporate-action source | Do not model as direct property. |
| REIT ETF/fund | fund/security position | basket of REITs/property securities | fund admin or exchange | Look-through may lag NAV/price. |
| Private real estate fund | commitment/NAV/private fund model | properties, leverage, appraisal marks | fund manager/admin | NAV and valuation date often lag. |
| Direct property | asset record/document-heavy model | specific property | appraisal/custody/legal records | Needs ownership, valuation and document controls. |
| Infrastructure fund | fund/private fund/security model | assets, concessions, utilities, renewables | manager/admin/exchange | Contract/regulatory risk matters. |
| Structured property exposure | structured product model | index/property/REIT reference | issuer/calculation agent | Underlying is exposure, not held property. |

Design rule:

```text
Accounting wrapper drives booking.
Economic exposure drives allocation, risk, concentration and explanation.
```

## 2. Listed REIT Distribution

Scenario:

- Client holds 10,000 REIT units.
- Distribution per unit: SGD 0.035.
- Withholding/tax adjustment: SGD 50.

Calculation:

```text
gross distribution = 10,000 x 0.035 = 350
net cash = 350 - 50 = 300
```

Platform treatment:

- entitlement date must use exchange/corporate-action source;
- distribution should be classified according to available source detail;
- withholding/tax should not be treated as market loss;
- distribution yield should use a clear denominator and period basis;
- REIT income should remain separate from property valuation movement.

## 3. REIT Rights Issue

Scenario:

- Client holds 5,000 REIT units.
- Rights ratio: 1 new unit for every 5 existing units.
- Subscription price: SGD 1.20.

Entitlement:

```text
rights entitlement = 5,000 / 5 = 1,000 rights
subscription cash required = 1,000 x 1.20 = 1,200
```

Controls:

- capture election options and deadline;
- model rights as entitlement until exercised/sold/lapsed;
- if exercised, book new units and cash debit;
- if sold, book sale proceeds;
- if lapsed, record lapse with audit trail;
- ensure DPM/advisory authority for election is clear.

## 4. Private Real Estate Capital Call

Scenario:

- Commitment: USD 750,000.
- Capital call: 15%.
- Due date: 10 business days.

Calculation:

```text
cash required = 750,000 x 15% = 112,500
remaining unfunded commitment = 750,000 - 112,500 = 637,500
```

Implementation:

- store capital call notice;
- update paid-in and unfunded commitment;
- create cash-planning alert;
- do not update NAV until manager/admin valuation is sourced;
- report unfunded commitment as future obligation.

## 5. Appraisal Restatement

Scenario:

- Property fund reports NAV of USD 1,200,000 for Q2.
- Later appraisal restates NAV to USD 1,150,000.
- Published client report used the original NAV.

Required workflow:

```text
receive restatement -> preserve original NAV -> store restated NAV -> assess report impact -> recalculate analytics -> disclose or correct according to policy
```

Controls:

- valuation date and received date must be separate;
- restatement reason should be captured;
- performance and allocation should be recalculated for impacted periods;
- stale or restated values should be visible in client and advisor review.

## 6. Infrastructure Fund Distribution

Scenario:

- Infrastructure fund distribution: USD 40,000.
- Source classification:
  - operating income: USD 25,000;
  - return of capital: USD 10,000;
  - realized gain: USD 5,000.

Treatment:

| Component | Reporting treatment |
|---|---|
| operating income | income section where policy supports it |
| return of capital | capital return / basis reduction where applicable |
| realized gain | realized gain section |
| cash receipt | cash balance increase |

Do not show the full distribution as recurring income. Infrastructure distributions may include return of capital, refinancing proceeds, asset-sale gains or one-off items.

## 7. Redemption Queue

Scenario:

- Client requests redemption of USD 300,000 from a semi-liquid real estate fund.
- Fund accepts USD 90,000 in current window.
- USD 210,000 remains queued.

Platform states:

- requested redemption;
- accepted amount;
- queued/deferred amount;
- expected next window;
- settlement date;
- remaining NAV exposure;
- gate/queue source notice.

Reporting:

- do not show queued amount as settled cash;
- show remaining exposure until units are redeemed;
- flag liquidity constraint for advisor and DPM monitoring.

## 8. Direct Property Valuation

Scenario:

- Client has direct property exposure through a reportable entity.
- Latest appraisal: SGD 5,000,000.
- Appraisal date: 2026-03-31.
- Report date: 2026-06-30.

Treatment:

- show appraisal value with appraisal date;
- do not imply daily valuation;
- store property identifier, ownership share, currency, valuation source and document reference;
- if ownership is indirect through a company/trust, report ownership route and access restrictions;
- apply FX translation using report-date FX while preserving appraisal currency/date.

## 9. Leverage And Interest-Rate Sensitivity

Scenario:

- REIT total assets: SGD 10 billion.
- Debt: SGD 4 billion.
- Gearing: 40%.
- Interest cost rises by 1%.

Simplified annual interest impact:

```text
incremental interest cost = 4,000,000,000 x 1% = 40,000,000
```

Analytics implication:

- listed REIT price risk includes equity-market behavior and property fundamentals;
- distribution sustainability depends on rental income, financing cost, occupancy, debt maturity and refinancing risk;
- reporting should not describe REIT distribution yield as bond coupon;
- mandate risk should include sector/geography/gearing and interest-rate sensitivity where available.

## 10. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Objective | income, inflation hedge, diversification, growth, real asset exposure, or tactical trade? |
| Wrapper | listed REIT, fund, private fund, direct property, infrastructure, structured product? |
| Liquidity | exchange liquidity, fund dealing, lock-up, redemption queue, secondary market, property sale horizon? |
| Valuation | exchange price, NAV, appraisal, model, manager estimate, stale or restated value? |
| Leverage | property-level debt, fund leverage, REIT gearing, refinancing and rate exposure? |
| Concentration | property sector, geography, tenant, manager, vehicle, currency, infrastructure subsector? |
| DPM mandate | allowed wrapper, illiquid allocation, income target, leverage limit, concentration cap? |
| Reporting | legal holding, economic exposure, income classification, valuation date and liquidity label? |

## 11. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed REIT | security position, trades, price, distributions, corporate actions | property-sector look-through and gearing analytics |
| Private real estate fund | commitment, calls, NAV, distributions, valuation date | appraisal restatement workflow and liquidity queue simulation |
| Direct property | ownership record, appraisal value, source date, documents | document-backed ownership graph and property-level cashflow model |
| Infrastructure exposure | fund/security/private fund position and sector tags | concession, inflation-linkage and regulatory-risk analytics |
| Reporting | wrapper, exposure, value, income, source date, liquidity label | consolidated real-asset income and stress dashboard |

## 12. Regression Test Pack

Minimum release-gate scenarios:

1. Listed REIT distribution books gross, tax and net cash correctly.
2. REIT rights issue creates entitlement, election and new units when exercised.
3. Private real estate capital call updates paid-in and unfunded commitment.
4. Appraisal restatement preserves original and restated NAV.
5. Infrastructure distribution separates income, capital return and gain.
6. Redemption queue shows accepted and deferred amounts.
7. Direct property valuation preserves appraisal date and report-date FX.
8. Stale appraisal or NAV labels report output degraded.
9. REIT leverage/rate sensitivity appears as analytics, not as bond duration.
10. Missing look-through source prevents unsupported property-sector allocation claims.
