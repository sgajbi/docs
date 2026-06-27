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

## 10. Property-Level Debt Maturity Ladder

Scenario:

- A property vehicle owns three assets.
- Property-level debt is financed through separate loans.
- Refinancing risk matters because interest rates have risen.

| Property | Debt balance | Maturity | Interest rate |
|---|---:|---|---:|
| Office Tower A | 20,000,000 | 2027 | 3.20% |
| Retail Centre B | 15,000,000 | 2029 | 4.10% |
| Logistics Park C | 25,000,000 | 2031 | 4.50% |

Debt maturity view:

| Bucket | Debt balance | Portfolio share |
|---|---:|---:|
| 0 to 2 years | 20,000,000 | 33.3% |
| 3 to 5 years | 15,000,000 | 25.0% |
| 6+ years | 25,000,000 | 41.7% |

Reporting treatment:

- show debt maturity as property-level operating risk, not client loan exposure unless the client is directly liable;
- include floating/fixed-rate split and hedging where source-backed;
- label missing loan maturity or covenant data as incomplete;
- connect refinancing risk to income sustainability and valuation sensitivity.

## 11. Tenant Concentration

Scenario:

A property fund reports rental income concentration by tenant.

| Tenant | Annual rent |
|---|---:|
| Tenant A | 4,000,000 |
| Tenant B | 2,500,000 |
| Tenant C | 1,500,000 |
| Other tenants | 12,000,000 |

Calculation:

```text
total annual rent = 20,000,000
top tenant concentration = 4,000,000 / 20,000,000 = 20.0%
top three tenant concentration = (4,000,000 + 2,500,000 + 1,500,000) / 20,000,000 = 40.0%
```

Platform treatment:

- source tenant concentration from manager/property reports;
- classify concentration by tenant, sector, lease expiry and geography where available;
- use concentration in risk and mandate monitoring, not as accounting position data;
- hide or aggregate tenant names where confidentiality restrictions apply.

## 12. Occupancy Change And Income Impact

Scenario:

Occupancy falls from 95% to 88% in a property portfolio with potential gross rent of 30,000,000.

Calculation:

```text
prior occupied rent base = 30,000,000 x 95% = 28,500,000
current occupied rent base = 30,000,000 x 88% = 26,400,000
estimated annual rent reduction = 2,100,000
```

Reporting and analytics:

- occupancy is an operating metric, not a cash transaction by itself;
- income projections, valuation assumptions and distribution sustainability may change;
- valuation impact should be source-backed by appraisal, manager estimate or analyst model;
- report occupancy date and source quality.

## 13. Lease Expiry And Weighted Average Lease Expiry

Scenario:

A REIT reports lease expiries for three properties.

| Lease group | Annual rent | Years to expiry |
|---|---:|---:|
| Group A | 6,000,000 | 1 |
| Group B | 9,000,000 | 3 |
| Group C | 5,000,000 | 6 |

Calculation:

```text
WALE = sum(annual rent x years to expiry) / total annual rent
WALE = (6,000,000 x 1 + 9,000,000 x 3 + 5,000,000 x 6) / 20,000,000
WALE = 3.15 years
```

Correct treatment:

- disclose whether WALE is based on rent, area or another basis;
- connect near-term lease expiries to vacancy, renewal, rent reversion and valuation risk;
- use lease expiry buckets in DPM concentration and income-risk review where source-backed;
- label stale lease data before using it in client-ready reporting.

## 14. Infrastructure Concession Renewal

Scenario:

An infrastructure fund owns a toll-road concession expiring in eight years. Renewal is possible but not guaranteed.

| Attribute | Value |
|---|---|
| Remaining concession term | 8 years |
| Revenue model | Regulated toll revenue |
| Inflation linkage | Partial |
| Renewal status | Not yet confirmed |

Platform treatment:

- store concession expiry as an operating-risk field, not as a security maturity unless the instrument legally matures;
- distinguish contractual cashflow period from valuation assumption;
- report renewal uncertainty and regulatory dependency;
- avoid treating projected post-renewal cashflows as guaranteed income.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Renewal is unconfirmed | Reporting labels post-expiry assumptions as projected/uncertain. |
| Regulator changes tariff rules | Valuation and income assumptions carry source and effective date. |
| Concession expires | Exposure, valuation and income projections update from source event. |

## 15. Renewable Power-Price Exposure

Scenario:

An infrastructure vehicle owns renewable assets with part of generation under fixed power-purchase agreements and part exposed to merchant power prices.

| Revenue type | Share of expected revenue |
|---|---:|
| Fixed PPA | 70% |
| Merchant power price | 30% |

Risk treatment:

| Area | Correct treatment |
|---|---|
| Income stability | Fixed PPA portion is more contracted, subject to counterparty and contract terms. |
| Market exposure | Merchant portion is sensitive to power prices, curtailment and production volume. |
| Inflation | Indexation applies only if contract terms support it. |
| Reporting | Do not describe all renewable revenue as stable contracted income. |

QA assertions:

| Scenario | Expected behavior |
|---|---|
| PPA counterparty rating changes | Counterparty risk and income-quality labels update. |
| Merchant price source is stale | Revenue sensitivity is stale or blocked. |
| Production volume is missing | Power-price exposure is labelled partial. |

## 16. Direct-Property Ownership Transfer

Scenario:

A family holding company transfers a 40% ownership interest in a direct property to a trust.

| Attribute | Before | After |
|---|---:|---:|
| Holding company ownership | 100% | 60% |
| Trust ownership | 0% | 40% |
| Appraised property value | 5,000,000 | 5,000,000 |

Calculation:

```text
trust reportable value = 5,000,000 x 40% = 2,000,000
holding company reportable value = 5,000,000 x 60% = 3,000,000
```

Implementation treatment:

- transfer requires legal document, effective date, ownership percentage and valuation basis;
- do not duplicate 100% property value in both owner hierarchies;
- preserve historical reporting lineage before and after transfer;
- access control, beneficial ownership, tax reporting and collateral eligibility may change.

## 17. Advisory And Mandate Checklist

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

## 18. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed REIT | security position, trades, price, distributions, corporate actions, gearing where sourced | property-sector look-through and detailed operating-metric analytics |
| Private real estate fund | commitment, calls, NAV, distributions, valuation date, queues and holdbacks where sourced | advanced liquidity queue simulation and manager-level stress modelling |
| Direct property | ownership record, appraisal value, source date, documents, ownership percentage | document-backed ownership graph and property-level cashflow model |
| Infrastructure exposure | fund/security/private fund position, sector tags, concession and revenue model where sourced | advanced concession, inflation-linkage, offtake and regulatory-risk analytics |
| Reporting | wrapper, exposure, value, income, source date, liquidity label, operating metrics where sourced | consolidated real-asset income and stress dashboard |

## 19. Regression Test Pack

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
11. Property-level debt maturity ladder labels missing loan and covenant data.
12. Tenant concentration calculates top-tenant and top-three exposure with confidentiality controls.
13. Occupancy change updates analytics without booking a cash transaction.
14. Lease expiry WALE states basis and source date.
15. Infrastructure concession renewal separates legal term, valuation assumption and projected cashflow.
16. Renewable power-price exposure separates contracted and merchant revenue.
17. Direct-property ownership transfer updates ownership hierarchy without duplicating value.
