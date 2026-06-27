# 05 — Hedge Funds and Liquid Alternatives

## 1. What is a hedge fund?

A hedge fund is a pooled investment vehicle that uses flexible investment strategies. It may invest long and short, use leverage, trade derivatives, run relative-value strategies, pursue event-driven opportunities, or seek absolute return.

Hedge funds are often private funds available only to eligible/sophisticated investors. Liquid alternatives are regulated/public fund wrappers that attempt to provide alternative-like strategies with more liquidity and investor protections, though they may have lower flexibility than private hedge funds.

## 2. Hedge fund strategy taxonomy

| Strategy | Description | Key risks |
|---|---|---|
| Long/short equity | Long preferred stocks, short disliked stocks | Market beta, short squeeze, leverage |
| Equity market neutral | Offset long/short beta to seek stock-selection alpha | Model/crowding risk |
| Global macro | Rates, FX, commodities, equity indices across macro themes | Leverage, model, policy risk |
| Managed futures / CTA | Trend-following futures across assets | Trend reversal, whipsaw |
| Event-driven | M&A, spin-offs, restructurings | Deal break, legal/regulatory risk |
| Merger arbitrage | Long target/short acquirer or deal spread | Deal failure risk |
| Distressed credit | Stressed/defaulted securities | Legal/restructuring and liquidity risk |
| Relative value | Pricing differences between related securities | Leverage, basis risk |
| Fixed income arbitrage | Yield curve/spread dislocations | Leverage, liquidity, rates |
| Convertible arbitrage | Convertible bond + equity hedge | Volatility, credit, borrow risk |
| Multi-strategy | Allocates across strategies | Manager allocation and transparency risk |
| Fund of hedge funds | Diversifies across hedge funds | Extra fee layer, manager selection |

## 3. Hedge fund wrappers

| Wrapper | Typical liquidity | Platform treatment |
|---|---|---|
| Offshore fund | Monthly/quarterly/annual redemption | Alternative fund position |
| UCITS/regulated liquid alternative | Daily/weekly/monthly | Fund-like with alt strategy tags |
| Managed account | Strategy in client/account-specific account | Direct positions + overlay analytics |
| Fund of hedge funds | Monthly/quarterly | Fund position with manager look-through |
| Structured note linked to hedge fund index | Note lifecycle | Structured product model |

## 4. Hedge fund lifecycle

```text
Subscription notice -> NAV acceptance -> shares/units allocated -> monthly/quarterly NAV -> fees crystallise -> redemption notice -> redemption dealing date -> redemption settlement
```

Key terms:

| Term | Meaning |
|---|---|
| Subscription date | Date investor enters fund |
| Dealing day | Date NAV used for subscription/redemption |
| Notice period | Advance notice required for redemption |
| Lock-up | Period during which redemption not allowed or penalised |
| Gate | Limit on redemption amount |
| Side pocket | Illiquid assets segregated from main redeemable pool |
| High-water mark | Performance fee only above prior peak NAV |
| Hurdle rate | Minimum return before incentive fee |
| Crystallisation | Period when performance fee is calculated/charged |
| Series accounting | Different share series for fee equalisation |

## 5. Hedge fund transaction types

| Transaction type | Use |
|---|---|
| HF_SUBSCRIPTION | Buy fund units/shares |
| HF_REDEMPTION_REQUEST | Request redemption; not yet economic settlement |
| HF_REDEMPTION_SETTLEMENT | Units redeemed and cash received |
| HF_NAV_UPDATE | Periodic NAV valuation |
| HF_INCOME_DISTRIBUTION | Income distribution if applicable |
| HF_GAIN_DISTRIBUTION | Realised gain distribution if applicable |
| HF_MANAGEMENT_FEE | Explicit fee if outside NAV |
| HF_PERFORMANCE_FEE | Incentive fee if explicitly booked |
| HF_GATE_APPLIED | Lifecycle event restricting redemption |
| HF_SIDE_POCKET_ALLOCATION | Move portion to side pocket |
| HF_SIDE_POCKET_REALISATION | Cash received from side-pocket asset |
| HF_TRANSFER_IN/OUT | Transfer between accounts/custodians |

Most hedge fund fees are reflected in NAV. Do not double-count fees unless the administrator provides explicit external fee transactions.

## 6. Position model

| Field | Meaning |
|---|---|
| fund_id | Hedge fund instrument/share class |
| share_class | Currency, fee class, liquidity class |
| units | Units/shares held |
| nav_per_unit | Latest NAV |
| nav_date | NAV valuation date |
| stale_nav_days | Valuation staleness |
| market_value | Units × NAV |
| subscription_date | Entry date |
| lockup_end_date | Earliest redemption date |
| redemption_frequency | Monthly/quarterly/etc. |
| notice_period_days | Required notice |
| gate_terms | Redemption restriction |
| side_pocket_units | Illiquid side-pocket units |
| strategy | L/S equity, macro, CTA, etc. |
| leverage_indicator | If available |
| liquidity_bucket | Daily/monthly/quarterly/annual/locked |

## 7. Valuation and NAV

Hedge fund positions are usually valued using administrator NAV. NAV may be:

- monthly or quarterly;
- estimated first, final later;
- restated after audit or correction;
- reported net of fund-level fees;
- subject to side-pocket adjustments.

Platform should support:

| NAV status | Meaning |
|---|---|
| ESTIMATED | Preliminary NAV |
| FINAL | Final administrator NAV |
| RESTATED | Correction/revised NAV |
| STALE | Older than expected |
| SUSPENDED | NAV or redemptions suspended |

## 8. Hedge fund risk analytics

Hedge funds require different analytics from long-only public funds.

| Metric | Use |
|---|---|
| Net exposure | Long exposure minus short exposure |
| Gross exposure | Long + absolute short exposure |
| Leverage | Economic exposure / NAV |
| Beta | Market sensitivity |
| Volatility | Return variability |
| Drawdown | Peak-to-trough loss |
| Sharpe/Sortino | Risk-adjusted return |
| Correlation | Relationship to equities/bonds |
| VaR/CVaR | Tail risk estimate |
| Liquidity terms | Ability to redeem |
| Side-pocket exposure | Illiquid portion |
| Strategy concentration | Exposure to same strategy/manager |

Where look-through is unavailable, strategy-level proxies may be used but must be labelled as estimates.

## 9. Advisory treatment

Hedge funds are often sold as diversifiers or absolute-return strategies. Advisors must avoid oversimplification.

Key client explanation:

> A hedge fund may aim to reduce dependence on traditional markets, but it can still lose money, use leverage, be illiquid, charge high fees and have opaque risk exposures.

Checklist:

| Area | Question |
|---|---|
| Strategy understanding | Does the client understand what the fund actually does? |
| Liquidity | Can the client accept lock-ups, notice periods and gates? |
| Leverage | Is leverage permitted by client profile/mandate? |
| Transparency | Is limited look-through acceptable? |
| Fees | Are management and performance fees understood? |
| Drawdown | Can client tolerate strategy-specific drawdowns? |
| Concentration | Is exposure diversified by manager and strategy? |
| Suitability | Is product allowed for the client type and jurisdiction? |

## 10. Reporting considerations

Client reporting should include:

- latest NAV and NAV date;
- estimated/final status;
- strategy classification;
- liquidity terms;
- lock-up and redemption notice;
- manager/fund concentration;
- return contribution;
- rolling volatility/drawdown if data available;
- correlation to portfolio/public benchmarks;
- side-pocket or suspended redemption flags;
- fee treatment notes where relevant.

## 11. Liquid alternatives

Liquid alternatives are public/regulated funds that use alternative strategies.

Examples:

- long/short equity mutual fund;
- market-neutral UCITS fund;
- managed futures ETF;
- merger arbitrage fund;
- multi-alternative fund.

They are easier to model operationally as normal funds but should retain alternative strategy tags for advisory, risk and mandate rules.

Important distinction:

```text
Liquid alternative wrapper may be liquid, but the strategy can still be complex.
```
