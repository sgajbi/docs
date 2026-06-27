# 01 - Worked Example Methodology and Canonical Template

## 1. Why worked examples matter

Financial products are difficult to understand only through definitions.

Worked examples help teams understand:

- how product terms become cashflows
- how lifecycle events create transactions
- how positions change
- how valuation changes market value
- how P&L is realized or unrealized
- how performance and contribution are calculated
- how reporting should explain the result
- how QA should validate expected outcomes

## 2. Canonical example template

Use this structure for each example:

```text
1. Business scenario
2. Starting data
3. Event or transaction
4. Expected transaction records
5. Expected position impact
6. Expected cash impact
7. Expected valuation impact
8. Expected P&L / income / cost impact
9. Expected performance treatment
10. Expected risk / exposure treatment
11. Expected reporting output
12. Expected controls and QA assertions
13. Edge cases
```

## 3. Example data standards

Use consistent sample assumptions:

| Item | Standard example |
|---|---|
| Base currency | USD |
| Secondary currency | SGD |
| Portfolio | P-100 |
| Account | A-100 |
| Client | C-100 |
| Trade date | T |
| Settlement date | S |
| Quantity precision | product-specific |
| Money precision | 2 decimal places |
| Calculation precision | high precision decimal internally |
| FX quote | explicit direction |
| Price quote | specify clean, dirty, NAV, percentage of par or per unit |

## 4. Transaction modelling standards

Each example should distinguish:

| Concept | Meaning |
|---|---|
| Lifecycle event | business/product event, may not move money immediately |
| Transaction | economic posting affecting cash, position, income, fee or P&L |
| Ledger entry | accounting debit/credit representation |
| Position | resulting holding after transactions |
| Valuation | market value at a point in time |
| Reporting snapshot | report-ready state with lineage |

## 5. Position impact pattern

For every example, capture:

| Field | Example |
|---|---|
| instrument_id | BOND-ABC |
| quantity_delta | +100 |
| nominal_delta | +100,000 |
| cash_delta | -99,500 |
| cost_basis_delta | +99,500 |
| accrued_income_delta | +500 |
| market_value_delta | depends on price |
| realized_pnl_delta | 0 on buy |
| unrealized_pnl_delta | derived |
| income_delta | 0 unless income event |
| fee_delta | if transaction fee |

## 6. Performance treatment pattern

Classify each cashflow:

| Cashflow | Performance treatment |
|---|---|
| external client deposit | external inflow |
| external client withdrawal | external outflow |
| security purchase | internal investment activity |
| security sale | internal disposal |
| coupon/dividend | investment income |
| fee | expense |
| tax | tax/withholding |
| FX conversion inside portfolio | internal transaction |
| physical settlement | internal conversion |
| write-off | investment loss |
| capital call funded from portfolio cash | internal investment activity |
| capital call funded by new money | external inflow then investment |

## 7. QA assertion types

| Assertion type | Example |
|---|---|
| Balance assertion | cash + positions reconcile |
| Lifecycle assertion | event state transitions are correct |
| Valuation assertion | market value formula correct |
| P&L assertion | realized/unrealized split correct |
| Income assertion | coupon/dividend recognized correctly |
| Performance assertion | external/internal cashflow classification correct |
| Risk assertion | exposure reflects product economics |
| Reporting assertion | labels and disclosures correct |
| Data-quality assertion | stale/missing source flagged |
| Audit assertion | transaction lineage preserved |

## 8. Common mistakes

| Mistake | Better approach |
|---|---|
| Treating all events as transactions | only economic postings become transactions |
| Mixing legal holding and look-through exposure | keep both separate |
| Treating all cashflows as external | classify correctly for performance |
| Using rounded values internally | use high precision internally |
| Ignoring settlement date | distinguish trade date, value date and booking date |
| Ignoring source lineage | every report value needs source/calculation trace |
| Hiding exceptions | report degraded data clearly |
| Hardcoding tax/accounting rules | use configurable jurisdiction/accounting policy |

## 9. Key takeaway

Every worked example should be usable by business, engineering, QA, reporting and operations.
