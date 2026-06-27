# 07 - Equity Advisory, Mandate, Analytics and Reporting Deep Dive

## 1. Purpose

This file expands the equity pack for wealth-management advisory, DPM mandates, portfolio analytics, risk monitoring and client reporting. It is written from the perspective of a portfolio analytics / client reporting platform where equities are not only traded instruments but portfolio decisions that must be explained, monitored and reported.

## 2. Advisory lifecycle for equities

```text
Client profile and objective
  -> portfolio review and gap analysis
  -> house view / research / idea generation
  -> portfolio-fit assessment
  -> product and client suitability
  -> proposal / client explanation / consent
  -> pre-trade mandate and buying-power checks
  -> order and execution
  -> post-trade confirmation
  -> monitoring of price, risk, mandate and corporate actions
  -> periodic review, rebalance, hold/sell decision
```

## 3. Questions an advisor should answer before recommending an equity

| Area | Questions |
|---|---|
| Client objective | Is the objective growth, income, diversification, tactical opportunity, thematic exposure or transition from cash/fixed income? |
| Portfolio role | Is the stock a core holding, satellite idea, income holding, defensive position, cyclical exposure, hedge, or liquidity sleeve? |
| Risk profile | Can the client tolerate equity volatility and possible capital loss? |
| Time horizon | Is the expected holding period aligned with the client's investment horizon? |
| Liquidity need | Could the client need to exit quickly, and is the stock liquid enough? |
| Concentration | How much single-name, sector, country, currency, theme and issuer-group exposure is added? |
| Currency | Does the trade introduce FX risk relative to client base currency or liabilities? |
| Product knowledge | Does the client understand ordinary share risk, corporate actions, market gaps and dividend uncertainty? |
| Research status | Is the stock covered, recommended, approved, watchlisted, restricted or prohibited? |
| Valuation | Is the recommendation supported by earnings, cashflow, valuation, catalyst or relative opportunity? |
| Income quality | If dividend-focused, is the dividend sustainable or a possible yield trap? |
| Event risk | Are there upcoming earnings, rights issues, merger votes, tender deadlines, delisting risks or regulatory events? |
| Mandate | Is the trade allowed under IPS, model portfolio and mandate constraints? |
| Exit discipline | What would trigger review, reduce, switch or sell? |

## 4. Equity investment thesis template

A high-quality advisory thesis should include:

```text
Security:
Recommendation: Buy / Hold / Sell / Switch / Avoid
Portfolio role:
Client objective alignment:
Investment rationale:
Expected return drivers:
Valuation support:
Income/dividend view:
Risk factors:
Position size:
Time horizon:
Monitoring triggers:
Exit conditions:
Mandate/suitability result:
Impact on portfolio allocation and concentration:
```

Example:

```text
Security: ABC Ltd
Recommendation: Buy
Portfolio role: Global growth satellite; technology sector exposure
Rationale: Strong earnings growth, high free cash flow conversion, sector tailwind and attractive valuation relative to peers
Expected horizon: 12-24 months
Risks: Valuation compression, earnings miss, regulation, USD exposure
Sizing: Keep below 5% of portfolio and within technology sector tolerance
Monitoring triggers: Earnings downgrade, price target reached, sector view change, mandate breach
Suitability: Suitable for growth/aggressive risk profile; not suitable for capital-preservation-only mandate
```

## 5. Equity suitability dimensions

| Dimension | What to check | Examples |
|---|---|---|
| Client risk profile | Is equity risk allowed? | Conservative client may allow diversified equity fund but not concentrated single stock |
| Product complexity | Is it ordinary equity or complex equity-linked exposure? | ADR, preferred, warrant, rights, penny stock, suspended security |
| Market risk | How volatile is the stock/security? | High beta, small cap, emerging market |
| Liquidity risk | Can it be sold in normal size? | Thinly traded stock, odd lot, halted or suspended security |
| Concentration | Does it increase exposure too much? | Single-name > 10%, sector > 30% |
| Currency | Base currency mismatch | USD stock in SGD portfolio |
| Knowledge/experience | Client understands equity downside | Direct single stocks vs diversified funds |
| Restrictions | Is the security allowed? | Restricted list, sanctions, ESG exclusion |
| Time horizon | Does holding period fit? | Short-term need vs long-term growth stock |
| Corporate action risk | Any pending event requiring action? | Rights issue, tender offer, merger |

## 6. Mandate and IPS treatment

A discretionary mandate or advisory mandate should define allowed equity behaviour.

| Mandate rule | Example |
|---|---|
| Equity allocation range | 40%-70% |
| Regional ranges | US 0%-40%, Europe 0%-30%, Asia 0%-40% |
| Single-name limit | Max 5% or 10% depending mandate |
| Issuer-group limit | Include equity, bonds, notes and look-through exposure |
| Sector limit | Max 25% per sector |
| Country limit | Max 30% per country unless home bias mandate |
| Currency limit | Non-base currency max 40% |
| Market-cap limit | Small-cap max 10% |
| Liquidity bucket | Illiquid/suspended/restricted equities max 5% |
| Approved list | Only research-covered or approved securities |
| Restricted list | Hard block or forced review |
| Benchmark | MSCI World, S&P 500, MSCI ACWI, custom benchmark |
| Tracking error | Active risk target/tolerance |
| Turnover | Max turnover or review threshold |
| ESG/exclusions | Industry/security exclusions |
| Tax constraints | Avoid short-term gains or client-specific restrictions |
| Income target | Dividend yield or income requirement |

## 7. Breach taxonomy

| Breach type | Example | Treatment |
|---|---|---|
| Hard breach | Restricted security buy | Block |
| Soft breach | Sector slightly above tolerance | Warning / approval |
| Drift breach | Price move makes stock too large | Monitor and rebalance window |
| Corporate-action breach | Spin-off creates prohibited stock | Exception workflow |
| Data breach | Missing classification prevents check | Block or route to exception |
| Liquidity breach | Stock becomes suspended | Monitor, disclose, fair-value process |
| Currency breach | USD exposure exceeds limit | Hedge/rebalance/review |
| Concentration breach | Single issuer group too high | Reduce, waiver or review |

## 8. Advisory vs DPM vs execution-only

| Model | Decision maker | Platform focus |
|---|---|---|
| Advisory | Client decides after recommendation | Suitability, proposal, disclosure, consent, audit |
| DPM / discretionary | Portfolio manager decides within mandate | IPS, model portfolio, pre/post-trade compliance, breach monitoring |
| Execution-only | Client directs trade | Appropriateness/product eligibility, restricted list, risk disclosure |

For DPM, the system should support model portfolios, target weights, tolerance bands, rebalancing proposals, trade generation, compliance checks and post-trade drift monitoring.

## 9. Equity model portfolio and rebalancing

Model portfolio fields:

| Field | Example |
|---|---|
| Model ID | GLOBAL_GROWTH_EQ_01 |
| Benchmark | MSCI World |
| Target holdings | Security + target weight |
| Tolerance bands | Security +/-1%, sector +/-5% |
| Cash buffer | 1%-3% |
| Rebalance frequency | Monthly/quarterly/ad hoc |
| Exclusions | Restricted list/ESG/client-specific |
| Client overlays | No tobacco, no China, no single stock above 3% |
| Trade rounding | Board lots, odd lots, fractional eligibility |
| Liquidity rule | Max x% ADV or days-to-liquidate |
| Tax rule | Lot selection or gain/loss constraints |

Rebalancing flow:

```text
Current holdings
  -> classify by security/sector/country/currency
  -> compare to target model
  -> apply client restrictions and mandate limits
  -> calculate target trades
  -> check liquidity, lots, cash, FX and buying power
  -> generate orders
  -> execute and update positions
  -> monitor settlement and drift
```

## 10. Equity analytics required for portfolio review

| Analytics | Business meaning |
|---|---|
| Equity allocation | Overall growth/risk exposure |
| Single-name concentration | Idiosyncratic risk |
| Issuer-group exposure | Combined risk across shares, bonds, notes, funds and derivatives |
| Sector allocation | Sector bets and diversification |
| Country/region allocation | Macro/geopolitical exposure |
| Currency exposure | FX risk relative to client base currency |
| Market-cap exposure | Large/mid/small risk profile |
| Dividend yield and income forecast | Income expectation |
| Dividend concentration | Dependence on a few dividend payers |
| Volatility | Risk magnitude |
| Beta | Market sensitivity |
| Drawdown | Downside experience |
| Tracking error | Active risk versus benchmark |
| Active share | Degree of benchmark deviation |
| Factor exposure | Growth/value/momentum/quality/size/low-vol |
| Liquidity profile | Exit ability |
| Unrealized gain/loss | Behavioural and tax context |
| Realized P&L | Trading outcome |
| Contribution | Return drivers |
| Attribution | Allocation/selection/currency effects |

## 11. Equity health-check framework

| Check | Purpose | Example output |
|---|---|---|
| Single-name concentration | Detect idiosyncratic risk | ABC is 12% of portfolio; above 10% alert |
| Sector concentration | Detect sector crowding | Technology 34%; above 30% warning |
| Currency mismatch | Identify FX exposure | USD assets 65% vs SGD base |
| Low liquidity | Detect exit risk | 8 days to liquidate at 20% ADV |
| High beta | Identify market sensitivity | Beta 1.6 vs benchmark |
| Large drawdown | Monitor client pain points | Stock down 38% from cost |
| Dividend risk | Avoid yield trap | Yield high but earnings coverage weak |
| Corporate action due | Action needed | Rights election deadline in 3 days |
| Restricted/watchlist | Compliance/advisory | Stock moved to watchlist |
| Model deviation | DPM drift | Active weight +4% vs tolerance +2% |
| Tax-lot opportunity | Tax-aware review | Unrealized loss available for harvesting |
| Collateral haircut risk | Buying power impact | Equity LTV/haircut reduced |

## 12. Equity reporting requirements

### Holdings section

Show security name, ticker, quantity, price and price date, market value in local/base currency, portfolio weight, cost, unrealized P&L, sector/country/currency, dividend yield or expected income, risk flags and corporate action flags.

### Performance section

Show equity sleeve return, price vs income return, local vs FX return, top contributors/detractors, benchmark comparison, attribution by sector/country/security and trading effect.

### Risk section

Show concentration, sector/country/currency allocation, volatility, drawdown, beta, VaR/CVaR if used, liquidity profile, stress scenarios and mandate breaches/warnings.

### Corporate action section

Show upcoming dividends, rights, tender or merger deadlines, elections required, processed corporate actions, cash/shares received, cost-basis impact and missed/lapsed events if any.

## 13. Contribution and attribution

```text
Contribution_i = Beginning Weight_i x Return_i
```

| Attribution lens | Question answered |
|---|---|
| Sector allocation | Did overweight/underweight sectors help? |
| Security selection | Did selected stocks outperform within sectors? |
| Country allocation | Did regional/country bets help? |
| Currency attribution | How much came from FX? |
| Factor attribution | Was return driven by value/growth/momentum/quality? |
| Trading effect | Did buys/sells during the period help? |
| Dividend contribution | How much came from income? |

## 14. Reporting commentary example

> Equity performance was positive during the period, mainly from U.S. technology and healthcare holdings. ABC and XYZ were the top contributors, while DEF detracted after weaker earnings guidance. The portfolio remains overweight U.S. equities and technology versus benchmark. Single-name concentration is within mandate, but technology exposure is close to the upper tolerance band and should be monitored. There is one upcoming rights election requiring action before the client deadline.

## 15. Platform data dependencies

| Analytics/report | Required data |
|---|---|
| Holdings | Security master, positions, prices, FX |
| Income | Dividend events, tax, cash ledger |
| P&L | Cost lots, transactions, valuations |
| Performance | Prices, transactions, dividends, FX, corporate action adjustments |
| Contribution | Beginning weights and returns |
| Attribution | Benchmark weights/returns and classifications |
| Risk | Historical returns, covariance, benchmark, sector/country |
| Liquidity | Volume, bid/ask, market cap, trading status |
| Mandate | IPS rules, restrictions, classifications, look-through |
| Corporate actions | Event feed, elections, entitlements, postings |

## 16. Practitioner answer

> In advisory and mandate platforms, equities should not be treated as only quantity times price. The platform must understand the portfolio role of the holding, suitability for the client, mandate constraints, single-name and sector concentration, dividend quality, liquidity, FX exposure and corporate-action risks. For DPM, equities must be mapped to model portfolios and benchmarks so allocation, selection, contribution, attribution, tracking error and breach monitoring can be produced consistently. Reporting should explain not only market value and P&L, but why equity performance happened, which stocks contributed, whether risk is within mandate, and whether any corporate action or advisory action is required.
