# 03 — Private Real Estate Funds, Direct Property and Property-Linked Vehicles

## 1. Private real estate fund overview

Private real estate funds invest in property assets through private fund vehicles. They may buy stabilised buildings, develop assets, redevelop assets, provide property debt, or pursue opportunistic strategies.

They should be modelled similarly to private-market funds, not daily-traded public securities.

Core mechanics:

- commitment;
- capital calls;
- distributions;
- NAV updates;
- appraisal valuation;
- leverage at fund/property level;
- lock-up;
- limited secondary liquidity;
- fund life and extension periods;
- property-level exposure reporting.

## 2. Real estate investment strategies

| Strategy | Description | Risk/return profile | Key platform analytics |
|---|---|---|---|
| Core | Stabilised, high-quality income assets | Lower risk, income-oriented | NAV, income yield, occupancy, leverage |
| Core-plus | Mostly stabilised assets with some improvement | Moderate risk | Income + modest appreciation |
| Value-add | Repositioning, leasing, refurbishment | Higher risk | Capex, lease-up, valuation uplift |
| Opportunistic | Development/distressed/high leverage | Highest risk | J-curve, project risk, write-downs |
| Real estate debt | Loans secured by property | Credit/income | LTV, covenants, collateral value |
| Development fund | Build new properties | Construction/market risk | Milestones, drawdowns, completion risk |
| Sector specialist | Data centres/logistics/hospitality etc. | Sector-specific | Sector concentration and tenant risk |
| Fund of funds | Invests in multiple property funds | Diversified but layered fees | Look-through, fee layering, NAV lag |
| Co-investment | Deal-specific exposure | Concentrated | Deal-level risk and valuation |
| Secondaries | Buys existing fund interests | Earlier cashflows, pricing risk | Secondary discount/premium vs NAV |

## 3. Private real estate fund lifecycle

| Stage | Description | System object |
|---|---|---|
| Offering | Client reviews PPM, strategy, fees, risks | Product/offering |
| Subscription | Client commits capital | Commitment record |
| Closing | Client accepted into fund | Active commitment |
| Capital calls | Fund requests capital | Capital call event + cash transaction |
| Asset acquisition | Fund buys property | Usually not direct client transaction; look-through update |
| NAV reporting | Manager reports NAV | Valuation/NAV record |
| Income distribution | Rental income distributed | Distribution transaction |
| Return of capital | Sale/refinancing proceeds returned | Distribution component |
| Recallable capital | Distribution can be recalled | Commitment adjustment |
| Extension | Fund life extended | Lifecycle event |
| Secondary transfer | Interest sold/transferred | Sale/transfer transaction |
| Liquidation | Fund exits assets and winds up | Final distributions + close position |

## 4. Position model for private real estate funds

| Field | Meaning |
|---|---|
| commitment_amount | Total client commitment |
| called_capital | Cumulative capital called |
| unfunded_commitment | Commitment minus called capital plus recallable adjustments |
| contributed_capital | Capital contributed by client |
| distributed_capital | Cumulative distributions |
| recallable_distributions | Capital that can be called again |
| remaining_nav | Current reported NAV/residual value |
| valuation_date | Date of NAV report |
| valuation_received_date | Date bank received NAV |
| nav_lag_days | Reporting lag |
| vintage_year | Fund vintage |
| fund_life_stage | Investment, harvest, extension, liquidation |
| strategy | Core/core-plus/value-add/opportunistic |
| property_type_exposure | Office/retail/logistics etc. |
| geographic_exposure | Country/region |
| leverage_pct | Fund/property leverage |
| liquidity_terms | Lock-up, transfer restrictions |

## 5. Direct property exposure

Direct property may not be custodied by the bank but can appear in client wealth reporting as a declared asset, advisory asset, collateral asset or family-office reporting asset.

### Direct property attributes

| Attribute | Example |
|---|---|
| property_id | PROP123 |
| property_type | Residential/commercial/industrial |
| address/country/city | Singapore, London, Sydney |
| ownership_pct | 50% |
| valuation_amount | SGD 3,000,000 |
| valuation_date | 2026-06-30 |
| valuation_method | Appraisal/market estimate/purchase price |
| rental_income | Monthly/annual rent |
| expenses | Taxes, maintenance, insurance, management fees |
| mortgage_outstanding | Debt linked to property |
| net_equity_value | Property value less debt |
| liquidity_class | Illiquid |
| collateral_eligible | Yes/no |
| haircut/LTV | If used for lending |

### Direct property transaction types

| Transaction type | Use |
|---|---|
| PROPERTY_PURCHASE | Property acquired |
| PROPERTY_SALE | Property sold |
| PROPERTY_VALUATION_UPDATE | Appraisal/market value update |
| PROPERTY_RENTAL_INCOME | Rent received |
| PROPERTY_EXPENSE | Maintenance/tax/insurance expense |
| PROPERTY_MORTGAGE_DRAW | Mortgage/loan drawdown |
| PROPERTY_MORTGAGE_REPAYMENT | Principal repayment |
| PROPERTY_INTEREST_PAYMENT | Loan interest |
| PROPERTY_CAPEX | Renovation/improvement |
| PROPERTY_OWNERSHIP_ADJUSTMENT | Ownership percentage change |
| PROPERTY_TRANSFER_IN/OUT | Reporting/custody transfer |

## 6. Property-linked vehicles

| Vehicle | Description | Modelling approach |
|---|---|---|
| Fractional property platform | Small interest in property asset | Private/direct asset or fund-like position |
| Club deal | Group investment in one property | Private deal/alternative asset |
| SPV | Company holding property | Private equity/direct asset wrapper |
| Real estate certificate | Securitised claim | Security position with property exposure |
| Property-backed note | Note backed/linked to property | Structured note or credit instrument |
| Mortgage fund | Fund lending against property | Private credit/real estate debt |

## 7. Valuation of private/direct real estate

| Valuation method | Use | Weakness |
|---|---|---|
| Independent appraisal | Common for private/direct property | Periodic and subjective |
| Manager NAV | Private funds | Lagged and manager-dependent |
| Discounted cashflow | Income-producing assets | Sensitive to assumptions |
| Comparable sales | Residential/commercial market | Requires relevant transactions |
| Cap-rate method | Income property | Sensitive to cap-rate estimate |
| Cost method | Development/special assets | May not reflect market demand |
| Broker estimate | Informal reporting | Low control/reliability |

## 8. Real estate performance metrics

| Metric | Meaning |
|---|---|
| Net operating income | Rental income minus operating expenses |
| Cap rate | NOI / property value |
| Occupancy | Leased space / rentable space |
| Loan-to-value | Debt / property value |
| Debt service coverage | Cashflow / debt service |
| IRR | Money-weighted return from cashflows and residual value |
| Equity multiple | Total value / paid-in capital |
| Distribution yield | Annual distributions / NAV or cost |
| Appraisal gain/loss | Change in valuation |
| Income return | Income component of return |
| Capital return | Valuation/sale gain component |

## 9. Platform design principle

Private/direct property should have a more flexible valuation and cashflow model than listed securities.

```text
Public REIT = security position + market price + distributions
Private real estate fund = commitment + calls + NAV + distributions + lag
Direct property = asset value + rental income + expenses + debt + appraisal updates
```
