# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns client, account, portfolio, household, mandate, booking-centre and relationship-master concepts into practical examples for advisory, DPM, reporting, data governance, access control, migration and QA.

## Example 1. One legal client, multiple source identifiers

### Scenario

A client has records in three source systems.

| Source | Source identifier | Name | Booking centre |
|---|---|---|---|
| Core banking SG | CIF-SG-1001 | S. Kumar | Singapore |
| Core banking HK | CIF-HK-8821 | Sandeep Kumar | Hong Kong |
| CRM | CRM-50177 | Sandeep K. | Global coverage |

### Correct model

The platform should create one canonical party only after matching and governance review, while retaining all source identifiers with lineage.

| Object | Treatment |
|---|---|
| Party | Canonical legal/natural person. |
| Party identifier | Multiple source IDs linked to the party. |
| Source lineage | Source, effective date, match confidence and override evidence. |
| Booking-centre account | Remains separate at account level. |

### QA assertions

| Test | Expected result |
|---|---|
| Same party has multiple CIFs | Canonical party links identifiers without losing source lineage. |
| Name differs across sources | Data-quality workflow reviews matching confidence. |
| Historical report is regenerated | Effective-dated source mapping is used. |
| One CIF is closed | Canonical party is not deleted if other relationships remain active. |

## Example 2. Account versus portfolio mapping

### Scenario

A client has two custody accounts and one consolidated reporting portfolio.

| Account | Booking centre | Currency | Included in portfolio |
|---|---|---|---|
| ACC-SG-01 | Singapore | SGD | Yes |
| ACC-SG-02 | Singapore | USD | Yes |
| ACC-HK-01 | Hong Kong | HKD | No, excluded by report scope |

### Correct model

An account is the legal/accounting container. A portfolio is the reporting or analytical container. The same account may belong to different portfolio views for reporting, advisory, collateral or mandate purposes.

### QA assertions

| Test | Expected result |
|---|---|
| Portfolio report is generated | Only accounts effective in portfolio membership are included. |
| Account is excluded by scope | Exclusion reason is traceable. |
| Account closes mid-period | Report policy determines whether closing-period activity appears. |
| Portfolio membership changes | Historical reports use effective-dated membership. |

## Example 3. Household reporting without legal ownership confusion

### Scenario

A family office wants a household report across parents, children, trust accounts and an investment company.

| Entity | Legal owner type | Household inclusion |
|---|---|---|
| Parent personal account | Individual | Included |
| Child personal account | Individual | Included with consent |
| Family trust | Trust | Included for family-office report |
| Holding company | Legal entity | Included |

### Reporting principle

Household reporting is a view, not a legal ownership claim. Statements, tax reports and account authority must still follow legal owner and authorized-person rules.

### QA assertions

| Test | Expected result |
|---|---|
| Household report includes trust assets | Report labels legal owner and reporting basis. |
| Child consent is missing | Account is excluded or masked according to policy. |
| RM uses household AUM for advisory | Suitability still uses correct client/account profile. |
| Legal statement is generated | Household grouping does not override legal account owner. |

## Example 4. Mandate and service-model conflict

### Scenario

An account is marked advisory, but the portfolio is linked to a DPM mandate.

| Field | Value |
|---|---|
| Account service model | Advisory |
| Portfolio mandate | DPM balanced mandate |
| Trade instruction source | Portfolio manager |

### Control issue

This is a governance conflict unless the account is allowed to have a DPM portfolio sleeve. The platform must not infer trading authority from only one field.

### QA assertions

| Test | Expected result |
|---|---|
| Account and portfolio authority conflict | Mandate validation flags exception. |
| DPM trade is generated | Account must be eligible for DPM execution. |
| Advisory proposal is created for DPM-only sleeve | Workflow blocks or redirects. |
| Service model changes | Mandate and portfolio membership are reviewed. |

## Example 5. Booking-centre eligibility and cross-border control

### Scenario

A client resident in Country A has an account booked in Singapore and is advised by an RM located in another jurisdiction.

| Dimension | Required data |
|---|---|
| Client residence | Effective-dated country and tax residence. |
| Account booking centre | Legal booking entity and jurisdiction. |
| RM location | Servicing jurisdiction. |
| Product eligibility | Allowed booking centre and client segment. |
| Disclosure rules | Jurisdiction-specific documents and language. |

### QA assertions

| Test | Expected result |
|---|---|
| Client residence changes | Product eligibility and disclosure rules re-evaluate. |
| Booking centre is missing | Recommendation and report workflows degrade or block. |
| RM location is restricted | Cross-border servicing control triggers. |
| Historical recommendation is reviewed | Original residence, booking centre and rule version are replayable. |

## Example 6. Access-control and relationship role

### Scenario

A trustee, beneficiary and authorized signer are linked to a trust account.

| Party | Role | Access implication |
|---|---|---|
| Trustee | Legal authority | Can instruct according to trust deed and account rules. |
| Beneficiary | Economic interest | May receive limited reporting if authorized. |
| Authorized signer | Operational authority | Can instruct within assigned limits. |

### Correct model

Access should derive from relationship role, account authority, privacy restrictions, effective dates and entitlement policy. It should not derive only from household membership or CRM coverage.

### QA assertions

| Test | Expected result |
|---|---|
| Beneficiary opens portal | Sees only permitted data. |
| Authorized signer expires | Access is removed by effective date. |
| RM changes team | Coverage and data access update through entitlement workflow. |
| Household member lacks legal role | No automatic access to account-level detail. |

## Example 7. Suitability profile lifecycle

### Scenario

A client risk profile expires before an advisory recommendation.

| Attribute | Value |
|---|---|
| Risk profile | Balanced |
| Last review date | 2024-06-30 |
| Review frequency | Annual |
| Recommendation date | 2026-06-27 |

### Correct behavior

The recommendation workflow should not rely on stale suitability data. It should block, warn or require profile refresh depending on policy.

### QA assertions

| Test | Expected result |
|---|---|
| Risk profile is expired | Recommendation cannot silently proceed. |
| Client has multiple accounts | Suitability basis is account/mandate specific where required. |
| Profile is updated after recommendation | Historical recommendation still shows old profile state. |
| Product complexity is high | Knowledge and experience checks are required. |

## Example 8. Migration reconciliation for client/account hierarchy

### Scenario

A platform migration loads party, account, portfolio and household data from legacy systems.

| Reconciliation area | Required check |
|---|---|
| Party count | Source party records mapped to canonical parties. |
| Account count | Open, closed and dormant accounts classified. |
| Portfolio membership | Every account included/excluded with reason. |
| Household membership | Consent and reporting authority preserved. |
| Mandate mapping | Advisory, DPM and execution-only states reconciled. |
| Access roles | Signers, trustees, EAMs and RMs preserved. |

### QA assertions

| Test | Expected result |
|---|---|
| Counts match but household membership differs | Migration is not signed off until resolved or accepted. |
| Closed account has historical transactions | Account remains available for historical reporting. |
| Role effective dates are missing | Access-control migration fails. |
| Duplicate parties are merged | Merge evidence and survivor mapping are retained. |

## Example 9. Client-reporting entitlement decision

### Scenario

A consolidated report is requested for a client group.

| Question | Why it matters |
|---|---|
| Who requested the report? | Determines entitlement and privacy rules. |
| Which accounts are included? | Prevents unauthorized aggregation. |
| Are all owners consenting? | Required for household/family reports. |
| Are restricted assets present? | May require masking or separate report. |
| Which reporting currency applies? | Drives FX and consolidation. |

### QA assertions

| Test | Expected result |
|---|---|
| Requester lacks entitlement | Report generation is blocked. |
| One account is confidential | Account is excluded or masked by policy. |
| Reporting currency is missing | Report cannot publish final consolidated totals. |
| Membership changes after report date | Snapshot uses report-date membership. |
