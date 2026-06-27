# 10 - Worked Examples and Implementation Patterns

Use these examples when modelling trusts, estates, foundations, family offices, holding companies, beneficiaries, authority, access control, reporting groups, advisory workflows, QA, and implementation boundaries.

## 1. Relationship Graph Pattern

Wealth structures should be modelled as relationships, not as account labels.

| Node | Example |
|---|---|
| legal entity | trust, foundation, company, estate, VCC, family office |
| person | settlor, beneficiary, protector, director, executor, advisor |
| account/portfolio | custody account, DPM portfolio, advisory portfolio, loan account |
| authority | trustee instruction authority, director authority, executor authority |
| benefit | income beneficiary, capital beneficiary, discretionary class |
| reporting group | family consolidated report, entity report, beneficiary view |
| restriction | distribution rule, investment restriction, privacy limit, tax flag |

Core rule:

```text
legal owner, beneficial owner, controller, reporting recipient and instruction authority can be different parties
```

## 2. Trust Distribution

Scenario:

- Trust holds a portfolio.
- Trustee approves USD 100,000 distribution.
- Beneficiary receives USD 100,000 cash.

Required workflow:

```text
distribution request -> authority check -> trustee approval -> liquidity check -> cash movement -> beneficiary reporting -> audit record
```

Controls:

- beneficiary identity and entitlement must be source-backed;
- trustee authority must be valid on approval date;
- distribution category should be captured if source provides income/capital split;
- tax/legal treatment should not be inferred;
- portfolio performance should classify the distribution according to reporting policy.

## 3. Estate Account After Death Notification

Scenario:

- Individual client dies.
- Account becomes estate-managed pending probate.
- Executor authority is not yet validated.

Expected platform states:

| State | Treatment |
|---|---|
| death notification received | freeze or restrict actions according to policy |
| executor pending validation | no unrestricted instruction authority |
| probate document received | authority review workflow |
| estate account active | reporting and permitted actions follow estate rules |

Do not let previous individual authority continue automatically after death notification if policy requires restriction.

## 4. Holding Company Pledge

Scenario:

- Holding company owns securities portfolio.
- Beneficial owner is an individual family member.
- Holding company pledges assets for a credit facility.

Modelling:

| Dimension | Source-backed fact |
|---|---|
| legal owner | holding company |
| beneficial owner | individual or structure according to KYC/source |
| pledgor | holding company |
| borrower | may be company, individual, trust or related entity |
| collateral pool | pledged company account/portfolio |
| authority | company signatories/directors |

Implementation warning:

Do not calculate individual client buying power from company-owned pledged assets unless cross-pledge and authority rules explicitly support it.

## 5. Beneficiary Reporting Access

Scenario:

- Trust has three beneficiaries.
- Beneficiary A has full report access.
- Beneficiary B has income-only report access.
- Beneficiary C has no direct reporting access.

Access-control model:

| Role | Holdings | Income | Capital distributions | Other beneficiaries | Trustee notes |
|---|---|---|---|---|---|
| Beneficiary A | allowed | allowed | allowed | restricted | restricted |
| Beneficiary B | restricted | allowed | restricted | restricted | restricted |
| Beneficiary C | restricted | restricted | restricted | restricted | restricted |

QA assertion:

Reporting access must follow relationship role and consent/authority rules, not account ownership alone.

## 6. Family Office Consolidated Report

Scenario:

- Family office requests consolidated report across:
  - personal account;
  - trust account;
  - holding company account;
  - foundation account.

Controls:

- every included entity must have reporting authority and consent evidence;
- beneficial ownership and legal ownership should remain visible;
- restricted accounts or restricted beneficiaries must be excluded or masked;
- consolidation currency and valuation date must be explicit;
- inter-entity loans or transfers should not be double counted.

Reporting output:

| Section | Required treatment |
|---|---|
| consolidated allocation | show inclusion perimeter |
| entity breakdown | legal owner and reporting owner |
| performance | avoid combining incompatible mandates without disclosure |
| liabilities/pledges | show entity-specific obligations and cross-pledges |
| restricted data | mask or omit according to access policy |

## 7. Insurance Policy Ownership Role

Scenario:

- Policy owner: trust.
- Life insured: settlor.
- Beneficiaries: trust beneficiaries.
- Premium payer: holding company.

Platform implication:

| Role | Why it matters |
|---|---|
| policy owner | legal control and reporting ownership |
| life insured | claim/lifecycle trigger |
| beneficiary | claim recipient or economic benefit |
| premium payer | cashflow source and funding relationship |
| advisor | access and service relationship |

Do not assume the policy owner, insured life, premium payer and beneficiary are the same party.

## 8. Authority Change

Scenario:

- A family office director resigns.
- New director is appointed.
- Existing trading authority and report access must change.

Workflow:

```text
source document received -> authority effective date -> entitlement update -> access certification -> audit record -> impacted workflow review
```

Controls:

- effective date must be stored;
- pending orders or approvals by old authority should be reviewed;
- report delivery lists must be refreshed;
- access changes must be testable through entitlement evidence.

## 9. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Legal owner | Which entity owns the account or asset? |
| Beneficial owner | Who economically benefits, and how is this sourced? |
| Authority | Who may instruct, approve, revoke, view or receive reports? |
| Suitability | Which person/entity profile applies to advisory or mandate decisions? |
| Mandate | Is the mandate at account, entity, family, sub-portfolio or beneficiary level? |
| Tax/reporting | Which jurisdiction, residency, entity and beneficiary flags matter? |
| Privacy | Which parties must be masked or excluded from reports? |
| Succession | What changes on death, incapacity, resignation, removal or distribution? |

## 10. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Entity/account hierarchy | party, account, portfolio, legal owner, relationship role | graph-based ownership and influence analytics |
| Authority | role, effective date, document evidence, access scope | authority workflow with expiry and recertification |
| Reporting group | inclusion perimeter, access rights, masking rules | dynamic family-office report builder |
| Distributions | approved cash movement and beneficiary recipient | income/capital distribution classification engine |
| Estate state | death notification, executor validation, account restrictions | probate lifecycle workflow |
| Cross-entity collateral | pledgor, borrower, collateral, pledge direction | multi-entity credit optimization with legal controls |

## 11. Regression Test Pack

Minimum release-gate scenarios:

1. Trust distribution requires valid trustee approval.
2. Beneficiary access shows only permitted report sections.
3. Death notification restricts authority pending estate validation.
4. Holding-company pledge does not create individual buying power without cross-pledge rule.
5. Family-office consolidated report respects inclusion perimeter and restricted data.
6. Insurance policy displays distinct owner, insured, payer and beneficiary roles.
7. Authority change revokes old access and preserves audit trail.
8. Suitability check uses correct entity/person profile.
9. Consolidated performance excludes restricted accounts or labels partial perimeter.
10. Missing source document blocks client-ready ownership or authority claim.
