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

## Example 10. Joint Account Authority And Reporting

### Scenario

A joint account is owned by two individuals. Either owner can view statements, but trade instructions require both owners for certain product types.

| Party | Role | Authority |
|---|---|---|
| Owner A | Joint legal owner | View, standard instruction |
| Owner B | Joint legal owner | View, standard instruction |
| Both owners | Joint approval group | Required for high-complexity products |

### Correct treatment

Joint ownership does not automatically mean every action can be performed by either owner alone. Authority should be action-specific, product-specific, effective-dated and source-backed by account documents.

### QA assertions

| Test | Expected result |
|---|---|
| Owner A submits complex-product order alone | Workflow requires Owner B approval where policy requires joint consent. |
| Owner B revokes online access | Reporting access updates without changing legal ownership. |
| One owner dies | Account state changes according to survivorship/probate policy. |
| Household report includes account | Report labels joint ownership and consent basis. |

## Example 11. External Asset Manager Relationship

### Scenario

An external asset manager is authorized to manage a client account under a limited power of attorney.

| Party | Role | Scope |
|---|---|---|
| Client | Legal account owner | Owns account and receives statements. |
| EAM firm | Investment manager | Can instruct within mandate and agreement. |
| EAM user | Authorized representative | Access depends on firm role and user certification. |
| Bank RM | Coverage owner | Oversight and service relationship. |

### Correct treatment

EAM access should be scoped by firm, user, account, mandate, product eligibility, country restrictions, expiry and certification status. Removing an EAM user should not remove the EAM firm relationship unless source documents say so.

### QA assertions

| Test | Expected result |
|---|---|
| EAM agreement expires | Trade and report access are blocked or recertified. |
| EAM user leaves firm | User access ends while firm relationship remains. |
| Product is outside EAM mandate | Order is blocked even if account owner is eligible. |
| Client terminates EAM | Authority, report delivery and pending orders are reviewed. |

## Example 12. Trust-Owned Company Account

### Scenario

A trust owns a holding company, and the holding company owns the investment account.

| Layer | Entity |
|---|---|
| Beneficial owner | Family trust |
| Legal account owner | Holding company |
| Authorized signers | Company directors |
| Reporting recipient | Trustee and authorized family office |

### Correct treatment

The account's legal owner is the company, but beneficial-owner, tax, report-perimeter and authority data must preserve the trust relationship. Do not use trustee authority to instruct on the company account unless company documents authorize it.

### QA assertions

| Test | Expected result |
|---|---|
| Company director changes | Account authority updates; trust beneficial ownership remains. |
| Trustee requests report | Entitlement uses reporting authority and privacy rules. |
| Collateral view is generated | Pledgor and legal owner remain company-specific. |
| Beneficial-owner source is missing | Reporting and onboarding state are exceptioned. |

## Example 13. Closed Account Historical Reporting

### Scenario

An account closed in March, but the client requests a full-year report in December.

| Attribute | Value |
|---|---|
| Account status | Closed |
| Closure date | 2026-03-31 |
| Report period | 2026-01-01 to 2026-12-31 |
| Historical activity | January to March |

### Correct treatment

Closed accounts should remain available for historical reporting, audit, tax, reconciliation and complaint handling. They should not be available for new trading, funding or standing instructions.

### QA assertions

| Test | Expected result |
|---|---|
| Annual report includes closed account | Activity is included for the period before closure if report scope allows. |
| User tries to trade | Trading is blocked because account is closed. |
| Advisor coverage changed after closure | Historical report uses coverage and entitlements effective at report date/policy. |
| Data retention period expires | Report access follows archive and retention policy. |

## Example 14. Account Transfer Between Booking Centres

### Scenario

A client transfers an account from Booking Centre A to Booking Centre B. Holdings move, but historical activity and original source identifiers remain relevant.

| Attribute | Before | After |
|---|---|---|
| Booking centre | A | B |
| Account identifier | ACC-A-100 | ACC-B-900 |
| Portfolio membership | Global portfolio | Global portfolio |

### Correct treatment

The transfer should preserve old account history, new account identity, transfer date, asset movement, cash movement, cost basis policy, tax-lot migration and report continuity. Do not overwrite the old account identifier with the new one.

### QA assertions

| Test | Expected result |
|---|---|
| Transfer completes | Old account closes or restricts, new account opens with lineage. |
| Historical report is generated | Uses old account for pre-transfer activity and new account after transfer. |
| Tax lots are missing | Realized P&L and tax reporting are partial or blocked. |
| Product eligibility differs in new booking centre | Holdings and future trade eligibility are reviewed. |

## Example 15. Beneficial-Owner Change

### Scenario

A holding company account has a beneficial-owner change after a family restructuring event.

| Attribute | Before | After |
|---|---|---|
| Beneficial owner | Parent | Family trust |
| Effective date | 2026-06-30 | 2026-07-01 |
| Legal account owner | Holding company | Holding company |

### Correct treatment

Beneficial-owner changes affect KYC, tax documentation, reportability, sanctions screening, related-party exposure and reporting visibility. They do not automatically change legal account ownership or historical reporting basis.

### QA assertions

| Test | Expected result |
|---|---|
| Beneficial owner changes mid-period | Reports are effective-dated. |
| New documentation is missing | Account enters remediation or restricted state according to policy. |
| Legal owner remains same | Account ownership is not changed incorrectly. |
| Historical report before effective date | Prior beneficial-owner basis is preserved. |

## Example 16. Privacy Masking For Restricted Relationship

### Scenario

A family group report includes an account with a restricted beneficiary relationship. The requester may see aggregate value but not beneficiary identity.

| Report field | Treatment |
|---|---|
| Account value | Included in aggregate if consent permits. |
| Legal owner | Shown or masked according to policy. |
| Beneficiary name | Masked. |
| Transaction detail | Excluded or summarized. |

### Correct treatment

Masking should be field-level and policy-driven. Do not solve privacy by excluding all financial value if policy permits aggregate reporting, and do not reveal restricted identity through notes, filenames, footers or drill-down links.

### QA assertions

| Test | Expected result |
|---|---|
| Restricted beneficiary is present | Name and sensitive fields are masked. |
| User exports report | Masking persists in export. |
| Drill-down is clicked | Entitlement is rechecked before detail display. |
| Advisor note mentions beneficiary | Note is masked or excluded according to policy. |

## Example 17. Advisor Coverage Change

### Scenario

A relationship manager moves to another team. The client remains with the original booking centre, but advisor coverage and report delivery change.

| Attribute | Before | After |
|---|---|---|
| Primary RM | RM A | RM B |
| Coverage team | Team 1 | Team 2 |
| Effective date | 2026-08-01 | 2026-08-02 |

### Correct workflow

```text
coverage source update -> effective-dated RM change -> entitlement recalculation -> task reassignment -> report delivery refresh -> audit record
```

### QA assertions

| Test | Expected result |
|---|---|
| RM changes | Access, task queues and report delivery lists update. |
| Pending advisory proposal exists | Proposal ownership and client communication are reviewed. |
| Historical activity is audited | Prior RM remains visible for past actions. |
| Team restrictions differ | Cross-border and data-scope controls recalculate. |
