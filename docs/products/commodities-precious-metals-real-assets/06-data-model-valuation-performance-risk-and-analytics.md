# 06 - Data Model, Valuation, Performance, Risk and Analytics

## 1. Canonical data model

The model should separate:

1. legal instrument/wrapper;
2. underlying commodity exposure;
3. position and quantity basis;
4. valuation method;
5. lifecycle events;
6. look-through analytics;
7. custody/collateral details.

## 2. Core instrument fields

| Field | Example |
|---|---|
| instrument_id | XAU_ALLOC_ZRH / GLD US / CLZ6 |
| security_type | COMMODITY / ETF / ETN / FUTURE / OPTION / NOTE |
| product_family | COMMODITY / PRECIOUS_METAL / REAL_ASSET |
| wrapper_type | PHYSICAL / ALLOCATED / UNALLOCATED / ETF / ETC / ETN / FUTURE / OPTION / SWAP / STRUCTURED_PRODUCT |
| commodity_id | XAU / WTI / COPPER |
| commodity_family | Precious metal / energy / industrial metal / agriculture |
| currency | USD / SGD / etc. |
| unit_of_measure | oz / gram / barrel / tonne / unit |
| price_quote_unit | USD per oz / USD per barrel |
| listing_status | Listed / unlisted / OTC |
| exchange | CME / ICE / SGX / LME / etc. |
| issuer_id | ETN/certificate issuer |
| custodian_id | Vault/custodian for physical |
| derivative_flag | true/false |
| leverage_factor | 1x / 2x / inverse |
| settlement_type | Cash / physical |
| complexity_class | Simple / complex / SIP |

## 3. Commodity master

| Field | Description |
|---|---|
| commodity_id | Unique commodity identifier |
| commodity_name | Gold, Brent crude, copper |
| commodity_family | Precious metal, energy, metal, agriculture |
| standard_unit | oz, barrel, tonne, bushel |
| quality_grade | Grade/specification |
| benchmark_price | LBMA Gold Price, Brent, LME Copper, etc. |
| benchmark_currency | USD |
| market_calendar | Relevant trading calendar |
| delivery_locations | For deliverable contracts |
| price_sources | Vendor/exchange/benchmark/provider |
| risk_factor_id | Risk model factor |

## 4. Valuation by wrapper

| Wrapper | Valuation method | Key source |
|---|---|---|
| Physical metal | Quantity × benchmark/bid price × FX | LBMA/exchange/bank quote/vendor |
| Allocated metal | Same as physical, with custody adjustments | Custodian + benchmark |
| Unallocated metal | Quantity × provider bid/offer price | Provider/bank quote |
| Commodity ETF | Exchange price; NAV/iNAV for validation | Exchange + fund data |
| ETC/ETN | Exchange price; issuer indicative value | Exchange + issuer |
| Futures | Exchange settlement price × contract size | Exchange settlement |
| Option | Exchange price or option model | Exchange/model |
| OTC swap/forward | Mark-to-market model | Curves/forwards/counterparty |
| Structured product | Issuer bid/evaluated/model price | Issuer/vendor/model |
| Commodity equity/fund | Exchange price or NAV | Exchange/fund admin |

## 5. Physical metal valuation example

| Input | Value |
|---|---:|
| Quantity | 100 oz |
| Gold price | USD 2,300/oz |
| FX USD/SGD | 1.35 |
| Storage fee accrual | SGD 200 |

```text
Market Value in USD = 100 × 2,300 = USD 230,000
Market Value in SGD = 230,000 × 1.35 = SGD 310,500
Custody-adjusted Value = 310,500 - 200 = SGD 310,300
```

## 6. Futures valuation example

| Input | Value |
|---|---:|
| Contracts | 2 |
| Contract size | 100 oz |
| Futures price | USD 2,320/oz |
| Previous settlement | USD 2,300/oz |

```text
Notional Exposure = 2 × 100 × 2,320 = USD 464,000
Daily Variation Margin = 2 × 100 × (2,320 - 2,300) = USD 4,000
```

The futures position may have large notional exposure while requiring a smaller margin amount. This is why exposure and cash margin must be reported separately.

## 7. Futures-based product return decomposition

For a long commodity futures strategy:

```text
Total Return = Spot Price Return + Roll Yield + Collateral Return - Fees - Tracking Error
```

| Component | Meaning |
|---|---|
| Spot return | Movement in commodity price |
| Roll yield | Gain/loss from rolling expiring contracts |
| Collateral return | Interest earned on collateral backing futures exposure |
| Fees | Product expense, transaction cost, manager fee |
| Tracking error | Difference versus benchmark/index |

## 8. Roll yield

Roll yield is central to commodity analytics.

### Contango

If the next futures contract is more expensive than the expiring contract, a long strategy may sell cheaper near contracts and buy more expensive next contracts. This creates negative roll yield.

### Backwardation

If the next futures contract is cheaper than the expiring contract, a long strategy may sell higher-priced near contracts and buy lower-priced next contracts. This creates positive roll yield.

Reporting should show, where possible:

| Metric | Purpose |
|---|---|
| Spot return | Explains commodity price movement |
| Roll return | Explains futures curve effect |
| Collateral return | Explains cash collateral yield |
| Product fees | Explains expense drag |
| Residual/tracking error | Captures implementation difference |

## 9. Performance treatment

| Product | Income | P&L driver | Performance issue |
|---|---|---|---|
| Physical gold | None | Price + FX - fees | Fees and spreads matter |
| Gold ETF | Usually none or minimal | Price/NAV change | Expense ratio embedded |
| Futures | Variation margin | Futures price movement + roll | Margin cashflows vs P&L classification |
| Options | Option price | Delta/gamma/vega/time decay | Expiry/exercise events |
| Commodity note | Coupon + price | Payoff and issuer valuation | Coupon classification and barriers |
| Commodity equity | Dividends possible | Equity + commodity sensitivity | Not pure commodity exposure |

## 10. Risk analytics

| Risk metric | Use |
|---|---|
| Volatility | Commodity price fluctuation |
| VaR/CVaR | Downside loss estimate |
| Drawdown | Peak-to-trough loss |
| Beta to commodity index | Sensitivity to commodity benchmark |
| Correlation | Diversification with equities/bonds |
| Tracking error | Product versus benchmark |
| Roll risk | Risk from curve shape changes |
| Basis risk | Difference between product and reference commodity |
| Liquidity score | Exit risk and spread impact |
| Counterparty/issuer exposure | ETN, unallocated metal, OTC swap, note |
| Leverage exposure | Futures/options/leveraged ETPs |
| Collateral exposure | Margin and pledged assets |
| Concentration | Commodity, sector, issuer, custodian, provider |

## 11. Look-through exposure

A gold ETF, gold future and gold-linked note may all create gold exposure, but reporting should show both legal position and economic exposure.

| Legal position | Look-through exposure |
|---|---|
| Gold ETF | Gold spot/bullion exposure |
| Gold future | Gold futures notional exposure |
| Gold ETN | Gold index exposure + issuer credit |
| Gold-linked note | Conditional gold payoff + issuer credit |
| Gold miner equity | Equity exposure + gold sensitivity |

## 12. Portfolio analytics dimensions

Recommended analytics dimensions:

| Dimension | Examples |
|---|---|
| Asset class | Commodities / Real assets / Alternatives |
| Commodity family | Precious metals, energy, industrial metals, agriculture |
| Commodity | Gold, oil, copper, etc. |
| Wrapper | Physical, ETF, ETN, futures, option, structured product |
| Liquidity bucket | Daily, exchange-traded, issuer bid, OTC, physical delivery |
| Counterparty/issuer | Provider, ETN issuer, OTC counterparty |
| Custody location | Zurich, London, Singapore |
| Currency | USD, SGD, EUR |
| Exposure type | Direct, synthetic, leveraged, inverse, equity proxy |
| Mandate eligibility | Allowed/restricted/prohibited |

## 13. Reporting measures

| Measure | Description |
|---|---|
| Market value | Value in portfolio currency |
| Notional exposure | Especially for derivatives |
| Delta-adjusted exposure | Options and nonlinear products |
| Commodity family exposure | Precious metals/energy/metals/agriculture |
| Wrapper exposure | ETF/futures/physical/note |
| Pledged value | Collateralized amount |
| Available value | Unencumbered value |
| Unrealized P&L | Market value less cost |
| Realized P&L | Closed gains/losses |
| Income | Usually limited; distributions/coupons only |
| Fees | Storage, expense ratio, trading, margin financing |
| Roll contribution | Futures-based products |
| FX contribution | Non-base-currency exposure |

## 14. Edge analytics

Advanced platform capabilities:

- separate physical return from FX return;
- separate futures spot return from roll return;
- show futures margin versus notional exposure;
- show ETN issuer concentration separately from commodity exposure;
- track storage location and custody risk for physical assets;
- expose collateral eligibility and haircut values;
- stress test commodity price shocks;
- stress test USD and real-rate shocks for gold;
- stress test oil/gas shocks and roll curve shifts;
- identify if commodity exposure is achieved through equity proxy rather than direct commodity.
