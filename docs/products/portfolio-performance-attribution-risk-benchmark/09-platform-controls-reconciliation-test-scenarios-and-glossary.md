# 09 - Platform Controls, Reconciliation, Test Scenarios and Glossary

## 1. Control objective

Portfolio analytics is high-risk because clients, advisors, portfolio managers and regulators rely on the output. Controls must ensure that analytics are complete, accurate, timely, explainable and reproducible.

## 2. Key reconciliations

| Reconciliation | Purpose |
|---|---|
| Position reconciliation | Platform positions match custodian/core records |
| Cash reconciliation | Cash balances by currency match source systems |
| Transaction reconciliation | All trades/income/events loaded once |
| Valuation reconciliation | Market value matches source/custodian tolerance |
| FX reconciliation | FX rates match approved source |
| Income reconciliation | Dividends/coupons/distributions match expected/received |
| Corporate action reconciliation | Entitlements and positions adjusted correctly |
| Benchmark reconciliation | Benchmark returns match vendor/index provider |
| Performance reconciliation | Return reconciles to value movement and cashflows |
| Contribution reconciliation | Contributions sum to total return within tolerance |
| Attribution reconciliation | Effects reconcile to active return |
| Mandate reconciliation | Exposure and classifications align with rule engine |
| Report reconciliation | PDF/API/dashboard show same official numbers |

## 3. Daily controls

- Missing price check
- Stale price ageing
- Missing FX check
- Negative/zero market value exceptions
- Unrealistic return outlier detection
- Large cashflow detection
- Large P&L movement detection
- Corporate action pending check
- Benchmark return missing check
- Classification missing check
- Mandate breach generation
- Recalculation job failure check
- Report generation status check

## 4. Tolerance examples

| Check | Example tolerance |
|---|---|
| Market value reconciliation | Absolute and percentage tolerance |
| Contribution sum | +/- 1 basis point |
| Attribution sum | +/- 1-5 basis points depending method |
| FX translation | Minor rounding tolerance |
| Price variance | Alert if daily price move > threshold |
| Private NAV age | Alert if NAV older than policy threshold |
| Benchmark return | Exact or small tolerance versus vendor |
| Cash balance | Exact or currency-specific small tolerance |

## 5. Data quality flags

| Flag | Meaning |
|---|---|
| MISSING_PRICE | No valid price for valuation date |
| STALE_PRICE | Price older than allowed threshold |
| MISSING_FX | No FX rate for conversion |
| UNMAPPED_ASSET_CLASS | Instrument lacks classification |
| UNMAPPED_BENCHMARK | Portfolio lacks benchmark |
| STALE_NAV | Fund/private NAV outdated |
| MODEL_PRICE | Valuation is model-based |
| ISSUER_PRICE | Structured product price from issuer |
| ESTIMATED_VALUE | Value is estimate, not final |
| BACKDATED_ACTIVITY | Historical period changed |
| PERFORMANCE_RESTATED | Return changed after report |
| RESIDUAL_HIGH | Contribution/attribution residual high |

## 6. Test scenario set: performance

| Scenario | Expected result |
|---|---|
| No cashflow, price up | Return equals price movement |
| External deposit mid-period | TWR neutralizes deposit timing |
| Withdrawal before loss | TWR reflects asset performance, MWR reflects client timing |
| Dividend paid | Income contributes to return |
| Coupon accrued and paid | No double counting between accrual and payment |
| Fee paid | Gross and net returns differ correctly |
| Withholding tax | Net income and tax reporting align |
| Multi-currency asset | Base return decomposes into local + FX effect |
| Zero beginning value | Return handled per policy |
| Backdated buy | Historical returns recalculated from impact date |
| Late price correction | Return and report restatement handled correctly |

## 7. Test scenario set: contribution and attribution

| Scenario | Expected result |
|---|---|
| Single asset portfolio | Contribution equals portfolio return |
| Multi-asset portfolio | Contributions sum to total return |
| Missing classification | Unclassified bucket used, alert raised |
| Multi-period attribution | Smoothed effects reconcile to active return |
| FX attribution | Local/FX/interaction reconcile to base return |
| Benchmark missing return | Attribution not calculated or fallback applied |
| Benchmark change mid-period | Period split by effective date |
| Portfolio security not in benchmark | Selection/active weight handled per method |
| Fee included in portfolio but benchmark gross | Active return basis disclosed |
| High residual | Residual shown and exception generated |

## 8. Test scenario set: risk and exposure

| Scenario | Expected result |
|---|---|
| Equity concentration | Single security alert generated |
| Bond duration breach | Duration limit breach generated |
| Structured note issuer concentration | Issuer exposure includes notes |
| Worst-of note underlying exposure | Look-through shows all underlyings |
| FX forward hedge | Net currency exposure reduced |
| Futures exposure | Notional exposure shown despite small cash margin |
| Private fund unfunded commitment | Liquidity planning includes unfunded |
| Pledged collateral | Liquidity/availability adjusted |
| Loan drawdown | LTV and leverage updated |
| Option expiring | Exposure drops or exercise transaction generated |

## 9. Test scenario set: reporting

| Scenario | Expected result |
|---|---|
| Client report generated | Snapshot stored |
| Report regenerated later | Same numbers if using frozen snapshot |
| Report after correction | Restatement process creates new version |
| Dashboard and PDF | Values match official API |
| Stale NAV holding | Disclosure appears |
| Model-priced note | Valuation caveat appears |
| Mandate breach | Report shows breach or advisor-only flag per policy |
| Benchmark unavailable | Relative section suppressed or caveated |
| Client has trust hierarchy | Aggregation respects ownership/access rules |
| Multi-currency report | Base and local currency values consistent |

## 10. Operational runbook checklist

When analytics numbers look wrong:

1. Check portfolio/account scope.
2. Check date range and period definition.
3. Check opening and closing market values.
4. Check external cashflows.
5. Check prices and NAVs.
6. Check FX rates.
7. Check income events.
8. Check corporate actions.
9. Check fees/taxes.
10. Check benchmark mapping.
11. Check classification mapping.
12. Check calculation policy.
13. Check prior restatements/backdated activity.
14. Recalculate from earliest impacted date.
15. Compare with custodian/source report.
16. Explain delta and update report if needed.

## 11. Governance controls

| Control | Purpose |
|---|---|
| Calculation policy approval | Prevent uncontrolled methodology changes |
| Benchmark approval workflow | Prevent cherry-picked benchmarks |
| Manual price override approval | Control valuation risk |
| Report restatement governance | Control client communication |
| Mandate waiver approval | Controlled breach exceptions |
| Data lineage logging | Auditability |
| Access control | Prevent unauthorized client/entity views |
| Versioned methodology | Reproducible calculations |
| Regression test suite | Prevent calculation defects |
| Golden test portfolios | Validate known expected results |

## 12. Glossary

| Term | Meaning |
|---|---|
| Active return | Portfolio return minus benchmark return |
| Alpha | Return unexplained by benchmark exposure |
| Attribution | Explanation of active return |
| Benchmark | Reference index/portfolio used for comparison |
| Beta | Sensitivity to benchmark/market |
| Carino smoothing | Method for linking attribution effects across periods |
| Composite | Group of portfolios managed to same strategy |
| Contribution | Portion of portfolio return from holding/category |
| CVaR | Expected loss beyond VaR threshold |
| Dirty price | Bond price including accrued interest |
| Drawdown | Decline from previous peak |
| External cashflow | Client capital entering/leaving portfolio |
| Gross exposure | Long exposure plus absolute short exposure |
| Gross return | Return before selected fees/taxes |
| Information ratio | Active return divided by tracking error |
| MWR | Money-weighted return / IRR |
| Net exposure | Long exposure minus short exposure |
| Net return | Return after selected fees/taxes |
| Residual | Unallocated effect due to rounding/timing/compounding/etc. |
| Sharpe ratio | Excess return per unit of volatility |
| Sortino ratio | Excess return per unit of downside deviation |
| TWR | Time-weighted return |
| Tracking error | Volatility of active return |
| VaR | Value at Risk |
| XIRR | IRR using actual cashflow dates |

## 13. Interview-ready explanation

> Portfolio analytics is the controlled layer that converts holdings, transactions, prices, FX, benchmarks and mandates into performance, attribution, risk and reporting insight. TWR is used to evaluate strategy or manager performance because it neutralizes external cashflow timing, while MWR reflects the client's actual money experience. Contribution explains what drove total return; attribution explains why the portfolio performed differently from the benchmark. Risk analytics must include not just volatility but also drawdown, concentration, liquidity, leverage, issuer/counterparty exposure and stress scenarios. From a platform perspective, the most important design choices are transaction classification, valuation source control, benchmark governance, effective-dated classifications, recalculation lineage and report reproducibility.

## 14. Expert summary

Controls are not an afterthought in analytics. They are part of the product. Without reconciliation, lineage, data-quality flags, methodology governance and test portfolios, performance and risk numbers can become untrusted. A strong platform makes every number explainable from source transaction to final report.
