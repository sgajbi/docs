# 05 - Reconciliation, Exception Management, Fails, Breaks and Controls

## 1. Purpose

Reconciliation verifies that internal platform records agree with external and upstream sources. It is the safety net for investment operations.

A wealth platform should reconcile not only end-of-day positions, but the full event chain: orders, trades, transactions, cash, positions, prices, income, corporate actions, fees, FX, valuations and reporting outputs.

## 2. Reconciliation domains

| Domain | Internal record | External/source record |
|---|---|---|
| Trade reconciliation | OMS/trade capture trade | Broker/counterparty confirmation |
| Settlement reconciliation | Settlement instruction/status | Custodian/CSD status |
| Cash reconciliation | Internal cash ledger | Bank/custodian cash statement |
| Position reconciliation | Internal position | Custodian/depot/TA statement |
| Transaction reconciliation | Booked transaction | Custodian transaction statement |
| Corporate action reconciliation | Internal event/entitlement | Custodian/vendor/issuer event |
| Income reconciliation | Internal dividend/coupon | Custodian income payment |
| Price reconciliation | Internal price | Market data/vendor/custodian price |
| Valuation reconciliation | Internal market value | Custodian or independent valuation |
| Performance reconciliation | Calculated returns | Benchmark/control reports |
| Tax reconciliation | Withholding/tax lots | Tax statements/custodian reports |

## 3. Break types

| Break type | Example |
|---|---|
| Quantity break | Internal 1,000 shares, custodian 900 |
| Cash break | Internal USD 10,000, custodian USD 9,850 |
| Transaction missing internally | Custodian booked dividend, internal system missing |
| Transaction missing externally | Internal trade booked, custodian not settled |
| Price break | Internal price differs beyond tolerance |
| FX break | Different FX rate used for reporting |
| Accrued interest break | Bond accrued interest mismatch |
| Cost-basis break | Corporate action adjusted basis differently |
| Corporate-action entitlement break | Wrong eligible quantity or rate |
| Settlement-status break | Internal settled, custodian pending/fail |
| Classification break | Income vs capital vs return of capital classification differs |

## 4. Break severity

| Severity | Meaning |
|---|---|
| Critical | Client money/security risk, regulatory issue, material reporting error |
| High | Material position/cash/performance impact or settlement fail |
| Medium | Non-material financial impact but needs correction |
| Low | Cosmetic/reference data issue with no immediate financial impact |

Severity should consider market value, client type, reporting deadline, regulatory impact, product complexity and ageing.

## 5. Break lifecycle

```text
Detected
  -> classified
  -> assigned
  -> investigated
  -> root cause identified
  -> correction proposed
  -> approval if needed
  -> corrected/rebooked
  -> verified
  -> closed
  -> root-cause trend reported
```

Fields:

| Field | Meaning |
|---|---|
| break_id | Unique break |
| break_type | Quantity/cash/transaction/price/etc. |
| source_a / source_b | Compared systems |
| account_id | Impacted account |
| instrument_id | Impacted instrument |
| amount/quantity_difference | Break magnitude |
| severity | Critical/high/medium/low |
| age_days | Days open |
| owner_team | Ops/product/control team |
| status | New/in review/pending external/closed |
| root_cause | Cause taxonomy |
| corrective_action | Reversal/rebook/reference-data fix/etc. |
| evidence_link | Supporting proof |

## 6. Root-cause taxonomy

| Category | Example |
|---|---|
| Timing | Internal booked before custodian, late feed, holiday mismatch |
| Reference data | Wrong ISIN, currency, factor, maturity, corporate-action terms |
| Transaction data | Missing trade, duplicate trade, wrong quantity/price/fees |
| Settlement | Fail, partial settlement, wrong SSI, late instruction |
| Corporate action | Missed event, wrong entitlement, stale event version |
| Price/valuation | Stale price, wrong source, clean/dirty mismatch |
| FX | Wrong rate source/date/time/base currency |
| Tax | Wrong withholding rate, missing tax reclaim, treaty status |
| System defect | Code mapping/calculation issue |
| Manual error | Incorrect override or upload |
| Source correction | Upstream restatement/custodian correction |

## 7. Reconciliation frequency

| Reconciliation | Frequency |
|---|---|
| Order/trade status | Intraday/near real-time |
| Settlement status | Intraday around market cut-offs and EOD |
| Cash and positions | Daily EOD minimum, intraday for critical functions |
| Corporate action events | Daily and near deadline/payable dates |
| Prices/valuations | Daily EOD and as-of reporting runs |
| Performance | Daily/month-end/regression after corrections |
| Tax lots/cost basis | Daily for equities, event-driven for corporate actions |
| Private markets | On capital call/NAV/distribution statements |
| Insurance/policies | On policy statement update cycle |

## 8. Tolerances

Use tolerance by domain, product and materiality.

| Domain | Tolerance example |
|---|---|
| Listed equities quantity | Usually zero tolerance |
| Cash | Small currency rounding tolerance possible |
| Bond accrued interest | Day-count rounding tolerance |
| Valuation | Percentage or absolute value tolerance |
| FX translated value | Rate/time tolerance |
| Fund NAV | NAV precision tolerance |
| Corporate action entitlement | Usually zero for quantity, small for cash-in-lieu rounding |
| Performance | Small basis-point tolerance, but explainable |

Never use broad tolerances to hide systematic errors.

## 9. Fails management

Failed trades should be managed as operational risk.

Fail fields:

| Field | Meaning |
|---|---|
| fail_id | Unique fail |
| trade_id | Linked trade |
| intended_settlement_date | Contractual date |
| fail_start_date | First failed date |
| fail_age | Ageing |
| fail_reason | Reason taxonomy |
| financial_impact | Penalty, interest, overdraft, market claim |
| client_impact | Reporting, availability, collateral, buying power |
| resolution_owner | Ops/custodian/broker/client/advisor |
| expected_resolution_date | Projection |
| status | Open/resolved/buy-in/cancelled |

## 10. Reconciliation and client reporting

Client reports should not blindly rely on unreconciled data.

Reporting policies should define:

- whether unreconciled positions are shown
- whether estimates are labelled
- whether stale prices are flagged
- whether pending trades are included
- whether failed trades are disclosed internally/advisor only
- whether revised statements are generated after corrections
- whether NAV-lag/private-market values are marked as as-of prior date

## 11. Reconciliation and performance

Breaks can materially affect performance:

| Break | Performance impact |
|---|---|
| Missing dividend | Understates income and return |
| Wrong split | Creates artificial loss/gain |
| Wrong FX rate | Distorts base-currency return |
| Missing fee | Overstates net performance |
| Late trade | Wrong subperiod return |
| Wrong external cashflow classification | TWR/MWR distortion |
| Stale private-market NAV | Smooths return and understates volatility |
| Corporate-action correction | Requires recalculation from event date |

Performance engines need recalculation lineage and restatement controls.

## 12. Exception dashboards

Useful dashboards:

- open breaks by severity and age
- settlement fails by market/custodian/broker
- cash breaks by currency
- position breaks by product class
- corporate action exceptions by deadline
- stale prices by asset class
- unresolved performance exceptions
- recurring source-system issues
- client-reporting impact queue
- SLA breaches by team

## 13. Controls for corrections

Corrections should use controlled patterns:

| Pattern | Use |
|---|---|
| Reversal and rebook | Wrong transaction/trade booked |
| Delta adjustment | Minor fee/tax/cash adjustment |
| Reference data correction with recalculation | Wrong static data or classification |
| Corporate action version reprocessing | Revised event terms |
| Price override | Approved valuation correction |
| Manual break closure | Only when immaterial and evidence retained |

Every correction should have:

- reason
- maker/checker approval where material
- source evidence
- impacted clients/accounts
- recalculation scope
- reporting restatement decision

## 14. Golden source hierarchy

Define source priority by data domain.

Example:

| Data domain | Golden source |
|---|---|
| Client instruction | Advisory/order system |
| Execution | Broker/OMS execution feed |
| Trade economics | Confirmed trade record |
| Settled position | Custodian/CSD statement |
| Cash balance | Custodian/bank statement |
| Market price | Approved market data source |
| Fund NAV | Fund administrator/TA/vendor |
| Corporate action event | Custodian/vendor with issuer validation |
| Tax rate | Tax engine/custodian/client tax profile |
| Performance | Internal performance engine after reconciliation |

Golden source may vary by booking centre and product.

## 15. Platform design recommendations

- Build a generic reconciliation engine with domain-specific adapters.
- Store reconciliation results, not only current breaks.
- Keep break history for audit and root-cause analysis.
- Link breaks to positions, transactions, corporate actions and reports.
- Support tolerances by product/field/currency/materiality.
- Support automated matching plus manual workflow.
- Feed critical breaks into reporting hold/release workflow.
- Trigger recalculation when corrected data affects analytics.
- Track whether client statements were generated before correction.
