# 03 - Rebalancing, Drift, Trade Generation and Optimization

## 1. What is rebalancing?

Rebalancing is the process of adjusting a portfolio back toward a target allocation or permitted range.

It may involve:

- selling overweight positions;
- buying underweight positions;
- investing new cash;
- raising cash for withdrawals;
- switching funds;
- replacing matured bonds;
- reinvesting coupons/dividends;
- hedging currency;
- reducing concentration;
- correcting mandate breaches;
- implementing tactical views.

The primary purpose is risk control and mandate alignment, not mechanically maximizing short-term return.

## 2. Why portfolios drift

| Cause | Example |
|---|---|
| Market movement | Equity rally increases equity weight from 40% to 48% |
| FX movement | USD assets rise in SGD reporting currency |
| Coupons/dividends | Cash balance increases |
| Client cashflow | Withdrawal creates cash deficit |
| Capital call | Private equity commitment requires cash |
| Corporate action | Stock split, merger, spin-off changes exposure |
| Maturity/redemption | Bond matures, structured note autocalls |
| Model change | CIO changes target weights |
| Advisor trade | Tactical idea adds concentration |
| Lending/collateral | Pledged assets cannot be sold freely |

## 3. Drift calculation

Basic drift:

```text
Drift = Current Weight - Target Weight
```

Example:

| Asset class | Current weight | Target weight | Drift |
|---|---:|---:|---:|
| Equity | 48% | 40% | +8% |
| Fixed income | 38% | 45% | -7% |
| Alternatives | 9% | 10% | -1% |
| Cash | 5% | 5% | 0% |

Portfolio is overweight equity and underweight fixed income.

## 4. Absolute drift versus relative drift

| Type | Formula | Example |
|---|---|---|
| Absolute drift | Current - Target | 48% - 40% = +8 percentage points |
| Relative drift | (Current - Target) / Target | +8% / 40% = +20% relative drift |

Both can be useful. Absolute drift is easier to explain. Relative drift is useful for small target allocations.

Example: a 2% target private credit allocation moving to 4% is only +2 percentage points, but +100% relative drift.

## 5. Rebalancing policies

| Policy | Description | Pros | Risks |
|---|---|---|---|
| Calendar-based | Review monthly/quarterly/annually | Simple, predictable | May trade unnecessarily or too late |
| Threshold-based | Rebalance only when drift exceeds tolerance | Risk-sensitive, reduces small trades | Requires monitoring and threshold design |
| Hybrid | Scheduled review plus threshold triggers | Practical for wealth platforms | More complex governance |
| Cashflow-based | Use inflows/outflows to correct drift | Low turnover and cost | May be slow if no cashflows |
| Opportunistic | Rebalance around market or tax events | Flexible | Can become subjective |
| Mandate-breach-driven | Rebalance when portfolio breaches limits | Compliance-focused | May wait until breach occurs |
| Tax-aware | Optimize around gains/losses | After-tax efficient | Jurisdiction-specific complexity |
| Liquidity-aware | Avoid illiquid positions; plan cash | Practical for private wealth | Requires accurate liquidity data |

## 6. Tolerance bands

Tolerance bands define when drift becomes actionable.

Example:

| Asset class | Target | Warning band | Action band | Mandate range |
|---|---:|---:|---:|---:|
| Equity | 40% | +/-3% | +/-5% | 30-50% |
| Fixed income | 45% | +/-3% | +/-5% | 35-60% |
| Alternatives | 10% | +/-2% | +/-4% | 0-15% |
| Cash | 5% | +/-2% | +/-3% | 0-15% |

Interpretation:

- Within warning band: no action.
- Warning band breached: monitor or discuss.
- Action band breached: generate proposal/rebalance candidate.
- Mandate range breached: compliance/remediation workflow.

## 7. Trade generation flow

```text
Input portfolio
  -> normalize valuation and exposure
  -> map holdings to model categories
  -> calculate current weights
  -> compare to target and ranges
  -> identify drift and breaches
  -> determine available cash and restrictions
  -> create candidate sells and buys
  -> apply suitability / mandate / product eligibility
  -> optimize for constraints
  -> create proposed trade list
  -> run pre-trade checks
  -> route for approval / client consent
```

## 8. Simple rebalance example

Portfolio value = USD 1,000,000

| Asset class | Current value | Current weight | Target | Target value | Action |
|---|---:|---:|---:|---:|---:|
| Equity | 480,000 | 48% | 40% | 400,000 | Sell 80,000 |
| Fixed income | 380,000 | 38% | 45% | 450,000 | Buy 70,000 |
| Alternatives | 90,000 | 9% | 10% | 100,000 | Buy 10,000 |
| Cash | 50,000 | 5% | 5% | 50,000 | No change |

This is simplistic. Real rebalancing must consider lots, taxes, fees, minimum order sizes, liquidity, product eligibility, settlement cycles and client restrictions.

## 9. Candidate sell logic

Sell candidates can be ranked by:

1. highest overweight versus target;
2. mandate breach severity;
3. lowest conviction / sell-rated product;
4. concentrated positions;
5. liquidity availability;
6. tax lot impact;
7. transaction cost;
8. client restriction;
9. pledged/collateral status;
10. unrealized gain/loss treatment;
11. product maturity/lock-in;
12. advisor/PM preference.

## 10. Candidate buy logic

Buy candidates can be ranked by:

1. model target underweight;
2. recommended product list;
3. best share class / lowest cost;
4. client eligibility;
5. product risk compatibility;
6. liquidity and minimum subscription;
7. currency fit;
8. existing exposure and concentration;
9. tax efficiency;
10. settlement feasibility;
11. portfolio income requirement;
12. investment office conviction.

## 11. Optimization constraints

A rebalancing engine should support hard and soft constraints.

### Hard constraints

Hard constraints cannot be violated.

Examples:

- product not eligible for client;
- client risk profile too low;
- instrument not approved in booking centre;
- no buy of restricted issuer;
- account has insufficient cash or buying power;
- pledged asset cannot be sold;
- DPM mandate prohibits derivatives;
- minimum order size not met;
- private fund subscription closed;
- sanctions / restricted list hit.

### Soft constraints

Soft constraints may be optimized or overridden with approval.

Examples:

- minimize turnover;
- reduce realized gains;
- prefer house model funds;
- avoid small residual positions;
- keep cash buffer above target;
- minimize FX conversions;
- avoid selling positions with exit fees;
- maintain income yield;
- prioritize highest drift correction.

## 12. Optimization objective examples

| Objective | Description |
|---|---|
| Minimize total drift | Move portfolio close to target |
| Minimize turnover | Reduce unnecessary trades |
| Minimize transaction cost | Avoid high-cost trading |
| Minimize realized gains | Tax-aware optimization |
| Maximize suitability score | Prefer products with best client fit |
| Maintain income | Preserve expected coupon/dividend yield |
| Maintain liquidity | Keep liquid assets above required threshold |
| Reduce concentration | Lower issuer/security/sector risk |
| Improve risk-adjusted metrics | Reduce volatility/drawdown or tracking error |

Common objective:

```text
Minimize: tracking error to target + transaction cost penalty + tax penalty + liquidity penalty + concentration penalty
Subject to: hard constraints and mandate rules
```

## 13. Rebalancing with cashflows

Client deposits USD 100,000 into a USD 1,000,000 portfolio. Instead of selling overweight assets, use new cash to buy underweight assets.

| Action | Benefit |
|---|---|
| Use inflows to buy underweights | Reduces turnover and realized gains |
| Use withdrawals from overweights | Corrects drift while raising liquidity |
| Reinvest coupons into underweights | Gradual rebalancing |
| Hold cash for near-term capital calls | Prevents forced selling |

## 14. Rebalancing with illiquid assets

Illiquid assets complicate targets.

Example:

| Asset | Target | Current | Issue |
|---|---:|---:|---|
| Private equity | 10% | 18% | Cannot sell easily |
| Public equity | 40% | 35% | Liquid |
| Fixed income | 45% | 42% | Liquid |
| Cash | 5% | 5% | Liquid |

Possible response:

- freeze illiquid asset weight;
- rebalance only liquid sleeve;
- set commitment pacing plan;
- use future inflows to dilute overweight;
- flag breach as structural / non-actionable if mandate permits;
- report separately to advisor/client.

## 15. Rebalancing with pledged/collateral assets

If securities are pledged to a Lombard credit line, selling them may reduce borrowing capacity.

Before recommending sell:

- check pledge status;
- check current LTV;
- check post-trade collateral value;
- check haircut changes;
- check in-flight orders;
- check margin-call buffer;
- check cross-pledging impact;
- check whether asset is shared collateral across credit lines.

A rebalance engine must not treat pledged assets as freely available.

## 16. Rebalancing with tax lots

For tax-aware jurisdictions, the engine should choose lots carefully.

Lot selection methods:

| Method | Description |
|---|---|
| FIFO | Sell oldest lots first |
| LIFO | Sell newest lots first |
| Average cost | Use average cost basis |
| Highest cost | Minimize realized gain |
| Lowest cost | Harvest gain or use losses elsewhere |
| Specific lot | Advisor/client selects exact lots |

Tax-aware rebalancing can change trade recommendations materially.

## 17. Rebalancing with FX

Global portfolios often drift because of FX movement.

Need separate treatment for:

- asset exposure currency;
- instrument trading currency;
- income currency;
- settlement currency;
- reporting currency;
- client base currency;
- hedged versus unhedged exposure.

Example: a USD bond fund held in SGD account may have USD asset exposure but SGD share class. Reporting must identify whether FX risk is hedged or unhedged.

## 18. Rebalancing order types

| Order type | Use |
|---|---|
| Market order | Liquid listed securities |
| Limit order | Price-sensitive equities/ETFs |
| Fund subscription/redemption | Mutual funds/unit trusts |
| Fund switch | Sell one fund and buy another |
| Bond RFQ | OTC bond execution |
| Structured product subscription | Primary issuance or secondary unwind |
| FX spot/forward | Currency conversion or hedge |
| Derivative trade | Overlay or hedge |
| Internal transfer | Move cash/assets between sleeves/accounts |

## 19. Trade grouping

Group related trades under one proposal.

Example:

```text
Proposal P123
  Trade group: Rebalance equity overweight
    Sell US Equity Fund
    Buy Global Bond Fund
    Buy Cash/MMF buffer
    FX conversion USD -> SGD
```

Grouping matters for:

- explanation;
- approval;
- suitability;
- pre-trade checks;
- settlement planning;
- performance attribution;
- audit.

## 20. Execution sequencing

Trade sequence matters.

| Situation | Preferred sequence |
|---|---|
| Need cash to buy | Sell first, then buy after cash availability |
| Fund switch | Redeem/switch according to fund platform workflow |
| FX needed | FX before security settlement if required |
| Low liquidity bond | Confirm execution before dependent buy |
| Collateral asset sale | Confirm credit-line impact first |
| Capital call funding | Raise cash before capital call date |

## 21. Drift, breach and recommendation status

Recommended statuses:

| Status | Meaning |
|---|---|
| IN_TARGET | Within tolerance |
| MONITOR | Warning threshold breached |
| REBALANCE_CANDIDATE | Action threshold breached |
| PROPOSAL_CREATED | Trade list generated |
| SUITABILITY_FAILED | Cannot proceed without change/override |
| CLIENT_PENDING | Waiting for client approval |
| APPROVED | Approved for order placement |
| ORDERED | Orders created |
| PARTIALLY_EXECUTED | Some trades filled |
| COMPLETED | Portfolio updated |
| CANCELLED | Proposal cancelled |
| EXPIRED | Proposal no longer valid |
| BREACH | Mandate/rule breach |
| REMEDIATION_REQUIRED | Breach action required |

## 22. Good rebalancing controls

- Do not rebalance stale valuations.
- Do not generate orders for restricted products.
- Do not sell pledged assets without collateral impact check.
- Do not ignore unsettled trades.
- Do not treat pending dividends/coupons as settled cash.
- Do not propose illiquid private-market sales as if they were listed securities.
- Do not mix advisory and DPM approval workflows.
- Do not ignore client-specific exclusions.
- Do not rebalance using old model version unless explicitly intended.
- Do not execute if suitability or product governance data is missing.

## 23. Summary

Rebalancing is not only a mathematical target-weight exercise. It is a controlled recommendation and execution workflow that must account for suitability, product eligibility, liquidity, cost, tax, FX, collateral, settlement, restrictions, client consent and audit lineage.
