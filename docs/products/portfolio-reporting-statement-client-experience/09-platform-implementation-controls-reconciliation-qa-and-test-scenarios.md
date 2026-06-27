# 09 - Platform Implementation, Controls, Reconciliation, QA and Test Scenarios

## 1. Target architecture

A reporting platform should be designed as a controlled pipeline.

```text
Sources
  -> Curated domain data
  -> Calculation engines
  -> Reporting data mart / report dataset
  -> Validation and controls
  -> Renderer / API / portal
  -> Delivery and archive
```

## 2. Core services

| Service | Responsibility |
|---|---|
| Report orchestration service | Schedule, trigger, run and monitor reports. |
| Scope service | Resolve client/account/mandate/entity hierarchy. |
| Data cut service | Select correct data cut and valuation basis. |
| Report dataset service | Assemble structured report data. |
| Calculation services | Performance, risk, income, exposure, mandate. |
| Validation service | Completeness, reconciliation, exception checks. |
| Renderer service | PDF/HTML/Excel generation from templates. |
| Disclosure service | Resolve disclosures by product/jurisdiction/client type. |
| Archive service | Store immutable report output and dataset. |
| Delivery service | Portal/email/print/API delivery and evidence. |

## 3. Data contracts

Report APIs should be explicit.

Example sections:

```json
{
  "reportRunId": "RPT-2026-06-CLIENT-123",
  "reportType": "PORTFOLIO_REVIEW",
  "scope": {
    "clientId": "CLIENT-123",
    "accounts": ["ACC-1", "ACC-2"],
    "periodStart": "2026-01-01",
    "periodEnd": "2026-06-30",
    "currency": "USD",
    "basis": "TRADE_DATE"
  },
  "sections": {
    "summary": {},
    "holdings": [],
    "transactions": [],
    "performance": {},
    "risk": {},
    "mandate": {},
    "exceptions": [],
    "disclosures": []
  }
}
```

## 4. Key controls

| Control | Example |
|---|---|
| Completeness | Expected account count equals reported account count. |
| Balance tie-out | Total market value equals sum of holdings plus cash minus liabilities. |
| Cash reconciliation | Opening cash + activity = closing cash. |
| Position reconciliation | Opening position + transactions/events = closing position. |
| Price validation | Price source, date and stale threshold checked. |
| FX validation | FX source and date checked. |
| Income tie-out | Income detail equals income summary. |
| Performance sanity | Large returns flagged for review. |
| Mandate controls | Breaches shown and explained. |
| Disclosure controls | Required disclosures included. |
| Delivery controls | Correct recipient and entitlement. |
| Archive controls | Published report archived immutably. |

## 5. Reconciliation checks

### 5.1 Holdings reconciliation

```text
Reported total value
= Sum(position market value)
+ Sum(cash balance)
- Sum(liabilities)
+/- reporting adjustments
```

### 5.2 Cash roll-forward

```text
Closing cash
= Opening cash
+ External cash in
- External cash out
- Buys
+ Sells
+ Income
- Fees
- Taxes
+ Loan drawdowns
- Loan repayments
+ FX/cash transfers
```

### 5.3 Position roll-forward

```text
Closing quantity
= Opening quantity
+ Buys/subscriptions/transfers in
- Sells/redemptions/transfers out
+ Corporate action increases
- Corporate action decreases
+/- corrections
```

## 6. Performance QA

Test:

- no cashflow period;
- large external cashflow;
- full liquidation;
- inception during period;
- account opened/closed during period;
- missing price day;
- multi-currency portfolio;
- dividend/coupon retained;
- dividend withdrawn;
- fee paid from portfolio;
- loan drawdown/repayment;
- private-market NAV lag;
- structured note maturity;
- derivative MTM and collateral.

## 7. Product-specific QA scenarios

| Product | Scenario |
|---|---|
| Bond | Coupon accrual, coupon payment, sale between coupon dates, maturity. |
| Equity | Dividend, stock split, rights issue, spin-off, merger. |
| Fund | Subscription, redemption, NAV lag, distribution reinvestment. |
| Structured note | Coupon, barrier event, autocall, physical settlement. |
| Derivative | Option exercise/expiry, futures variation margin, FX forward settlement. |
| Private fund | Capital call, distribution, recallable distribution, stale NAV. |
| Loan | Drawdown, interest accrual, margin call, collateral revaluation. |
| Insurance | Premium, surrender, policy loan, claim, annuity payment. |

## 8. Report-generation QA scenarios

- client with one account;
- client with many accounts;
- family group with multiple entities;
- trust account with restricted beneficiary access;
- long product names;
- non-base currency positions;
- negative cash;
- zero market value position;
- missing cost basis;
- stale price;
- report with no activity;
- report with many pages;
- report with corrected/reversed transaction;
- report restatement after publication.

## 9. Regression test matrix

| Test group | Examples |
|---|---|
| Data completeness | Missing account, missing holding, missing cash. |
| Calculation correctness | MV totals, P&L, performance, risk, income. |
| Layout correctness | Pagination, totals, number formatting, disclosure placement. |
| Exception handling | Stale/missing/estimated values. |
| Security/entitlement | Client can only access own reports. |
| Archive/replay | Historical report output can be retrieved. |
| Performance/scalability | Batch generation for large client population. |
| Localization | Currency/date/number/language formatting. |
| Accessibility | PDF tags, readable labels, contrast, table structure where applicable. |

## 10. Observability

Metrics:

- report generation count;
- report success/failure rate;
- average generation time;
- queue depth;
- retries;
- failed sections;
- exception count by type;
- stale price count;
- manual override count;
- restatement count;
- delivery success rate;
- archive success rate.

Logs should include:

- report run ID;
- client/account scope;
- data cut ID;
- calculation run IDs;
- template version;
- exception IDs;
- correlation ID.

## 11. Security and privacy controls

Reports contain sensitive financial data.

Controls:

- role-based access control;
- relationship-manager entitlement;
- account-level restriction;
- family-office hierarchy access;
- beneficiary/trust visibility rules;
- encryption at rest and in transit;
- secure delivery;
- watermarking where appropriate;
- download audit;
- masking for support users;
- data-retention policy;
- deletion/retention legal hold controls.

## 12. Common implementation anti-patterns

| Anti-pattern | Problem |
|---|---|
| PDF directly queries production databases. | No reproducibility, poor performance, inconsistent values. |
| Report logic embedded in template. | Hard to test and reuse. |
| No data cut concept. | Values change between sections. |
| Missing exception model. | Bad data becomes wrong reports. |
| No archive dataset. | Cannot explain historical reports. |
| One product table for all display fields. | Leads to null-heavy and inconsistent treatment. |
| No source lineage. | Operations cannot investigate client queries. |
| Manual Excel corrections outside platform. | Loss of auditability. |

## 13. Release readiness checklist

Before releasing a reporting capability:

- source ownership documented;
- data contracts versioned;
- section definitions approved;
- calculations reconciled;
- product-specific tests passed;
- exception behavior approved;
- disclosures mapped;
- template regression passed;
- entitlement controls tested;
- archive replay tested;
- production dashboard available;
- runbook created;
- support triage guide created.
