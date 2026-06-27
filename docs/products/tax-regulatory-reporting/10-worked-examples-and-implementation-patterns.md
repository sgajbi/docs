# 10 - Worked Examples and Implementation Patterns

Use these examples when designing tax-aware reporting, cost-basis records, withholding treatment, cross-border documentation, regulatory outputs, client statements, operations controls, QA, and implementation boundaries.

## 1. Economic Event To Tax Classification

Do not mix product economics with tax conclusions.

```text
economic event -> accounting transaction -> source classification -> tax/reporting enrichment -> report output
```

| Layer | Example |
|---|---|
| economic event | equity dividend declared |
| accounting transaction | gross income, withholding tax, net cash |
| source classification | dividend, tax withheld, country of source |
| tax/reporting enrichment | treaty rate, reportable income bucket, tax document link |
| report output | client statement, tax pack, regulatory file |

## 2. Equity Dividend With Treaty Withholding

Scenario:

- Shares held: 1,000.
- Dividend: USD 2.00 per share.
- Statutory withholding: 30%.
- Treaty withholding: 15%.
- Client documentation valid.

Calculation:

```text
gross dividend = 1,000 x 2.00 = 2,000
withholding tax = 2,000 x 15% = 300
net cash = 1,700
```

Controls:

- documentation validity must be checked as of dividend record/payment date;
- source country, income type, rate and treaty basis should be stored;
- invalid or expired documentation should not silently receive treaty rate;
- client report should distinguish gross income, withholding and net cash.

## 3. Bond Accrued Interest And Coupon Reporting

Scenario:

- Client buys a bond between coupon dates.
- Dirty settlement includes accrued interest paid to seller.
- Later full coupon is received.

Reporting distinction:

| Component | Treatment |
|---|---|
| accrued interest paid on purchase | cost/accrual adjustment according to policy |
| full coupon received | cash income event |
| net taxable/reportable income | jurisdiction and policy dependent |
| performance income | should not double count paid accrued interest |

Implementation requirement:

Store accrued interest paid and coupon received as distinct facts. Do not infer tax-reporting treatment from cash movement alone.

## 4. Tax Lot Sale With FIFO

Scenario:

- Lot 1: 100 shares at USD 40.
- Lot 2: 100 shares at USD 55.
- Sell 150 shares at USD 70.
- FIFO method.

Cost basis:

```text
cost basis = (100 x 40) + (50 x 55) = 6,750
sale proceeds = 150 x 70 = 10,500
realized gain = 3,750
remaining lot = 50 shares from Lot 2 at 55
```

QA assertions:

- lot selection method must be explicit;
- partial lot depletion must preserve remaining quantity and basis;
- fees and taxes should be included or excluded according to configured policy;
- reporting currency gain may differ from local-currency gain due to FX.

## 5. Corporate Action Cost Basis Allocation

Scenario:

- Client owns original company shares with cost basis USD 10,000.
- Spin-off creates new company shares.
- Allocation ratio from source: 80% original, 20% spin-off.

Calculation:

```text
original company basis = 10,000 x 80% = 8,000
spin-off company basis = 10,000 x 20% = 2,000
```

Controls:

- use authoritative corporate-action or tax source;
- preserve original basis and allocation record;
- do not book the spin-off as zero-cost unless policy/source explicitly says so;
- report missing allocation as support-limited.

## 6. Fund Distribution Classification

Scenario:

- Fund distribution: USD 5,000.
- Source classification:
  - income: USD 2,000;
  - capital gain: USD 1,500;
  - return of capital: USD 1,500.

Treatment:

| Component | Reporting implication |
|---|---|
| income | income bucket |
| capital gain | capital gain bucket |
| return of capital | basis adjustment or capital return depending on policy |

Do not classify the full distribution as yield or income without source support.

## 7. Structured Note Physical Delivery

Scenario:

- Structured note physically delivers 2,000 shares.
- Note cost basis: USD 100,000.
- Cash-in-lieu: USD 120.

Implementation questions:

| Question | Why it matters |
|---|---|
| What basis transfers to delivered shares? | tax-lot and realized gain treatment |
| Is cash-in-lieu taxable/reportable? | reporting classification |
| Is note loss realized at delivery? | performance and tax treatment |
| What is acquisition date of delivered shares? | holding-period rules |

Correct posture:

Capture source policy and jurisdictional configuration. If not supported, show tax treatment as unavailable rather than inferring it.

## 8. CRS/FATCA Documentation Expiry

Scenario:

- Client tax self-certification expires before reporting cut-off.
- Account has reportable income during year.

Expected workflow:

```text
documentation expiry -> remediation task -> reportability status review -> reporting output or exception -> audit evidence
```

Controls:

- documentation status should be effective-dated;
- reportability should be based on applicable cut-off rules;
- missing documentation should appear in exception dashboards;
- client/advisor tasks should not alter historical facts without source evidence.

## 9. Regulatory Report Correction

Scenario:

- A prior tax report used an incorrect withholding rate.
- Corrected source file arrives after report publication.

Required workflow:

- preserve original report output;
- store corrected input and reason;
- recalculate impacted figures;
- determine whether amended report, client notice or next-period disclosure is required;
- preserve sign-off and audit trail.

Do not overwrite published report data without versioning.

## 10. Jurisdiction-Specific Rule Configuration

Scenario:

The same USD dividend is reportable differently for two client accounts because the clients have different tax residence, documentation and booking centre context.

| Attribute | Client A | Client B |
|---|---|---|
| Tax residence | Country X | Country Y |
| Documentation status | Valid treaty form | Missing self-certification |
| Gross dividend | 2,000 | 2,000 |
| Applicable withholding rate | 15% | 30% |

Calculation:

```text
Client A withholding = 2,000 x 15% = 300
Client B withholding = 2,000 x 30% = 600
```

Correct treatment:

- rule configuration should be effective-dated and source-controlled;
- client, account, product, income type, source country and documentation status are required inputs;
- missing documentation should fail closed to default treatment or exception state according to policy;
- reports should show basis, source date and supportability state without presenting personalized tax advice.

## 11. Tax Report Output Template

Scenario:

A client tax pack needs to summarize income, withholding and realized gains for a reporting period.

| Section | Required content |
|---|---|
| Header | client/account identifier, reporting period, currency, generation date, version. |
| Income summary | gross income, income category, source country, withholding, net income. |
| Realized gains/losses | proceeds, cost basis, fees, taxes, lot method, realized gain/loss. |
| Documentation | residency/document status, expiry, remediation state, missing items. |
| Exceptions | unsupported events, missing classifications, estimated figures, corrected items. |
| Lineage | source files, calculation version, report version, approval/sign-off status. |

Output control:

The report should be reproducible from stored inputs and should be versioned. If any material item is missing, estimated, stale, corrected, or unsupported, the report should show a clear exception rather than silently omitting the item.

## 12. Amended Filing Workflow

Scenario:

A regulatory file was submitted with income of 10,000. A corrected source classification later reclassifies 2,000 from income to return of capital.

| Attribute | Original | Corrected |
|---|---:|---:|
| Reported income | 10,000 | 8,000 |
| Return of capital | 0 | 2,000 |

Required workflow:

```text
source correction -> impact assessment -> materiality and policy decision -> amended file or next-period disclosure -> approval -> archive
```

Controls:

- keep original filing package and submission timestamp;
- store corrected source record and reason code;
- identify affected clients, accounts, products and periods;
- document whether amended filing, client notice or next-period correction is required;
- preserve maker/checker sign-off and final output hash where available.

## 13. Withholding Reclaim Tracking

Scenario:

A client paid excess withholding and a reclaim is filed with the tax authority.

| Attribute | Value |
|---|---:|
| Gross dividend | 10,000 |
| Tax withheld at source | 3,000 |
| Expected treaty withholding | 1,500 |
| Potential reclaim | 1,500 |
| Reclaim fee | 100 |

Calculation:

```text
gross reclaim receivable = tax withheld - expected treaty withholding
gross reclaim receivable = 3,000 - 1,500 = 1,500

net expected reclaim = gross reclaim receivable - reclaim fee
net expected reclaim = 1,500 - 100 = 1,400
```

Correct treatment:

- reclaim receivable should be labelled expected, filed, approved, rejected, paid, expired or written off;
- cash is not available until refund is received;
- fees should be tracked separately;
- client reporting should distinguish paid withholding, expected reclaim and received refund.

## 14. Multi-Entity Beneficial-Owner Reporting

Scenario:

A trust owns an investment account through a holding company. Reporting requires separate legal owner, beneficial owner and reporting recipient views.

| Role | Entity |
|---|---|
| Legal account owner | Holding Company A |
| Beneficial owner | Family Trust B |
| Controlling person | Settlor C |
| Reporting recipient | Trustee D |

Correct treatment:

- legal ownership, beneficial ownership, controlling person and reporting recipient are separate fields;
- visibility and report distribution follow entitlement and privacy rules;
- tax residency and reportability may attach to different parties depending on regime and source policy;
- do not duplicate income across legal and beneficial owner reports without consolidation rules.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Beneficial-owner data is missing | Reportability is exceptioned or fails closed according to policy. |
| Trustee changes | Reporting recipient updates from effective date with audit trail. |
| Holding company and trust both request report | Output scope and duplication controls are explicit. |
| Privacy restriction applies | Sensitive party details are masked or omitted according to entitlement policy. |

## 15. Tax-Lot Transfer Between Custodians

Scenario:

- Client transfers 300 shares from Custodian A to Custodian B.
- Custodian A sends position quantity immediately.
- Cost-basis lots arrive two days later.

Transferred lots:

| Lot | Quantity | Local cost per share | Acquisition date |
|---|---:|---:|---|
| Lot 1 | 100 | USD 40.00 | 2024-03-15 |
| Lot 2 | 200 | USD 52.00 | 2025-08-20 |

Correct treatment:

- do not treat the transfer as a sale when beneficial ownership is unchanged;
- receive and reconcile position quantity separately from tax-lot history;
- mark realized-gain reporting as partial until lot history is received;
- preserve original acquisition dates and cost basis where the source provides them;
- record custodian source, received date and any missing-lot exception.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Position arrives before lots | Holding can appear, but realized-gain reporting is lot-history incomplete. |
| Lot quantity does not equal transferred quantity | Transfer remains in reconciliation exception. |
| Custodian sends average cost only | Report labels method and support limitation. |
| Subsequent sale occurs before lot history arrives | Realized result is blocked or estimated according to policy. |

## 16. Multi-Currency Realized Gain Basis

Scenario:

A EUR security is bought and sold in EUR for a USD-reporting client. Local gain is positive, but USD gain differs because FX rates changed.

| Attribute | Buy | Sell |
|---|---:|---:|
| Local value | EUR 100,000 | EUR 110,000 |
| EUR/USD FX rate | 1.2000 | 1.0500 |
| USD value | 120,000 | 115,500 |

Calculation:

```text
local realized gain = 110,000 - 100,000 = 10,000 EUR
reporting-currency result = 115,500 - 120,000 = -4,500 USD
```

Correct treatment:

- local security gain and reporting-currency gain should be explainable separately;
- tax basis currency must come from jurisdiction/report policy;
- FX rate source, date policy and rounding should be visible;
- do not overwrite local economic gain with reporting-currency loss or vice versa.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| FX rate is stale | Reporting-currency gain is stale or blocked. |
| Jurisdiction requires local basis | Report does not substitute reporting-currency result. |
| Client statement shows both views | Labels distinguish local gain, FX effect and reporting-currency result. |

## 17. Withholding Pool Reconciliation

Scenario:

A pooled withholding file aggregates tax withheld for multiple clients. The platform must reconcile pool total to allocated client tax amounts.

| Client | Allocated withholding |
|---|---:|
| Client A | 300 |
| Client B | 450 |
| Client C | 250 |

Pool file withholding total: 1,050.

Calculation:

```text
allocated total = 300 + 450 + 250 = 1,000
reconciliation break = 1,050 - 1,000 = 50
```

Correct treatment:

- pool-level withholding and client-level allocations should both be stored;
- report output should not be final while material pool breaks remain unresolved;
- allocation rule, source file, currency and payment date must be traceable;
- resolved breaks should preserve adjustment reason and approval evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Pool total differs from allocations | Reconciliation break opens. |
| Break is immaterial by policy | Report can proceed only with exception label and approval. |
| Allocation changes after report | Correction workflow identifies impacted clients and outputs. |

## 18. Report Delivery Entitlement

Scenario:

A family office requests tax reports for multiple related accounts, but beneficiary visibility differs by entity and document.

| Recipient | Allowed scope |
|---|---|
| Trustee | Full trust tax pack |
| Investment advisor | Holdings and income summary, no beneficiary details |
| Beneficiary | Distribution statement only |

Correct treatment:

- report delivery should follow role, entity, governing document and effective date;
- report generation and report delivery are separate controls;
- restricted sections should be masked or omitted, not merely hidden in the UI;
- delivery audit should show recipient, version, channel, timestamp and scope.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Recipient lacks entitlement | Delivery is blocked even if report exists. |
| Entitlement changes mid-period | Delivery follows effective-dated authority. |
| Report is regenerated | Recipient receives only the authorized version and scope. |

## 19. Client Correction Notice

Scenario:

A corrected tax breakdown changes a prior client tax pack after publication.

| Field | Original | Corrected |
|---|---:|---:|
| Fund income | 8,000 | 6,500 |
| Return of capital | 0 | 1,500 |

Notice requirements:

- identify corrected report version and prior report version;
- state the source reason without giving personalized tax advice;
- show impacted fields, period and whether the notice supersedes or supplements the original;
- include support contact, generation date and approval evidence;
- retain delivery audit and client/advisor acknowledgement where required.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Correction is material | Notice workflow is triggered according to policy. |
| Correction is non-material | Decision and approval are recorded. |
| Recipient entitlement differs | Correction notice follows same delivery controls as the original report. |

## 20. Late Fund Tax-Breakdown Ingestion

Scenario:

A fund pays a distribution in March. At payment time, only an estimated income classification is available. The final tax breakdown arrives in June.

| Classification | Estimated | Final |
|---|---:|---:|
| Income | 5,000 | 3,500 |
| Capital gain | 0 | 1,000 |
| Return of capital | 0 | 500 |

Correct workflow:

```text
book distribution with estimated classification -> label report partial -> ingest final breakdown -> compare impact -> correct report output or carry forward according to policy
```

Controls:

- estimated and final classifications should both be preserved;
- report labels should show estimated state until final breakdown arrives;
- late breakdown should trigger impact assessment across income, withholding, cost basis and client statements;
- final classification source, received date and effective period should be stored.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Final breakdown arrives after report | Correction or next-period disclosure workflow is evaluated. |
| Final amount differs by category | Income, gain and return-of-capital labels update with lineage. |
| Final source is missing | Estimated classification remains labelled and not treated as final. |

## 21. Wash-Sale Style Control

Scenario:

A client sells a security at a loss and buys back the same or substantially similar security within a configured restriction window. The platform should flag the pattern for review without presenting a final legal tax conclusion.

| Attribute | Value |
|---|---:|
| Sale proceeds | 92,000 |
| Cost basis | 110,000 |
| Realized loss | 18,000 |
| Repurchase amount | 95,000 |
| Configured monitoring window | 30 days |
| Days between sale and repurchase | 12 |

Calculation:

```text
realized_loss = 92,000 - 110,000 = -18,000
within_monitoring_window = 12 <= 30
flagged_loss_amount = 18,000
```

Correct treatment:

- flag the pattern using configured jurisdiction/account policy rather than hardcoding a universal rule;
- preserve sale lot, repurchase trade, security similarity basis, dates and exception owner;
- label result as wash-sale style review, not final disallowance, unless a sourced tax rule applies;
- keep realized P&L and reportable tax treatment as separate states.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Repurchase is inside configured window | Review flag opens with linked trades/lots. |
| Security similarity mapping is missing | Flag is source-limited rather than silently absent. |
| Jurisdiction does not apply rule | Monitoring can be disabled or informational only by configuration. |
| Client report is generated | Output does not state final tax treatment without sourced rule. |

## 22. Cross-Border Tax-Lot Method Conflict

Scenario:

A client has reporting obligations in two jurisdictions. One report requires FIFO, while another permits average cost. The same sale must preserve method-specific realized results without corrupting the underlying lot history.

| Lot | Quantity | Cost per unit |
|---|---:|---:|
| Lot A | 100 | 40.00 |
| Lot B | 100 | 60.00 |
| Sale | 150 | 70.00 |

Calculation:

```text
fifo_cost = 100 x 40 + 50 x 60 = 7,000
fifo_gain = 150 x 70 - 7,000 = 3,500
average_cost = (100 x 40 + 100 x 60) / 200 = 50
average_cost_gain = 150 x 70 - 150 x 50 = 3,000
method_difference = 500
```

Correct treatment:

- store lot history independently from jurisdiction-specific reporting method;
- compute method-specific outputs with clear report labels and calculation version;
- prevent one jurisdiction's tax-lot method from overwriting another's basis view;
- preserve policy source, method, effective date and report output version.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Two jurisdictions use different methods | Both outputs are produced from the same source lots. |
| Method configuration is missing | Report is blocked or labelled incomplete. |
| Lots are later corrected | Both method-specific reports are impact-assessed. |

## 23. Mid-Year Tax Residency Change

Scenario:

A client changes tax residency during the year. Income, withholding, CRS/FATCA status and report delivery may need to be split by effective date.

| Attribute | Before change | After change |
|---|---|---|
| Residency | Country A | Country B |
| Effective date | 2026-01-01 | 2026-07-01 |
| Dividend before change | 8,000 | - |
| Dividend after change | - | 6,000 |
| Withholding rate | 15% | 25% |

Calculation:

```text
withholding_before = 8,000 x 15% = 1,200
withholding_after = 6,000 x 25% = 1,500
total_withholding = 2,700
```

Correct treatment:

- effective-date residency and documentation status by income event date;
- split reportability, withholding, treaty status and client report output by period where required;
- preserve residency evidence, approval, received date and remediation workflow;
- avoid retroactively changing earlier events unless the source confirms retroactive effect.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Residency changes mid-year | Event classification follows event date and effective date. |
| Documentation arrives late | Prior events are impact-assessed, not overwritten silently. |
| Report spans both periods | Output clearly separates or labels residency periods according to policy. |

## 24. Qualified Intermediary Pool Reporting

Scenario:

A qualified intermediary process reports pooled withholding by documentation pool. Client-level allocations must reconcile to pool totals and documentation status.

| Pool | Pool income | Pool withholding | Allocated client withholding |
|---|---:|---:|---:|
| Documented treaty pool | 500,000 | 75,000 | 75,000 |
| Undocumented pool | 120,000 | 36,000 | 34,500 |

Calculation:

```text
undocumented_pool_break = 36,000 - 34,500 = 1,500
```

Correct treatment:

- separate QI pool totals from client-level allocations;
- reconcile withholding by pool, currency, income type, source country and report period;
- block or exception final filing when material pool breaks remain;
- preserve documentation status, withholding statement, allocation rule and sign-off evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Pool total differs from allocation | Pool reconciliation break opens. |
| Documentation status changes | Client allocation moves pool only from effective date/source rule. |
| Filing is generated | Filing evidence includes pool totals, allocations and unresolved exceptions. |

## 25. Estate Or Trust Distribution Tax Statement

Scenario:

A trust distributes income and capital to beneficiaries. Beneficiary statements need distribution character, source period and entity authority without exposing unrelated beneficiary details.

| Beneficiary | Income distribution | Capital distribution | Withholding |
|---|---:|---:|---:|
| Beneficiary A | 20,000 | 5,000 | 2,000 |
| Beneficiary B | 12,000 | 8,000 | 1,200 |

Correct treatment:

- separate trust-level income, capital, expenses, withholding and beneficiary allocation;
- preserve trustee approval, beneficiary class, entitlement basis and statement version;
- apply delivery entitlements so each beneficiary sees only authorized information;
- avoid treating distribution character as personal tax advice.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Beneficiary class is missing | Statement generation is blocked or exceptioned. |
| Trustee approval changes | Statement version and distribution allocation are regenerated with lineage. |
| Beneficiary requests other beneficiary data | Delivery is blocked or masked according to entitlement. |

## 26. CRS/FATCA Remediation Campaign

Scenario:

A remediation campaign identifies clients with expiring or inconsistent CRS/FATCA documentation. The platform must track outreach, status, reporting impact and restrictions.

| Client group | Accounts | Expiring forms | Inconsistent self-certifications |
|---|---:|---:|---:|
| Individual clients | 240 | 35 | 8 |
| Entity clients | 90 | 18 | 12 |

Calculation:

```text
total_accounts_in_scope = 240 + 90 = 330
total_remediation_items = 35 + 8 + 18 + 12 = 73
remediation_item_rate = 73 / 330 = 22.12%
```

Correct treatment:

- track campaign id, client/account scope, document type, due date, outreach status and reportability impact;
- distinguish expired, missing, inconsistent and under-review documentation states;
- restrict report finalization or apply configured reporting status where documentation remains unresolved;
- preserve communications, reminders, waivers and compliance sign-off.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Documentation expires before report date | Reportability/remediation state updates by effective date. |
| Client submits inconsistent form | Review workflow opens instead of marking complete. |
| Campaign closes with unresolved items | Residual exceptions and reporting impact are visible. |

## 27. Regulatory Filing Rejection Workflow

Scenario:

A regulator rejects a submitted file because a required beneficial-owner identifier is missing for one reportable account. The filing must be corrected and resubmitted without losing the original submission record.

| Attribute | Value |
|---|---:|
| Submitted records | 12,000 |
| Rejected records | 1 |
| Original submission version | v1 |
| Corrected submission version | v2 |

Correct treatment:

- preserve original filing, rejection code, regulator acknowledgement and rejected record id;
- repair source data and regenerate corrected filing version;
- track resubmission, acceptance/rejection state, deadline and approval;
- notify internal owners and affected client-reporting workflows if policy requires.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Filing rejection arrives | Rejection workflow opens with original submission retained. |
| Source data is repaired | Corrected version is generated with delta and approval lineage. |
| Resubmission is accepted | Filing state closes with regulator acknowledgement. |
| Deadline is at risk | Escalation state and owner are visible. |

## 28. Treaty-Rate Expiry At Payment Date

Scenario:

A client had valid treaty documentation when a dividend was announced, but the form expired before payment date. The withholding rate must follow the governing event date policy, not an assumed announcement-date rate.

| Attribute | Value |
|---|---:|
| Gross dividend | 12,000 |
| Treaty rate if valid | 15% |
| Statutory rate if expired | 30% |
| Documentation status on payment date | Expired |

Calculation:

```text
treaty_withholding = 12,000 x 15% = 1,800
statutory_withholding = 12,000 x 30% = 3,600
withholding_difference = 1,800
```

Correct treatment:

- determine the governing documentation date from jurisdiction and income-event rules;
- preserve form expiry date, payment date, source country, applied rate and remediation state;
- apply statutory withholding when documentation is expired and no valid relief-at-source evidence exists;
- track potential reclaim separately from withholding applied at payment.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Documentation expires before payment date | Statutory or configured fallback rate applies. |
| Renewed form arrives after payment | Reclaim or correction workflow opens; original withholding remains versioned. |
| Jurisdiction uses record date instead | Rule configuration controls the governing date. |

## 29. Nominee Withholding Statement

Scenario:

A nominee receives a consolidated withholding statement and must allocate withholding to underlying beneficial owners. Pool totals, owner allocations and documentation states must reconcile.

| Beneficial-owner pool | Gross income | Withholding | Documentation state |
|---|---:|---:|---|
| Treaty-documented owners | 300,000 | 45,000 | Valid |
| Undocumented owners | 80,000 | 24,000 | Missing |
| Exempt owners | 40,000 | 0 | Valid exemption |

Calculation:

```text
total_gross_income = 300,000 + 80,000 + 40,000 = 420,000
total_withholding = 45,000 + 24,000 + 0 = 69,000
effective_withholding_rate = 69,000 / 420,000 = 16.43%
```

Correct treatment:

- preserve nominee statement, beneficial-owner allocation, documentation pool and income type;
- reconcile nominee-level totals to beneficial-owner allocations before final reporting;
- keep exempt, treaty and undocumented pools separate;
- prevent unrelated beneficial-owner details from appearing in client statements.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Pool allocations reconcile | Nominee statement can be used for owner-level reporting. |
| Allocation total differs from nominee total | Reconciliation break blocks or labels final output. |
| Documentation state changes after statement | Impact assessment versions allocations by effective date. |

## 30. Tax Voucher Correction Chain

Scenario:

A custodian issues an original tax voucher, then two corrected vouchers after issuer and withholding-agent updates. Reporting must preserve the chain instead of overwriting the voucher in place.

| Voucher version | Gross income | Withholding | Reason |
|---|---:|---:|---|
| v1 | 10,000 | 2,500 | Original |
| v2 | 10,000 | 2,000 | Treaty rate corrected |
| v3 | 10,500 | 2,100 | Gross income corrected |

Correction impact:

```text
withholding_delta_v3_vs_v1 = 2,100 - 2,500 = -400
gross_income_delta_v3_vs_v1 = 10,500 - 10,000 = 500
```

Correct treatment:

- preserve each voucher version, source timestamp, correction reason and superseded status;
- regenerate impacted client reports and filings from the approved latest voucher;
- retain original report versions and client correction notices where required;
- do not delete prior voucher evidence or lose audit lineage.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Corrected voucher arrives | New voucher version supersedes prior version with lineage. |
| Report was already delivered | Correction notice or amended output workflow is triggered. |
| Voucher correction conflicts with cash ledger | Reconciliation exception opens rather than silent overwrite. |

## 31. Portfolio Transfer Year-End Statement

Scenario:

A portfolio transfers to another custodian during the year. The client needs a year-end tax pack that combines pre-transfer and post-transfer activity without duplicating income or realized gains.

| Period | Gross income | Withholding | Realized gain |
|---|---:|---:|---:|
| Pre-transfer custodian | 18,000 | 3,200 | 11,500 |
| Post-transfer custodian | 9,000 | 1,350 | 4,000 |

Year-end totals:

```text
year_end_income = 18,000 + 9,000 = 27,000
year_end_withholding = 3,200 + 1,350 = 4,550
year_end_realized_gain = 11,500 + 4,000 = 15,500
```

Correct treatment:

- preserve old custodian, new custodian, transfer date, activity periods and tax-lot continuity state;
- reconcile transferred positions, lots and income cut-off before final statement;
- prevent duplicate reporting when both custodians report the same transfer-date income event;
- label missing pre-transfer or post-transfer evidence explicitly.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Both custodian files are present | Year-end statement combines periods without duplication. |
| Transfer-date income appears in both files | Duplicate detection opens reconciliation break. |
| Old custodian lot history is missing | Realized-gain output is provisional or blocked. |

## 32. Withholding-Rate Override Governance

Scenario:

Operations proposes a temporary withholding-rate override after a vendor file applies the wrong rate. The override must be approved, scoped and expired.

| Attribute | Value |
|---|---:|
| Gross income affected | 250,000 |
| Vendor withholding rate | 30% |
| Approved override rate | 15% |
| Override expiry | Next vendor correction file |

Override impact:

```text
vendor_withholding = 250,000 x 30% = 75,000
override_withholding = 250,000 x 15% = 37,500
withholding_reduction = 37,500
```

Correct treatment:

- preserve override request, approval, scope, reason, effective date and expiry condition;
- apply override only to approved income events, clients, products and jurisdictions;
- automatically re-evaluate when corrected source files arrive;
- keep audit evidence so the override does not become permanent hidden logic.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Override is approved | Calculations use scoped override with source label and expiry. |
| Override approval is missing | Vendor rate remains or report is exceptioned. |
| Corrected vendor file arrives | Override expires or is reconciled against corrected source. |

## 33. Cross-Border Report Suppression Rule

Scenario:

A client is in scope for a report, but cross-border delivery rules suppress the report because the recipient is not authorized in the destination jurisdiction.

| Attribute | Value |
|---|---|
| Report type | Annual tax pack |
| Client booking centre | Country A |
| Recipient location | Country B |
| Delivery rule | Suppress without approved cross-border permission |
| Advisor override | Not approved |

Correct treatment:

- preserve report generation state separately from delivery suppression state;
- apply cross-border delivery rules by recipient, booking centre, report type and effective date;
- show authorized internal users that the report exists but cannot be delivered externally;
- require approved override evidence before release.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Report is generated but suppressed | Output is retained internally and not delivered externally. |
| Recipient location changes | Delivery eligibility recalculates from effective-dated evidence. |
| Override is approved | Delivery proceeds with approval lineage and report version. |

## 34. Tax Residency Self-Certification Conflict

Scenario:

A client has an existing self-certification for one jurisdiction, a new address record in another jurisdiction and a recently submitted tax form listing a third jurisdiction. The platform must not collapse these signals into a single inferred tax residency.

| Evidence source | Jurisdiction | Effective date | Status |
|---|---|---|---|
| Existing self-certification | Country A | 1 Jan | Active |
| KYC address update | Country B | 15 Jun | Pending review |
| New tax form | Country C | 20 Jun | Submitted |

Conflict indicator:

```text
active_jurisdiction_signal_count = count_distinct(Country A, Country B, Country C) = 3
conflict_count = active_jurisdiction_signal_count - 1 = 2
```

Correct treatment:

- preserve every source, effective date, document status and review state;
- open a residency conflict exception instead of selecting the latest value blindly;
- use only source-backed, approved residency values for withholding, reportability and delivery rules;
- label downstream reports as blocked, provisional or remediation-dependent when the conflict affects output.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Multiple active residency signals exist | Conflict exception opens with source lineage. |
| One document is only submitted | It does not become approved reporting status until validated. |
| Conflict affects a filing period | Filing or client report is blocked or explicitly exception-labelled. |

## 35. Beneficial-Owner Look-Through Break

Scenario:

An entity account holds reportable assets, but the look-through chain is incomplete because one controlling person record is missing tax documentation. Reporting cannot treat the entity as fully resolved.

| Component | Reportable value | Documentation status |
|---|---:|---|
| Documented beneficial owners | 7,500,000 | Complete |
| Undocumented controlling person allocation | 2,500,000 | Missing |
| Entity total | 10,000,000 | Partially complete |

Coverage:

```text
look_through_coverage = documented_value / entity_total
look_through_coverage = 7,500,000 / 10,000,000 = 75%
undocumented_value = 2,500,000
```

Correct treatment:

- separate legal owner, beneficial owner, controlling person and reporting recipient roles;
- preserve ownership percentage, documentation state, review owner and remediation due date;
- block final beneficial-owner reporting where the unresolved gap is material;
- avoid exposing unrelated beneficial-owner details to unauthorized recipients.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| One controlling person is undocumented | Entity reporting is partial or blocked based on materiality rule. |
| Documentation later arrives | Coverage recalculates with effective date and audit lineage. |
| User lacks authority for owner details | Report shows exception state without leaking restricted data. |

## 36. Amended CRS File Cancellation

Scenario:

A CRS filing has already been submitted. A correction file is prepared, but operations determines that the original correction indicator was wrong and the amended file must be cancelled before a clean replacement is sent.

| File version | Records | State |
|---|---:|---|
| Original filing | 1,200 | Accepted |
| Incorrect amendment | 80 | Prepared for cancellation |
| Replacement amendment | 75 | Pending approval |

Net replacement scope:

```text
net_reportable_records_after_cancellation = original_records - cancelled_amendment_records + replacement_amendment_records
net_reportable_records_after_cancellation = 1,200 - 80 + 75 = 1,195
```

Correct treatment:

- preserve original filing id, incorrect amendment id, cancellation id and replacement id;
- prevent duplicate submission of both the cancelled amendment and the replacement amendment;
- retain regulator acknowledgement, correction reason and approval evidence;
- show report operators which version is active for regulator and client-impact views.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Amendment is cancelled | Cancelled file cannot be resent as active output. |
| Replacement amendment is approved | Active filing lineage points to original plus replacement, not cancelled draft. |
| Regulator acknowledgement is missing | Filing state remains pending or exceptioned. |

## 37. Withholding Reclaim Denial

Scenario:

A withholding reclaim is filed on a dividend where the client expected treaty relief. The reclaim is denied because the residency certificate was expired on the income event date.

| Attribute | Value |
|---|---:|
| Gross income | 120,000 |
| Statutory withholding rate | 30% |
| Expected treaty rate | 15% |
| Reclaim fee | 600 |

Denied reclaim:

```text
statutory_withholding = 120,000 x 30% = 36,000
expected_treaty_withholding = 120,000 x 15% = 18,000
expected_reclaim_before_fee = 36,000 - 18,000 = 18,000
expected_reclaim_after_fee = 18,000 - 600 = 17,400
denied_reclaim_amount = 17,400
```

Correct treatment:

- keep reclaim expectation, filing, denial notice, denial reason and fee separately from original withholding;
- do not book denied reclaim as cash or available liquidity;
- trigger document remediation or client communication where appropriate;
- preserve the distinction between tax authority denial and custodian cash settlement.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Residency certificate is expired | Reclaim denial is linked to documentation evidence. |
| Reclaim was previously estimated | Estimate reverses or is labelled denied, not received cash. |
| New certificate is supplied later | Future events can use new status, but denied event is not silently restated. |

## 38. Instrument Tax-Classification Override

Scenario:

A structured income event is initially classified as ordinary debt interest. A source-backed tax-classification update later reclassifies part of the amount as return of capital. The platform must version the classification without changing the underlying cash event.

| Classification | Original amount | Corrected amount |
|---|---:|---:|
| Interest income | 50,000 | 35,000 |
| Return of capital | 0 | 15,000 |
| Total cash event | 50,000 | 50,000 |

Classification delta:

```text
interest_delta = 35,000 - 50,000 = -15,000
return_of_capital_delta = 15,000 - 0 = 15,000
total_cash_delta = 50,000 - 50,000 = 0
```

Correct treatment:

- preserve the original cash event, original tax category and corrected category version;
- require source-backed override evidence, approval, effective date and impacted report period;
- recalculate report classifications without creating duplicate cash;
- show amended client outputs when the classification change affects delivered reports.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Tax category changes but cash is unchanged | Report classification changes while ledger cash remains stable. |
| Override lacks approval | Original classification remains active or report is exceptioned. |
| Delivered tax pack is impacted | Restatement workflow identifies affected fields and report versions. |

## 39. Client Tax-Pack Restatement Notice

Scenario:

A client annual tax pack has already been delivered. A late fund tax-character update changes the income mix, so the client needs a restatement notice that explains the changed fields, prior version and corrected version.

| Field | Original pack | Restated pack | Delta |
|---|---:|---:|---:|
| Ordinary income | 42,000 | 36,500 | -5,500 |
| Capital gain distribution | 18,000 | 23,500 | 5,500 |
| Total reportable amount | 60,000 | 60,000 | 0 |

Restatement:

```text
ordinary_income_delta = 36,500 - 42,000 = -5,500
capital_gain_delta = 23,500 - 18,000 = 5,500
total_reportable_delta = 60,000 - 60,000 = 0
```

Correct treatment:

- preserve original tax pack, restated tax pack, source update, approval and delivery evidence;
- identify changed fields, unchanged totals and impacted reporting period clearly;
- apply delivery entitlement and cross-border suppression rules before sending the notice;
- keep advisor, operations and client-service views aligned to the same active report version.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Source update changes classification only | Restatement notice shows field deltas while total cash remains unchanged. |
| Recipient is not delivery-authorized | Restatement is retained internally and not delivered externally. |
| Advisor opens client view | Active version and prior version are both visible with correction reason. |

## 40. Tax Voucher Duplicate Suppression

Scenario:

A custodian sends two tax voucher files for the same dividend event. The second file has a later timestamp but identical voucher identity and amounts. The platform must suppress duplicate delivery while preserving audit lineage.

| Voucher | Gross income | Withholding | State |
|---|---:|---:|---|
| Original voucher | 80,000 | 12,000 | Delivered |
| Duplicate voucher | 80,000 | 12,000 | Suppressed |

Duplicate check:

```text
duplicate_key = income_event_id + voucher_number + gross_income + withholding + tax_year
suppressed_duplicate_count = 1
```

Correct treatment:

- preserve both inbound files, source timestamps, voucher number, income event id and duplicate decision;
- do not create a second client voucher, second report line or duplicate withholding record;
- allow a corrected voucher only when version, amount, correction reason or source status changes;
- expose suppression evidence to operations without delivering duplicate client output.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Exact duplicate voucher arrives | Duplicate is suppressed and original active voucher remains unchanged. |
| Same voucher number has changed amount | Correction workflow opens rather than duplicate suppression. |
| Client tax pack is generated | Only one active voucher appears with audit lineage. |

## 41. Cross-Border Reporting Blackout Window

Scenario:

A booking centre enforces a blackout window before regulatory filing cut-off. Reports can be generated for review but cannot be delivered externally until compliance release is confirmed.

| Attribute | Value |
|---|---:|
| Reports generated | 420 |
| Reports in blackout scope | 95 |
| Externally deliverable before release | 0 |
| Internal review exceptions | 8 |

Blackout control:

```text
delivery_blocked_count = reports_in_blackout_scope - compliance_released_count
delivery_blocked_count = 95 - 0 = 95
```

Correct treatment:

- preserve blackout rule, jurisdiction, booking centre, report type, effective dates and compliance release state;
- allow internal generation, reconciliation and sign-off without external client delivery;
- apply delivery entitlement and blackout rule together, not as independent manual filters;
- release only the approved version after compliance confirmation and report-version freeze.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Report is generated during blackout | Report remains internal and externally blocked. |
| Compliance release is approved | Delivery resumes only for the approved report version. |
| Advisor attempts delivery | UI/API shows blocked state and reason without exposing restricted documents. |

## 42. Entity Classification Remediation

Scenario:

An entity account has conflicting classification between onboarding and tax documentation. Filing cannot be finalized until the active entity classification is remediated.

| Classification source | Entity type | State |
|---|---|---|
| Onboarding/KYC | Passive NFFE | Active |
| Tax self-certification | Financial institution | Active |
| Operations remediation | Pending review | Open |

Conflict count:

```text
active_entity_classifications = 2
classification_conflict_count = active_entity_classifications - 1 = 1
```

Correct treatment:

- preserve each source classification, effective date, document version, reviewer, decision owner and remediation due date;
- block or label impacted filings while the material classification conflict remains unresolved;
- avoid overwriting source documents with a manually selected classification without approval evidence;
- update reportability, withholding pool and CRS/FATCA output only from the approved active classification.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Two active classifications conflict | Remediation case opens and impacted output is blocked or partial. |
| Reviewer approves classification | Active classification changes with effective-dated lineage. |
| Filing is regenerated | Reportability and withholding pools use the approved classification only. |

## 43. Missing Treaty Documentation Ageing

Scenario:

A client is eligible for treaty relief only when required documentation is current. Documentation is missing for multiple income events and must be aged for operations follow-up.

| Age bucket | Income events | Gross income | Potential relief |
|---|---:|---:|---:|
| 0-30 days | 6 | 90,000 | 9,000 |
| 31-60 days | 4 | 55,000 | 5,500 |
| 61+ days | 3 | 70,000 | 7,000 |

Ageing exposure:

```text
aged_potential_relief_61_plus = 7,000
total_potential_relief_at_risk = 9,000 + 5,500 + 7,000 = 21,500
```

Correct treatment:

- preserve document requirement, event date, source country, income type, outreach state, due date and ageing bucket;
- apply statutory or fallback withholding when treaty documentation is missing at event date;
- track potential relief at risk separately from booked cash, reclaim estimates and final tax output;
- trigger escalation when ageing exceeds policy threshold or filing deadline risk increases.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Treaty document is missing on event date | Treaty rate is not applied unless policy explicitly allows it. |
| Ageing crosses threshold | Operations escalation and report exception state are updated. |
| Document arrives later | Future events use updated status; prior events follow correction or reclaim workflow. |

## 44. Withholding Pool Variance Remediation

Scenario:

A qualified intermediary withholding pool file does not reconcile to allocated client withholding. The variance must be remediated before final filing.

| Attribute | Value |
|---|---:|
| Pool file withholding total | 1,250,000 |
| Sum of client allocations | 1,238,500 |
| Materiality threshold | 2,000 |
| Unallocated variance | 11,500 |

Variance:

```text
pool_variance = pool_file_total - sum_client_allocations
pool_variance = 1,250,000 - 1,238,500 = 11,500
```

Correct treatment:

- preserve pool file, allocation run, client allocation records, variance reason, remediation owner and sign-off state;
- block final filing or label it exceptioned when variance exceeds threshold;
- separate true source variance, timing difference, duplicate allocation and manual adjustment cases;
- release filing only after corrected source, approved adjustment or documented immateriality decision.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Variance exceeds threshold | Filing remains blocked or exceptioned. |
| Corrected allocation arrives | Pool variance recalculates and remediation lineage is retained. |
| Manual adjustment is approved | Adjustment carries owner, reason and sign-off evidence. |

## 45. Multi-Jurisdiction Tax-Pack Delivery Controls

Scenario:

A family-office client requires tax packs for several jurisdictions. Each output uses different report sections, delivery restrictions and recipient entitlements.

| Jurisdiction pack | Generated | Delivery allowed | Suppressed |
|---|---:|---:|---:|
| Jurisdiction A | 12 | 12 | 0 |
| Jurisdiction B | 12 | 9 | 3 |
| Jurisdiction C | 12 | 0 | 12 |

Delivery control:

```text
total_generated = 36
total_deliverable = 12 + 9 + 0 = 21
total_suppressed = 0 + 3 + 12 = 15
```

Correct treatment:

- preserve jurisdiction, report template, recipient role, entity scope, language, suppression rule and delivery approval;
- generate jurisdiction-specific packs from the same source events without mixing incompatible tax classifications;
- prevent delivery when recipient authority, cross-border rule or documentation state blocks the output;
- keep internal evidence for suppressed packs while exposing only permitted outputs to advisors and clients.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Pack is generated but suppressed | Internal evidence remains, external delivery is blocked. |
| Recipient has partial authority | Only authorized jurisdictions/entities are deliverable. |
| Source event changes | Impact assessment identifies every jurisdiction pack affected. |

## 46. Advisory And Reporting Boundary

| Activity | Platform can support | Platform should not imply |
|---|---|---|
| show gross/net income | source-backed amounts and withholding | personalized tax advice |
| show realized gain | configured lot method and source data | final tax liability |
| show missing documentation | remediation status | legal conclusion on residency |
| show reportable event | configured reporting regime | universal reporting obligation |
| simulate rebalance tax impact | estimated gains/losses under assumptions | advice to execute for tax reasons |

## 47. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Income and withholding | gross, tax, net, source country, rate, document status | treaty optimization workflow |
| Tax lots | lot quantity, basis, method, sale depletion, transferred-lot lineage | multi-jurisdiction tax-lot engine |
| Corporate actions | source event, entitlement, basis allocation when supplied | automated tax impact classification |
| Cross-border documentation | residency, FATCA/CRS status, expiry, remediation state, beneficial-owner role where sourced | deeper jurisdiction-specific rule engine |
| Report output | versioned statement/tax pack with lineage, exception section, calculation version and delivery entitlement | configurable regulatory filing generator |
| Corrections | restatement, amended-output workflow, client correction notice and sign-off evidence | automated client notice workflow |
| Reclaims and pools | expected reclaim, filed status, fees, refund receipt, pool total and allocation reconciliation when source-backed | tax authority workflow integration |
| Ownership reporting | legal owner, beneficial owner, controlling person, reporting recipient and delivery scope where sourced | cross-regime entity classification engine |
| Late classifications | estimated/final tax breakdown, received date and impact assessment where sourced | automated late-breakdown correction and client-notice orchestration |
| Cross-border controls | residency changes, documentation pools, method-specific output and filing status where sourced | full jurisdiction-specific tax-rule engine and regulator integration |
| Conflicts and cancellations | conflicting self-certifications, look-through gaps, cancelled filing versions, denied reclaims, classification overrides and restatement notices where source-backed | automated regulator workflow orchestration and legal interpretation |
| Delivery and remediation controls | duplicate voucher suppression, blackout states, entity classification remediation, treaty-document ageing, pool variance remediation and multi-jurisdiction delivery controls where source-backed | automated tax authority workflow orchestration, jurisdiction-specific legal interpretation and client-specific tax advice |

## 48. Regression Test Pack

Minimum release-gate scenarios:

1. Dividend applies treaty rate only when documentation is valid.
2. Bond accrued interest paid is not double counted as coupon income.
3. FIFO sale depletes lots and computes realized gain.
4. Corporate action basis allocation preserves source evidence.
5. Fund distribution splits income, gain and return of capital.
6. Structured note physical delivery creates support-limited tax treatment if policy is missing.
7. Expired CRS/FATCA documentation creates remediation and reporting exception.
8. Corrected withholding source creates versioned report correction.
9. Missing source classification blocks client-ready tax category.
10. Tax estimate is labelled as estimate, not final liability.
11. Jurisdiction-specific rule configuration applies different rates by documentation and effective date.
12. Tax report output includes lineage, exceptions, report version and sign-off state.
13. Amended filing preserves original submission and corrected output.
14. Withholding reclaim is tracked separately from received cash.
15. Multi-entity beneficial-owner reporting separates legal owner, beneficial owner, controlling person and report recipient.
16. Tax-lot transfer preserves ownership continuity while marking realized-gain reporting incomplete until lots reconcile.
17. Multi-currency realized gain separates local economic result, FX effect and reporting-currency basis.
18. Withholding pool reconciliation blocks or labels final output when pool and allocation totals break.
19. Report delivery entitlement prevents unauthorized tax-pack delivery.
20. Client correction notice identifies prior version, corrected fields, impact period, delivery scope and approval.
21. Late fund tax-breakdown ingestion preserves estimated and final classifications with correction lineage.
22. Wash-sale style control flags linked loss sale and repurchase patterns without asserting final tax treatment.
23. Cross-border tax-lot method conflict preserves source lots and produces method-specific report outputs.
24. Mid-year residency change applies event-date effective residency and documentation status.
25. QI pool reporting reconciles pool totals, client allocations and documentation status before filing.
26. Estate or trust distribution statement separates beneficiary allocations and delivery entitlement.
27. CRS/FATCA remediation campaign tracks document states, outreach, reportability impact and unresolved exceptions.
28. Regulatory filing rejection preserves original submission and corrected resubmission evidence.
29. Treaty-rate expiry applies the governing event-date rule and tracks reclaim or correction workflow separately.
30. Nominee withholding statement reconciles pool totals to beneficial-owner allocations and documentation states.
31. Tax voucher correction chain preserves original and corrected voucher versions with report impact lineage.
32. Portfolio transfer year-end statement combines pre-transfer and post-transfer activity without duplication.
33. Withholding-rate override governance requires scoped approval, source label and expiry handling.
34. Cross-border report suppression prevents unauthorized delivery while preserving internal report evidence.
35. Tax residency self-certification conflict opens an exception and blocks affected final output until resolved.
36. Beneficial-owner look-through break calculates coverage and prevents unsupported final owner reporting.
37. Amended CRS file cancellation preserves original, cancelled and replacement filing lineage.
38. Withholding reclaim denial reverses estimated reclaim availability and links to documentation evidence.
39. Instrument tax-classification override changes report classification without duplicating cash events.
40. Client tax-pack restatement notice identifies prior version, corrected fields, delivery entitlement and approval lineage.
41. Tax voucher duplicate suppression prevents duplicate client output while preserving inbound file lineage.
42. Cross-border reporting blackout windows allow internal review but block external delivery until release.
43. Entity classification remediation blocks or labels filings until source-backed active classification is approved.
44. Missing treaty documentation ageing tracks potential relief at risk and escalates stale remediation items.
45. Withholding pool variance remediation blocks final filing when material pool/allocation breaks remain.
46. Multi-jurisdiction tax-pack delivery controls generate evidence while delivering only authorized outputs.
