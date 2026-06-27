# 06 - Data Model: Vendor Inventory, Risk Tiering and Control Register

## 1. Why a vendor data model matters

A bank needs a live inventory of third-party relationships. Without it, the bank cannot answer:

- Which vendors support critical services?
- Which vendors access client data?
- Which vendors process data offshore?
- Which vendors support AI or model outputs?
- Which vendors have unresolved risks?
- Which vendor contracts are expiring?
- Which services need exit plans?
- Which fourth parties are in the supply chain?

## 2. Core vendor entity

```json
{
  "vendorId": "VND-001",
  "legalName": "Example Analytics Pte Ltd",
  "countryOfIncorporation": "SG",
  "ownershipType": "PRIVATE_COMPANY",
  "vendorType": "SOFTWARE_SAAS",
  "criticalityTier": "HIGH",
  "relationshipOwner": "Wealth Platform",
  "riskOwner": "Technology Risk",
  "status": "ACTIVE"
}
```

## 3. Vendor service entity

A vendor may provide multiple services.

| Field | Example |
|---|---|
| service_id | SVC-123 |
| vendor_id | VND-001 |
| service_name | Portfolio Analytics API |
| service_type | SaaS / API / data feed / support |
| business_owner | Advisory Platform |
| technology_owner | Portfolio Services |
| criticality | Critical / High / Medium / Low |
| data_classification | Confidential client data |
| countries_in_scope | SG, HK, AU |
| booking_centres | Singapore, Hong Kong |
| regulatory_activity | Advisory support |
| production_status | Live |

## 4. Risk assessment entity

| Field | Example |
|---|---|
| assessment_id | RA-001 |
| service_id | SVC-123 |
| assessment_type | Initial / annual / change / incident |
| inherent_risk | High |
| control_effectiveness | Adequate |
| residual_risk | Medium |
| approval_status | Approved with conditions |
| approval_date | 2026-06-27 |
| next_review_date | 2027-06-27 |
| risk_acceptor | COO / CIO / Committee |

## 5. Control register

| Field | Example |
|---|---|
| control_id | CTRL-SEC-001 |
| control_domain | IAM |
| control_requirement | MFA for privileged users |
| evidence_type | SOC report / screenshot / attestation |
| evidence_status | Received |
| evidence_date | 2026-05-30 |
| control_owner | Vendor CISO |
| bank_reviewer | Technology Risk |
| finding_status | Closed / Open / Waived |

## 6. Contract register

| Field | Example |
|---|---|
| contract_id | CON-001 |
| vendor_id | VND-001 |
| effective_date | 2026-01-01 |
| renewal_date | 2027-01-01 |
| termination_notice_days | 90 |
| data_processing_agreement | Yes |
| audit_rights | Yes |
| regulator_access | Yes |
| exit_plan_required | Yes |
| governing_law | Singapore |
| liability_cap | 12 months fees / negotiated |

## 7. Subcontractor / fourth-party entity

| Field | Example |
|---|---|
| subcontractor_id | SUB-001 |
| vendor_id | VND-001 |
| service_supported | Cloud hosting |
| legal_name | Cloud Provider X |
| country | US |
| data_access | Yes |
| criticality | High |
| approved | Yes |
| notification_required | Yes |

## 8. Issue and remediation entity

| Field | Example |
|---|---|
| issue_id | ISS-001 |
| source | Due diligence / incident / audit |
| severity | High |
| description | Missing DR test evidence |
| owner | Vendor CTO |
| remediation_due_date | 2026-09-30 |
| status | Open |
| risk_acceptance | Temporary waiver |

## 9. AI vendor extension

| Field | Example |
|---|---|
| model_provider | OpenAI / Anthropic / Internal |
| model_name | GPT / Claude / internal model |
| model_version_tracking | Required |
| training_on_bank_data | Prohibited |
| prompt_retention | Disabled / controlled |
| human_approval_required | Yes |
| evaluation_suite_id | EVAL-001 |
| red_team_status | Completed |
| citation_required | Yes |
| tool_action_allowed | Restricted |

## 10. Key takeaway

A vendor inventory must be dynamic, risk-based and connected to systems, data, controls, contracts and incidents.
