# 07 - Performance, Attribution, Risk, Benchmark and Reporting Worked Examples

## 1. Simple TWR with external cashflow

### Business scenario

Portfolio starts at USD 100,000. Before external cashflow, value grows to USD 105,000. Client contributes USD 50,000. End value is USD 160,000.

### Sub-period 1

```text
R1 = (105,000 - 100,000) / 100,000 = 5%
```

### Sub-period 2

After contribution, starting value is 155,000.

```text
R2 = (160,000 - 155,000) / 155,000 = 3.2258%
```

### Linked TWR

```text
TWR = (1 + 5%) x (1 + 3.2258%) - 1
    = 8.3871%
```

### QA assertions

- External contribution does not itself create return.
- Return is geometrically linked.
- Cashflow timing is explicit.

## 2. MWR / investor experience intuition

### Scenario

Investor contributes a large amount before a loss. MWR may be lower than TWR because more capital experienced the loss.

### Reporting explanation

TWR measures portfolio/manager performance. MWR measures investor-specific experience based on timing and size of cashflows.

### QA assertions

- MWR uses dated cashflows and ending value.
- TWR and MWR are not expected to match.
- Reporting labels explain difference.

## 3. Contribution by asset class

### Business scenario

Portfolio has:

| Asset class | Beginning weight | Return |
|---|---:|---:|
| Equity | 60% | 10% |
| Bonds | 40% | 2% |

### Contribution

```text
Equity contribution = 60% x 10% = 6.0%
Bond contribution = 40% x 2% = 0.8%
Total simple contribution = 6.8%
```

### QA assertions

- Contribution sums to total only under simple assumptions.
- Residual/non-allocable effect may appear with cashflows, multi-period linking or rounding.
- Reporting should explain residual if shown.

## 4. Brinson attribution simple example

### Scenario

| Segment | Portfolio weight | Benchmark weight | Portfolio return | Benchmark return |
|---|---:|---:|---:|---:|
| Equity | 70% | 60% | 8% | 6% |
| Bonds | 30% | 40% | 2% | 3% |

### Allocation effect simplified

```text
Equity allocation = (70%-60%) x 6% = 0.6%
Bonds allocation = (30%-40%) x 3% = -0.3%
Total allocation = 0.3%
```

### Selection effect simplified

```text
Equity selection = 70% x (8%-6%) = 1.4%
Bonds selection = 30% x (2%-3%) = -0.3%
Total selection = 1.1%
```

### QA assertions

- Methodology is explicitly defined.
- Interaction effect treatment is configurable.
- Weights and returns use consistent timing.

## 5. Relative performance

### Business scenario

Portfolio return 8%. Benchmark return 6%.

```text
Arithmetic excess return = 8% - 6% = 2%
Geometric relative return = (1.08 / 1.06) - 1 = 1.8868%
```

### QA assertions

- Arithmetic and geometric relative return are labelled.
- Report does not mix methodologies without explanation.

## 6. Volatility example

### Scenario

Monthly returns: 1%, -2%, 3%, 0%.

### Simplified treatment

Calculate standard deviation of returns and annualize if appropriate.

### QA assertions

- Annualization frequency is correct.
- Insufficient history is flagged.
- Outliers and missing returns are handled by policy.

## 7. Maximum drawdown example

### Portfolio values

| Date | Value |
|---|---:|
| Day 1 | 100 |
| Day 2 | 110 |
| Day 3 | 90 |
| Day 4 | 95 |
| Day 5 | 120 |

Peak before drawdown is 110. Trough is 90.

```text
Drawdown = (90 - 110) / 110 = -18.18%
```

### QA assertions

- Drawdown uses peak-to-trough.
- Recovery date is tracked if available.
- Cashflows should be handled appropriately depending methodology.

## 8. Reporting exception example

### Scenario

Private market fund NAV is 120 days old.

### Expected report treatment

- show market value as latest available NAV
- flag stale valuation
- show NAV date
- exclude from daily volatility if unsupported
- explain limitation

### QA assertions

- Report does not silently present stale NAV as current.
- Analytics limitations are visible.
- Advisor dashboard shows exception.

## 9. Key takeaway

Analytics examples must define methodology, timing, inputs, exclusions and reporting labels.
