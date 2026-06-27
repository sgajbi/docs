# 11 - Worked Examples and Implementation Patterns

This file turns portfolio performance, attribution, risk, benchmark and mandate analytics into concrete examples for requirements, calculations, QA, reporting, advisory workflows and implementation design.

The numbers are intentionally simple. A production platform should use decimal precision, stored methodology versions, source lineage and controlled rounding rules.

## Example 1. Daily TWR with an external deposit

**Scenario:** A portfolio starts at USD 1,000,000. It gains value on day 1, receives a client deposit after the day-1 valuation point, then loses value on day 2.

| Item | Value |
|---|---:|
| Day 1 beginning market value | 1,000,000 |
| Day 1 ending market value before external flow | 1,010,000 |
| External deposit after day-1 valuation | 200,000 |
| Day 2 beginning market value after deposit | 1,210,000 |
| Day 2 ending market value | 1,203,950 |

Daily returns:

```text
day_1_return = 1,010,000 / 1,000,000 - 1 = 1.00%
day_2_return = 1,203,950 / 1,210,000 - 1 = -0.50%
twr = (1 + 1.00%) x (1 - 0.50%) - 1 = 0.495%
```

**Reporting interpretation:** The portfolio strategy returned 0.495%. The USD 200,000 deposit increased assets, but it should not inflate manager or strategy performance.

**QA assertion:** A deposit after the valuation point changes next-day beginning value but does not create day-1 investment return.

## Example 2. MWR/XIRR with a mid-period contribution

**Scenario:** A client invests USD 1,000,000 at inception, contributes USD 500,000 halfway through the year, and ends the year with USD 1,560,000.

| Cashflow | Timing | Sign convention |
|---|---:|---:|
| Initial investment | 0.0 years | -1,000,000 |
| Additional contribution | 0.5 years | -500,000 |
| Ending value | 1.0 years | +1,560,000 |

MWR is the annual rate `r` that makes NPV equal zero:

```text
0 = -1,000,000 - 500,000 / (1 + r)^0.5 + 1,560,000 / (1 + r)
r ~= 4.8113%
```

**Reporting interpretation:** MWR reflects the client's money experience, including the timing of the mid-year contribution.

**Implementation pattern:** Store the cashflow date, amount, inclusion policy and day-count convention. If the equation has multiple roots, no stable root, missing valuation, or inconsistent cashflow signs, block or label MWR rather than publishing a misleading result.

## Example 3. Return contribution by asset class

**Scenario:** A portfolio has three asset classes with beginning values and period returns.

| Asset class | Beginning value | Weight | Return | Contribution |
|---|---:|---:|---:|---:|
| Equities | 500,000 | 50.0% | 6.0% | 3.0% |
| Bonds | 400,000 | 40.0% | 1.5% | 0.6% |
| Alternatives | 100,000 | 10.0% | -2.0% | -0.2% |
| Total | 1,000,000 | 100.0% |  | 3.4% |

```text
contribution = beginning_weight x asset_class_return
portfolio_return = sum(contributions) = 3.4%
```

**Advisor explanation:** Equities drove most of the positive return, bonds added modestly, and alternatives detracted.

**QA assertion:** Contribution should reconcile to portfolio return within the configured rounding tolerance.

## Example 4. Single-period Brinson attribution

**Scenario:** Compare a two-asset portfolio with its benchmark.

| Asset class | Portfolio weight | Portfolio return | Benchmark weight | Benchmark return |
|---|---:|---:|---:|---:|
| Equities | 60% | 8% | 50% | 6% |
| Bonds | 40% | 1% | 50% | 2% |

Portfolio return:

```text
(60% x 8%) + (40% x 1%) = 5.2%
```

Benchmark return:

```text
(50% x 6%) + (50% x 2%) = 4.0%
active_return = 5.2% - 4.0% = 1.2%
```

Brinson attribution:

| Effect | Equities | Bonds | Total |
|---|---:|---:|---:|
| Allocation `(Wp - Wb) x (Rb_i - Rb_total)` | 0.2% | 0.2% | 0.4% |
| Selection `Wb x (Rp_i - Rb_i)` | 1.0% | -0.5% | 0.5% |
| Interaction `(Wp - Wb) x (Rp_i - Rb_i)` | 0.2% | 0.1% | 0.3% |
| Total active attribution | 1.4% | -0.2% | 1.2% |

**Reporting interpretation:** The portfolio outperformed by 1.2%. Overweighting equities helped, equity selection helped, bond selection detracted, and the interaction effect was positive.

**Implementation pattern:** Store the attribution model, classification hierarchy, benchmark version, weight basis, return basis and residual policy.

## Example 5. Currency attribution for a non-base-currency asset

**Scenario:** A USD-reporting portfolio holds a EUR asset. The asset rises 4% in EUR terms, while EUR strengthens 5% versus USD.

| Item | Value |
|---|---:|
| Local asset return | 4.0% |
| FX return | 5.0% |
| Base-currency return | `(1 + 4.0%) x (1 + 5.0%) - 1 = 9.2%` |
| Portfolio weight | 30.0% |
| Base-currency contribution | `30.0% x 9.2% = 2.76%` |

Approximate contribution split:

| Component | Contribution |
|---|---:|
| Local return effect | `30.0% x 4.0% = 1.20%` |
| Currency effect | `30.0% x 5.0% = 1.50%` |
| Cross effect | `30.0% x 4.0% x 5.0% = 0.06%` |
| Total | 2.76% |

**Advisor explanation:** Most of the contribution came from both local asset performance and EUR strength. Currency was a material part of the result.

**QA assertion:** Base-currency return must reconcile to local return, FX return and cross effect.

## Example 6. Benchmark rebalancing versus drifted benchmark weights

**Scenario:** A 60/40 benchmark has monthly rebalancing. During month 1, equities return 10% and bonds return -2%.

| Component | Starting weight | Return | Ending value on USD 1,000 benchmark |
|---|---:|---:|---:|
| Equities | 60% | 10% | 660 |
| Bonds | 40% | -2% | 392 |
| Total | 100% | 5.2% | 1,052 |

End-of-month drifted weights:

```text
equity_weight = 660 / 1,052 = 62.74%
bond_weight = 392 / 1,052 = 37.26%
```

If the official benchmark methodology rebalances monthly, the next month should begin at 60/40, not 62.74/37.26.

**Implementation pattern:** Benchmark weights should come from a benchmark source or methodology engine. Do not infer future benchmark weights from stale drifted weights when the benchmark has a defined rebalance policy.

## Example 7. Tracking error and information ratio

**Scenario:** Monthly active returns are:

```text
+0.40%, -0.20%, +0.10%, +0.50%, -0.10%, +0.30%
```

Calculated values:

| Metric | Value |
|---|---:|
| Average monthly active return | 0.1667% |
| Sample monthly tracking error | 0.2805% |
| Annualized tracking error | `0.2805% x sqrt(12) = 0.9716%` |
| Annualized active return | `0.1667% x 12 = 2.0004%` |
| Information ratio | `2.0004% / 0.9716% = 2.06` |

**Reporting interpretation:** A high information ratio over a short six-month sample should be labelled carefully. The platform should show period length and methodology.

**QA assertion:** Tracking error must use active returns, not absolute portfolio returns.

## Example 8. Drawdown and stress loss

**Scenario:** A portfolio has the following month-end values.

| Month | Value | High-water mark | Drawdown |
|---|---:|---:|---:|
| 0 | 1,000,000 | 1,000,000 | 0.00% |
| 1 | 1,080,000 | 1,080,000 | 0.00% |
| 2 | 1,020,000 | 1,080,000 | -5.56% |
| 3 | 1,110,000 | 1,110,000 | 0.00% |
| 4 | 1,000,000 | 1,110,000 | -9.91% |

Maximum drawdown is -9.91%.

Stress example:

| Exposure | Market value | Shock | Stress P&L |
|---|---:|---:|---:|
| Equities | 600,000 | -12% | -72,000 |
| Bonds | 300,000 | -3% | -9,000 |
| Cash | 100,000 | 0% | 0 |
| Total | 1,000,000 |  | -81,000 |

Stress loss is USD 81,000, or -8.1% of portfolio value.

**Advisor explanation:** Drawdown shows historical peak-to-trough experience. Stress loss estimates how the current portfolio might react to a defined scenario.

## Example 9. Composite return and dispersion

**Scenario:** Three discretionary portfolios belong to the same strategy composite.

| Portfolio | Beginning assets | Period return |
|---|---:|---:|
| A | 1,000,000 | 4.0% |
| B | 2,000,000 | 5.0% |
| C | 1,000,000 | 7.0% |

Asset-weighted composite return:

```text
((1,000,000 x 4.0%) + (2,000,000 x 5.0%) + (1,000,000 x 7.0%)) / 4,000,000
= 5.25%
```

Equal-weighted average return is 5.33%. Sample dispersion of member returns is approximately 1.53%.

**Implementation pattern:** Composite reporting should state whether returns are asset-weighted or equal-weighted, which portfolios are included, and whether the composite membership changed during the period.

## Example 10. Degraded analytics state from stale valuation

**Scenario:** A private fund NAV is 45 days old, while the reporting policy allows a maximum stale age of 30 days for normal analytics.

| Output | Expected treatment |
|---|---|
| Holdings report | Show position with valuation date and stale flag. |
| Portfolio total value | Include if policy permits, labelled with stale valuation disclosure. |
| TWR/MWR | Label affected period as stale or partial. |
| Contribution | Show contribution as source-limited if dependent on stale NAV. |
| Risk | Exclude from daily market VaR if no source-backed risk proxy exists, or show proxy limitation. |
| Advisor dashboard | Show remediation task or data-quality exception. |
| Client report | Include methodology disclosure and valuation date. |

**QA assertion:** Stale source data should not silently produce clean analytics. The stale flag must travel from valuation source to reporting output.

## Example 11. Backdated transaction restatement

**Scenario:** A dividend transaction dated two weeks ago is received after a monthly report was generated.

Expected handling:

1. identify the earliest impacted valuation and performance date,
2. recalculate holdings, cash, income, performance and contribution from that date,
3. compare old and new published report values,
4. apply materiality policy,
5. issue a restatement or internal correction note if required,
6. preserve old and new calculation lineage.

**Implementation pattern:** Do not overwrite prior analytics without versioning. Published reports need reproducibility even when corrected data arrives later.

## Example 12. Mandate breach analytics

**Scenario:** A balanced mandate allows equities between 40% and 65%. After a rally, equity exposure reaches 67%.

| Item | Value |
|---|---:|
| Mandate lower bound | 40% |
| Mandate upper bound | 65% |
| Current equity exposure | 67% |
| Breach amount | 2% above upper bound |

Expected workflow:

1. classify the breach as market-movement driven unless trades caused it,
2. show breach age and severity,
3. link exposure to positions and prices,
4. create advisor or portfolio-manager review task,
5. show available remediation options such as rebalance, tolerance approval or client discussion,
6. preserve the mandate version used for the decision.

**QA assertion:** Mandate analytics must use effective-dated mandate rules and exposure definitions, not hard-coded current limits.
