# 04 - Cross-Border Reporting, CRS, FATCA, QI and Client Tax Documentation

## 1. Why cross-border reporting matters

Private-banking clients often have:

- multiple tax residencies;
- accounts booked outside their home jurisdiction;
- trusts, holding companies and family-office structures;
- US securities, European funds, Singapore accounts and offshore funds;
- beneficial owners different from account holders;
- multiple controlling persons.

A wealth platform must therefore support client and account classification, tax documentation, reportability checks and regulatory reporting.

## 2. CRS overview

The Common Reporting Standard, or CRS, is the global framework for automatic exchange of financial account information between tax jurisdictions. It requires financial institutions in participating jurisdictions to identify reportable account holders and controlling persons and report account information to their local tax authority for exchange with relevant jurisdictions.

For Singapore, IRAS states that CRS is an internationally agreed standard for automatic exchange of financial account information for tax purposes. IRAS also states that Reporting Singaporean Financial Institutions must submit CRS returns, including nil returns where applicable, by 31 May of the year following the calendar year to which the return relates.

## 3. FATCA overview

FATCA is a US regime targeting offshore accounts held by US persons. IRAS states that FATCA requires financial institutions outside the US to report information about financial accounts held by US persons to the US IRS, and that non-compliant financial institutions can face 30% FATCA-related withholding on certain US-source payments.

For Singapore, financial institutions generally report FATCA information to IRAS under the Singapore-US FATCA intergovernmental arrangement rather than directly to the IRS in normal reporting workflows.

## 4. CRS vs FATCA comparison

| Area | CRS | FATCA |
|---|---|---|
| Origin | OECD/global standard | US law/regime |
| Focus | Tax residents of participating jurisdictions | US persons and certain US-owned entities |
| Reporting route | FI -> local tax authority -> partner jurisdictions | FI -> local tax authority/IRS depending IGA model |
| Documentation | Self-certification of tax residence, TIN, entity type | W-9, W-8 series, GIIN, FATCA classification |
| Reportable persons | Tax residents of reportable jurisdictions | Specified US persons and relevant entities |
| Entity look-through | Passive NFEs and controlling persons | Passive NFFEs/substantial US owners |
| Key risk | Missing/mismatched tax residency/TIN | US indicia, invalid forms, withholding exposure |

## 5. Client tax documentation

Typical documents and data points:

| Document/data | Purpose |
|---|---|
| Tax residency self-certification | CRS classification |
| TIN | Tax identification number for reportable jurisdiction |
| W-8BEN | US withholding/FATCA documentation for non-US individuals |
| W-8BEN-E | US withholding/FATCA documentation for non-US entities |
| W-9 | US person certification |
| Entity classification | FI, active NFE/NFFE, passive NFE/NFFE, exempt entity |
| Controlling person details | Look-through for passive entities |
| GIIN | FATCA registration identifier for participating FIs |
| Treaty claim data | Supports reduced withholding where applicable |
| Form validity dates | Determines whether documentation is usable at payment date |

## 6. Beneficial owner and controlling person

For tax reporting, the account holder is not always enough.

Example structures:

| Structure | Reporting challenge |
|---|---|
| Personal account | Account holder is usually beneficial owner |
| Joint account | Multiple beneficial owners/tax residencies |
| Trust | Trustee may be legal owner; settlor/beneficiaries/protector may matter |
| Company account | Entity classification and controlling persons matter |
| Foundation | Founder/council/beneficiaries may matter |
| Family office | Multiple entities, mandates and ultimate beneficial owners |

The platform should store:

- legal owner;
- beneficial owner;
- controlling persons;
- tax residents;
- ownership percentages;
- roles and authorities;
- reportability status;
- evidence and effective dates.

## 7. Account reportability model

Suggested fields:

| Field | Description |
|---|---|
| account_id | Financial account identifier |
| account_holder_id | Legal account holder |
| beneficial_owner_ids | Beneficial owners |
| controlling_person_ids | Relevant controlling persons |
| account_status | Open/closed/dormant |
| account_balance | Year-end balance/value |
| reportable_jurisdictions | CRS jurisdictions to report |
| fatca_status | US reportable / non-reportable / undocumented / exempt |
| crs_status | Reportable / non-reportable / undocumented / excluded |
| documentation_status | Complete / missing / expired / inconsistent |
| reportable_year | Calendar/reporting year |
| closure_date | If account closed during year |

## 8. Reportable data categories

CRS/FATCA reporting commonly requires account and income/value data such as:

- account holder identity;
- address;
- jurisdiction(s) of tax residence;
- TIN;
- date/place of birth for individuals where required;
- account number;
- reporting financial institution;
- account balance or value;
- gross interest;
- gross dividends;
- gross other income;
- gross proceeds from sale/redemption;
- account closure indicator;
- controlling person details for passive entities.

Exact schema and fields depend on local implementation and reporting year.

## 9. US withholding and QI-style concepts

Private banks dealing with US securities often need processes related to:

- chapter 3 withholding on US-source FDAP income;
- chapter 4 FATCA withholding;
- documentation using W-8/W-9 forms;
- treaty-rate application;
- 1042-S reporting for certain foreign-person US-source income;
- 1099 reporting for US persons where applicable;
- QI/non-QI intermediary classification depending operating model.

A platform should not assume every US dividend has the same withholding rate. Rate depends on documentation, client/entity status, treaty claim and product/source classification.

## 10. Indicia and reasonableness checks

Cross-border reporting requires consistency checks.

Examples:

| Check | Risk |
|---|---|
| US address but non-US certification | Possible US indicia conflict |
| US phone number but W-8BEN | Possible remediation needed |
| Missing TIN for reportable jurisdiction | CRS reporting error |
| Entity self-certification inconsistent with KYC data | Wrong entity classification |
| Passive entity but no controlling persons | Incomplete CRS/FATCA classification |
| Expired W-8 form | Incorrect withholding rate |
| Account opened without self-certification | Reportability breach |
| Address country differs from tax residence | Not automatically wrong, but needs evidence |

## 11. Withholding rate determination flow

```text
Payment event
  -> identify income type and source country
  -> identify account/legal holder
  -> identify beneficial owner and tax classification
  -> check valid tax documentation on payment date
  -> apply statutory or treaty/reduced rate
  -> calculate gross, WHT and net
  -> produce tax reporting records
```

## 12. Documentation validity

Tax forms should be effective-dated.

| Field | Meaning |
|---|---|
| form_type | W-8BEN, W-8BEN-E, W-9, CRS self-cert |
| received_date | Date received |
| signed_date | Date signed |
| effective_from | When accepted |
| effective_to | Expiry date if applicable |
| validation_status | Valid/invalid/pending/remediated |
| validation_reason | Missing TIN, mismatch, expired, unsigned |
| applies_to_accounts | Account scope |
| applies_to_income | Income/source scope if relevant |

## 13. Report generation lifecycle

| Stage | Description |
|---|---|
| Data capture | KYC, onboarding, forms, account ownership |
| Classification | CRS/FATCA/tax status assigned |
| Transaction enrichment | Income/proceeds assigned reporting categories |
| Year-end snapshot | Account balance/value captured |
| Report extraction | Reportable accounts and amounts selected |
| Validation | Schema, mandatory fields, TIN/address checks |
| Submission | File submitted to tax authority |
| Acknowledgement | Acceptance/rejection stored |
| Correction | Amended records generated if needed |
| Audit | Evidence retained for statutory retention period |

## 14. Cross-border reporting architecture principle

Do not embed CRS/FATCA rules only in client reporting. Treat CRS/FATCA as a regulatory sub-ledger with:

- client/entity classification;
- account reportability;
- transaction aggregation;
- year-end balances;
- validation rules;
- submission status;
- correction workflow;
- audit evidence.

