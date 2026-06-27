# 06 — Data Model, Valuation, NAV, Income, Performance, Risk and Analytics

## 1. Data model overview

Real estate and infrastructure require a model that supports security data, fund data, private-market data and property-level data.

```text
Instrument / Product
  ├── Wrapper attributes
  ├── Real asset exposure attributes
  ├── Income and distribution attributes
  ├── Leverage/debt attributes
  ├── Valuation/NAV/appraisal records
  ├── Look-through holdings or property exposures
  ├── Lifecycle events
  └── Transactions and positions
```

## 2. Common product fields

| Field | Description |
|---|---|
| instrument_id | Unique product/security ID |
| product_type | REIT, property stock, ETF, private fund, direct property, infrastructure fund |
| wrapper_type | Listed security, fund, private fund, direct asset, trust, structured note |
| asset_class | Real Estate, Infrastructure, Alternatives, Equity, Fund, Private Markets |
| issuer/manager/sponsor | REIT manager, fund manager, sponsor |
| domicile | Legal domicile |
| listing_status | Listed/unlisted/private/direct |
| exchange | If listed |
| trading_currency | Market trading currency |
| valuation_currency | Reporting/NAV currency |
| distribution_frequency | Monthly/quarterly/semi-annual/annual/ad hoc |
| liquidity_terms | Daily, periodic, lock-up, gated, illiquid |
| eligible_client_segment | Retail/professional/accredited/qualified |
| complexity_flag | Simple/complex/SIP/alternative |
| collateral_eligible | Yes/no |
| default_haircut | Lending haircut if used |

## 3. Exposure fields

| Field | Example |
|---|---|
| real_asset_sector | Real estate / infrastructure / natural resources |
| property_type | Retail / office / logistics / residential |
| infrastructure_type | Utility / transport / renewables / digital |
| geography | Singapore, Australia, Europe, US |
| lease_contract_type | Fixed rent, CPI-linked, revenue-linked |
| tenant_counterparty_quality | Investment grade / diversified / concentrated |
| occupancy | 95% |
| WALE / lease maturity | 4 years |
| leverage_pct | 35% |
| debt_maturity_profile | 2026/2027/2028 etc. |
| fixed_rate_debt_pct | % debt fixed/hedged |
| development_exposure_pct | % assets under development |
| appraisal_frequency | Quarterly/annual |

## 4. Valuation hierarchy

| Level | Product | Valuation source |
|---|---|---|
| Level 1 | Listed REIT/property security | Exchange price |
| Level 1/2 | REIT ETF | Exchange price and/or NAV |
| Level 2 | Listed fund with less liquidity | Market maker / NAV / evaluated price |
| Level 2/3 | Non-traded REIT | Manager NAV / redemption price / evaluated NAV |
| Level 3 | Private real estate fund | Manager NAV/appraisal |
| Level 3 | Direct property | Appraisal/model/manual valuation |
| Level 3 | Infrastructure fund | DCF/appraisal/manager NAV |

## 5. Listed REIT valuation

```text
Market Value = Units × Market Price × FX Rate
```

Additional analytics:

```text
Distribution Yield = Annualised Distribution per Unit / Market Price
Price-to-NAV = Market Price / NAV per Unit
Premium/Discount to NAV = (Market Price - NAV per Unit) / NAV per Unit
```

## 6. Private fund NAV valuation

```text
Client NAV = Fund NAV per unit/share × Client units
```

or for commitment-style funds:

```text
Residual Value = Reported NAV
Total Value = Residual Value + Cumulative Distributions
Unfunded Exposure = Commitment - Called Capital + Recallable Capital Adjustments
```

Important dates:

| Date | Meaning |
|---|---|
| NAV date | Economic valuation date |
| received date | Date platform received NAV |
| booking date | Date NAV posted |
| effective date | Date used for performance/portfolio reporting |
| stale date threshold | When NAV becomes too old for normal reporting confidence |

## 7. Direct property valuation

```text
Gross Client Property Value = Property Appraisal Value × Ownership Percentage
Net Client Property Equity = Gross Client Property Value - Linked Debt Share
```

Where property is collateral, apply haircut:

```text
Collateral Value = Appraisal Value × Ownership % × Eligible LTV / Haircut Policy
```

## 8. Income analytics

| Income source | Product |
|---|---|
| REIT distributions | Listed REITs |
| Fund distributions | Private/public funds |
| Rental income | Direct property |
| Availability/concession income | Infrastructure |
| Interest income | Real estate debt/infrastructure debt |
| Capital gain distribution | Fund/REIT asset sale |
| Return of capital | Fund/REIT capital return |

Do not combine all of these blindly as “dividend.” Client reporting should distinguish:

- recurring income;
- non-recurring distribution;
- return of capital;
- realised gain distribution;
- tax withheld;
- income reinvested;
- gross versus net income.

## 9. Performance measurement

### Listed REITs

Use normal security performance:

```text
Total Return = Price Return + Distribution Return
```

Breakdown:

- price return;
- income return;
- FX return;
- fees/tax impact;
- corporate-action impact.

### Private real estate/infrastructure funds

Use both TWR and money-weighted metrics.

| Metric | Meaning |
|---|---|
| TWR | Portfolio manager performance excluding external cashflow timing |
| IRR/MWR | Investor cashflow return including calls/distributions |
| TVPI | Total value / paid-in capital |
| DPI | Distributions / paid-in capital |
| RVPI | Residual NAV / paid-in capital |
| Income yield | Distributions classified as income / NAV or cost |
| Commitment utilisation | Called capital / commitment |

### Direct property

Performance should separate:

- rental income;
- operating expenses;
- financing costs;
- capital expenditure;
- valuation changes;
- FX changes;
- realised sale gain/loss.

## 10. Risk analytics

| Risk | Analytics |
|---|---|
| Market risk | Price volatility, beta, drawdown for listed REITs |
| Interest-rate risk | Rate sensitivity, debt reset profile, duration-like behaviour |
| Leverage risk | Gearing, LTV, debt maturity, interest coverage |
| Liquidity risk | Trading volume, redemption terms, lock-up, gates |
| Valuation risk | NAV lag, appraisal age, valuation confidence |
| Concentration risk | Property type, geography, tenant, sponsor, manager |
| Income risk | Distribution coverage, occupancy, lease rollover |
| FX risk | Asset currency vs client/reporting currency |
| Regulatory risk | REIT rules, rent controls, concessions, tax changes |
| Climate/physical risk | Flood, heat, storm, transition risk |
| Development risk | Capex, construction delays, vacancy risk |
| Counterparty risk | Tenant/offtaker/concession grantor quality |

## 11. Look-through analytics

A REIT/fund/direct property should support exposure breakdown.

| Look-through dimension | Example |
|---|---|
| Geography | Singapore 40%, Australia 30%, US 30% |
| Property type | Logistics 70%, office 30% |
| Tenant sector | Technology, retail, healthcare |
| Lease maturity | <1Y, 1-3Y, 3-5Y, >5Y |
| Debt maturity | Year buckets |
| Currency | SGD, USD, AUD, EUR |
| ESG/climate | Energy intensity, green certifications |
| Development exposure | Stabilised vs under development |

## 12. Valuation quality flags

| Flag | Meaning |
|---|---|
| MARKET_PRICE | Exchange price available |
| MANAGER_NAV_CURRENT | Current NAV received |
| MANAGER_NAV_LAGGED | NAV available but lagged |
| APPRAISAL_CURRENT | Recent appraisal |
| APPRAISAL_STALE | Appraisal stale |
| MANUAL_VALUATION | Manually entered value |
| NO_RECENT_VALUATION | Use caution in reporting |
| SIDE_POCKET_OR_GATED | Liquidity/valuation restricted |
| SUSPENDED | Trading/redemption suspended |

## 13. Reporting analytics table

| Report section | Metrics to show |
|---|---|
| Overview | Market value/NAV, allocation %, unrealised P&L |
| Income | Distributions, rental income, yield, tax withheld |
| Risk | leverage, liquidity, concentration, valuation confidence |
| Performance | price return, income return, FX return, IRR for private |
| Liquidity | listed/liquid, gated, locked-up, redemption terms |
| Exposure | property type, geography, currency, manager/sponsor |
| Mandate | eligible/ineligible, limit usage, breach reason |
| Advisory | client objective match, risks, scenario notes |

## 14. Implementation principle

The valuation engine should not hide valuation quality. A daily portfolio report may need to combine daily REIT prices with quarterly private property NAVs. The report must show valuation date and confidence so users understand that the total portfolio value mixes different valuation frequencies.
