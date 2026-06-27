# 08 - Platform Controls, Reconciliation, Test Scenarios and Glossary

## 1. Key controls

| Control | Purpose |
|---|---|
| Gross-tax-net reconciliation | Gross income minus tax equals net cash |
| Cash ledger reconciliation | Tax and income entries tie to cash movements |
| Tax-lot reconciliation | Open lots tie to positions |
| Withholding rate validation | Applied rate supported by product/source/client documentation |
| Form validity check | Tax documentation valid at payment date |
| CRS/FATCA completeness | Required fields available for reportable accounts |
| Year-end balance snapshot | Reportable account values frozen and auditable |
| Correction workflow | Reversals and restatements handled consistently |
| Source-to-report lineage | Every report line traceable to source event |
| Maker-checker for overrides | Manual changes controlled |

## 2. Reconciliation examples

### Income reconciliation

```text
Sum gross income
- sum withholding tax
- sum income-related fees/taxes
= net income cash movement
```

### Position and lot reconciliation

```text
Position quantity = sum open tax-lot quantities
Position cost = sum open tax-lot cost basis
```

### CRS/FATCA aggregation

```text
Reportable income by account/year
= sum classified reportable income events for that account/year
```

## 3. Exception categories

| Exception | Example |
|---|---|
| Missing documentation | W-8 expired, CRS self-cert missing |
| Inconsistent data | Address country conflicts with tax residence |
| Missing TIN | Reportable jurisdiction requires TIN |
| Unknown cost basis | Transfer-in without historical cost |
| Missing source country | Income cannot be assigned WHT logic |
| Unclassified distribution | Fund distribution components unavailable |
| Negative withholding | Correction not linked to original income |
| Broken gross/net | Gross minus WHT does not equal net cash |
| Corporate action mismatch | Basis adjustment not equal to original cost |
| Report rejection | Tax authority schema/business-rule rejection |

## 4. QA scenarios

### Scenario 1: Foreign dividend with withholding

Given:

- gross dividend USD 1,000;
- withholding tax USD 300;
- net cash USD 700.

Expected:

- income report shows gross, WHT and net;
- cash ledger shows USD 700 cash increase;
- tax withheld report shows USD 300;
- performance can show gross/net depending policy;
- source country and tax form basis captured.

### Scenario 2: Expired tax form causes higher withholding

Given:

- client previously had valid W-8BEN;
- form expired before dividend payment date;
- statutory withholding applied.

Expected:

- system applies non-reduced/default rate;
- exception raised for expired form;
- advisor dashboard shows remediation item;
- later form update does not silently restate old payment unless approved correction/reclaim process exists.

### Scenario 3: Fund distribution component received late

Given:

- fund distribution paid as provisional income;
- later official component split says 60% dividend, 30% capital gain, 10% return of capital.

Expected:

- original transaction preserved;
- tax classification updated/versioned;
- cost basis adjusted for return of capital if applicable;
- statement correction or year-end tax pack reflects final classification.

### Scenario 4: Stock split and later sale

Given:

- buy 100 shares at 50;
- 2-for-1 split;
- sell 50 shares at 30.

Expected:

- quantity becomes 200;
- basis per share becomes 25;
- sale basis for 50 shares is 1,250;
- realised P&L calculated correctly.

### Scenario 5: Transfer-in with missing cost

Given:

- transfer in 1,000 shares;
- custodian sends no cost basis.

Expected:

- position created;
- cost basis marked unknown;
- realised P&L report flags incomplete cost;
- no silent zero-cost assumption unless configured and disclosed.

### Scenario 6: CRS passive entity with missing controlling person

Given:

- account holder is passive entity;
- no controlling person details captured.

Expected:

- CRS reportability exception raised;
- account excluded from final submission until remediated or handled by compliance rule;
- audit trail records remediation.

### Scenario 7: Tax reclaim receipt

Given:

- original dividend had WHT USD 300;
- reclaim of USD 100 received six months later.

Expected:

- reclaim receipt linked to original dividend;
- income/tax reports show reclaim separately;
- client cash increases by USD 100;
- performance treatment follows policy.

### Scenario 8: Backdated sale changes lots

Given:

- sale booked late with trade date before a later sale;
- FIFO lots affected.

Expected:

- lot engine replays from backdated trade date;
- realised P&L restated for affected sales;
- statement correction generated if already issued.

## 5. Production support checklist

When a client challenges a tax/reporting number, check:

1. Is the transaction booked correctly?
2. Is the gross/net/tax split correct?
3. Is source country correct?
4. Is product classification correct?
5. Was valid tax documentation present on payment date?
6. Was treaty/reduced rate applied correctly?
7. Is the cash ledger reconciled?
8. Are tax lots complete and correct?
9. Was there a corporate action or correction?
10. Was the report generated using correct version/date?
11. Was the number restated later?
12. Is this official tax reporting or advisory-only reporting?

## 6. Glossary

| Term | Meaning |
|---|---|
| Beneficial owner | Person/entity that economically benefits from account/assets |
| CRS | Common Reporting Standard for automatic exchange of financial account information |
| FATCA | US Foreign Account Tax Compliance Act regime |
| W-8BEN | US tax form for non-US individual beneficial owners |
| W-8BEN-E | US tax form for non-US entity beneficial owners |
| W-9 | US tax form for US persons |
| TIN | Tax identification number |
| Withholding tax | Tax deducted at source before payment is received |
| Gross income | Income before tax and deductions |
| Net income | Income after withholding/fees as defined |
| Tax lot | Specific acquisition lot used for cost basis |
| Cost basis | Cost allocated to a holding or disposed lot |
| Realised gain/loss | Gain/loss from sale/redemption/disposal |
| Return of capital | Distribution that reduces cost basis rather than ordinary income in some regimes |
| Tax reclaim | Process to recover over-withheld tax where eligible |
| Reportable account | Account that must be reported under a regime |
| Controlling person | Natural person exercising control over certain entities/trusts |
| Source country | Country from which income is treated as arising |
| Treaty rate | Reduced withholding rate under tax treaty, if applicable |
| Year-end snapshot | Account balance/value captured for annual reporting |
| Restatement | Corrected version of prior report/statement |

