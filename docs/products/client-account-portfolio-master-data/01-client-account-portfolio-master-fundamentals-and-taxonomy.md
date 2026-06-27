# 01 - Client, Account and Portfolio Master Fundamentals and Taxonomy

## 1. Executive summary

The client/account/portfolio master is the structural foundation of a wealth-management platform. It answers these questions:

- Who is the client?
- Who legally owns the assets?
- Who benefits from the assets?
- Who controls the account?
- Which entity or booking centre services the account?
- Which advisor or team covers the relationship?
- Which portfolio is used for reporting, advisory, DPM, performance or billing?
- Which suitability, tax, regulatory, mandate and access rules apply?
- How should holdings and analytics be consolidated?

A portfolio platform can calculate market value, performance and risk only after this hierarchy is correct. A technically accurate position can still be business-wrong if attached to the wrong client, wrong portfolio, wrong booking centre or wrong reporting group.

## 2. Core concepts

| Concept | Meaning | Platform importance |
|---|---|---|
| Party | Any person or organisation known to the bank. | Generic entity model for clients, beneficial owners, trustees, companies, advisors, counterparties. |
| Client | Party receiving banking/investment services. | Drives onboarding, KYC, suitability, reporting, consent and service model. |
| CIF / Customer ID | Source-system customer identifier. | Key integration identifier; should not be confused with legal entity model. |
| BR / Business Relationship | Relationship-level grouping used by some banks. | Often drives relationship manager assignment, credit line grouping, pledge logic and reporting. |
| Account | Legal/accounting contract with the bank/custodian. | Holds cash, securities, transactions and entitlements. |
| Portfolio | Investment/reporting container. | Used for performance, risk, advisory, statements, mandate checks and portfolio review. |
| Household / Client Group | Reporting/advisory grouping of related clients/accounts. | Supports family-level reporting, RM coverage and consolidated analytics. |
| Mandate | Set of investment rules and authority model. | Defines advisory, discretionary, execution-only or restricted treatment. |
| Booking centre | Jurisdiction/entity where account is booked. | Controls legal terms, tax reporting, product eligibility and cross-border restrictions. |
| Relationship manager | Primary coverage officer/team. | Drives servicing, advisory, sales, reviews and client communication. |
| Beneficial owner | Natural person who ultimately owns or controls assets/entity. | Critical for AML, KYC, CRS/FATCA, trust/company structures and reporting. |
| Authorized person | Party allowed to instruct or view. | Drives access control and order authorization. |
| Suitability profile | Client risk/knowledge/objectives profile. | Drives product eligibility, advisory recommendation checks and disclosures. |
| Tax profile | Tax residency, TINs and reporting classification. | Drives CRS/FATCA and tax-document/reporting outputs. |

## 3. Why account and portfolio are not the same

An account is usually legal/accounting. A portfolio is often analytical/reporting/investment-management.

| Dimension | Account | Portfolio |
|---|---|---|
| Legal contract | Yes | Sometimes no |
| Custody account | Usually yes | Not necessarily |
| Cash settlement | Usually yes | May aggregate accounts |
| Holdings source | Accounting book/custodian | Derived from one or more accounts |
| Performance | Can be calculated | Usually primary calculation unit |
| DPM mandate | May apply at account or portfolio level | Often applies at portfolio level |
| Client report | May be included | Often report section/grouping |
| Product eligibility | Often booking-account based | May be mandate based |

Example:

```text
Client A
  Account 001 - Cash and custody account
  Account 002 - Lombard loan account
  Portfolio P1 - Investment portfolio using Account 001 holdings
  Portfolio P2 - Consolidated view including Account 001 + loan exposure from Account 002
```

## 4. Why CIF and legal entity are not the same

A CIF is commonly a source-system customer identifier. It is not always the same as a legal person.

Common issues:

| Issue | Example |
|---|---|
| Duplicate CIFs | Same individual appears in multiple legacy systems. |
| One legal entity, many CIFs | A client has booking-centre-specific customer IDs. |
| One CIF, multiple roles | Same client acts as beneficial owner, authorized signer and joint holder. |
| Legacy merge/split | Migration creates new CIFs for operational reasons. |
| Household not in CIF | Family grouping exists outside core banking. |

Recommended principle:

```text
Do not treat CIF as the enterprise client identity.
Use CIF as a source identifier linked to a canonical party/client record.
```

## 5. Key hierarchy patterns

### 5.1 Simple individual client

```text
Individual Client
  Account
    Portfolio
      Holdings / Transactions / Cash
```

### 5.2 Joint account

```text
Client A + Client B
  Joint Account
    Portfolio
```

Key modelling issue: both clients have roles. Ownership percentage, authority, survivorship, tax treatment and reporting may differ.

### 5.3 Family / household reporting

```text
Household H
  Client Father
    Account A
  Client Mother
    Account B
  Trust Account C
  Family Office Entity D
```

Householding is often reporting/advisory grouping, not legal ownership.

### 5.4 Trust structure

```text
Settlor
  Trust
    Trustee account
      Portfolio
    Beneficiaries
    Protector / Investment committee
```

Legal owner, beneficial owner and controlling person may be different.

### 5.5 Company / holding vehicle

```text
Company Client
  Directors / Authorized signers
  Shareholders / Beneficial owners
  Account
  Portfolio
```

The account may be legally owned by the company, but KYC/AML and CRS/FATCA need controlling-person and beneficial-owner data.

### 5.6 DPM mandate

```text
Client
  Managed Account
    DPM Portfolio
      Mandate Profile
      Model Portfolio / Benchmark
      Restrictions
```

In DPM, the bank/manager has discretionary authority within mandate constraints.

### 5.7 Advisory mandate

```text
Client
  Advisory Account
    Advisory Portfolio
      Recommendations
      Client approvals
      Suitability evidence
```

Advisory is recommendation-led and client-approved; DPM is manager-executed under agreed authority.

## 6. Product dependency matrix

| Product / capability | Client/account data needed |
|---|---|
| Bonds | Account, portfolio, tax profile, product eligibility, settlement currency, income reporting. |
| Notes / structured products | Suitability profile, complexity eligibility, booking centre, issuer concentration, target-market rules. |
| Funds | Share-class eligibility, dealing currency, distribution election, trailer/fee profile, tax classification. |
| Equities | Exchange access, trading permissions, market restrictions, corporate-action entitlement account. |
| Derivatives | Trading authority, margin agreement, collateral account, knowledge/experience, counterparty agreement. |
| Loans / Lombard | Borrower, pledgor, collateral portfolio, credit line, cross-pledge relationships. |
| Insurance | Policy owner, insured life, beneficiary, payer, assignee, surrender authority. |
| Trusts/family office | Legal owner, beneficial owner, controlling person, reporting group, access restrictions. |
| Reporting | Household, statement preference, reporting currency, language, disclosure jurisdiction. |
| Performance | Portfolio grouping, inception date, cashflow classification, external/internal transfers. |
| Suitability | Risk profile, investment objectives, product knowledge, loss capacity, time horizon. |

## 7. Master-data anti-patterns

| Anti-pattern | Why dangerous |
|---|---|
| Copying client fields into every position | Creates stale and inconsistent data. |
| Treating portfolio as account | Breaks accounting, settlement and legal reporting. |
| Treating account as portfolio | Breaks performance, mandate and consolidated reporting. |
| Hardcoding booking-centre rules | Blocks cross-border scaling and future migrations. |
| Ignoring role effective dates | Historical reporting becomes inaccurate. |
| Ignoring household versioning | Past statements cannot be reproduced. |
| Using RM name as ownership field | Coverage is not legal ownership. |
| Treating beneficial owner as holder | Legal owner and beneficial owner may differ. |
| No relationship confidence/source | Duplicate relationships and wrong householding. |

## 8. Design principles

1. Model parties, accounts, portfolios and relationships separately.
2. Every relationship should be role-based and effective-dated.
3. Distinguish legal ownership, beneficial ownership, authority and reporting grouping.
4. Keep booking-centre rules configurable.
5. Treat suitability, tax, AML and reporting profiles as governed subdomains.
6. Preserve source identifiers but map them to canonical IDs.
7. Make history reproducible using effective dating and snapshot versioning.
8. Support degraded states with explicit flags instead of silent assumptions.
9. Use lineage for every client/account/portfolio attribute used in reporting or controls.
10. Design for migration, duplicates, merges, splits and legacy source conflicts.

## 9. Practitioner summary

The client/account/portfolio master is not just static reference data. It is the decision layer that determines ownership, authority, eligibility, reporting, suitability, access, mandate, tax and consolidation. For wealth platforms, this layer must be as carefully governed as transactions, prices and positions.
