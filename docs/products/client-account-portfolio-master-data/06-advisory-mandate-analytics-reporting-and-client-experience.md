# 06 - Advisory, Mandate, Analytics, Reporting and Client Experience

## 1. Overview

Client/account master data directly shapes advisory, DPM, portfolio analytics and reporting. It determines which products can be recommended, which portfolio is used for suitability checks, how concentration is measured, what is reported to the client, and how family/household views are consolidated.

## 2. Advisory use cases

| Advisory workflow | Required master data |
|---|---|
| Client review | Client, household, portfolios, RM assignment, risk profile, objectives. |
| Investment recommendation | Suitability profile, product eligibility, account status, booking centre. |
| Proposal generation | Current portfolio, available cash, restrictions, target allocation, product universe. |
| Concentration check | Household/portfolio scope, issuer/product/currency exposure. |
| Product suitability | Client knowledge, risk rating, time horizon, liquidity needs, product complexity. |
| Client consent | Authorized persons, signing rules, communication preference. |
| Cross-border advisory | RM location, client residence, booking centre, product distribution permissions. |
| Post-trade review | Account/portfolio mapping and recommendation lineage. |

## 3. DPM / mandate use cases

| DPM workflow | Required master data |
|---|---|
| Mandate onboarding | Mandate type, benchmark, model portfolio, restrictions. |
| Rebalancing | Portfolio membership, target allocation, cash, restrictions, tax constraints. |
| Breach monitoring | Mandate limits, effective-dated rules, product classifications. |
| Drift monitoring | Model weights, portfolio market values, exclusions. |
| PM authority | Discretionary authority scope and portfolio manager assignment. |
| Periodic review | Risk profile, performance, benchmark, deviations, actions. |
| Client report | Mandate objective, benchmark, risk, actions, exceptions. |

## 4. Analytics impact

### 4.1 Performance

Performance depends on portfolio scope.

| Master-data field | Impact |
|---|---|
| Portfolio membership | Determines included holdings/cashflows. |
| Inception date | Start of since-inception performance. |
| Reporting currency | FX translation and return base. |
| External/internal cashflow classification | TWR/MWR accuracy. |
| Account transfers | Avoid artificial performance jumps. |
| Household consolidation | Multi-portfolio return aggregation. |
| Mandate changes | Performance periods may require breakpoints. |

### 4.2 Risk and exposure

| Master-data field | Impact |
|---|---|
| Portfolio/account scope | Determines exposure denominator. |
| Household grouping | Family-level concentration. |
| Beneficial ownership | Ultimate owner exposure and regulatory controls. |
| Pledged assets | Available vs restricted assets. |
| Booking centre | Product restrictions and jurisdictional flags. |
| Mandate restrictions | Breach checks. |
| Product eligibility | Exposures may be report-only vs investable. |

### 4.3 Lending and buying power

Lending depends heavily on the client/account/pledge hierarchy.

| Data element | Why it matters |
|---|---|
| Borrower | Who owes the loan. |
| Pledgor | Who provides collateral. |
| Collateral portfolio | Which assets are pledged. |
| Credit line | Limit, exposure, availability. |
| Cross-pledge relationships | A's assets support B's credit line. |
| Account restrictions | Blocked assets cannot be used freely. |
| In-flight orders | Reduce available collateral/buying power. |

## 5. Reporting use cases

| Report type | Required master data |
|---|---|
| Client statement | Account owner, account status, holdings, cash, reporting currency. |
| Portfolio review | Portfolio scope, benchmark, mandate, risk profile, advisor notes. |
| Consolidated household report | Household membership and inclusion rules. |
| DPM review | Mandate, PM actions, benchmark, model deviations, breaches. |
| Lending report | Borrower, pledgor, collateral, LTV, availability, margin call status. |
| Tax report | Tax profile, account classification, beneficial owners, CRS/FATCA status. |
| Management dashboard | RM/team/booking centre grouping. |
| Regulatory report | Client classification, tax residency, account values, controlling persons. |

## 6. Client experience impact

Good client/account master data improves:

- accurate portfolio review
- consistent relationship-manager coverage
- correct household/family consolidation
- correct statement recipient and delivery preference
- clear product eligibility and advisory explanation
- transparency on mandate restrictions
- fewer operational errors and duplicate requests
- better digital access and privacy control

Bad master data creates:

- wrong report recipient
- missing accounts in consolidated report
- inconsistent risk profile across channels
- incorrect product recommendation eligibility
- wrong RM/team visibility
- failed CRS/FATCA reporting
- duplicate client records
- privacy breaches from over-broad householding

## 7. Reporting hierarchy patterns

### 7.1 Legal-owner view

Shows what each legal entity owns.

Best for:

- official statement
- tax reporting
- custody records
- audit and operations

### 7.2 Household view

Shows family/group wealth across legal owners.

Best for:

- relationship review
- planning
- advisory conversation
- UHNW family view

Caution: must label clearly as consolidated/advisory view.

### 7.3 Mandate view

Shows assets under a specific mandate.

Best for:

- DPM review
- benchmark comparison
- mandate compliance
- model drift

### 7.4 Collateral view

Shows assets pledged to credit lines.

Best for:

- lending reports
- buying power
- margin call analysis
- collateral shortfall reporting

### 7.5 Beneficial-owner view

Shows exposure attributed to ultimate owner/control person.

Best for:

- KYC/AML risk
- concentration and connected-party monitoring
- tax/regulatory reporting where applicable

## 8. Advisor dashboard fields

| Section | Data elements |
|---|---|
| Client overview | Name, segment, RM, booking centre, service model, household. |
| Relationship health | KYC due, suitability due, tax docs missing, restrictions. |
| Wealth overview | Total AUM, loans, net wealth, cash, pledged assets. |
| Portfolio analytics | Performance, allocation, risk, concentration, liquidity. |
| Mandate status | Model drift, breaches, restrictions, pending reviews. |
| Advisory opportunities | Cash excess, maturity events, income needs, concentration issues. |
| Alerts | KYC expiry, blocked account, failed trade, corporate action election. |
| Reporting | Last statement, delivery failure, pending portfolio review. |

## 9. Analytics rules of thumb

1. Performance should be calculated at a clearly defined portfolio scope.
2. Concentration can be measured at account, portfolio, household, beneficial owner or credit group level.
3. Suitability usually applies to client/account/mandate, not only product.
4. Reporting currency can differ from account currency and product currency.
5. Household consolidation is not legal ownership consolidation.
6. DPM mandate changes should create analytics breakpoints or explanatory labels.
7. Account closure should not destroy historical analytics.
8. Cross-pledged assets require separate legal ownership and collateral-use views.

## 10. Practitioner summary

Client/account master data is not a static onboarding artifact. It is active business logic used by advisory, mandates, performance, risk, lending, reporting and client experience. Every client-facing number should be traceable to a well-defined client/account/portfolio/reporting scope.
