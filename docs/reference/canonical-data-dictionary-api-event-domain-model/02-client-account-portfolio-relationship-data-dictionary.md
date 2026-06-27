# 02 - Client, Account, Portfolio and Relationship Data Dictionary

## 1. Party / client

| Field | Type | Description |
|---|---|---|
| party_id | string | stable internal party identifier |
| party_type | enum | individual, joint, company, trust, foundation, partnership, family_office |
| display_name | string | display name |
| legal_name | string | legal name where applicable |
| date_of_birth | date | individual only, if permitted |
| incorporation_date | date | entity only |
| country_of_residence | ISO country | residence country |
| tax_residencies | array | list of tax residency countries |
| nationality | ISO country | if relevant and permitted |
| risk_profile | enum | conservative, balanced, growth, aggressive etc. |
| suitability_profile_id | string | link to suitability profile |
| kyc_status | enum | pending, active, refresh_due, restricted, exited |
| aml_risk_rating | enum | low, medium, high |
| pep_flag | boolean | politically exposed person indicator |
| data_privacy_region | string | privacy/data residency control |
| status | enum | active, dormant, restricted, closed |
| source_system | string | source of party data |
| source_last_updated | datetime | source timestamp |

## 2. Relationship / CIF / BR

| Field | Type | Description |
|---|---|---|
| relationship_id | string | relationship/CIF/BR identifier |
| relationship_type | enum | individual, household, family, corporate, trust, booking_relationship |
| primary_party_id | string | main party |
| related_party_ids | array | linked parties |
| advisor_id | string | relationship manager/advisor |
| service_model | enum | advisory, execution_only, discretionary, family_office |
| segment | enum | mass_affluent, affluent, hnw, uhnw, institutional |
| booking_centre | string | Singapore, Hong Kong, Australia, etc. |
| domicile | string | relevant legal/tax domicile |
| relationship_status | enum | active, restricted, exiting, closed |
| opened_date | date | relationship start |
| closed_date | date | relationship close |

## 3. Account

| Field | Type | Description |
|---|---|---|
| account_id | string | legal/custody account identifier |
| relationship_id | string | owning relationship |
| account_number | string | masked display account number |
| account_type | enum | custody, cash, margin, loan, trust, insurance, external |
| booking_centre | string | booking location |
| custodian_id | string | custodian or bank entity |
| base_currency | ISO currency | reporting/account base currency |
| account_status | enum | active, restricted, closed |
| opening_date | date | open date |
| closure_date | date | closure date |
| tax_status | string | tax/documentation status |
| restrictions | array | account-level restrictions |
| source_system | string | master source |

## 4. Portfolio

| Field | Type | Description |
|---|---|---|
| portfolio_id | string | portfolio identifier |
| account_id | string | parent account |
| relationship_id | string | parent relationship |
| portfolio_name | string | display name |
| portfolio_type | enum | advisory, discretionary, execution_only, model, consolidated, sleeve |
| mandate_id | string | DPM or advisory mandate if applicable |
| base_currency | ISO currency | reporting currency |
| benchmark_id | string | default benchmark |
| inception_date | date | performance start date |
| performance_start_date | date | may differ from inception |
| status | enum | active, closed, restricted |
| consolidation_group_id | string | reporting consolidation group |
| source_system | string | source |

## 5. Household / consolidation group

| Field | Type | Description |
|---|---|---|
| consolidation_group_id | string | household/family/reporting group |
| group_name | string | display name |
| group_type | enum | household, family, trust_group, family_office, custom |
| member_relationship_ids | array | relationships included |
| reporting_currency | ISO currency | group reporting currency |
| access_policy_id | string | who can view consolidated data |
| status | enum | active, inactive |

## 6. Suitability profile

| Field | Type | Description |
|---|---|---|
| suitability_profile_id | string | suitability record |
| party_id | string | owner |
| risk_profile | enum | conservative/balanced/growth/etc. |
| investment_objectives | array | income, growth, preservation, speculation |
| time_horizon | enum | short, medium, long |
| liquidity_needs | enum | low, medium, high |
| knowledge_experience | object | product knowledge by category |
| loss_tolerance | enum | low, medium, high |
| complex_product_allowed | boolean | eligibility flag |
| derivatives_allowed | boolean | eligibility flag |
| leverage_allowed | boolean | eligibility flag |
| last_review_date | date | last suitability review |
| next_review_date | date | next due review |
| status | enum | active, expired, incomplete |

## 7. Access control scope

| Field | Type | Description |
|---|---|---|
| access_scope_id | string | scope identifier |
| user_id | string | user/advisor/client |
| party_id | string | party scope if applicable |
| relationship_id | string | relationship scope |
| account_id | string | account scope |
| portfolio_id | string | portfolio scope |
| permissions | array | view, trade, approve, report, administer |
| effective_from | date | start |
| effective_to | date | end |
| status | enum | active, revoked, expired |

## 8. Validation rules

- Every portfolio must link to one account.
- Every account must link to one relationship.
- Every relationship must have at least one party.
- Every account and portfolio must have base currency.
- Closed accounts should not allow new trades.
- Restricted clients should block advisory/order workflows by rule.
- Suitability profile must be current before recommendation.
- Access scope must be enforced in APIs, reports and AI retrieval.

## 9. API examples

### Get portfolio summary

```http
GET /v1/portfolios/{portfolio_id}
```

Response:

```json
{
  "portfolioId": "P-100",
  "accountId": "A-100",
  "relationshipId": "R-100",
  "portfolioName": "Balanced Advisory Portfolio",
  "portfolioType": "advisory",
  "baseCurrency": "USD",
  "benchmarkId": "BM-6040",
  "inceptionDate": "2024-01-01",
  "status": "active"
}
```

## 10. Key takeaway

Client/account/portfolio data defines ownership, reporting scope, suitability context, access control and advisory accountability.
