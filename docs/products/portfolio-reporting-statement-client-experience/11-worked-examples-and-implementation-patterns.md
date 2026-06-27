# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns portfolio reporting and client-experience concepts into practical examples for statements, portfolio reviews, advisor dashboards, report generation, QA, audit replay and client explanations.

## Example 1. Month-end statement snapshot

### Scenario

A client statement is generated for 2026-06-30. Source data continues to change after month-end because late trades, price corrections and corporate-action adjustments arrive.

| Data area | Snapshot requirement |
|---|---|
| Positions | Effective as of report cut-off and selected basis. |
| Cash | Ledger, settled or available basis explicitly stated. |
| Prices | Price source, valuation date and quality status stored. |
| FX rates | Reporting FX source and timestamp stored. |
| Transactions | Included by booking, trade, settlement or value date policy. |
| Performance | Methodology, period, benchmark and calculation version stored. |

### Implementation pattern

```text

report_run_id
  -> report_snapshot_id
  -> input data cut
  -> calculation versions
  -> template version
  -> rendered output
  -> archive and approval metadata

```

### QA assertions

| Test | Expected result |
|---|---|
| Price is corrected after report publication | Original statement remains reproducible; restatement workflow decides whether to republish. |
| Report is regenerated one month later | Same snapshot produces same values. |
| Template changed after report date | Archived report uses original template version. |
| Report basis is missing | Publication is blocked or flagged as incomplete. |

## Example 2. Holdings section with clean report labels

### Scenario

A portfolio includes cash, bonds, funds, structured products and private-market holdings.

| Holding | Required reporting label |
|---|---|
| Cash | Currency, ledger/available basis and restrictions. |
| Bond | Nominal, clean/dirty market value policy, accrued interest and yield basis. |
| Fund | Units, NAV, NAV date, share class and dealing restrictions. |
| Structured product | Issuer, payoff type, underlier, observation status and valuation source. |
| Private market fund | Commitment, called capital, NAV date, unfunded commitment and liquidity restrictions. |

### Reporting principle

Do not force every holding into the same generic columns. Use a common summary view plus product-specific detail panels where product meaning would otherwise be lost.

### QA assertions

| Test | Expected result |
|---|---|
| Private-market NAV is 90 days old | NAV date and stale/lagged valuation label appear. |
| Bond accrued interest is material | Report clearly states whether market value includes accrued interest. |
| Structured-product valuation is indicative | Report quality label does not imply executable sale price. |
| Fund share class is missing | Data-quality exception is visible to operations and report QA. |

## Example 3. Activity section: trade, cashflow and income classification

### Scenario

During the month, a client deposits cash, buys a bond, receives coupon income and pays advisory fees.

| Event | Reporting classification |
|---|---|
| Client cash deposit | External contribution |
| Bond purchase | Investment activity / purchase |
| Coupon receipt | Investment income |
| Accrued interest paid on purchase | Accrual settlement, not income loss |
| Advisory fee | Fee/expense |

### Performance implication

The client deposit should affect MWR and capital flow reporting, but should not be treated as investment return. Coupon income is investment return. Fees may be shown gross or net depending methodology.

### QA assertions

| Test | Expected result |
|---|---|
| Bond purchase is classified as external cashflow | Performance QA fails. |
| Coupon is missing from income section | Income reconciliation fails. |
| Advisory fee is omitted from net performance | Net return report fails methodology check. |
| Accrued interest paid is treated as negative income | Income reporting is misleading. |

## Example 4. Performance report explanation

### Scenario

A portfolio return is 2.4% for the quarter, while benchmark return is 1.8%.

| Component | Contribution |
|---|---:|
| Equity selection | +1.10% |
| Bond duration effect | -0.20% |
| FX translation | -0.30% |
| Cash drag | -0.10% |
| Fees | -0.10% |
| Other / residual | +2.00% |

### Client explanation

The report should explain both outcome and drivers. It should distinguish market return, income, FX, fees and residual effects, and it should avoid implying precision when inputs are stale or estimated.

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark is missing | Relative performance is suppressed or flagged. |
| Attribution components do not reconcile | Report QA fails or shows residual explicitly. |
| Fee basis is net but report says gross | Methodology label check fails. |
| FX contribution is unavailable | Explanation uses a degraded-state label. |

## Example 5. Degraded data disclosure

### Scenario

A report includes one stale bond price, one lagged private-market NAV and one missing look-through file.

| Issue | Report treatment |
|---|---|
| Stale bond price | Show valuation date and stale-price warning. |
| Lagged private-market NAV | Show NAV date and lag note. |
| Missing look-through file | Suppress look-through exposure or show coverage gap. |

### Publication decision

| Severity | Example action |
|---|---|
| Low | Publish with internal warning only. |
| Medium | Publish with advisor-visible explanation. |
| High | Publish with client-facing disclosure. |
| Critical | Block publication until resolved. |

### QA assertions

| Test | Expected result |
|---|---|
| Critical price is missing | Report publication is blocked. |
| Stale value is used | Stale flag appears in report dataset and rendered output. |
| Look-through is partial | Exposure report shows coverage percentage. |
| Advisor explanation is required | Report workflow cannot complete without review evidence. |

## Example 6. Restatement workflow

### Scenario

A corporate action correction arrives after statements are delivered. A stock split was processed with the wrong ratio.

### Correct workflow

| Step | Action |
|---|---|
| Detect | Reconciliation or vendor correction identifies wrong ratio. |
| Assess | Determine impacted positions, P&L, performance, reports and clients. |
| Correct | Book reversal/adjustment with lineage. |
| Recalculate | Recompute affected reporting snapshots. |
| Decide | Apply materiality policy for restatement. |
| Publish | Supersede prior statement if needed; never silently overwrite. |

### QA assertions

| Test | Expected result |
|---|---|
| Published report is replaced | Archive keeps old and new report with supersession metadata. |
| Correction is immaterial | Decision and evidence are still recorded. |
| Report values change but no restatement record exists | Audit QA fails. |
| Client portal caches old report | Cache invalidation and version label are tested. |

## Example 7. PDF rendering and portal consistency

### Scenario

The same reporting dataset feeds a PDF statement and a client portal holdings page.

### Control principle

The PDF and portal should not independently calculate report numbers. They should consume the same approved reporting dataset or the same versioned calculation service.

### QA assertions

| Test | Expected result |
|---|---|
| PDF and portal market values differ | Release is blocked unless difference is explained by basis or timing. |
| Long product name overflows PDF table | Rendering QA catches layout failure. |
| Negative cash is hidden in portal | Client-experience QA fails. |
| Disclosure appears in PDF but not portal | Disclosure mapping check fails. |
