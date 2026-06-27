# 04 - Benchmarks, Composites, Mandates and Relative Analytics

## 1. Why benchmarks matter

A return number is incomplete without context. A portfolio that returned 6% may be good if the benchmark returned 2%, but poor if the benchmark returned 12%.

Benchmarks support:

- Performance comparison
- DPM mandate evaluation
- Investment strategy monitoring
- Relative risk measurement
- Attribution
- Client reporting
- Fee/performance review
- Governance and oversight

## 2. Benchmark types

| Benchmark type | Example | Use |
|---|---|---|
| Market index | MSCI World, S&P 500, Bloomberg Global Aggregate | Compare to market segment |
| Blended benchmark | 60% equity index + 40% bond index | Multi-asset portfolios |
| Custom benchmark | Client-specific allocation | Advisory/DPM mandates |
| Peer group | Similar funds/managers | Fund/manager selection |
| Liability benchmark | Future cashflow needs | Pension/liability-driven portfolios |
| Absolute return target | Cash + 3%, SOFR + spread | Hedge funds/absolute return |
| Inflation-linked target | CPI + 4% | Real return mandates |
| Cash benchmark | SORA/SOFR/T-bills | Liquidity portfolios |
| ESG/sustainable benchmark | Paris-aligned index | Sustainable mandates |

## 3. Good benchmark characteristics

A good benchmark should be:

| Characteristic | Meaning |
|---|---|
| Unambiguous | Securities/weights/rules are clear |
| Investable | Can reasonably be replicated or approximated |
| Measurable | Returns are available and reliable |
| Appropriate | Matches mandate objective and risk |
| Specified in advance | Not chosen after results are known |
| Owned/governed | Has clear administrator and methodology |
| Effective-dated | Changes are tracked over time |

## 4. Benchmark governance

Benchmark governance should cover:

- Who selects benchmark
- Who approves benchmark
- Effective date
- Change reason
- Methodology version
- Data source
- Holiday/calendar treatment
- Currency treatment
- Rebalancing frequency
- Fees/tax basis
- Benchmark retirement/replacement process

A benchmark change can materially affect relative performance and attribution. It should be auditable.

## 5. Benchmark mapping

Mapping can happen at several levels:

| Level | Example |
|---|---|
| Portfolio | Balanced mandate benchmark |
| Sleeve | Equity sleeve mapped to MSCI World |
| Asset class | Fixed income mapped to bond index |
| Security | Fund mapped to fund benchmark |
| Client goal | Education goal mapped to inflation + return target |
| Composite | Strategy composite benchmark |

## 6. Blended benchmark construction

Example 60/40 benchmark:

| Component | Weight | Index |
|---|---:|---|
| Global equity | 60% | MSCI World |
| Global bonds | 40% | Bloomberg Global Aggregate |

Daily blended return:

```text
Benchmark Return = 60% x Equity Index Return + 40% x Bond Index Return
```

If benchmark is rebalanced monthly, daily weights drift inside the month and reset at month-end. If rebalanced daily, weights reset every day.

The rebalancing rule must be stored.

## 7. Dynamic and effective-dated benchmarks

A mandate may change from balanced to growth. The benchmark must be effective-dated:

| Date range | Benchmark |
|---|---|
| 2024-01-01 to 2024-12-31 | 50/50 Balanced Benchmark |
| 2025-01-01 onward | 70/30 Growth Benchmark |

Do not use the latest benchmark for historical periods before the benchmark became effective.

## 8. Benchmark currency

Benchmark currency must align to reporting purpose.

| Case | Treatment |
|---|---|
| Portfolio base USD, index USD | Direct comparison |
| Portfolio base SGD, index USD | Convert benchmark return to SGD or use SGD index version |
| Hedged mandate | Use hedged benchmark where appropriate |
| Multi-currency mandate | Use currency policy benchmark |
| Local-market sleeve | Use local index in local currency for local manager skill |

## 9. Price return versus total return benchmarks

| Benchmark type | Includes dividends/coupons? | Use |
|---|---:|---|
| Price return | No | Price-only comparison; usually incomplete for total portfolio |
| Gross total return | Includes income before withholding tax | Institutional/gross basis |
| Net total return | Includes income after withholding tax assumptions | More realistic for cross-border investors |
| Hedged total return | Includes currency hedge effect | Currency-hedged mandates |

Portfolio total return should usually be compared with total return benchmarks, not price return benchmarks.

## 10. Relative analytics

| Metric | Formula / Meaning |
|---|---|
| Active return | Portfolio return - benchmark return |
| Excess return | Often same as active return; sometimes return over cash |
| Tracking error | Volatility of active return |
| Information ratio | Active return / tracking error |
| Beta | Sensitivity to benchmark return |
| Alpha | Return unexplained by beta exposure |
| Up capture | Portfolio performance when benchmark is up |
| Down capture | Portfolio performance when benchmark is down |
| Hit ratio | Percentage of periods outperforming benchmark |
| Max relative drawdown | Worst underperformance from peak relative value |

## 11. Composite performance

A composite is an aggregation of portfolios managed according to a similar strategy or mandate.

Uses:

- Strategy track record
- Manager performance reporting
- Model portfolio comparison
- DPM mandate reporting
- GIPS-style performance presentation
- Sales and proposal material

## 12. Composite membership

A composite requires rules:

| Rule | Example |
|---|---|
| Inclusion criteria | Discretionary balanced portfolios |
| Minimum size | At least USD 500,000 |
| Start timing | Include after first full month |
| Exclusion criteria | Significant restriction / non-discretionary |
| Termination timing | Remove after mandate termination |
| Cashflow policy | Large flows handled consistently |
| Fee basis | Gross or net |
| Currency basis | Composite base currency |

## 13. Composite return calculation

Common methods:

| Method | Use |
|---|---|
| Asset-weighted portfolio returns | Standard composite return approach |
| Equal-weighted portfolio returns | Less common; useful for client-count perspective |
| Model portfolio return | Used when actual portfolios differ |
| Carve-out/sleeve return | Used for asset-class sleeves, with controls |

Composite return should be reproducible and membership changes must be versioned.

## 14. Mandate analytics

Mandates define what the portfolio is allowed or intended to do.

Common mandate rules:

| Rule type | Example |
|---|---|
| Asset allocation | Equity 40-60%, bonds 30-50%, cash 0-15% |
| Currency | Non-base currency exposure max 30% |
| Concentration | Single issuer max 10% |
| Product eligibility | No derivatives / no complex products |
| Credit quality | Minimum average rating A- |
| Duration | Portfolio duration 3-7 years |
| Liquidity | Minimum 5% cash or near-cash |
| Alternatives | Max 20% private markets |
| Leverage | No borrowing or max loan-to-value |
| ESG restrictions | Exclude certain sectors/issuers |
| Income target | Expected yield at least 4% |
| Drawdown control | Alert if drawdown exceeds threshold |

## 15. Target, actual and drift

Mandate analytics often compares target allocation with actual allocation.

| Asset class | Minimum | Target | Maximum | Actual | Status |
|---|---:|---:|---:|---:|---|
| Equities | 40% | 50% | 60% | 63% | Breach |
| Bonds | 30% | 40% | 50% | 28% | Breach |
| Cash | 0% | 5% | 15% | 6% | OK |
| Alternatives | 0% | 5% | 15% | 3% | OK |

Drift:

```text
Drift = Actual Weight - Target Weight
```

## 16. Mandate breach lifecycle

| Stage | Description |
|---|---|
| Pre-trade check | Order is tested before execution |
| Warning | Portfolio close to limit |
| Breach | Limit exceeded |
| Grace period | Allowed temporary breach window |
| Remediation | Rebalance / exception approval |
| Waiver | Approved exception |
| Closure | Breach resolved |

Mandate breaches should be effective-dated and auditable.

## 17. Advisory versus DPM benchmarks

| Topic | Advisory | DPM |
|---|---|---|
| Decision maker | Client decides, advisor recommends | Manager has discretion |
| Benchmark use | Advisory context and suitability | Formal performance evaluation |
| Mandate rule strictness | Often guidance/eligibility | Formal limits and investment policy |
| Rebalancing | Client consent often needed | Manager can act within mandate |
| Reporting | Advisor explains options | Manager explains actions and results |

## 18. Benchmark data model

Core objects:

```text
Benchmark
BenchmarkComponent
BenchmarkReturnSeries
BenchmarkConstituent
PortfolioBenchmarkAssignment
MandateBenchmarkAssignment
BenchmarkPolicy
BenchmarkVersion
```

Essential fields:

| Field | Purpose |
|---|---|
| benchmark_id | Unique benchmark |
| benchmark_type | Index/blended/custom/absolute |
| currency | Benchmark currency |
| return_type | Price/gross total/net total/hedged |
| administrator | Index provider/admin |
| methodology_version | Governance |
| effective_from/to | Historical accuracy |
| component_weight | Blended benchmark calculation |
| rebalance_frequency | Daily/monthly/quarterly/static |
| data_source | Vendor/source |

## 19. Benchmark edge cases

| Edge case | Treatment |
|---|---|
| Missing benchmark return | Use fallback or mark unavailable |
| Benchmark holiday differs from portfolio | Calendar alignment policy |
| Benchmark discontinued | Effective-dated replacement |
| Benchmark currency differs | Convert with approved FX rates |
| New portfolio with no benchmark | Use mandate default or no relative reporting |
| Client changes mandate mid-period | Split period by effective date |
| Private-market benchmark unavailable | Use vintage/PME/peer group cautiously |
| Structured product benchmark mismatch | Use underlying/reference or cash-plus benchmark depending purpose |

## 20. Expert summary

Benchmarks and mandates turn raw performance into judgment. The platform must know what the portfolio was supposed to do, which benchmark applied at that time, how the benchmark was constructed, and whether the portfolio remained within approved limits. Without effective-dated benchmark and mandate governance, relative analytics can become misleading.
