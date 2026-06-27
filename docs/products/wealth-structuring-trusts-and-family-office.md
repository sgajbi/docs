# Wealth Structuring, Trusts And Family Office

This map summarizes trusts, foundations, estate planning, family office structures, holding vehicles, beneficial ownership, decision authority, reporting groups, governance controls, and platform implementation considerations.

Use it with the detailed pack in [`trusts-estate-family-office-wealth-structuring/`](trusts-estate-family-office-wealth-structuring/README.md).

For practical examples, use [`trusts-estate-family-office-wealth-structuring/10-worked-examples-and-implementation-patterns.md`](trusts-estate-family-office-wealth-structuring/10-worked-examples-and-implementation-patterns.md).

## Core Principle

Do not treat a trust, estate, foundation, holding company, family office, VCC, nominee arrangement, or insurance wrapper as a simple account label.

Treat wealth structures as relationship graphs connecting:

1. legal entities,
2. natural persons,
3. roles,
4. accounts,
5. portfolios,
6. mandates,
7. beneficial interests,
8. powers and authorities,
9. restrictions,
10. tax and reporting profiles,
11. lifecycle events,
12. access-control rules.

## Relationship Model

| Layer | Examples | Platform Meaning |
|---|---|---|
| Party | Individual, company, trustee, foundation, executor, family office, nominee, insurer. | Master identity, KYC/AML, documents, restrictions, role eligibility. |
| Structure | Trust, foundation, estate, holding company, VCC, family office vehicle, insurance wrapper. | Ownership container and governance context. |
| Role | Settlor, trustee, protector, beneficiary, executor, director, donor, donee, advisor, investment committee member. | Determines authority, access, approvals, reporting, and conflict checks. |
| Account | Custody account, cash account, loan account, policy account, estate account. | Booking and operational perimeter. |
| Portfolio | Advisory, discretionary, execution-only, consolidated family view, sub-portfolio. | Investment and reporting perimeter. |
| Mandate | DPM, advisory, restricted, income, growth, ESG, liquidity, currency, risk profile. | Controls investment activity and suitability. |
| Entitlement | Income interest, capital interest, discretionary benefit, reporting access, voting/control right. | Determines who benefits and who may see or receive information. |
| Event | Establishment, contribution, distribution, change of trustee, beneficiary change, death, incapacity, termination. | Drives workflow, documents, reporting changes, and audit trail. |

## Structure Types

| Structure | Business Purpose | Key Platform Questions |
|---|---|---|
| Trust | Hold assets under trustee control for beneficiaries according to trust terms. | Who is settlor, trustee, protector, beneficiary, investment decision maker, reporting recipient, and tax-relevant person? |
| Foundation | Entity-like wealth structure often used for succession, governance, or philanthropic purposes. | Who controls council/board decisions, who benefits, and what documents govern distributions? |
| Estate | Temporary structure after death before probate/administration and distribution. | Who is executor/administrator, what assets are frozen, what reporting is allowed, and when can transfer occur? |
| Holding company | Corporate owner of investment assets or operating interests. | Who owns shares, who controls directors, what beneficial-owner and tax classifications apply? |
| Family office | Operating model for family governance, investment oversight, reporting, and administration. | Which users act for which family members, entities, mandates, and consolidated reporting groups? |
| VCC or fund-like family vehicle | Segregated investment vehicle with sub-funds or share classes. | What is legal owner, sub-fund perimeter, beneficial owner, valuation source, and investor reporting scope? |
| Insurance wrapper | Policy-based ownership and beneficiary structure. | Who is policyholder, life insured, beneficiary, payer, assignee, and authorized viewer? |
| Nominee or custody arrangement | Legal title may differ from beneficial owner. | Which party is legal holder, beneficial owner, reporting recipient, and authorized instructor? |

## Cross-Product Impact

| Product Area | Wealth-Structure Impact |
|---|---|
| Cash and deposits | Account owner, authorized signatories, restrictions, estate freeze, trust distribution funding, available cash by entity. |
| Bonds and structured notes | Legal owner and beneficial owner may differ; issuer concentration may need family-group and entity-level views. |
| Equities | Voting rights, corporate-action elections, restricted insiders, tax lots, transfer events, beneficial-owner reporting. |
| Funds and private markets | Subscription eligibility, beneficial-owner documentation, capital calls, transfers, side letters, lock-ups, family-entity allocations. |
| Derivatives | Authority, eligible counterparty, suitability, collateral source, margin obligations, governance approvals. |
| Real estate and infrastructure | Direct title, holding-company ownership, appraisal source, family use, distribution restrictions, leverage. |
| Insurance and annuities | Policyholder, insured life, beneficiary, assignee, premium payer, surrender authority, claim privacy. |
| Loans and collateral | Borrower, guarantor, pledgor, collateral owner, cross-pledge authority, covenant and margin-call recipient. |
| Tax and regulatory reporting | Legal owner, beneficial owner, controlling person, tax residence, documentation status, reportable account scope. |

## Advisory And Mandate Implications

| Question | Why It Matters |
|---|---|
| Who is the client for advice purposes? | The beneficial owner, trustee, company, executor, or family office may not be the same person. |
| Who has investment authority? | Execution, advisory acceptance, DPM changes, distribution approvals, and collateral pledges require authority checks. |
| Which risk profile applies? | Structure-level purpose may differ from individual beneficiary risk tolerance. |
| Which liquidity need matters? | Trust distributions, estate expenses, tax payments, capital calls, and family-office operating needs may drive liquidity. |
| Which reporting view is valid? | A trustee, beneficiary, settlor, family office user, and investment committee may see different scopes. |
| Which conflicts exist? | Trustee duties, beneficiary interests, related-party transactions, loans, pledges, and family governance may conflict. |
| Which restrictions apply? | Trust deed, mandate, investment policy statement, regulatory status, sanctions, KYC, tax documentation, and court/probate restrictions. |

## Reporting Model

Reports should distinguish:

1. legal owner,
2. beneficial owner,
3. reporting recipient,
4. authorized user,
5. account perimeter,
6. portfolio perimeter,
7. family-group consolidation,
8. mandate perimeter,
9. restricted or privacy-sensitive fields,
10. tax and regulatory reporting classification,
11. source date and governing document,
12. unresolved documentation or authority exceptions.

## Implementation Guidance

| Requirement | Good Implementation Pattern |
|---|---|
| Relationship graph | Model parties, entities, accounts, portfolios, roles, powers, mandates, restrictions, and entitlements explicitly. |
| Role-based access | Compute visibility and action rights from role, structure, document, jurisdiction, and approval state. |
| Source ownership | Store source owner for party data, governing documents, role assignments, mandates, reports, and restrictions. |
| Event model | Track establishment, funding, distribution, appointment, resignation, beneficiary change, death, incapacity, probate, and termination events. |
| Reporting hierarchy | Support entity-level, beneficiary-level, trustee-level, family-group, and mandate-level views without leaking restricted data. |
| Suitability controls | Link product suitability to the correct advised party, mandate, risk profile, and authority path. |
| Auditability | Preserve who instructed, who approved, which document authorized it, and which users could see the outcome. |
| Failure behavior | Block or degrade workflows when authority, beneficial ownership, tax documentation, or governing documents are missing. |

## Worked Examples

| Example | Useful For |
|---|---|
| Trust owns a portfolio, beneficiary receives reporting-only view, trustee has instruction authority. | Access control, reporting scope, advisory workflow. |
| Estate account after death freezes withdrawals until executor authority is verified. | Operations, lifecycle events, cash availability. |
| Family office user views consolidated family reporting across multiple entities with restricted beneficiary details hidden. | Reporting hierarchy and permissions. |
| Holding company borrows against portfolio collateral owned by the company, not the individual shareholder. | Lending, collateral, legal owner, beneficial owner. |
| Insurance policy has different policyholder, life insured, premium payer, beneficiary, and assignee. | Insurance modelling and privacy. |
| Private-market capital call funded by a trust account with trustee approval and beneficiary reporting note. | Private markets, liquidity, governance. |

The detailed worked-example file expands these patterns into workflow, authority, access-control, support-boundary and regression scenarios.

## QA And Control Scenarios

1. Unauthorized beneficiary cannot instruct or view restricted trustee notes.
2. Trustee can view and instruct only within documented powers.
3. Family office staff access is scoped to assigned family entities and mandates.
4. Estate account blocks withdrawal until executor documentation is verified.
5. Beneficial-owner change updates reporting and tax documentation requirements.
6. Trust distribution creates cash movement and beneficiary reporting without corrupting portfolio performance.
7. Collateral pledge checks legal owner and pledgor authority.
8. DPM mandate change requires correct authorized party approval.
9. Consolidated family report excludes restricted data for users without permission.
10. Missing governing document blocks structure-level authority assumptions.

## Related Guides

Use this guide together with:

1. [Trusts, Estate Planning, Family Office And Wealth Structuring Pack](trusts-estate-family-office-wealth-structuring/README.md)
2. [Tax, Regulatory And Reporting](tax-regulatory-and-reporting.md)
3. [Advisory, Mandate And Reporting Decision Guide](advisory-mandate-reporting-decision-guide.md)
4. [Client Reporting And Portfolio Review Guide](client-reporting-and-portfolio-review-guide.md)
5. [Product Lifecycle, Cashflow And Event Guide](product-lifecycle-cashflow-and-event-guide.md)
6. [Source Ownership, Calculation And Reporting Matrix](source-ownership-calculation-reporting-matrix.md)
7. [Product Capability Boundary Matrix](product-capability-boundary-matrix.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, reporting, documentation, and QA work. It is not investment, legal, tax, regulatory, fiduciary, estate-planning, accounting, or client advice.
