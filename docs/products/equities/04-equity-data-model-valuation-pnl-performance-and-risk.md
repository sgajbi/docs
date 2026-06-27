# 04 - Equity Data Model, Valuation, P&L, Performance and Risk

## 1. Design goal

An equity platform should support:

- instrument master and issuer master;
- multiple listings and identifiers;
- market data and FX;
- transactions and lots;
- settled and unsettled positions;
- corporate actions and elections;
- valuation and P&L;
- income and tax;
- performance measurement;
- risk and concentration analytics;
- reconciliation and audit.

The model should be accounting-grade for positions and transactions, but also rich enough for advisory, portfolio analytics and client reporting.

## 2. Core data model

Recommended high-level model:

```text
Issuer
  +-- Instrument
        +-- Listing[]
        +-- Identifier[]
        +-- CorporateAction[]
        +-- MarketPrice[]
        +-- RiskClassification

Portfolio
  +-- Transaction[]
  |     +-- TransactionLeg[]
  +-- Position[]
  |     +-- PositionLot[]
  +-- Valuation[]
  +-- PerformanceReturn[]
  +-- RiskExposure[]
```

## 3. Issuer master

| Field | Meaning |
|---|---|
| issuer_id | Internal issuer identifier. |
| legal_name | Legal company name. |
| ultimate_parent_id | Parent issuer for concentration. |
| country_of_incorporation | Legal country. |
| country_of_risk | Economic/risk country. |
| sector | GICS/ICB/internal sector. |
| industry | More granular classification. |
| credit_rating | Issuer rating if available. |
| market_cap_category | Large/mid/small/micro. |
| listed_flag | Whether issuer has listed securities. |
| restricted_flag | Restricted/watchlist status. |
| related_party_flag | Internal compliance flag. |

Issuer master is essential for concentration risk. Multiple instruments can map to one issuer: common shares, preferred shares, ADRs, rights and warrants.

## 4. Instrument master

| Field | Meaning |
|---|---|
| instrument_id | Internal instrument identifier. |
| instrument_type | COMMON_EQUITY, PREFERRED_EQUITY, REIT, ADR, GDR, RIGHT, WARRANT, PRIVATE_EQUITY. |
| issuer_id | Issuer link. |
| primary_identifier | ISIN/CUSIP/SEDOL/local code. |
| name | Security name. |
| security_currency | Primary economic currency. |
| asset_class | Equity or equity-like. |
| sub_asset_class | Common, preferred, REIT, DR, etc. |
| voting_rights | Voting/non-voting/super-voting. |
| dividend_currency | Dividend payment currency. |
| active_status | Active, suspended, delisted, inactive, matured/expired. |
| risk_rating | Product risk classification. |
| complexity_flag | Simple/complex, based on local suitability rules. |
| eligible_for_margin | Whether usable as collateral. |
| haircut | Lending/buying-power haircut. |
| tax_country | Withholding tax country. |

## 5. Listing model

An instrument may have multiple listings, and the same issuer may have multiple instruments.

| Field | Meaning |
|---|---|
| listing_id | Unique listing identifier. |
| instrument_id | Instrument being listed. |
| exchange_mic | Market identifier code. |
| ticker | Exchange ticker. |
| trading_currency | Currency of trading. |
| board_lot | Minimum trading lot. |
| tick_size | Minimum price increment. |
| trading_status | Active, halted, suspended, delisted. |
| primary_listing_flag | Primary listing. |
| settlement_cycle | Market-specific settlement cycle. |
| trading_calendar_id | Exchange calendar. |
| price_source_priority | Preferred price source. |

Examples:

| Scenario | Modelling implication |
|---|---|
| Same company listed in Hong Kong and U.S. ADR | Separate instruments linked by issuer and DR mapping. |
| Same ordinary share dual-listed | Same or separate instrument depending on custody/fungibility and identifier rules. |
| Local share and GDR | Separate instruments with receipt ratio and underlying mapping. |

## 6. ADR/GDR mapping model

| Field | Meaning |
|---|---|
| dr_instrument_id | ADR/GDR instrument. |
| underlying_instrument_id | Local ordinary share. |
| depositary_bank | Depositary. |
| custodian_market | Local market where shares are held. |
| receipt_ratio | DR-to-underlying share ratio. |
| sponsored_flag | Sponsored/unsponsored. |
| fee_schedule | Depositary fee rules. |
| dividend_conversion_policy | Currency conversion and fee rules. |
| corporate_action_policy | How local events pass through to DR holders. |

## 7. Position valuation

Basic local-currency valuation:

```text
Local Market Value = Quantity x Market Price
```

Base-currency valuation:

```text
Base Market Value = Quantity x Market Price x FX Rate
```

For short positions:

```text
Short Exposure = Absolute Quantity x Market Price x FX Rate
Position Market Value may be shown as negative liability depending on reporting convention
```

## 8. Price hierarchy

| Priority | Price source | Use |
|---|---|---|
| 1 | Official exchange close | Default for listed liquid equities. |
| 2 | Vendor close | If exchange close not directly available. |
| 3 | Custodian price | Reconciliation/statement alignment. |
| 4 | Last traded price | Intraday/backup. |
| 5 | Bid/ask/mid | Illiquid or intraday valuation. |
| 6 | Manual/evaluated price | Suspended, delisted, private or illiquid security. |
| 7 | Zero/write-off price | Only when approved by policy/custodian confirmation. |

Every valuation should carry:

- price source;
- price timestamp;
- valuation date;
- stale flag;
- manual override flag;
- currency;
- FX rate source;
- confidence/quality indicator.

## 9. Clean historical price series

For analytics, distinguish:

| Price type | Use |
|---|---|
| Raw close | Accounting valuation on that date. |
| Split-adjusted close | Return analytics across stock splits. |
| Dividend-adjusted close | Total return analytics. |
| Corporate-action-adjusted price | Backtesting and charts. |
| Official custodian valuation price | Client statement and reconciliation. |

Do not use adjusted historical prices for accounting valuation unless explicitly intended. Accounting positions should use actual historical transaction prices and corporate-action postings.

## 10. Unrealized P&L

Local-currency unrealized P&L:

```text
Unrealized P&L = Current Market Value - Remaining Cost Basis
```

Base-currency unrealized P&L:

```text
Base Unrealized P&L = Current Base Market Value - Base Cost Basis
```

Breakdown:

| Component | Meaning |
|---|---|
| Price P&L | Gain/loss from security price movement in local currency. |
| FX P&L | Gain/loss from currency movement against portfolio base currency. |
| Cost-basis adjustment | Corporate action or return-of-capital effects. |

## 11. Realized P&L

For a sale:

```text
Realized P&L = Net Sale Proceeds - Cost Basis of Shares Sold
```

Where net sale proceeds may include:

```text
Gross Sale Proceeds - Commission - Taxes - Fees
```

Realized P&L depends on lot selection method:

| Lot method | Impact |
|---|---|
| FIFO | Oldest cost lots consumed first. |
| Average cost | Blended cost consumed. |
| Specific ID | Selected lots consumed. |
| Tax lot | Jurisdiction-specific. |

## 12. Dividend income and tax

Dividend reporting should separate:

| Field | Meaning |
|---|---|
| gross_dividend | Dividend before tax and fees. |
| withholding_tax | Tax deducted at source. |
| reclaimable_tax | Tax that may be reclaimed. |
| non_reclaimable_tax | Final tax cost. |
| depositary_fee | ADR/GDR fee. |
| net_dividend | Cash actually credited. |
| dividend_currency | Payment currency. |
| base_currency_amount | Converted amount. |

For performance, decide whether return is shown gross of tax, net of tax, or both. Private-banking reports often show client-relevant net cash while performance methodologies may need consistent gross/net definitions.

## 13. Total return

Single-period total return for equity:

```text
Total Return = (Ending Price - Beginning Price + Dividend per Share) / Beginning Price
```

For a portfolio position:

```text
Position Return = (Ending Market Value - Beginning Market Value - Net Purchases + Net Sales + Income) / Beginning Market Value adjusted for flows
```

In a performance engine, daily TWR should account for:

- beginning market value;
- buys and sells;
- income;
- fees and tax;
- corporate-action effects;
- ending market value;
- external cashflows.

## 14. Equity contribution and attribution inputs

For contribution by asset class/security/sector:

| Input | Why needed |
|---|---|
| Beginning weight | Contribution = weight x return. |
| Security return | Price + income return. |
| FX return | Currency contribution. |
| Transactions | Flows and trading effects. |
| Corporate actions | Avoid artificial return. |
| Classification | Sector/country/asset-class grouping. |
| Benchmark mapping | Attribution and active return. |

For equity attribution, common approaches include:

| Attribution type | Meaning |
|---|---|
| Brinson sector attribution | Allocation and selection by sector/country/region. |
| Security-level contribution | Each stock's contribution to total return. |
| Currency attribution | Impact of local currency versus portfolio/reporting currency. |
| Trading effect | Difference caused by intra-period buys/sells. |
| Income contribution | Dividend contribution. |
| Residual/non-allocable effect | Rounding, timing, classification, corporate-action and data limitations. |

## 15. Risk analytics for equities

| Metric | Meaning |
|---|---|
| Volatility | Dispersion of equity returns. |
| Beta | Sensitivity to market/benchmark movement. |
| Alpha | Return not explained by benchmark/factor exposure. |
| Sharpe ratio | Excess return per unit of volatility. |
| Sortino ratio | Downside-risk-adjusted return. |
| Maximum drawdown | Worst peak-to-trough decline. |
| VaR | Estimated potential loss under confidence/time assumptions. |
| CVaR / expected shortfall | Average loss beyond VaR threshold. |
| Tracking error | Volatility of active return versus benchmark. |
| Information ratio | Active return per unit of tracking error. |
| Correlation | Relationship with other assets. |
| Liquidity score | Ability to exit position. |

## 16. Concentration analytics

Equities are often the biggest driver of concentration risk.

| Concentration lens | Example |
|---|---|
| Single issuer | Apple exposure > 15% of portfolio. |
| Ultimate parent | Multiple share classes/ADRs aggregate to same issuer. |
| Sector | Technology > 40%. |
| Country of risk | U.S. equities > 70%. |
| Currency | USD exposure > 80%. |
| Market cap | Small-cap exposure too high. |
| Liquidity | Large position relative to average daily volume. |
| Related issuer | Employer stock exposure. |
| Collateral value | Equity LTV/haircut for lending. |

For your kind of portfolio platform, concentration should aggregate direct shares, ADRs/GDRs, equity funds look-through where available, and structured notes look-through exposure where policy requires.

## 17. Suitability and product risk attributes

| Attribute | Use |
|---|---|
| Product risk rating | Suitability check. |
| Complexity flag | Ordinary share versus warrant/structured equity. |
| Market cap/liquidity | Risk and appropriateness. |
| Country/sector | Portfolio fit. |
| Dividend yield | Income objective, but not guarantee. |
| Volatility/beta | Risk profile. |
| Concentration impact | Portfolio-level suitability. |
| Restricted/security watchlist | Compliance. |
| Margin eligibility/haircut | Buying power/collateral. |

## 18. Market data and reference data quality controls

| Control | Purpose |
|---|---|
| Missing price check | Prevent zero/stale valuation. |
| Stale price flag | Identify unchanged price beyond threshold. |
| Extreme price move check | Detect unprocessed split or bad price. |
| Corporate-action price adjustment check | Validate split-adjusted series. |
| FX rate availability | Ensure base-currency valuation. |
| Identifier mapping check | Prevent ADR/local share confusion. |
| Suspended/delisted status | Prevent unsupported trading/valuation. |
| Market calendar alignment | Avoid pricing on market holidays. |
| Duplicate listing check | Avoid double counting. |
| Sector/country completeness | Required for concentration and reporting. |

## 19. Common valuation edge cases

| Edge case | Handling |
|---|---|
| No closing price | Use hierarchy fallback and flag. |
| Suspended security | Carry last price or manual price with stale flag. |
| Delisted security | Update listing status; value based on OTC/private/custodian/manual price. |
| Stock split but price not adjusted | Detect price move and reconcile CA. |
| ADR ratio change | Adjust quantity/price interpretation. |
| Multi-currency dividend | Use dividend FX rate and base FX rate separately if required. |
| Negative price impossible | Reject except special data error. |
| Corporate action pending | Show expected entitlement separately from settled position. |
| Fractional shares | Cash-in-lieu or fractional holding based on market/custodian rules. |

## 20. Practitioner explanation

> Equity valuation is usually quantity times market price, translated into portfolio base currency. But the real challenge is data quality and lifecycle accuracy. The platform needs correct instrument/listing mapping, price source hierarchy, FX rates, stale-price controls, corporate-action adjustments, lot-level cost basis and tax/fee treatment. P&L should separate realized and unrealized components, and performance should include price return, dividend income and FX return while avoiding artificial returns from splits, spin-offs and other corporate actions. Risk analytics should cover volatility, beta, drawdown, VaR, sector/country/currency concentration and liquidity.
