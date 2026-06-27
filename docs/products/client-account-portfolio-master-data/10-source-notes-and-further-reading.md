# 10 - Source Notes and Further Reading

## 1. Purpose

This file lists useful source categories for client, account, portfolio, household, booking-centre and relationship master-data design. It is not a legal or regulatory opinion. Always use approved compliance/legal interpretation for production policy rules.

## 2. Regulatory / official sources

### FATF recommendations

FATF Recommendations are useful for global AML/CFT concepts, including risk-based approaches, customer due diligence, beneficial ownership, PEPs and ongoing monitoring.

Useful source:

```text
https://www.fatf-gafi.org/en/publications/Fatfrecommendations/Fatf-recommendations.html
```

### MAS AML/CFT material

For Singapore banking context, MAS AML/CFT notices and guidelines are relevant to customer due diligence, beneficial ownership, PEPs, source of wealth/funds, sanctions and ongoing monitoring.

Useful entry point:

```text
https://www.mas.gov.sg/regulation/anti-money-laundering
```

### IRAS CRS guidance

CRS is relevant for tax residency, financial-account reporting, reportable jurisdictions, due diligence and TIN collection.

Useful source:

```text
https://www.iras.gov.sg/taxes/international-tax/common-reporting-standard-(crs)/crs-overview-and-latest-developments
```

### IRAS FATCA guidance

FATCA is relevant for identifying and reporting US financial accounts and US persons in Singapore's Model 1 IGA context.

Useful source:

```text
https://www.iras.gov.sg/taxes/international-tax/foreign-account-tax-compliance-act-(fatca)/fatca-overview-and-latest-developments
```

### Singapore Personal Data Protection Act

Client/account master data includes sensitive personal data. PDPA concepts are relevant to consent, purpose limitation, access/correction, accuracy, protection, retention, cross-border transfer, breach notification and accountability.

Useful source:

```text
https://sso.agc.gov.sg/Act/PDPA2012
```

### MAS fair dealing and financial advisory notices

For advisory/suitability context, MAS fair dealing and financial advisory requirements should inform platform controls.

Useful entry points:

```text
https://www.mas.gov.sg/regulation/guidelines/fair-dealing-guidelines
https://www.mas.gov.sg/regulation/notices/notice-faa-n16
```

## 3. Industry and standards references

| Topic | Useful standards / references |
|---|---|
| Legal entity identity | LEI / GLEIF materials. |
| Currency codes | ISO 4217. |
| Market identifiers | ISO 10383 MIC. |
| Data governance | BCBS 239, ISO 8000 data quality concepts. |
| Security and access | Least privilege, IAM, zero-trust, audit logging practices. |
| Reporting | GIPS-style fair representation concepts for performance reporting. |
| Tax transparency | OECD CRS materials and implementation handbooks. |

## 4. Internal source systems to document

For any implementation, create a source-ownership matrix for:

- core banking customer master
- CRM / RM coverage system
- KYC/AML platform
- tax documentation platform
- suitability/advisory platform
- account/custody system
- loan/collateral system
- portfolio/reporting platform
- IAM/entitlement platform
- document management system
- statement/report delivery platform

## 5. Questions to ask stakeholders

### Client identity

- What is the enterprise canonical client ID?
- Which source owns legal name and status?
- How are duplicate clients merged or split?
- Are source identifiers preserved?

### Account

- Which source owns account status, booking centre and account type?
- Can one account be linked to multiple portfolios?
- Are closed accounts retained for historical reporting?

### Portfolio

- Who creates portfolios?
- Are portfolios legal, analytical, reporting or mandate containers?
- How are memberships effective-dated?

### Household

- Who approves household grouping?
- Can household data be shown to all members?
- Which accounts are included/excluded from consolidated reports?

### Suitability

- Is suitability client-level, account-level, mandate-level or recommendation-level?
- What happens when suitability is missing or expired?
- Are complex-product permissions separately stored?

### Tax

- Where are CRS/FATCA profiles stored?
- How are TIN exceptions handled?
- How are controlling persons modelled for entities/trusts?

### Access

- Who can view consolidated household data?
- Who can trade, approve or only view?
- How are EAM/family-office delegated users controlled?

## 6. Implementation principle

Treat regulatory and advisory rules as configurable policies. Treat client/account/portfolio data as the governed input into those policies. Do not hardcode jurisdictional, tax or suitability outcomes into product, position or report logic.
