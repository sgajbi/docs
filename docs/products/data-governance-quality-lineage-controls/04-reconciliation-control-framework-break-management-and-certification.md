# 04 - Reconciliation Control Framework, Break Management and Certification

## 1. Purpose of reconciliation

Reconciliation compares two or more independently produced views of the same business reality. It detects whether data is complete, accurate and consistent enough for downstream use.

In wealth management, reconciliation is required across:

- cash balances;
- security positions;
- transactions;
- market values;
- accrued income;
- fees;
- corporate actions;
- fund subscriptions/redemptions;
- structured-product lifecycle events;
- collateral and credit-line values;
- performance and reporting snapshots.

## 2. Reconciliation types

| Type | Purpose | Example |
|---|---|---|
| Source-to-source | Compare two authoritative or semi-authoritative systems. | Custodian position vs accounting book. |
| Source-to-calculated | Compare source value with derived platform value. | Position from transaction replay vs custodian position. |
| Internal consistency | Check totals against components. | Portfolio total MV equals sum of holdings + cash + accruals. |
| Lifecycle reconciliation | Check event status across systems. | Corporate action processed in custody but not in reporting. |
| Control-total reconciliation | Check file-level totals. | Number of trades and total consideration match source control file. |
| Time-series reconciliation | Check continuity over time. | Yesterday EOD position + today activity = today EOD position. |
| Report reconciliation | Check report output against certified snapshot. | Statement holdings tie to reporting database snapshot. |

## 3. Key reconciliations by domain

| Domain | Reconciliation |
|---|---|
| Cash | Core banking vs reporting cash; settled vs available; multi-currency total. |
| Positions | Custodian vs accounting vs reporting; trade-date vs settlement-date basis. |
| Transactions | OMS execution vs custodian settlement vs accounting transaction. |
| Prices | Market data feed vs custodian valuation vs independent vendor. |
| FX | Treasury rate vs market data rate vs reporting applied rate. |
| Corporate actions | CA source event vs entitlement vs booked transaction vs position outcome. |
| Income | Coupon/dividend/accrual source vs cash receipt vs report income. |
| Funds | Order status vs transfer agent confirmation vs unit position and NAV. |
| Derivatives | Contract terms vs valuation engine vs collateral/margin. |
| Lending | Credit line/exposure/collateral vs buying power calculation. |
| Performance | BOD/EOD market values and cashflows tie to position/cash snapshots. |
| Reports | Published PDF/HTML values tie to immutable report snapshot. |

## 4. Reconciliation basis

Many false breaks happen because teams compare different bases.

| Basis | Meaning | Example |
|---|---|---|
| Trade date | Includes executed trades before settlement. | Advisor portfolio may show bought equity immediately. |
| Settlement date | Includes only settled holdings/cash. | Custody statement may exclude unsettled trade. |
| Accounting date | Booking date for accounting records. | Backdated corrections may be booked today effective prior date. |
| Value date | Date cash economically settles. | FX spot and deposit maturity. |
| Report date | Snapshot used for client report. | Month-end statement locked on 2026-06-30. |
| Local currency | Security currency value. | USD bond value. |
| Base/report currency | Portfolio reporting currency. | SGD report using USD/SGD FX. |
| Clean price | Excludes accrued income. | Bond price 98.50. |
| Dirty price | Includes accrued income. | Bond market value including accrued. |

Before escalating a break, confirm both sides use the same basis.

## 5. Tolerance design

Tolerances should be risk-based.

| Reconciliation | Tolerance approach |
|---|---|
| Cash | Usually zero or very low absolute tolerance. |
| Listed equity quantity | Zero quantity tolerance. |
| Fund units | Tiny decimal tolerance based on share precision. |
| Bond nominal | Zero nominal tolerance, with rounding for denominations. |
| Market value | Absolute and percentage tolerance. |
| FX converted value | Tolerance based on decimal precision and FX source. |
| Accrued income | Tolerance based on day-count/rounding differences. |
| Performance return | Basis-point tolerance with input tie-out. |
| Private-market NAV | Tolerance based on latest official capital account statement. |

## 6. Break classification

| Break type | Example | Owner |
|---|---|---|
| Timing | Custodian has not settled trade yet. | Operations / settlement. |
| Missing data | No price for bond. | Market data / pricing steward. |
| Duplicate | Trade event replayed twice. | Source/integration owner. |
| Mapping | Instrument ID mapped to wrong security. | Reference data. |
| Transformation | Wrong FX quote direction applied. | Engineering/calculation owner. |
| Definition | One system includes accrued, another excludes it. | Data governance/product owner. |
| Source error | Custodian sends wrong quantity. | Source-system owner. |
| Policy difference | Reporting uses bid price, accounting uses mid. | Product control / policy. |
| Manual override | Override active in one system only. | Data steward. |
| Migration | Legacy-to-target mapping mismatch. | Migration team/domain owner. |

## 7. Break lifecycle

```text
Detected -> Classified -> Assigned -> Investigated -> Remediated -> Validated -> Closed -> Root-cause analysed -> Regression added
```

Each break should have:

| Field | Purpose |
|---|---|
| Break ID | Unique traceable item. |
| Detection rule | Which control found it. |
| Domain | Cash, position, price, performance, etc. |
| Severity | Critical/high/medium/low. |
| Impact | Client, report, mandate, risk, settlement. |
| Owner | Accountable resolver. |
| Age | Time since detection. |
| Root cause | Missing price, source delay, mapping error, etc. |
| Resolution | Source fix, override, transformation fix, expected timing. |
| Evidence | Before/after values, screenshots/logs/run IDs. |
| Preventive action | Rule/test/runbook update. |

## 8. Certification model

Certification is a controlled statement that data is ready for a purpose.

| Certification | Purpose |
|---|---|
| EOD data certification | Core feeds, positions, cash, prices and FX complete enough for downstream. |
| Valuation certification | Prices and valuation controls pass thresholds. |
| Performance certification | Return calculations tie to source values and cashflow classifications. |
| Report certification | Report snapshots generated with acceptable data quality and exceptions. |
| Migration certification | Target platform output matches agreed legacy/source benchmark. |
| Release certification | New rules/logic pass regression and control evidence. |

Certification should not mean "no issues." It should mean known issues are within tolerance, disclosed, approved or blocked.

## 9. Product-specific reconciliation examples

### Bonds

| Check | Example |
|---|---|
| Nominal | Custodian nominal equals accounting nominal. |
| Accrued | Accrued interest matches coupon/day-count convention. |
| Price | Vendor price agrees within tolerance with custodian/evaluated price. |
| Maturity | Matured bonds not shown as active unless failed/redemption pending. |

### Structured notes

| Check | Example |
|---|---|
| Barrier event | Issuer/corporate action event agrees with lifecycle engine. |
| Coupon payment | Coupon entitlement matches cash receipt. |
| Redemption | Autocall/maturity closes note position and books cash/securities. |

### Funds

| Check | Example |
|---|---|
| Units | Transfer agent confirmation agrees with reporting units. |
| NAV | NAV date and share class match fund holding. |
| Distribution | Distribution/reinvestment increases cash or units correctly. |

### Equities

| Check | Example |
|---|---|
| Split | Quantity and cost per share adjusted without changing total cost basis. |
| Dividend | Entitlement based on record date and position. |
| Rights | Entitlement, exercise, lapse and cash movement reconciled. |

### Lending and collateral

| Check | Example |
|---|---|
| Pledged assets | Collateral portfolio holdings agree with pledge engine. |
| LTV/haircut | Eligible value equals price x quantity x LTV/haircut. |
| Exposure | Loan, overdraft, derivative exposure and in-flight orders included correctly. |

## 10. Reconciliation dashboards

A good dashboard should show:

- break count by severity;
- break amount by market value;
- ageing buckets;
- product family affected;
- source system affected;
- report/advisory/mandate impact;
- owner and SLA status;
- recurring root causes;
- open breaks blocking certification;
- historical trend.

Avoid dashboards that only show count. Ten small low-impact breaks are different from one critical portfolio-reporting break.

## 11. Common reconciliation anti-patterns

| Anti-pattern | Risk |
|---|---|
| Reconcile only totals | Offsetting errors hide true issues. |
| Ignore basis differences | False breaks waste effort. |
| Close breaks without root cause | Same incident repeats. |
| Manual Excel reconciliation | No lineage, weak auditability, operational fragility. |
| No ageing escalation | Old breaks become accepted risk. |
| No consumer impact | Teams solve low-impact issues while client-impact issues wait. |
| No regression tests | Fixed issues reappear in later releases. |

## 12. Practitioner takeaway

Reconciliation is not an operations afterthought. It is the practical proof that source ownership, calculations and reporting outputs are working. A strong platform turns reconciliation results into business decisions: certify, degrade, block, remediate or disclose.
