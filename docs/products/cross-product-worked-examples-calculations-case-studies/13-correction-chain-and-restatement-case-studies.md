# 13 - Correction Chain and Restatement Case Studies

## 1. Purpose

Correction chains are among the hardest areas to model in a wealth platform because they cut across product events, cash movements, accounting entries, valuation snapshots, tax lots, client reports, reconciliation breaks and operational evidence.

Use these case studies when designing or testing:

- reversal and replacement transactions,
- amended source files,
- duplicate-suppression logic,
- report restatements,
- cash and product leg linkage,
- reconciliation break closure,
- client/advisor explanation,
- QA golden scenarios.

These examples should be used with [`../cross-product-transaction-position-data-model.md`](../cross-product-transaction-position-data-model.md).

## 2. Correction Chain Model

Every correction chain should preserve the original economic record and create explicit correction evidence. Do not overwrite history.

| Concept | Required Treatment |
|---|---|
| Original record | Keep the first posted transaction, event, valuation, report or source record. |
| Reversal record | Use when the original economic effect must be negated. Link to the original record. |
| Replacement record | Use when corrected economics must be posted after reversal or adjustment. |
| Amendment record | Use when source terms or disclosure change without a new economic posting. |
| Restatement version | Increment when historical portfolio, performance, tax or report truth changes. |
| Lifecycle event group | Link announcements, corrections, elections, postings, reversals and report impacts. |
| Cash/product linkage | Link product-side and cash-side records through a common economic event or transaction group. |
| Reconciliation break | Keep the break visible until source, book, cash, position, lot and report evidence agree. |
| Supportability state | Mark impacted views as complete, partial, stale, unreconciled, break-open, blocked or unknown. |

## 3. Common Correction Types

| Correction Type | Typical Products | Common Records |
|---|---|---|
| Duplicate event suppression | Corporate actions, cash sweeps, fund notices, report delivery | Duplicate source event, suppression decision, no-op audit record, reconciliation evidence. |
| Reversal and replacement | Trades, income, fees, tax, FX, corporate actions | Original transaction, reversal transaction, replacement transaction, cash legs, tax-lot update. |
| Amended statement or report | Client reports, tax packs, fund statements, ABS trustee reports | Original report, amended dataset, restatement version, corrected report, archive and delivery evidence. |
| Source notice withdrawal | Calls, tenders, payment notices, proxy votes, corporate actions | Original lifecycle event, withdrawal event, affected posting assessment, disclosure update. |
| Appeal or waiver reversal | Collateral, product governance, cash restrictions, entitlements | Original decision, appeal event, decision reversal, control evidence, client/advisor notification. |
| Late source file | Prices, NAVs, FX, tax breakdowns, corporate actions | Late source batch, stale/degraded state, recalculation, restatement or no-restatement decision. |

## 4. Case Study A - Bond Call Reinstatement Duplicate Correction

### Business scenario

A callable bond has a call notice for 40% of outstanding nominal. The custodian sends the call notice twice. The first notice was processed; the second was incorrectly ingested as a new call event. Operations later receives a corrected instruction confirming only one 40% call should apply.

### Starting data

| Field | Value |
|---|---|
| Portfolio | P-100 |
| Instrument | BOND-CALL-2030 |
| Original nominal | USD 500,000 |
| Call percentage | 40% |
| Call price | 101.00 |
| Accrued interest at call | USD 4,250 |
| Original event id | CA-CALL-001 |
| Duplicate event id | CA-CALL-001-DUP |

### Expected lifecycle chain

```text
call announcement received
-> call event processed
-> product nominal reduced by 200,000
-> principal and accrued cash posted
-> duplicate call notice ingested
-> duplicate detected after reconciliation break
-> duplicate event suppressed or reversed
-> corrected position and cash certified
-> report restatement assessed
```

### Expected records

| Record | Type | Expected Treatment |
|---|---|---|
| Original call event | lifecycle event | Remains authoritative. |
| Original principal redemption | transaction | Reduces nominal by USD 200,000. |
| Original cash receipt | cash leg | Posts USD 202,000 principal proceeds plus USD 4,250 accrued interest. |
| Duplicate event | lifecycle event | Marked duplicate with source evidence. |
| Duplicate posting if already booked | reversal transaction | Reverse second nominal reduction and cash receipt. |
| Replacement posting | transaction | Not needed if original posting was correct. |
| Reconciliation break | control record | Open while book/custodian nominal differs; close only after reversal and certification. |
| Report impact | report assessment | Restate client report only if duplicate affected a published report. |

### Position and cash impact

Correct final position:

| Measure | Expected |
|---|---:|
| Final nominal | USD 300,000 |
| Principal cash received | USD 202,000 |
| Accrued interest received | USD 4,250 |
| Duplicate economic impact | zero after reversal or suppression |

### QA assertions

- The duplicate notice cannot create a second economic call after duplicate detection.
- Reversal links point to the duplicate posting, not the valid original posting.
- Final nominal reconciles to custodian and source call terms.
- Cash legs reverse exactly when duplicate cash postings occurred.
- Client reporting shows either no issue or a corrected version with explanation if a report was published.

## 5. Case Study B - Cash Sweep Duplicate Suppression Appeal

### Business scenario

An overnight cash sweep moved idle USD cash into a liquidity fund. The sweep provider sent two files with the same business event but different file identifiers. The platform suppressed the second file as duplicate. Operations later receives an appeal because the second file contains a valid fee correction line.

### Starting data

| Field | Value |
|---|---|
| Portfolio | P-100 |
| Cash account | USD-CASH-01 |
| Sweep instrument | MMF-SWEEP-USD |
| Sweep amount | USD 250,000 |
| Original fee | USD 12.50 |
| Corrected fee | USD 11.75 |

### Expected lifecycle chain

```text
sweep file received
-> cash outflow and sweep-fund subscription posted
-> fee posted
-> second file suppressed as duplicate
-> appeal identifies valid fee amendment
-> no duplicate sweep subscription posted
-> fee adjustment posted
-> cash, fee and report totals certified
```

### Expected records

| Record | Type | Expected Treatment |
|---|---|---|
| Original sweep | transaction group | Cash outflow and MMF subscription remain valid. |
| Original fee | fee transaction | Retained as originally booked. |
| Duplicate sweep line | source event | Suppressed as duplicate with evidence. |
| Fee correction line | amendment plus adjustment transaction | Post only USD 0.75 fee reversal, not a second sweep. |
| Cash correction | cash leg | Credit USD 0.75 back to cash if original fee was too high. |
| Reconciliation evidence | control record | Confirms sweep units, cash and fee totals. |

### Position and cash impact

| Measure | Expected |
|---|---:|
| Sweep fund subscription | USD 250,000 |
| Net corrected fee | USD 11.75 |
| Fee reversal | USD 0.75 |
| Duplicate sweep amount | zero |

### QA assertions

- Duplicate suppression applies at business-event level, not blindly at file level.
- A valid amendment inside a duplicate-looking file can be extracted under controlled workflow.
- The fee adjustment does not repost the sweep subscription.
- Available cash reflects the corrected fee.
- Statement activity explains sweep subscription and fee correction separately.

## 6. Case Study C - Derivative Futures Delivery Invoice Reversal

### Business scenario

A commodity futures contract goes to delivery. The exchange sends a delivery invoice using the wrong quality grade. The original invoice is posted, then reversed after an appeal. A corrected invoice is received with a different cash amount and delivery-grade label.

### Starting data

| Field | Value |
|---|---|
| Portfolio | P-100 |
| Contract | FUT-COMM-MAR |
| Contracts delivered | 5 |
| Original invoice | USD 410,000 |
| Corrected invoice | USD 402,500 |
| Grade adjustment | USD -7,500 |

### Expected lifecycle chain

```text
delivery notice received
-> original invoice posted
-> cash payable created
-> delivery grade appealed
-> original invoice reversed
-> corrected invoice posted
-> delivery exposure, cash payable and report label updated
```

### Expected records

| Record | Type | Expected Treatment |
|---|---|---|
| Delivery notice | lifecycle event | Links contract, delivery date, grade and warehouse/location data. |
| Original invoice | transaction | Creates payable and delivery-related cost. |
| Appeal event | lifecycle event | Does not change economics until accepted. |
| Invoice reversal | reversal transaction | Reverses USD 410,000 payable and cost. |
| Corrected invoice | replacement transaction | Posts USD 402,500 payable and corrected grade label. |
| Reporting adjustment | reporting snapshot | Updates activity, exposure and delivery disclosure. |

### Position, cash and risk impact

| Measure | Expected |
|---|---:|
| Net payable after correction | USD 402,500 |
| Cash improvement versus original | USD 7,500 |
| Delivered exposure | unchanged unless delivery quantity changes |
| Basis/quality risk label | corrected to accepted grade |

### QA assertions

- Appeal event alone does not change cash or position.
- Original invoice and corrected invoice remain visible in audit history.
- Replacement transaction is linked to both original invoice and appeal event.
- Margin, exposure and cash forecast use corrected payable after acceptance.
- Report output explains the delivery invoice correction without hiding the original posting.

## 7. Case Study D - Amended Client Statement After Late Tax Classification

### Business scenario

A monthly statement was published with a fund distribution classified entirely as income. Two weeks later, the fund administrator sends an amended tax breakdown: 70% income, 30% return of capital. The report must be assessed for restatement because income, cost basis and tax reporting changed.

### Starting data

| Field | Value |
|---|---|
| Portfolio | P-100 |
| Instrument | FUND-INCOME-01 |
| Distribution received | USD 10,000 |
| Original classification | 100% income |
| Corrected classification | 70% income, 30% return of capital |
| Published report | MONTHLY-2026-05-v1 |

### Expected lifecycle chain

```text
distribution posted
-> monthly report published
-> amended tax breakdown received
-> income and cost-basis treatment recalculated
-> restatement decision made
-> corrected report version generated
-> original and amended reports archived
-> client/advisor notification recorded
```

### Expected records

| Record | Type | Expected Treatment |
|---|---|---|
| Original distribution | transaction | Retained with original source and classification. |
| Tax amendment | lifecycle event | Links amended source file and effective date. |
| Income adjustment | adjustment transaction | Reduces taxable/reportable income by USD 3,000 if policy requires economic adjustment. |
| Cost-basis adjustment | cost-basis event | Reduces cost basis by return-of-capital amount. |
| Report v1 | report archive | Preserved as originally delivered. |
| Report v2 | amended report | Generated with restatement version and explanation. |
| Delivery evidence | operational record | Tracks recipient, timestamp, bounceback, acknowledgement and replacement delivery if needed. |

### Calculation impact

| Measure | Original | Corrected |
|---|---:|---:|
| Income | USD 10,000 | USD 7,000 |
| Return of capital | USD 0 | USD 3,000 |
| Cost basis reduction | USD 0 | USD 3,000 |
| Cash received | USD 10,000 | USD 10,000 |

### QA assertions

- Cash received does not change when only tax classification changes.
- Income, tax and cost-basis views are corrected according to policy.
- Report v1 and report v2 are both archived with version lineage.
- Advisor/client view identifies the latest valid report.
- Delivery bouncebacks or replacement packages are visible to operations.

## 8. Case Study E - Cross-Product Correction Packet

Use this packet shape when a correction affects more than one domain.

| Packet Section | Required Contents |
|---|---|
| Business event summary | What happened, who owns the source, which portfolios/accounts are impacted. |
| Source evidence | Original file/notice, amended file/notice, timestamps, source owner, batch fingerprint and validation result. |
| Transaction impact | Original, reversal, replacement, adjustment and no-op decisions. |
| Position impact | Quantity, nominal, units, lots, commitment, collateral or exposure changes. |
| Cash impact | Settlement cash, value date, fee, tax, income, margin, collateral and pending/settled state. |
| Valuation impact | Price/NAV/FX/model-value changes and stale/override flags. |
| Performance impact | External/internal classification, income, expense, realized/unrealized P&L and restatement policy. |
| Reporting impact | Report sections, labels, disclosures, archived versions, delivery evidence and latest-valid marker. |
| Reconciliation impact | Break id, owner, severity, materiality, expected/observed value and closure evidence. |
| Supportability state | Complete, partial, stale, unreconciled, break-open, blocked or unknown. |
| QA evidence | Golden scenario id, expected outputs, regression assertions and approval evidence. |

## 9. Golden Scenario Checklist

Every correction-chain golden scenario should assert:

1. original records are immutable,
2. reversal and replacement records are explicitly linked,
3. cash and product legs share a common economic event or transaction group,
4. report versions are archived and latest-valid version is clear,
5. tax lots, cost basis and performance are recalculated only when impacted,
6. reconciliation breaks remain open until evidence is complete,
7. degraded or support-limited states are visible to APIs, UI and reports,
8. duplicate suppression does not hide valid amendments,
9. late source files trigger restatement assessment,
10. operations can explain what changed, why it changed, who approved it and what evidence supports it.

## 10. Key Takeaway

Correction handling is not a cleanup task. It is a first-class lifecycle capability. A professional portfolio platform should explain the original state, corrected state, source evidence, financial impact, report impact and supportability status without losing audit history.
