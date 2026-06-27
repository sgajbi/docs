# 07 - Analytics, Reporting, Dashboards and Client Experience

## 1. Purpose

Advisory and DPM platforms need analytics that turn raw positions into clear actions and explanations.

The analytics must support three audiences:

| Audience | Needs |
|---|---|
| Advisor/RM | Explain portfolio state and recommended action to client |
| Portfolio manager/investment team | Manage model drift, mandate alignment, trade actions and risk |
| Client | Understand portfolio, proposal, risk, costs and expected changes |

## 2. Core analytics for advisory proposals

| Analytics area | Questions answered |
|---|---|
| Allocation | What does the client hold by asset class, region, sector, currency and product type? |
| Drift | How far is the portfolio from target/model/mandate? |
| Concentration | Is the client overexposed to an issuer, security, sector, country or currency? |
| Risk | What is volatility, drawdown, VaR, duration, credit risk, FX risk? |
| Performance | How has portfolio performed versus benchmark/model? |
| Contribution | Which holdings/asset classes contributed to return? |
| Income | What coupons/dividends/distributions are expected? |
| Liquidity | How much can be liquidated by T+1/T+3/T+7/monthly/illiquid buckets? |
| Cashflow | Upcoming maturities, coupons, capital calls, withdrawals, fees |
| Cost | Estimated transaction cost and product cost impact |
| Suitability | Does proposed action fit the client? |
| Scenario | What happens if markets fall/rates rise/FX moves? |

## 3. Advisor dashboard

Recommended dashboard sections:

1. client summary;
2. risk profile and mandate;
3. portfolio valuation and performance;
4. allocation versus target;
5. drift and breach alerts;
6. recommended actions;
7. suitability warnings;
8. upcoming maturities/cashflows;
9. concentration alerts;
10. liquidity and cash buffer;
11. product opportunities;
12. pending proposals and orders;
13. client approval status;
14. post-trade follow-up.

## 4. DPM dashboard

DPM needs portfolio population analytics.

| View | Purpose |
|---|---|
| Model drift by portfolio | Identify accounts needing rebalance |
| Mandate breach heatmap | Prioritize compliance remediation |
| Cash buffer monitor | Identify under/over cash portfolios |
| Trade impact summary | Estimate aggregate orders by instrument |
| Fair allocation monitor | Ensure fair treatment across accounts |
| Product restriction conflicts | Identify accounts excluded from model trade |
| Liquidity monitor | Avoid forced trading and settlement issues |
| Post-trade compliance | Confirm portfolios within limits after execution |

## 5. Proposal report structure

Client-facing proposal should include:

1. Executive summary.
2. Portfolio objective and risk profile.
3. Current allocation.
4. Target/proposed allocation.
5. Recommended actions.
6. Investment rationale.
7. Key benefits.
8. Key risks.
9. Product details and documents.
10. Costs and fees.
11. Liquidity and holding period.
12. Currency exposure.
13. Income impact.
14. Scenario analysis.
15. Suitability/disclosure statements.
16. Client decision and signatures/consent.

## 6. Before/after portfolio review

Example:

| Metric | Current | Proposed | Interpretation |
|---|---:|---:|---|
| Equity | 48% | 42% | Lower growth/risk exposure |
| Fixed income | 35% | 43% | More defensive income allocation |
| Cash | 10% | 7% | Deploy some idle cash |
| Alternatives | 7% | 8% | Slight increase in diversifier |
| Expected yield | 2.8% | 3.2% | Higher income expectation |
| Volatility | 12.0% | 10.3% | Lower estimated risk |
| Max issuer exposure | 18% | 11% | Concentration reduced |
| Liquidity within T+7 | 60% | 68% | Liquidity improved |

## 7. Drift visualization concepts

Use simple indicators:

| Indicator | Meaning |
|---|---|
| Green | Within target tolerance |
| Amber | Warning drift |
| Red | Action required or breach |
| Grey | No target / not classified |

For client-facing output, avoid over-technical language. For example:

- "Your portfolio is currently above the target equity range."
- "The proposal reduces single-stock concentration."
- "The proposal keeps enough liquidity for expected withdrawals."

## 8. Suitability explanation

A good client report should not simply say "suitable." It should explain why.

Example:

```text
The proposed portfolio remains aligned with your Balanced risk profile. It reduces equity exposure from 48% to 42%, increases investment-grade fixed income, and maintains a 7% cash allocation. No proposed product exceeds your approved product complexity level. The portfolio remains within single issuer and non-base currency limits.
```

If warnings exist:

```text
This proposal includes a structured note with conditional capital risk and issuer credit risk. The product is classified as complex and requires your acknowledgement of the product terms, potential loss scenario and limited secondary liquidity.
```

## 9. Scenario analytics

Useful scenarios:

| Scenario | Use |
|---|---|
| Equity market -10% / -20% | Show downside sensitivity |
| Interest rates +100 bps | Fixed-income duration impact |
| Credit spreads +100 bps | Credit portfolio impact |
| USD weakens by 5% | FX risk in reporting currency |
| Liquidity stress | What can be sold quickly? |
| Structured note barrier breach | Tail risk explanation |
| Private-market capital call | Liquidity planning |
| Margin/LTV stress | Collateral shortfall risk |

## 10. Income analytics

Income-seeking clients need:

- expected coupon schedule;
- dividend estimate;
- fund distribution frequency;
- bond maturity and coupon ladder;
- structured product coupon risk;
- withholding tax estimate;
- annuity/policy income;
- cash/deposit interest;
- income by currency;
- income stability/reliability score.

## 11. Liquidity analytics

Liquidity buckets:

| Bucket | Examples |
|---|---|
| Same day / T+0 | Cash, some money-market products |
| T+1/T+2 | Listed equities/ETFs in liquid markets |
| T+3/T+5 | Bonds, funds depending on settlement |
| Monthly/quarterly | Some alternative funds |
| Lock-up/gated | Hedge/private funds |
| Illiquid | Private equity, direct property, some structured products |

Proposal should show if liquidity improves or worsens.

## 12. Mandate reporting

Mandate report sections:

| Section | Purpose |
|---|---|
| Allocation versus range | Shows compliance with asset-class limits |
| Risk profile alignment | Shows portfolio remains within risk level |
| Benchmark-relative performance | Evaluates PM/advisor results |
| Breach summary | Lists active/remediated breaches |
| Turnover | Shows trading intensity |
| Cost | Tracks implementation cost |
| Cash and liquidity | Supports liquidity and withdrawal planning |
| Restrictions | Shows restricted list/exclusion compliance |
| Concentration | Shows issuer/security/sector risk |
| Income | Shows expected/actual income |

## 13. Model portfolio reporting

For investment office:

- model performance;
- model risk;
- model holdings;
- client adoption;
- client portfolios out of drift tolerance;
- products causing most breaches;
- model change impact analysis;
- rebalancing completion rate;
- client acceptance/rejection rate;
- proposal-to-execution conversion.

## 14. Proposal analytics after execution

Post-trade report should compare:

| Item | Expected | Actual |
|---|---:|---:|
| Trade amount | 100,000 | 99,820 |
| Execution price | 100.00 | 99.85 |
| Fees | 100 | 120 |
| Cash residual | 0 | 80 |
| Final equity weight | 42.0% | 42.2% |
| Final cash weight | 7.0% | 7.1% |
| Suitability status | Pass | Pass |

Reasons for difference:

- price movement;
- partial fill;
- FX rate difference;
- fees;
- fund NAV finalization;
- settlement failure;
- cancelled line;
- corporate action.

## 15. Client experience principles

A strong client experience should be:

- clear about objective;
- transparent about risk;
- specific about actions;
- visual but not misleading;
- simple enough for non-experts;
- detailed enough for sophisticated clients;
- consistent with regulatory disclosures;
- connected to portfolio goals;
- not purely product-push;
- explicit about costs and liquidity.

## 16. Reporting pitfalls

| Pitfall | Why it is dangerous |
|---|---|
| Showing target as guarantee | Misleads client |
| Showing expected return without risk | Incomplete advice |
| Ignoring fees/taxes | Overstates benefit |
| Ignoring FX | Understates risk |
| Treating illiquid assets as cash-like | Liquidity misrepresentation |
| Showing structured coupon as certain when conditional | Misleading income view |
| Not explaining product complexity | Suitability weakness |
| Not comparing do-nothing scenario | Client cannot evaluate trade-off |
| Aggregating all alternatives without liquidity split | Hides lock-up risk |

## 17. Summary

Analytics and reporting should make advisory and DPM workflows explainable. The platform should show current state, target state, proposed action, expected impact, suitability, risks, costs, liquidity and post-trade outcome in a traceable way.
