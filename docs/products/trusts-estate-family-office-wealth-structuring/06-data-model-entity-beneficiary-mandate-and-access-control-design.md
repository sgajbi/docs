    # Data Model: Entity, Beneficiary, Mandate and Access-Control Design

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. Design objective

The goal is to support complex wealth ownership without hardcoding every structure type. Use composable entities, roles, relationships, documents, mandates, accounts, reporting groups and permissions.

## 2. Core entity model

```json
{
  "partyId": "PTY123",
  "partyType": "LEGAL_ENTITY",
  "entitySubtype": "TRUST",
  "legalName": "ABC Family Trust",
  "jurisdiction": "SG",
  "taxResidency": ["SG"],
  "status": "ACTIVE",
  "effectiveDate": "2026-01-01"
}
```

Party types:

| Party type | Examples |
|---|---|
| INDIVIDUAL | Client, beneficiary, settlor, director |
| LEGAL_ENTITY | Company, trust, foundation, VCC, estate |
| OPERATING_ENTITY | Family office, trustee, advisor |
| GOVERNANCE_BODY | Investment committee, board |
| CHARITY / PURPOSE | Charitable beneficiary or purpose class |

## 3. Relationship model

```json
{
  "relationshipId": "REL789",
  "fromPartyId": "PTY_SETTLOR",
  "toPartyId": "PTY_TRUST",
  "relationshipType": "SETTLOR_OF",
  "effectiveFrom": "2026-01-01",
  "effectiveTo": null,
  "sourceDocumentId": "DOC_TRUST_DEED",
  "status": "ACTIVE"
}
```

Relationship types:

| Relationship | Direction example |
|---|---|
| SETTLOR_OF | Individual -> Trust |
| TRUSTEE_OF | Trustee -> Trust |
| PROTECTOR_OF | Protector -> Trust |
| BENEFICIARY_OF | Beneficiary -> Trust/Estate/Policy |
| DIRECTOR_OF | Individual -> Company |
| SHAREHOLDER_OF | Party -> Company |
| UBO_OF | Individual -> Entity |
| EXECUTOR_OF | Executor -> Estate |
| DONEE_FOR | Donee -> Donor |
| ADVISOR_TO | Family office/advisor -> Family/Entity |
| MANAGES | Fund manager -> VCC/Sub-fund |
| OWNS | Entity -> Account/Investment entity |
| REPORTS_TO_GROUP | Account/entity -> Reporting group |

## 4. Role and authority model

| Field | Example |
|---|---|
| role_type | Trustee / director / beneficiary / protector |
| authority_type | View / trade / approve / distribute / pledge / borrow |
| signature_rule | Sole / joint / board resolution / protector consent |
| limit_amount | 1,000,000 |
| product_scope | All / no derivatives / no lending |
| effective_from | Date |
| effective_to | Date |
| activation_condition | Normal / incapacity / death / court order |
| document_reference | Trust deed / LPA / board resolution |

## 5. Beneficiary entitlement model

```json
{
  "entitlementId": "ENT123",
  "structureId": "TRUST123",
  "beneficiaryPartyId": "BEN_A",
  "entitlementType": "DISCRETIONARY",
  "incomePct": null,
  "capitalPct": null,
  "className": "Children of settlor",
  "distributionPriority": null,
  "reportingAccessLevel": "SUMMARY_ONLY",
  "effectiveFrom": "2026-01-01"
}
```

Entitlement types:

| Type | Meaning |
|---|---|
| FIXED_INCOME | Fixed share of income |
| FIXED_CAPITAL | Fixed share of capital |
| DISCRETIONARY | Trustee discretion, no fixed share |
| CONTINGENT | Entitlement depends on event |
| LIFE_INTEREST | Income/lifetime interest, capital later to others |
| REMAINDER | Receives after prior interest ends |
| PURPOSE_CLASS | For purpose/charitable class |

## 6. Document model

| Document type | Examples |
|---|---|
| TRUST_DEED | Trust establishment and powers |
| LETTER_OF_WISHES | Settlor guidance |
| WILL | Testamentary document |
| PROBATE | Legal authority to administer estate |
| LPA | Lasting Power of Attorney |
| BOARD_RESOLUTION | Company authority |
| INVESTMENT_POLICY_STATEMENT | Mandate/guideline |
| FAMILY_CONSTITUTION | Governance principles |
| SHAREHOLDER_AGREEMENT | Transfer and voting rights |
| TAX_OPINION | Jurisdiction-specific treatment |
| BENEFICIARY_NOMINATION | Insurance/CPF/policy nomination |

Document metadata should include version, effective date, expiry, jurisdiction, source, verification status, and which structured rules were extracted.

## 7. Account and portfolio model

| Object | Purpose |
|---|---|
| Account | Custody/cash/loan/trading account |
| Portfolio | Analytical grouping of positions |
| Mandate | Advisory/DPM/investment policy rules |
| Reporting group | Consolidation hierarchy |
| Strategy sleeve | Asset allocation model, DPM sleeve |
| Collateral pool | Pledged assets and loan relationship |
| Tax lot | Cost basis and holding period |

## 8. Mandate model extensions for structures

| Field | Purpose |
|---|---|
| legal_structure_id | Trust/company/family office entity |
| governing_document_id | Trust deed/IPS/board resolution |
| allowed_asset_classes | Product eligibility |
| prohibited_assets | Restrictions |
| max_single_issuer_pct | Concentration rule |
| min_liquidity_pct | Distribution/expense reserve |
| max_leverage_pct | Credit and collateral rule |
| derivatives_allowed | Hedging/investment/speculation classification |
| private_assets_allowed | Illiquidity control |
| distribution_reserve_amount | Expected beneficiary payments |
| tax_sensitive_flag | Income/capital/tax-aware handling |
| beneficiary_constraints | Minors, vulnerable, income beneficiaries |

## 9. Access-control model

Access should be role-based and data-scope based.

| Role | Typical access |
|---|---|
| Trustee | Full trust account and beneficiary data |
| Beneficiary | Own entitlement/distribution/report only |
| Protector | Governance/approval view; not necessarily full detail |
| Family office CIO | Investment and consolidated reporting |
| Family office admin | Document and cashflow workflow; limited trading |
| External advisor | Mandate and portfolio analytics; limited PII |
| Relationship manager | Client-facing reporting and advisory workflow |
| Executor | Estate account and inventory after authority confirmed |

## 10. Audit model

Every sensitive action should record:

- who initiated;
- role used;
- legal authority basis;
- document reference;
- approval chain;
- affected accounts/beneficiaries;
- before/after values;
- timestamp and source system;
- compliance checks performed;
- exception overrides.

## 11. API design hints

Recommended endpoints:

```text
GET /parties/{partyId}/relationships
GET /structures/{structureId}/beneficiaries
GET /structures/{structureId}/authority
GET /reporting-groups/{groupId}/consolidated-positions
GET /mandates/{mandateId}/restrictions
POST /structures/{structureId}/distribution-requests
POST /authority-checks/evaluate
POST /reporting-access/evaluate
```

For performance, store effective-dated relationship snapshots for reporting dates. Do not recalculate full relationship graph for every historical report on the fly.

## 12. Event sourcing and auditability

Wealth-structure changes are highly auditable. Prefer event records such as:

- trustee appointed;
- beneficiary added;
- beneficiary entitlement changed;
- authority changed;
- LPA activated;
- death reported;
- estate opened;
- distribution approved;
- trust terminated.

Then materialise current relationship state from effective-dated events.
