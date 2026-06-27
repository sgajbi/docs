# 09. Glossary, Practitioner Reference and Review Checklists

## 1. Glossary

| Term | Meaning |
|---|---|
| Account | Legal/accounting container used to hold cash, securities, loans or policy values. |
| Account holder | Party legally associated with account ownership or authority. |
| Account role | Role a party plays on account, such as owner, signer, trustee or borrower. |
| Advisory | Service model where recommendations are made and client approves trades. |
| Authorized signer | Person/entity allowed to instruct on account. |
| Beneficial owner | Natural person who ultimately owns or controls an entity/arrangement or benefits from assets. |
| Booking centre | Jurisdiction/location/legal entity where account is booked. |
| BR / Business Relationship | Relationship-level grouping used by some banks for coverage, credit or reporting. |
| CIF | Customer identifier in a source/core banking system. |
| Client | Party receiving financial services. |
| Client group | Grouping of clients/entities for relationship, reporting or advisory purposes. |
| Consolidated portfolio | Portfolio/reporting view that aggregates multiple accounts or portfolios. |
| Controlling person | Individual controlling an entity or arrangement for regulatory/tax classification. |
| DPM | Discretionary portfolio management. |
| EAM | External asset manager. |
| Effective date | Business date from which a state/relationship is valid. |
| Household | Family or relationship grouping for reporting/advisory/coverage. |
| Joint account | Account with two or more holders. |
| KYC | Know Your Customer process and data. |
| Mandate | Investment authority and rules for account/portfolio. |
| Party | Canonical person, legal entity, trust, internal or external organisation. |
| PEP | Politically exposed person. |
| Portfolio | Investment/reporting container used for analytics, mandate and reporting. |
| Reporting currency | Currency used to present portfolio/report values. |
| Service model | Advisory, DPM, execution-only, EAM, family-office, etc. |
| Source of funds | Origin of funds entering the relationship or transaction. |
| Source of wealth | How client accumulated wealth. |
| Tax residence | Jurisdiction where client is tax resident. |
| TIN | Taxpayer identification number or functional equivalent. |

## 2. Practitioner Explanation

Practical framing:

> In a wealth platform, client, account and portfolio master data is the structural layer that defines who owns assets, who controls accounts, which booking centre applies, which mandate governs the portfolio, which suitability and tax profile should be used, and how holdings should be reported or consolidated. I would not model this as flat fields copied into positions. I would separate party, source identifiers, relationships, accounts, portfolios, households, mandates, access rights and lifecycle events. Every role and mapping should be effective-dated and source-owned so historical statements, suitability decisions, tax reports, mandate breaches and access decisions can be reproduced.

## 3. Account vs Portfolio Explanation

> An account is usually the legal and accounting container with the bank or custodian. It holds cash, securities and transactions and is tied to a booking centre and legal owner. A portfolio is an analytical or reporting container used for performance, risk, advisory, mandate checks and client reporting. Sometimes one account maps to one portfolio, but in wealth management portfolios can consolidate multiple accounts, split one account into sleeves, or represent a DPM mandate, advisory book or collateral view.

## 4. CIF vs Client Explanation

> A CIF is a source-system customer identifier. It is not the same as the canonical client. A single legal client may have multiple CIFs across booking centres or legacy systems, and one CIF can appear in multiple roles. A robust platform should keep source identifiers but map them to a canonical party model with relationship roles, effective dates and lineage.

## 5. Household vs Legal Ownership Explanation

> A household is usually a reporting or advisory grouping, not a legal owner. It helps an RM or family office view consolidated wealth, but it must not imply that assets are jointly owned. Official statements, tax reporting and account authority still follow legal account ownership and authorized-person rules.

## 6. Booking Centre Explanation

> Booking centre is the legal/regulatory account context. It affects product eligibility, disclosures, tax reporting, data access, settlement, fees and client reporting. It should be modelled at account/contract level, not inferred only from client residence or RM location.

## 7. Suitability and Mandate Explanation

> Suitability is typically client or account/mandate specific. Mandate rules define what may be recommended or executed. A platform should connect suitability profiles, product governance rules, account restrictions, booking-centre eligibility and mandate limits before generating recommendations or orders.

## 8. Data Model Review Checklist

| Entity | Must be separate? | Why |
|---|---:|---|
| Party | Yes | Canonical identity. |
| Party identifier | Yes | Multiple source IDs per party. |
| Party relationship | Yes | Roles and authority. |
| Household | Yes | Reporting/advisory grouping. |
| Account | Yes | Legal/accounting container. |
| Portfolio | Yes | Analytics/reporting container. |
| Portfolio membership | Yes | Multi-account and sleeve mapping. |
| Mandate | Yes | Investment rules and authority. |
| Suitability profile | Yes | Decision input and review lifecycle. |
| Tax profile | Yes | CRS/FATCA/tax reporting. |
| Coverage assignment | Yes | RM/team visibility and servicing. |
| Access entitlement | Yes | Security and privacy. |
| Lifecycle event | Yes | Audit and reproducibility. |

## 9. Common production questions

### Why is a client missing from an advisor dashboard?

Check coverage assignment, household membership, account status, branch/team mapping, IAM entitlements and effective dates.

### Why is a portfolio missing from a statement?

Check portfolio membership, account inclusion flags, report scope, closing date, statement preference and report cut-off snapshot.

### Why did suitability block a trade?

Check client risk profile, product complexity, knowledge/experience, account status, booking centre, product approval, tax restrictions and mandate limits.

### Why is household AUM different from legal account AUM?

Household may include or exclude accounts, loans, collateral, insurance, private assets or closed accounts differently from legal-owner reporting.

### Why is tax reporting incomplete?

Check tax residency, TIN, self-certification, FATCA/CRS classification, controlling persons, reportable jurisdiction and account status.

## 10. Practitioner checklist

Before using client/account data in reports or controls, confirm:

- legal owner present
- account status active or explainable
- booking centre present
- portfolio membership effective on report date
- household membership approved and authorized
- RM/team assignment effective
- suitability profile current where advice is given
- tax profile complete for reportable accounts
- beneficial-owner/controlling-person data complete for entities/trusts
- access rights match relationship and role
- source lineage exists
- historical snapshot can be reproduced

## 11. One-line mental model

```text
Party tells who. Account tells legal where. Portfolio tells analytical scope. Mandate tells permitted action. Booking centre tells jurisdictional context. Household tells reporting group. Relationships tell authority and control.
```
