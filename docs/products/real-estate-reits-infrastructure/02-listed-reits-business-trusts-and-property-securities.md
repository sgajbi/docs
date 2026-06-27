# 02 — Listed REITs, Business Trusts and Property Securities

## 1. Listed REIT overview

Listed REITs are exchange-traded units in a trust or company that owns or finances income-producing real estate. Operationally, a listed REIT trade resembles an equity trade: buy, sell, settle, receive distributions and process corporate actions.

Analytically, it is not enough to treat a REIT as a normal equity. A REIT has property-level economics:

- property type;
- geography;
- rental income;
- occupancy;
- lease maturity;
- sponsor quality;
- leverage;
- interest-rate exposure;
- distribution policy;
- net asset value;
- potential property revaluation gains/losses.

## 2. Listed REIT product attributes

| Attribute | Example | Why it matters |
|---|---|---|
| Exchange | SGX, NYSE, HKEX | Trading, settlement, market price |
| Currency | SGD, USD, HKD | Valuation and FX exposure |
| REIT sector | Industrial, retail, office, data centre | Sector concentration |
| Geography | Singapore, Australia, US, Europe | Country/property risk |
| Sponsor | Developer/asset manager | Pipeline and governance |
| Manager | REIT manager | Execution quality |
| Distribution frequency | Quarterly/semi-annual | Income projection |
| Distribution yield | 5% etc. | Income analysis, not guaranteed |
| NAV per unit | Book/appraised asset value per unit | Premium/discount analysis |
| Gearing | Debt/assets | Leverage and refinancing risk |
| Interest coverage | Income/debt service | Credit resilience |
| Occupancy | 95% | Rental stability |
| Weighted average lease expiry | 4 years | Lease rollover risk |
| Development exposure | Yes/no | Growth/risk |
| Tax treatment | Jurisdiction-specific | Net distribution to client |

## 3. REIT sectors

| Sector | Typical assets | Key risks |
|---|---|---|
| Retail | Malls, retail parks | Consumer demand, tenant sales, e-commerce |
| Office | Office buildings | Work-from-home, vacancy, tenant quality |
| Industrial/logistics | Warehouses, logistics parks | Trade, e-commerce, supply chain |
| Data centre | Data centre facilities | Power cost, tenant concentration, technology cycle |
| Hospitality | Hotels, serviced residences | Travel cycle, occupancy, room rates |
| Healthcare | Hospitals, clinics, senior housing | Regulation, operator quality, demographics |
| Residential | Apartments, student housing, multifamily | Affordability, regulation, vacancy |
| Self-storage | Storage facilities | Local demand, competition |
| Diversified | Mix of property types | Mixed drivers, diversification may hide weak assets |

## 4. Listed REIT lifecycle

| Lifecycle event | Platform object | Transaction? | Comments |
|---|---|---|---|
| Order placement | Order | No | Pre-trade suitability/concentration check |
| Trade execution | Trade | Yes | Equity-like buy/sell |
| Settlement | Settlement status/cash movement | Usually linked to trade | T+ market convention |
| Distribution declaration | Corporate action/income event | No initially | Record/ex-date/pay date captured |
| Distribution payment | Cash transaction | Yes | Income, return of capital or mixed classification |
| Dividend reinvestment plan | Corporate action | Yes | Cash distribution plus reinvestment or stock distribution |
| Rights issue | Corporate action | Maybe | Entitlement, exercise, lapse or sale of rights |
| Preferential offering | Corporate action | Yes if subscribed | Common for REIT equity raising |
| Unit consolidation/split | Corporate action | Yes | Quantity change, cost basis adjustment |
| Merger/acquisition | Corporate action | Yes | Cash/stock consideration |
| Delisting | Corporate action | Yes/position status | Liquidity and valuation changes |

## 5. Transaction types for listed REITs

| Transaction type | Use | Quantity impact | Cash impact |
|---|---|---:|---:|
| REIT_BUY | Exchange/secondary purchase | Increase units | Cash out |
| REIT_SELL | Exchange/secondary sale | Decrease units | Cash in |
| REIT_DISTRIBUTION_INCOME | Ordinary distribution/income | No unit change | Cash in |
| REIT_RETURN_OF_CAPITAL | Distribution classified as capital return | Usually no unit change | Cash in; cost basis may reduce |
| REIT_DISTRIBUTION_REINVESTMENT | Reinvested distribution | Increase units | Cash in/out or net zero |
| REIT_RIGHTS_ENTITLEMENT | Rights credited | Create rights instrument | No cash |
| REIT_RIGHTS_EXERCISE | Exercise rights | Increase REIT units | Cash out |
| REIT_RIGHTS_SALE | Sell rights | Reduce rights | Cash in |
| REIT_RIGHTS_LAPSE | Unexercised rights expire | Remove rights | No cash |
| REIT_UNIT_SPLIT | Split/consolidation | Quantity changes | No cash |
| REIT_MERGER_CASH | Cash consideration | Reduce units | Cash in |
| REIT_MERGER_STOCK | Stock/unit consideration | Remove old, add new | Usually no cash |
| REIT_TENDER_ACCEPTANCE | Accept offer | Reduce units | Cash/stock in |
| REIT_TRANSFER_IN/OUT | Custody transfer | Quantity movement | Usually no cash |
| REIT_FEE/TAX | Fees/tax | No unit change | Cash out |

## 6. Distribution classification

REIT distributions may contain different tax/accounting components depending on jurisdiction and reporting source. For platform reporting, avoid blindly treating every distribution as ordinary income.

| Component | Reporting treatment |
|---|---|
| Ordinary income | Investment income |
| Tax-exempt income | Income, with tax classification |
| Capital gain distribution | Realised gain/income classification depending policy |
| Return of capital | Cash received but may reduce cost basis |
| Foreign withholding tax | Tax cashflow or net distribution adjustment |
| Reinvestment | Income plus buy, or synthetic reinvestment transaction |

## 7. Business trusts and stapled securities

Business trusts may hold operating businesses rather than pure property rental assets. Stapled securities may combine units/shares that trade together.

Platform implications:

- security master must support stapled identifiers and component securities if needed;
- distribution components may differ from REITs;
- leverage and operating risk may be higher;
- income may not have the same regulatory distribution requirement as REITs;
- classification may be infrastructure, real asset, equity income, alternatives or trust depending mandate.

## 8. Listed property companies

A property company or developer is not the same as a REIT.

| REIT | Property company/developer |
|---|---|
| Often income/distribution oriented | Often earnings/development oriented |
| Owns income-producing assets | May develop, sell and manage property |
| Trust distribution rules may apply | Corporate dividend policy applies |
| NAV/premium-discount commonly watched | Earnings/book value and development pipeline watched |
| Leverage/property metrics important | Corporate leverage and project risk important |

## 9. REIT ETFs and funds

A REIT ETF should be modelled as a fund/ETF wrapper, not as multiple direct REIT positions, unless look-through is available.

| Layer | System treatment |
|---|---|
| Accounting position | ETF/fund units |
| Valuation | Market price or NAV |
| Income | ETF distributions |
| Look-through | Underlying REIT sector/country exposure if available |
| Mandate | Fund/ETF eligibility plus real estate exposure limits |

## 10. Key analytics for listed REITs

| Metric | Meaning |
|---|---|
| Distribution yield | Annualised distribution / market price |
| Dividend/distribution growth | Income trend |
| Price-to-NAV | Market price versus NAV per unit |
| Gearing | Borrowing relative to assets |
| Interest coverage | Ability to service debt |
| Fixed-rate debt ratio | Exposure to interest-rate reset |
| Debt maturity profile | Refinancing risk |
| Occupancy | Space leased |
| WALE | Weighted average lease expiry |
| Tenant concentration | Exposure to top tenants |
| Sector/geographic exposure | Real estate concentration |
| Premium/discount to NAV | Valuation sentiment |

## 11. Common platform mistakes

| Mistake | Better approach |
|---|---|
| Treat all REIT distributions as dividends | Capture income/return-of-capital/tax components where available |
| Treat REITs as ordinary equities only | Add property sector, gearing, yield and rate sensitivity analytics |
| Ignore rights issues | Rights are common in REIT capital raising and affect dilution/cost basis |
| Ignore currency exposure | Many REITs own foreign assets or trade in different currency |
| Use yield as risk-free income | Distributions can be reduced and high yield may signal risk |
| Ignore leverage | Gearing and refinancing risk are central to REIT analysis |
