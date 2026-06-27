# 03 - Energy, Industrial Metals, Agriculture and Real-Asset Exposures

## 1. Why non-precious commodities matter

Private-bank clients usually access non-precious commodities through funds, ETPs, futures, structured products or commodity equities rather than physical delivery. These products are important for tactical macro views, inflation positioning, sector allocation and portfolio diversification.

They are also operationally complex because many returns are driven by futures curves, not only spot prices.

## 2. Energy commodities

| Product | Examples | Key drivers |
|---|---|---|
| Crude oil | Brent, WTI | OPEC policy, supply disruption, inventory, refining demand, geopolitics |
| Refined products | Gasoline, diesel, heating oil, jet fuel | Refining margins, seasonality, transport demand |
| Natural gas | Henry Hub, TTF, LNG-linked exposures | Weather, storage, pipeline/LNG capacity, regional supply-demand |
| Power | Electricity futures/swaps | Load demand, generation mix, weather, regulation |
| Emissions | EU allowances, carbon credits | Policy design, compliance demand, cap reductions |

### Energy-specific risks

| Risk | Explanation |
|---|---|
| Storage constraint | Oil/gas storage capacity can distort futures curves |
| Delivery location | WTI, Brent and regional gas contracts differ by delivery hub |
| Seasonality | Heating/cooling demand affects gas and power |
| Geopolitical risk | Energy markets react to sanctions, war, shipping and OPEC decisions |
| Negative pricing | Some contracts can trade below zero under extreme storage/delivery stress |
| Roll risk | Futures-based exposure may lose value in persistent contango |
| Regulatory/policy risk | Carbon, energy transition and price controls affect exposures |

## 3. Industrial metals

| Commodity | Common use | Main drivers |
|---|---|---|
| Copper | Electrical wiring, construction, renewables | Global growth, China, electrification, supply disruption |
| Aluminium | Packaging, transport, construction | Energy cost, smelter capacity, demand cycle |
| Nickel | Stainless steel, batteries | Battery demand, Indonesian supply, substitution |
| Zinc | Galvanization | Construction and industrial cycle |
| Iron ore | Steelmaking | China property/infrastructure, steel margins, seaborne supply |
| Coking coal | Steelmaking | Steel demand, supply disruption, environmental policy |

Industrial metals are often used as macro-cycle exposures. Copper is sometimes described as a global-growth barometer, but the relationship is not perfect and can be distorted by inventory, supply constraints and financial flows.

## 4. Agricultural commodities

| Commodity | Examples | Key drivers |
|---|---|---|
| Grains | Wheat, corn, soybeans | Weather, planting, harvest, inventory, export policy |
| Softs | Coffee, cocoa, sugar, cotton | Weather, disease, crop cycles, consumer demand |
| Livestock | Cattle, hogs | Feed cost, disease, herd cycles |

Agricultural commodities introduce risks that are less common in financial products:

- weather shocks;
- crop disease;
- planting/harvest seasonality;
- government export/import controls;
- transport bottlenecks;
- basis differences by region and quality grade.

## 5. Commodity index exposures

A broad commodity index aims to represent a basket of commodity futures, often across energy, metals and agriculture.

Important design fields:

| Field | Meaning |
|---|---|
| index_name | Commodity benchmark/index |
| basket_components | Underlying futures/contracts |
| weights | Component allocation |
| roll_method | How expiring contracts are replaced |
| collateral_assumption | How collateral return is included |
| rebalancing_frequency | Monthly/quarterly/annual |
| return_type | Price return / excess return / total return |
| sector_caps | Controls energy or single-commodity dominance |

Return type matters:

| Return type | Meaning |
|---|---|
| Spot/price return | Movement of commodity price only |
| Excess return | Futures price movement plus roll effect |
| Total return | Excess return plus collateral return |

A client may think they are buying “commodity prices,” but the product may track an excess-return or total-return futures index.

## 6. Commodity equities and real-asset equities

Commodity-linked equities include miners, oil companies, energy producers, agriculture companies, infrastructure operators and related funds.

They should usually be classified as equities for position modelling, but may have commodity look-through exposure for analytics.

| Exposure | Position type | Important caveat |
|---|---|---|
| Gold miner | Equity | Equity risk + gold sensitivity |
| Oil major | Equity | Energy price + business + dividend + transition risk |
| Copper miner fund | Fund/equity | Mining risk, country risk, operational leverage |
| REIT | Equity/fund/real estate | Real estate income and rate sensitivity, not commodity |
| Infrastructure fund | Fund/private market | Contracted cashflows, regulation, illiquidity |

## 7. Real assets versus commodities

Real assets are broader than traded commodities.

| Category | Examples | Difference from commodities |
|---|---|---|
| Commodities | Gold, oil, copper, wheat | Raw materials traded in spot/futures markets |
| Real estate | Buildings, REITs, property funds | Income-producing physical assets |
| Infrastructure | Toll roads, utilities, airports | Long-life operating assets |
| Natural resources | Timberland, farmland, mining rights | Productive real assets with commodity linkage |
| Collectibles | Art, wine, watches | Alternative real assets, often illiquid and subjective valuation |

In wealth analytics, do not blindly place all real assets in “commodities.” A gold ETF, an infrastructure fund and a REIT have very different cashflow, risk, valuation and reporting behavior.

## 8. Product access by commodity family

| Family | Physical | ETP | Futures/options | OTC derivatives | Structured notes | Equity/fund proxy |
|---|---|---|---|---|---|---|
| Gold | Common | Common | Common | Common | Common | Gold miners |
| Silver | Common | Common | Common | Common | Common | Silver miners |
| Oil | Not practical for wealth | Common but complex | Common | Common | Common | Energy equities |
| Natural gas | Not practical | Available but complex | Common | Common | Limited | Energy equities |
| Copper | Not common | Available | Common | Common | Available | Miners |
| Iron ore | Not practical | Limited | Institutional | Common in Asia | Institutional | Miners/steel |
| Agriculture | Not practical | Available | Common | Institutional | Limited | Agri funds |
| Carbon | Not physical retail | Available in some markets | Available | Institutional | Emerging | Environmental funds |

## 9. Advisory message

For non-precious commodities, advisors should be careful to distinguish:

- spot commodity view;
- futures curve return;
- commodity equity exposure;
- ETP/ETN issuer or fund structure;
- leverage/margin effects;
- liquidity and volatility;
- client understanding of roll yield and product mechanics.

A client can be right about oil rising but still underperform if they used the wrong futures-based product during steep contango or paid large spreads and fees.
