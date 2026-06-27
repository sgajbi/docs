# 02 - Client, CIF, BR, Household, Beneficial Owner and Relationship Hierarchy

## 1. Overview

Wealth platforms need to represent complex client structures. The same person may be a client, beneficial owner, trustee, director, authorized signer, beneficiary, guarantor, pledgor, borrower and household member. These are not interchangeable labels; they are different legal, regulatory, advisory and reporting roles.

A robust model separates:

```text
Party identity
  from source identifiers
  from relationship roles
  from account ownership
  from portfolio reporting hierarchy
  from access authority
  from regulatory classification
```

## 2. Party model

A party is the canonical representation of a person or organisation.

| Party type | Examples | Key attributes |
|---|---|---|
| Individual | Private-banking client, spouse, child, beneficiary | Name, DOB, nationality, tax residency, ID documents, contact, suitability profile. |
| Legal entity | Company, holding vehicle, VCC, foundation | Legal name, registration number, jurisdiction, entity type, directors, beneficial owners. |
| Trust / legal arrangement | Discretionary trust, family trust | Trustee, settlor, protector, beneficiaries, governing law. |
| Joint relationship | Joint account holders | Holder roles, ownership share, authority rules. |
| Internal party | RM, advisor, PM, investment committee | Employee ID, team, coverage role, access rights. |
| External party | Custodian, broker, insurer, fund administrator | Role, system identifiers, operational contacts. |

## 3. CIF and source identifiers

A CIF is a source-system identifier. It is essential for integration but should not be treated as the whole client model.

Recommended structure:

| Table / concept | Purpose |
|---|---|
| `party` | Canonical entity/person. |
| `party_identifier` | CIF, BR ID, customer number, passport, UEN, LEI, tax ID, source IDs. |
| `source_party_link` | Maps source records to canonical party. |
| `party_merge_history` | Tracks duplicate merge/split decisions. |
| `party_quality_status` | Indicates verified, provisional, duplicate, deceased, closed, restricted. |

Example:

```text
party_id = P12345
  identifier: CIF_SG = 1000001
  identifier: CIF_HK = 8800112
  identifier: LEGACY_CS_ID = C98765
  identifier: PASSPORT = masked/encrypted value
```

## 4. BR / relationship grouping

Some banks use a Business Relationship ID to group clients, accounts, portfolios, credit lines or relationships. In lending systems, BR/CIF groupings may drive pledge, credit exposure and availability logic.

Important modelling guidance:

| Rule | Reason |
|---|---|
| Treat BR as a relationship/grouping object, not necessarily a legal person. | BR may group multiple accounts or portfolios. |
| Link BR to parties and accounts with roles. | One BR can have multiple clients; one client can participate in multiple BRs. |
| Make BR effective-dated. | Coverage and grouping can change. |
| Do not infer beneficial ownership from BR alone. | Beneficial ownership must be explicitly captured. |
| Keep BR hierarchy source-owned. | Core banking, credit, CRM and reporting may differ. |

## 5. Household and client group

A household is usually a reporting/advisory construct that groups related clients for relationship management, portfolio review, reporting and wealth planning.

### Household examples

| Household type | Example |
|---|---|
| Family household | Husband, wife, children, trust, holding company. |
| UHNW group | Principal, family office, investment companies, foundations. |
| Advisory group | Multiple accounts serviced by one RM team. |
| Reporting group | Consolidated statement across selected entities. |
| Credit group | Borrower/guarantor/pledge relationship. |

### Householding must not override legal ownership

A household may include legally separate clients. Consolidated reporting should make clear whether totals are legal holdings, advisory views, or family-level aggregation.

Example risk:

```text
Father's personal account + family trust + company account
```

A consolidated report may be useful for the RM, but ownership, tax reporting, authority and suitability may differ for each entity.

## 6. Relationship roles

Use a role-based relationship model.

| Role | Meaning | Can be multiple? | Effective-dated? |
|---|---|---:|---:|
| Legal owner | Owns account/asset legally | Yes | Yes |
| Beneficial owner | Ultimately owns/benefits/controls | Yes | Yes |
| Controlling person | Controls entity or trust for regulatory purposes | Yes | Yes |
| Authorized signer | Can instruct account | Yes | Yes |
| Power of attorney | Has delegated authority | Yes | Yes |
| Trustee | Holds trust assets | Yes | Yes |
| Settlor | Contributed assets to trust | Yes | Yes |
| Beneficiary | Benefits from trust/policy | Yes | Yes |
| Protector | Has oversight rights in trust | Yes | Yes |
| Director | Company officer | Yes | Yes |
| Shareholder | Owns shares in entity | Yes | Yes |
| Guarantor | Guarantees obligations | Yes | Yes |
| Pledgor | Provides collateral | Yes | Yes |
| Borrower | Draws credit line | Yes | Yes |
| RM | Relationship manager | Yes | Yes |
| Investment advisor | Advisor assigned | Yes | Yes |
| Portfolio manager | DPM manager | Yes | Yes |

## 7. Beneficial ownership

Beneficial ownership is critical for AML/KYC, tax reporting, entity classification, trusts, foundations and family-office structures. It should be modelled as a relationship with ownership/control attributes.

### Data attributes

| Field | Purpose |
|---|---|
| beneficial_owner_party_id | Natural person where applicable. |
| subject_party_id | Entity/trust/account to which ownership/control applies. |
| ownership_percentage | Direct/indirect economic ownership. |
| voting_percentage | Control through voting rights. |
| control_type | Ownership, voting, management, appointment power, settlor/control. |
| source_of_wealth | High-level source of wealth. |
| source_of_funds | Source of funds for relationship/transaction. |
| pep_status | PEP, RCA, non-PEP, unknown. |
| sanctions_screening_status | Clear, potential hit, confirmed hit, under review. |
| verification_status | Verified, pending, expired, exception. |
| effective_from / to | Historical correctness. |

### Beneficial owner vs beneficiary

| Term | Meaning |
|---|---|
| Beneficial owner | Person ultimately owning/controlling an entity or arrangement for KYC/tax/regulatory purposes. |
| Beneficiary | Person entitled to benefits under a trust, insurance policy or estate arrangement. |

They may overlap but should not be treated as the same field.

## 8. Joint accounts

Joint accounts require explicit role and authority rules.

| Requirement | Example |
|---|---|
| Holder roles | Primary, secondary, joint holder. |
| Signing rule | Any one to sign / all to sign / specific combination. |
| Ownership share | 50/50, equal, specified, unknown. |
| Survivorship rule | Jurisdiction/product-specific. |
| Tax reporting | May require separate allocation or full reporting. |
| Suitability | Joint recommendation may need suitability for all relevant holders. |
| Statements | Single joint statement or separate copy recipients. |

## 9. Relationship history and reproducibility

For reporting and audit, relationship data must be historical.

Example:

```text
2025-01-01 to 2025-12-31: RM = A
2026-01-01 onwards: RM = B
```

A 2025 statement should still show RM A if that was the official record at the report cut.

## 10. Common relationship quality issues

| Issue | Impact |
|---|---|
| Missing beneficial owner | KYC/AML/tax reporting cannot be completed. |
| Duplicate client | Incorrect consolidated wealth and risk. |
| Household over-grouping | Privacy/access-control breach. |
| Household under-grouping | Incomplete RM/advisory view. |
| Role without effective date | Cannot reproduce past reports. |
| Authorized person not captured | Order approval/control failure. |
| Deceased client not flagged | Incorrect servicing and reporting. |
| Sanctions/PEP stale | Regulatory control risk. |
| Legal owner confused with BO | Wrong statements and tax reporting. |

## 11. Recommended relationship model

```text
party
party_identifier
party_relationship
relationship_role_type
relationship_authority_rule
beneficial_ownership_detail
household
household_membership
business_relationship
business_relationship_membership
coverage_assignment
role_verification_status
relationship_event
```

## 12. Practitioner summary

The client layer should model identity, source identifiers and roles separately. Most serious downstream problems arise when systems flatten complex roles into one field such as `client_id`, `owner_name`, `rm_code` or `household_id`. A professional wealth platform needs explicit, effective-dated, source-owned, auditable relationships.
