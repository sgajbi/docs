# 07 - Cross-Border, Booking Centre, Distribution and Data Privacy Controls

## 1. Overview

Private banking is inherently cross-border. A client may live in one country, book assets in another, receive advice from a relationship manager in a third, hold products listed in multiple markets and report tax residency in multiple jurisdictions. This creates important platform obligations.

This file covers cross-border and privacy controls at a system-design level. Legal/regulatory details must remain configurable and compliance-approved.

## 2. Key location dimensions

| Dimension | Example | Why it matters |
|---|---|---|
| Client residence | India, Singapore, UAE | Cross-border advisory and tax profile. |
| Client nationality | Indian, Singaporean | KYC, sanctions, FATCA/CRS where relevant. |
| Tax residence | Singapore, India, US | CRS/FATCA and tax reporting. |
| Booking centre | Singapore, Hong Kong, Switzerland | Legal account context. |
| Booking entity | Bank legal entity | Contract, regulatory and accounting entity. |
| RM location | Singapore, Dubai, Hong Kong | Cross-border solicitation restrictions. |
| Product issuer jurisdiction | Luxembourg fund, US equity, Swiss note | Tax, product and documentation implications. |
| Exchange/market | NYSE, SGX, HKEX | Market access and settlement. |
| Data hosting region | Singapore, EU, cloud region | Data privacy and transfer rules. |

## 3. Booking-centre controls

| Control | Example |
|---|---|
| Product availability | Product allowed in SG but not in another booking centre. |
| Client segment eligibility | Retail vs accredited/professional/institutional. |
| Documentation | Local terms, product highlights, risk disclosures. |
| Tax reporting | CRS/FATCA/local reporting based on account and client profile. |
| Fees | Booking-centre fee schedule. |
| Reporting language | English, Chinese, local requirements. |
| Statement disclosure | Jurisdiction-specific disclaimers. |
| Order routing | Market access and broker/custodian chain. |
| Data residency | Who can access which data from where. |

## 4. Distribution and solicitation controls

Distribution controls determine whether a product, recommendation, document or marketing content may be shown to a client.

### Rule dimensions

| Dimension | Example |
|---|---|
| Client residence | Client resident in Country X. |
| Client classification | Accredited investor, professional investor, retail. |
| Product domicile | Luxembourg UCITS, Cayman fund, US ETF. |
| Product complexity | Simple, complex, SIP, derivative-linked. |
| Advisor location | RM in Singapore calling client abroad. |
| Channel | In-person, phone, digital, email, app. |
| Advice type | General information, recommendation, order execution. |
| Documentation | Required disclosure/KID/PHS/term sheet. |
| Language | Local language requirement. |
| Permission status | Allowed, blocked, manual approval, execution-only only. |

## 5. Cross-border states

| State | Meaning |
|---|---|
| Allowed | Product/advice/report can be provided. |
| Not allowed | Must not recommend/distribute. |
| Execution-only only | Client can instruct but bank should not recommend. |
| Hold-only | Existing position can remain; no new purchases. |
| Requires disclosure | Additional document/acknowledgement required. |
| Requires approval | Compliance/legal approval needed. |
| Unknown | Data missing; default to conservative restriction. |

## 6. Data privacy controls

Client/account data includes sensitive personal, financial, tax and KYC information.

### Data categories

| Category | Examples | Control expectation |
|---|---|---|
| Identity | Name, DOB, ID/passport, nationality | Masking, encryption, restricted access. |
| Contact | Address, email, phone | Consent and accuracy controls. |
| Financial | AUM, holdings, transactions, income | Need-to-know access and audit. |
| KYC/AML | Source of wealth, PEP, sanctions, adverse media | Highly restricted. |
| Tax | TIN, tax residence, FATCA/CRS status | Restricted and report-lineage controlled. |
| Suitability | Risk profile, objectives, knowledge | Advisory and compliance access. |
| Documents | IDs, contracts, tax forms, trust deeds | Document-level access and retention. |
| Beneficiaries | Trust/policy beneficiaries | Privacy-sensitive. |

## 7. Privacy-by-design platform requirements

| Requirement | Design implication |
|---|---|
| Purpose limitation | Capture why data is used. |
| Consent/notification | Store consent and communication preferences. |
| Access control | Least privilege by role, region, relationship and purpose. |
| Data minimization | Do not replicate sensitive data unnecessarily. |
| Masking/tokenization | Mask TIN, ID numbers and sensitive documents. |
| Encryption | At rest and in transit. |
| Retention | Delete/archive according to policy. |
| Data transfer | Control cross-border data access/export. |
| Audit logging | Log who viewed/changed/exported sensitive data. |
| Breach handling | Detect and report notifiable incidents per policy. |

## 8. Cross-border reporting risk examples

| Scenario | Risk |
|---|---|
| Household report sent to wrong family member | Privacy breach. |
| RM in one country views restricted client data | Cross-border/data-transfer issue. |
| Product shown to non-eligible resident | Distribution breach. |
| Tax residency missing | CRS/FATCA reporting exception. |
| Beneficial owner not captured | AML/KYC failure. |
| Booking centre changed without recalculating restrictions | Incorrect product eligibility. |
| Client address changed but tax profile not reviewed | Tax reporting stale. |

## 9. Control architecture

Recommended approach:

```text
Client/account master
  -> booking-centre policy engine
  -> product/distribution rules
  -> suitability/appropriateness rules
  -> access-control engine
  -> document/disclosure engine
  -> audit and exception workflow
```

## 10. Rule configuration example

```yaml
rule_id: CB_PRODUCT_DISTRIBUTION_001
scope: advisory_recommendation
client_residence: SG
booking_centre: SG
product_complexity: complex
client_classification: accredited_investor
required_documents:
  - product_highlights_sheet
  - risk_disclosure_acknowledgement
result: allowed_with_disclosure
```

## 11. Practitioner summary

Cross-border wealth servicing requires more than country fields. Platforms need configurable rules that combine client residence, tax residency, booking centre, RM/advisor location, product domicile, product complexity, client classification, documents, consent and access scope. Unknown or missing data should lead to conservative degraded states, not silent approval.
