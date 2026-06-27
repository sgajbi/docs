# 11 - Worked Examples and Implementation Patterns

Use these examples when modelling private equity, private credit, hedge funds, real assets, commitments, capital calls, distributions, NAV lag, performance, liquidity, advisory, mandate controls, reporting, operations, QA, and implementation boundaries.

## 1. Commitment Ledger Pattern

Private markets need a commitment ledger, not just a holding quantity.

| Field | Meaning |
|---|---|
| committed amount | legal commitment signed by the client |
| called capital | total capital called by the fund |
| paid-in capital | cash actually paid by the client |
| unfunded commitment | remaining commitment still callable |
| recallable capital | distributions that may be recalled |
| NAV/residual value | current manager/admin valuation |
| distributions | cash or stock returned to investor |
| valuation date | date of NAV, which may lag report date |

Core calculation:

```text
unfunded commitment = commitment - called capital + recallable capital
total exposure view = NAV + unfunded commitment
```

## 2. Private Equity Capital Call

Scenario:

- Commitment: USD 1,000,000.
- First capital call: 20%.
- Management fee component: USD 5,000.
- Investment funding component: USD 195,000.

Cashflow:

```text
total cash debit = 200,000
paid-in capital = 200,000
unfunded commitment = 800,000
```

Reporting:

| Item | Treatment |
|---|---|
| Commitment | 1,000,000 |
| Paid-in | 200,000 |
| Unfunded | 800,000 |
| NAV | 0 until first NAV or manager estimate is sourced |
| Fees | show according to administrator notice and reporting policy |

Controls:

- capital call notice must be stored;
- due date must drive cash-planning alerts;
- late payment risk should be visible;
- DPM or advisory workflow should check cash availability before due date;
- capital call should not be treated as a normal fund subscription with immediate NAV certainty.

## 3. Distribution Classification

Scenario:

- Fund distributes USD 120,000.
- Administrator classifies:
  - return of capital: USD 70,000;
  - income: USD 20,000;
  - realized gain: USD 30,000.

Platform treatment:

| Component | Treatment |
|---|---|
| return of capital | reduces invested capital basis where policy applies |
| income | income reporting and possible tax classification |
| realized gain | realized gain reporting |
| cash receipt | increases cash balance |

Do not classify the full distribution as income unless source classification supports it.

QA assertion:

Distribution classification should reconcile to administrator notice, and client reporting should expose unknown classification as source-limited rather than inventing a split.

## 4. NAV Lag And Report Date

Scenario:

- Client report date: 2026-06-30.
- Latest private fund NAV date: 2026-03-31.
- NAV received date: 2026-05-15.

Reporting treatment:

| Date | Meaning |
|---|---|
| report date | client statement as-of date |
| NAV date | valuation effective date |
| received date | when source provided valuation |
| price date | date used by platform valuation record |

Client reporting should show that private-market values may lag public-market values. Performance and allocation calculations should preserve valuation-date metadata.

Implementation requirement:

Do not overwrite NAV date with report date just to make reports look current.

## 5. NAV Restatement

Scenario:

- Original Q1 NAV: USD 950,000.
- Restated Q1 NAV: USD 925,000.
- Difference: -25,000.

Controls:

- store original and restated values;
- preserve received dates and source documents;
- decide whether reports already published need correction notice;
- recalculate performance for affected periods;
- preserve audit trail from source notice to client report impact.

QA scenarios:

| Scenario | Expected behavior |
|---|---|
| restatement before report publication | use restated NAV and retain superseded value |
| restatement after report publication | correction workflow or next-report disclosure |
| missing reason code | exception queue |
| conflicting administrator files | block automatic overwrite |

## 6. Partial-History IRR

Scenario:

- Migration imports current NAV and recent cashflows only.
- Original subscription and early capital calls are missing.

Correct posture:

- do not publish full-life IRR;
- show partial-history return as support-limited if methodology allows;
- preserve missing-history flag;
- create data-remediation task for historical cashflows.

Required source inputs for full-life IRR:

1. dated contributions;
2. dated distributions;
3. current terminal NAV;
4. fees and recallable treatment if included in methodology;
5. valuation date and currency basis.

## 7. Private Credit Interest And Principal Repayment

Scenario:

- Private credit fund distributes USD 50,000.
- Administrator classifies USD 30,000 as interest income and USD 20,000 as principal repayment.

Treatment:

| Component | Reporting |
|---|---|
| interest income | income section, tax category where available |
| principal repayment | capital return / realized cashflow |
| NAV impact | may reduce NAV or be separate depending on administrator accounting |
| performance | use source classification to avoid overstating yield |

Advisory note:

Private credit distributions can look like stable income, but credit losses, refinancing risk, illiquidity, valuation lag and manager marks still matter.

## 8. Hedge Fund Redemption Gate

Scenario:

- Client requests redemption of USD 500,000.
- Fund applies 25% gate.
- USD 125,000 accepted, USD 375,000 deferred.

Platform treatment:

```text
accepted redemption = 500,000 x 25% = 125,000
deferred redemption = 375,000
```

Required states:

- redemption requested;
- accepted amount;
- deferred amount;
- expected settlement date;
- remaining exposure;
- side-pocket or holdback if applicable.

Do not show the full redemption as available cash until settlement is confirmed.

## 9. Secondary Sale Of Fund Interest

Scenario:

- NAV: USD 1,000,000.
- Secondary buyer price: 92% of NAV.
- Sale proceeds before fees: USD 920,000.

Calculation:

```text
discount to NAV = 1,000,000 - 920,000 = 80,000
discount percentage = 8%
```

Implementation considerations:

- sale may require GP approval;
- unfunded commitment transfer must be explicit;
- buyer/seller economic date may differ from settlement date;
- realized P&L and remaining commitment must be recalculated;
- report should explain discount to latest NAV and NAV date.

## 10. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Client objective | long-term growth, income, diversification, inflation hedge, private credit yield, or opportunistic exposure? |
| Liquidity | lock-up, gates, notice period, redemption frequency, side pockets, secondary market? |
| Cash planning | capital calls, unfunded commitment, recallable capital, expected distributions? |
| Valuation | manager/admin NAV, appraisal, model, stale, restated, or estimated? |
| Performance | IRR, TVPI, DPI, RVPI, PME, time-weighted approximation, partial-history state? |
| Concentration | manager, strategy, vintage, geography, sector, currency, liquidity bucket? |
| Suitability | qualified investor status, knowledge, horizon, concentration, liquidity need? |
| DPM mandate | max alternatives, max illiquid assets, unfunded commitment treatment, cash reserve? |

## 11. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Commitment tracking | commitment, paid-in, unfunded, recallable, NAV, distributions | cash-call forecasting and liquidity stress |
| Capital calls | notice, due date, amount, cash posting, commitment update | automated notice ingestion and approval workflow |
| NAV valuation | NAV, valuation date, received date, source quality | restatement lineage dashboard and valuation challenge workflow |
| Performance | IRR/multiples when full cashflow history exists | PME, vintage benchmarking and factor decomposition |
| Liquidity state | lock-up, notice period, gate, queue, side-pocket label | redemption queue simulation |
| Reporting | commitment/NAV/unfunded/distribution labels with source dates | client-facing private-market cashflow projection |

## 12. Regression Test Pack

Minimum release-gate scenarios:

1. Capital call updates paid-in and unfunded commitment.
2. Capital call due date triggers cash-planning alert.
3. Distribution classification separates income, gain and return of capital.
4. NAV lag appears in client report with valuation date.
5. NAV restatement preserves original value and recalculates impacted analytics.
6. Partial-history IRR is blocked or labelled support-limited.
7. Private credit distribution separates interest from principal repayment.
8. Hedge fund gate accepts partial redemption and leaves deferred amount.
9. Secondary sale transfers unfunded commitment and realizes discount to NAV.
10. Missing administrator notice blocks source-backed lifecycle update.
