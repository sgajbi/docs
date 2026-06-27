# Data Model: Identity, Access, Workflow, Interaction and Lineage Design

## 1. Design objective

The data model should allow the platform to answer five questions:

1. Who is the actor?
2. What resource is being accessed or acted upon?
3. Why is the actor allowed or blocked?
4. What workflow state and evidence existed at the time?
5. Can the decision be reconstructed later?

## 2. Core entities

```text
Party / User / Client
  |-- IdentityAccount
  |-- AuthenticationFactor
  |-- RoleAssignment
  |-- RelationshipAssignment
  |-- Delegation
  |-- EntitlementGrant
  |-- PolicyDecision
  |-- WorkflowCase
  |-- Task
  |-- Approval
  |-- Interaction
  |-- Consent
  |-- DocumentDelivery
  `-- AuditEvent
```

## 3. Identity account

| Field | Purpose |
|---|---|
| identity_account_id | Login identity. |
| party_id | Link to employee/client/legal party. |
| identity_type | Internal user, client, delegate, service account. |
| status | Active, suspended, locked, closed. |
| username / subject | Login/federated subject. |
| identity_provider | IdP source. |
| last_login_at | Monitoring. |
| risk_status | Normal, monitored, compromised. |
| created_at / deactivated_at | Lifecycle. |

## 4. Role assignment

| Field | Purpose |
|---|---|
| role_assignment_id | Unique assignment. |
| identity_account_id | User. |
| role_code | RM, IC, PM, ops maker, checker, supervisor. |
| business_unit | Desk/team. |
| booking_centre_scope | Jurisdiction/location scope. |
| effective_from / effective_to | Validity. |
| assignment_source | HR, IAM, manual, workflow. |
| approved_by | Access approver. |
| recertification_status | Current review state. |

## 5. Relationship assignment

| Field | Purpose |
|---|---|
| relationship_assignment_id | Unique assignment. |
| subject_party_id | Advisor/user/delegate. |
| object_party_id | Client/household/entity. |
| account_id / portfolio_id | Optional resource scope. |
| relationship_type | Primary RM, backup RM, PM, signatory, POA. |
| authority_scope | View, advise, trade, approve, receive report. |
| effective_from / effective_to | Validity. |
| source_document_id | Agreement or authority evidence. |
| status | Active, suspended, revoked. |

## 6. Entitlement grant

| Field | Purpose |
|---|---|
| entitlement_id | Unique permission grant. |
| subject_id | User/role/group. |
| resource_type | Client, account, portfolio, report, function. |
| resource_scope | Specific resource or scoped expression. |
| action_code | View, create, approve, export, trade. |
| effect | Allow/deny. |
| conditions | Channel, time, geography, MFA, approval. |
| effective_from / effective_to | Validity. |
| source | Policy, IAM, delegation, workflow. |

## 7. Policy decision

Store decisions for material actions.

| Field | Purpose |
|---|---|
| decision_id | Unique decision. |
| subject_id | Actor. |
| action_code | Action requested. |
| resource_type / resource_id | Target. |
| decision | Allow, deny, allow with obligations. |
| decision_time | Point-in-time. |
| policy_version | Policy evaluated. |
| input_attribute_hash | What attributes were used. |
| reason_codes | Denial or warning reasons. |
| obligations | Required disclosures, step-up, approval. |
| correlation_id | Request lineage. |

## 8. Workflow case

| Field | Purpose |
|---|---|
| case_id | Unique case. |
| case_type | Proposal, KYC, margin, corporate action. |
| status | Open, pending, approved, closed. |
| client_id / account_id / portfolio_id | Context. |
| priority / severity | Operational handling. |
| owner_team / owner_user | Responsibility. |
| created_by / created_at | Audit. |
| due_at | SLA. |
| linked_business_object | Proposal/order/report/etc. |
| closure_code | Final outcome. |

## 9. Task

| Field | Purpose |
|---|---|
| task_id | Unique task. |
| case_id | Parent. |
| task_type | Review, approve, verify, contact client. |
| assignee_user/team | Owner. |
| status | Open, claimed, completed, cancelled. |
| due_at | SLA. |
| decision | Approve, reject, amend. |
| reason_code | Structured reason. |
| evidence_links | Documents/checks. |

## 10. Approval

| Field | Purpose |
|---|---|
| approval_id | Unique approval. |
| approver_id | Actor. |
| approval_type | Suitability, mandate, report, order. |
| approved_object_type/id/version | Exact approved object. |
| decision | Approved/rejected/conditional. |
| conditions | Limits/expiry/monitoring. |
| timestamp | Decision time. |
| policy_context | Approval policy version. |

## 11. Interaction

| Field | Purpose |
|---|---|
| interaction_id | Unique interaction. |
| interaction_type | Meeting, call, message, consent, document view. |
| client_id / household_id | Context. |
| account_id / portfolio_id | Optional context. |
| channel | Portal, mobile, in-person, advisor desktop. |
| participants | Client/internal/delegate. |
| summary | Narrative or structured details. |
| tags | Advice, complaint, instruction, risk. |
| linked_objects | Proposal/report/order/case. |
| retention_category | Compliance retention. |

## 12. Consent

| Field | Purpose |
|---|---|
| consent_id | Unique consent. |
| consenting_party_id | Client/authorized party. |
| consent_type | Proposal, disclosure, data, document. |
| authority_basis | Self, POA, signatory, trustee. |
| linked_object_id/version | What was approved. |
| document_hashes | Exact documents. |
| channel | Portal/mobile/in-person. |
| authentication_context | MFA, device, session. |
| timestamp | Consent time. |
| status | Active, revoked, expired. |

## 13. Document delivery

| Field | Purpose |
|---|---|
| delivery_id | Unique delivery. |
| document_id/version | Delivered document. |
| recipient_party_id | Recipient. |
| authority_basis | Why recipient can receive. |
| delivery_channel | Portal, email notification, mail, RM delivery. |
| delivery_status | Sent, failed, viewed, acknowledged. |
| delivered_at / viewed_at | Evidence. |
| expiry_at | Link expiry. |
| archive_id | Stored record. |

## 14. Audit event

| Field | Purpose |
|---|---|
| audit_event_id | Unique event. |
| event_type | Business or technical event. |
| event_time | Occurrence time. |
| actor_id | User/system. |
| resource | Target. |
| action | Action. |
| outcome | Success/failure. |
| before_hash / after_hash | Change evidence. |
| correlation_id | Request chain. |
| source_service | Service generating event. |
| immutable_store_ref | Durable audit store. |

## 15. Lineage design

Every important user action should link to:

- actor identity;
- entitlement decision;
- workflow case/task;
- business object version;
- data snapshot;
- document/disclosure version;
- consent/approval record;
- downstream transaction/order/report;
- audit event chain.

Example proposal lineage:

```text
Client 360 snapshot
-> Proposal v1
-> Suitability result v1
-> Disclosure document v3
-> Client consent C1
-> Order O1
-> Trade T1
-> Confirmation D1
-> Portfolio review report R1
```

## 16. Data-retention and immutability

Different records need different treatment.

| Record | Retention/immutability expectation |
|---|---|
| Access logs | Retain for investigation and audit. |
| Consent records | Strong retention and tamper-evidence. |
| Proposal versions | Immutable after client presentation. |
| Meeting notes | Lock after policy period; add addendum rather than rewrite. |
| Secure messages | Retain as communication records. |
| Workflow approvals | Immutable. |
| Document delivery | Retain delivery evidence. |
| Entitlement changes | Retain point-in-time history. |

## 17. API design patterns

| API | Design note |
|---|---|
| `POST /authorization/decisions` | Evaluate material action and return decision/obligations. |
| `GET /clients/{id}/workbench-view` | Entitlement-filtered client 360 aggregate. |
| `POST /workflows/{type}` | Start workflow with business object. |
| `POST /tasks/{id}/complete` | Complete task with decision and evidence. |
| `POST /consents` | Capture consent against immutable version. |
| `GET /interactions` | Search interaction history with scope controls. |
| `POST /document-deliveries` | Deliver/archive document with recipient authority. |
| `GET /audit-timeline` | Reconstruct timeline for case/proposal/order/report. |

## 18. Data-quality rules

| Rule | Severity |
|---|---|
| Active client-facing user must link to active party. | Critical. |
| Consent must link to immutable object version. | Critical. |
| Approval must have approver role at decision time. | Critical. |
| Entitlement grant must be effective-dated. | High. |
| Delegation must have expiry or review cycle. | High. |
| Workflow completion must have closure code. | High. |
| Document delivery must identify recipient authority. | High. |
| Audit event must have correlation ID for material actions. | Medium/high. |

## 19. Practitioner Explanation

"The data model should make access decisions and workflow evidence reconstructable. I would model identity, roles, relationship scope, delegations, entitlement grants, policy decisions, workflow cases, tasks, approvals, interactions, consents, document deliveries and audit events as separate but linked objects. This avoids burying control evidence in UI logs and supports audit, complaints, reporting and production triage."
