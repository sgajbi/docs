# 02 - Data Quality Dimensions, Rules, Scorecards and SLAs

## 1. Data quality in wealth management

Data quality means data is fit for the business decision or process that uses it. The same value can be acceptable for one purpose and unacceptable for another.

Example: a fund NAV that is two days old may be acceptable for a monthly private-market report but unacceptable for an advisor proposal generated today. A bond price that is indicative may be acceptable for portfolio reporting with a disclaimer, but not acceptable for collateral lending if the policy requires a conservative executable valuation.

## 2. Core data-quality dimensions

| Dimension | Meaning | Wealth-platform example |
|---|---|---|
| Completeness | Required data is present. | Every holding has instrument ID, quantity, price, currency and valuation date. |
| Accuracy | Data reflects the correct real-world value. | Coupon rate matches termsheet; price matches vendor or custodian. |
| Validity | Data follows allowed format/rules. | ISIN length and checksum; currency is ISO code; date is valid business date. |
| Consistency | Same concept agrees across systems. | Position quantity in reporting agrees with accounting book after settlement timing rules. |
| Timeliness | Data is available within required SLA. | EOD prices loaded before portfolio review report generation. |
| Freshness | Data is not stale relative to purpose. | FX rate date equals report valuation date. |
| Uniqueness | No unintended duplicates. | No duplicate trade booking from replayed event. |
| Integrity | Referential relationships are valid. | Transaction instrument exists in instrument master; pledge points to valid portfolio. |
| Reconciliation | Data agrees with authoritative source or control total. | Cash balance agrees with core banking. |
| Lineage | Origin and transformation can be traced. | Client report market value traces to source price, FX and position. |
| Precision | Decimal scale and rounding are appropriate. | Fractional fund units and bond accrued interest use required precision. |
| Stability | Data does not change unexpectedly after certification. | Locked report snapshot remains reproducible. |
| Explainability | Data can be explained to business users. | Performance residual has reason codes and non-allocable explanation. |

## 3. Product-specific quality examples

| Product area | Critical quality concerns |
|---|---|
| Bonds | Maturity, coupon, day count, call schedule, accrued interest, clean/dirty price, rating, yield. |
| Structured notes | Issuer, underlying, barrier, observation schedule, payoff terms, coupon condition, knock-in/autocall status. |
| Equities | Exchange, corporate actions, dividend entitlement, split ratio, rights terms, listing status. |
| Funds | NAV date, dealing cycle, share class, distribution/reinvestment, TER, liquidity gates, swing pricing. |
| Derivatives | Contract terms, notional, multiplier, strike, expiry, margin, valuation model, Greeks, collateral. |
| Private markets | Commitment, funded capital, unfunded commitment, NAV lag, capital calls, recallable distributions. |
| Lending/collateral | Pledge link, LTV, haircut, credit line, exposure, in-flight orders, collateral eligibility. |
| Reporting | Snapshot version, calculation date, disclosure, exception status, report basis and source lineage. |

## 4. Data-quality rule types

### 4.1 Schema rules

Schema rules ensure messages are technically readable.

Examples:

- mandatory fields present;
- valid data type;
- date format valid;
- decimal fields not sent as strings with commas;
- array not empty where required;
- enum values valid.

### 4.2 Business validation rules

Business rules test whether data makes sense.

| Rule | Example |
|---|---|
| Non-negative | Long-only portfolio quantity should not be negative unless shorting enabled. |
| Effective date | New product classification applies from approval date, not backdated silently. |
| Range check | Bond coupon cannot be 500% unless instrument flagged as exceptional. |
| Cross-field check | Maturity date must be after issue date. |
| Source hierarchy | If primary price exists and passes quality checks, do not use fallback price. |
| Lifecycle consistency | Matured bond should not have active holding unless unsettled or failed redemption. |

### 4.3 Referential integrity rules

Examples:

- every transaction instrument exists in instrument master;
- every benchmark link points to active benchmark definition;
- every account belongs to valid client/BR/CIF;
- every pledge maps to valid credit line and valid collateral portfolio;
- every lifecycle event has valid product contract and source event ID.

### 4.4 Temporal rules

Wealth systems are highly effective-dated.

| Rule | Example |
|---|---|
| No future price for past report | 2026-06-25 report cannot use 2026-06-26 price. |
| Corporate action effective date | Split adjustment applies from ex-date or effective date depending source convention. |
| Benchmark composition | Weight version must match performance calculation date. |
| Mandate rule version | Rule applied should be the active rule on proposal date. |
| Client risk profile | Suitability uses profile effective at recommendation time. |

### 4.5 Reconciliation rules

Examples:

- position quantity by account/instrument agrees with custodian;
- cash by currency agrees with core banking;
- transaction count and amount agree with source file control totals;
- market value by portfolio agrees with accounting book within tolerance;
- report totals agree with section subtotals;
- performance beginning and ending market value tie to valuation snapshots.

### 4.6 Statistical and anomaly rules

Examples:

- price change exceeds threshold compared with previous day;
- FX rate movement beyond market tolerance;
- bond yield moves abnormally without price or spread explanation;
- NAV jump exceeds peer/fund-specific threshold;
- transaction amount unusually large relative to account value;
- collateral availability changes sharply due to haircut or price movement.

## 5. Severity classification

A useful severity model:

| Severity | Meaning | Example | Action |
|---|---|---|---|
| Critical | Client/regulatory/material financial impact likely. | Missing price for 35% of portfolio market value. | Block report or mandate decision; immediate escalation. |
| High | Significant business impact or repeated exception. | Wrong bond coupon affects income projection. | Fix before publication or show explicit degraded state. |
| Medium | Localised issue with workaround. | Missing sector for small equity holding. | Degrade allocation, track remediation. |
| Low | Cosmetic or non-material. | Long instrument name truncated in internal table. | Fix in normal backlog. |

Severity should be configurable by consumer. A field may be medium for reporting but critical for collateral lending.

## 6. Quality thresholds

| Metric | Possible threshold |
|---|---|
| Price completeness | 99.5% by market value before report generation. |
| Critical CDE completeness | 100% for reportable holdings. |
| Cash reconciliation | Zero breaks above materiality before EOD certification. |
| Position reconciliation | No aged high-severity breaks older than T+1. |
| Fund NAV freshness | NAV date within expected fund valuation cycle. |
| FX freshness | Report valuation date FX unless approved fallback. |
| Corporate action processing | Mandatory events processed before ex-date reporting. |
| Failed-feed alerting | Alert within 5 minutes of SLA miss. |

## 7. Data-quality scorecards

A scorecard should combine completeness, timeliness, reconciliation, break ageing and consumer impact.

| Domain | Completeness | Freshness | Reconciliation | Open high breaks | Score |
|---|---:|---:|---:|---:|---:|
| Prices | 99.8% | 98.9% | 99.5% | 2 | Amber |
| FX | 100% | 100% | 100% | 0 | Green |
| Positions | 99.9% | 100% | 99.7% | 1 | Amber |
| Transactions | 100% | 99.8% | 99.9% | 0 | Green |
| Corporate actions | 98.5% | 97.0% | 98.0% | 5 | Red |

Scorecards should be business-facing, not only technical. They should say which reports, portfolios, clients, regions and product families are affected.

## 8. Data quality SLAs and OLAs

| Control | SLA/OLA example |
|---|---|
| Market price feed | Loaded by 06:00 local reporting time. |
| Missing price investigation | High-severity missing price triaged within 30 minutes. |
| Corporate action mandatory event | Processed before EOD on effective date. |
| Fund NAV | Loaded within expected vendor/fund cycle. |
| Reconciliation break | Critical breaks actioned same business day. |
| Manual override approval | Approved by data steward and expires automatically. |
| Report certification | Completed before report release window. |

## 9. Manual overrides

Manual overrides are sometimes necessary, but they must be controlled.

Required fields:

| Field | Purpose |
|---|---|
| Override ID | Unique reference. |
| Data field | Which value was overridden. |
| Old value / new value | Audit evidence. |
| Reason code | Source delay, vendor error, business correction, migration fix. |
| Approver | Steward/product-control approval. |
| Effective date/time | When override applies. |
| Expiry date/time | Prevents permanent hidden patch. |
| Downstream impact | Which reports/calculations consumed it. |
| Reversal process | How it is removed or replaced by source correction. |

Anti-pattern: override tables that are never reviewed. A good platform reports active overrides daily and requires expiry or renewal.

## 10. Degraded data states

A platform should not only say "success" or "failure." It should support controlled degraded states.

| State | Meaning | User-facing behaviour |
|---|---|---|
| Complete | All required data passed controls. | Normal report/calculation. |
| Warning | Non-critical quality issue. | Show advisory note or internal badge. |
| Degraded | Important data missing but calculation can proceed with limitations. | Show exception panel and exclude/flag affected metrics. |
| Blocked | Critical data missing or inconsistent. | Prevent report/proposal/mandate approval. |
| Estimated | Uses proxy/model/manual estimate. | Explicit disclosure and lineage. |
| Preliminary | Not final-certified. | Watermark or internal-only status. |
| Certified | Signed off. | Eligible for external release. |

## 11. Data-quality rule catalogue example

| Rule ID | Domain | Rule | Severity | Owner | Consumer impact |
|---|---|---|---|---|---|
| DQ-PRICE-001 | Pricing | Every reportable holding must have valuation price or approved fallback. | High/Critical | Pricing steward | Valuation, P&L, performance, report. |
| DQ-FX-002 | FX | Base-currency valuation must use approved EOD FX rate for valuation date. | High | Market data | Market value, allocation, performance. |
| DQ-TXN-003 | Transactions | Source transaction ID must be unique per source and event type. | Critical | Transaction source | Duplicate cashflow/position risk. |
| DQ-CA-004 | Corporate actions | Mandatory split event must adjust quantity and cost basis by ex-date. | Critical | CA operations | Position, P&L, performance. |
| DQ-MAND-005 | Mandate | Mandate rule set must be active and versioned at proposal time. | Critical | Mandate owner | Suitability/pre-trade. |
| DQ-REPORT-006 | Reporting | Published report snapshot must have immutable version and certification status. | Critical | Reporting owner | Audit/client dispute. |

## 12. Practitioner takeaway

Data quality is not a single dashboard. It is a controlled lifecycle of rules, thresholds, ownership, exceptions, remediation and certification. For wealth platforms, the highest-quality designs make data quality visible at the point of decision: proposal, trade, valuation, performance, buying power, mandate check and client report.
