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

## 21. Advisory And Reporting Boundary

| Activity | Platform can support | Platform should not imply |
|---|---|---|
| show gross/net income | source-backed amounts and withholding | personalized tax advice |
| show realized gain | configured lot method and source data | final tax liability |
| show missing documentation | remediation status | legal conclusion on residency |
| show reportable event | configured reporting regime | universal reporting obligation |
| simulate rebalance tax impact | estimated gains/losses under assumptions | advice to execute for tax reasons |

## 22. Current Support Boundary And Candidate Extensions

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

## 23. Regression Test Pack

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
