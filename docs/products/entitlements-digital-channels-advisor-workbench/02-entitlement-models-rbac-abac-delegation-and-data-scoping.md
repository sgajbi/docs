# Entitlement Models: RBAC, ABAC, Delegation and Data Scoping

## 1. Entitlement design objective

The objective of entitlement design is to ensure that the right user can perform the right action on the right client/account/portfolio through the right channel at the right time, with the right approval and evidence.

A strong model normally combines multiple authorization styles:

```text
Effective Permission = Function Permission + Data Scope + Context Policy + Authority + Workflow State
```

No single model is sufficient for private banking.

## 2. RBAC: role-based access control

RBAC grants permissions based on business roles.

| Role | Example permissions |
|---|---|
| Relationship Manager | View assigned clients, start portfolio review, create advisory proposal. |
| Investment Counsellor | View eligible clients, suggest products, contribute rationale. |
| Portfolio Manager | View DPM accounts, create rebalance, approve model trades. |
| Operations Maker | Create instruction, update case, prepare corporate-action election. |
| Operations Checker | Approve maker input, release instruction. |
| Compliance Reviewer | View exceptions, request evidence, record review. |
| Supervisor | Approve exceptions, monitor team activity. |
| Client | View own accounts, approve documents, send secure message. |

### RBAC strengths

- Easy to understand.
- Good for functional permissions.
- Works well with IAM groups.
- Useful for onboarding and access-review processes.

### RBAC weaknesses

- Does not handle client/account scope well.
- Can lead to role explosion.
- Cannot easily express cross-border or mandate-specific logic.
- Does not capture temporary delegation or point-in-time authority.

## 3. ABAC: attribute-based access control

ABAC evaluates policies using attributes of subject, resource, action and environment.

| Attribute category | Examples |
|---|---|
| Subject attributes | User role, desk, booking centre, employment status, certifications. |
| Resource attributes | Client booking centre, account type, mandate, product complexity. |
| Action attributes | View, advise, trade, approve, export, deliver document. |
| Environment attributes | Channel, location, device, time, session risk, step-up status. |
| Relationship attributes | Assigned RM, backup RM, portfolio manager, authorized signatory. |

Example:

```text
Allow advisor to create proposal if:
- advisor is active;
- advisor is assigned to the client or has active delegation;
- client KYC and suitability are active;
- product is eligible for booking centre and target market;
- channel is approved for advisory proposal creation;
- no legal hold or restricted status blocks advice.
```

### ABAC strengths

- Handles complex policies.
- Reduces role explosion.
- Supports contextual decisions.
- Works well for cross-border, product, mandate and channel controls.

### ABAC weaknesses

- Requires high-quality reference data.
- Harder to explain to auditors unless decisions are logged clearly.
- Policy testing must be rigorous.
- Poorly designed attribute models become fragile.

## 4. Relationship-based access control

Relationship-based access control is critical in wealth platforms.

| Relationship | Access meaning |
|---|---|
| Primary RM | Full servicing and advisory access for assigned client. |
| Backup RM | Temporary or limited access during absence/coverage period. |
| Desk head | Supervisory visibility over team clients. |
| Investment counsellor | Advisory support access, sometimes no execution authority. |
| Portfolio manager | Access to DPM accounts under assigned strategy/mandate. |
| Trust officer | Fiduciary servicing access for trust clients. |
| Operations queue owner | Case-level access based on work assignment. |
| Client delegate | View/action rights granted by client or legal document. |

Relationship access should be effective-dated.

| Field | Purpose |
|---|---|
| relationship_type | RM, IC, PM, beneficiary, trustee, POA, signatory. |
| subject_party_id | User/client/legal person receiving authority. |
| object_party_id | Client/entity/account/portfolio being accessed. |
| effective_from / effective_to | Point-in-time validity. |
| authority_scope | View, transact, approve, receive report, consent. |
| source_document_id | POA, board resolution, client instruction, mandate agreement. |
| approval_status | Pending, active, suspended, revoked. |

## 5. Data scoping

Data scope determines which records a user may retrieve.

| Scope dimension | Example |
|---|---|
| Client scope | Assigned CIF/BR only. |
| Account scope | Specific account under client. |
| Portfolio scope | Specific DPM or advisory portfolio. |
| Household scope | Consolidated group, but not necessarily every legal entity. |
| Booking-centre scope | Singapore clients only. |
| Desk scope | Clients under one desk/team. |
| Product scope | Only approved products or asset classes. |
| Case scope | Operations user can see only assigned workflow cases. |
| Report scope | User can generate statement only for authorized recipient group. |

### Common data-scope mistakes

| Mistake | Why dangerous |
|---|---|
| Scope enforced only in UI. | API or export can leak data. |
| Scope based only on RM assignment. | Misses legal authority, family-office structures and trust access. |
| Household gives full visibility to all members. | May breach confidentiality between family members/entities. |
| Operations broad access not logged. | Insider-risk and privacy weakness. |
| Data exports bypass entitlement filtering. | High-impact data leakage. |

## 6. Delegation model

Delegation allows one party to act for another.

| Delegation type | Example |
|---|---|
| Internal delegation | Backup RM covers client while primary RM is absent. |
| Supervisory delegation | Desk head delegates approval queue temporarily. |
| Client delegation | Client authorizes spouse/assistant to view statements. |
| Legal delegation | Power of attorney, guardian, trustee, company signatory. |
| System delegation | Service account acts on behalf of workflow service. |

Recommended fields:

| Field | Meaning |
|---|---|
| delegation_id | Unique delegation record. |
| delegator_id | Party granting authority. |
| delegate_id | Party receiving authority. |
| resource_scope | Client/account/portfolio/document/case. |
| action_scope | View, approve, trade, sign, receive, export. |
| effective_from / effective_to | Validity period. |
| revocation_date | When revoked. |
| source_evidence | Document, instruction, HR coverage, supervisor approval. |
| constraints | Channel, amount limit, product limit, geography. |

## 7. Access-control decision pattern

A robust authorization service should produce a decision object, not just true/false.

```json
{
  "decisionId": "AUTHZ-20260627-0001",
  "subjectId": "USER123",
  "resourceType": "PORTFOLIO",
  "resourceId": "PF456",
  "action": "CREATE_ADVISORY_PROPOSAL",
  "decision": "ALLOW",
  "decisionTime": "2026-06-27T10:15:22+08:00",
  "policiesEvaluated": [
    "ACTIVE_USER",
    "RM_ASSIGNMENT",
    "BOOKING_CENTRE_COMPATIBILITY",
    "CLIENT_SUITABILITY_ACTIVE",
    "PRODUCT_CHANNEL_ALLOWED"
  ],
  "obligations": [
    "SHOW_RISK_DISCLOSURE",
    "CAPTURE_CLIENT_CONSENT_BEFORE_ORDER"
  ],
  "reasonCodes": []
}
```

For a denial:

```json
{
  "decision": "DENY",
  "reasonCodes": [
    "CLIENT_KYC_EXPIRED",
    "USER_NOT_ASSIGNED_TO_CLIENT",
    "CROSS_BORDER_VIEW_BLOCKED"
  ],
  "safeMessage": "You do not have access to this client or the client profile is not active."
}
```

## 8. Policy decision versus policy enforcement

| Layer | Responsibility |
|---|---|
| Policy administration point | Author policy rules and manage lifecycle. |
| Policy decision point | Evaluate access request and return allow/deny/obligations. |
| Policy enforcement point | Enforce decision in API, UI, workflow or data service. |
| Policy information point | Supply attributes such as client, account, KYC, mandate, booking centre. |

The same policy decision should be reused by advisor workbench, client portal, reporting APIs and workflow services.

## 9. Segregation of duties

| Rule | Example |
|---|---|
| Maker cannot check own work. | Operations user creating a payment cannot approve it. |
| Advisor cannot approve own suitability override. | Supervisor/compliance approval required. |
| Portfolio manager cannot bypass mandate breach alone. | Investment committee or risk approval required. |
| Production support cannot modify data and approve correction. | Separate data-fix and approval process. |
| Client delegate cannot change own authority. | Client/legal owner approval required. |

## 10. Break-glass access

Break-glass access is emergency access used for incident or critical support.

Minimum controls:

- strong approval or emergency justification;
- time-limited access;
- elevated logging;
- post-use review;
- automatic revocation;
- immutable audit trail;
- no silent data export;
- management attestation.

## 11. Entitlement review and recertification

| Review type | Frequency | Evidence |
|---|---|---|
| User access recertification | Monthly/quarterly depending on policy. | Manager attestation, exceptions. |
| Privileged access review | More frequent. | Admin account list, activity log. |
| Orphan access review | Daily/weekly. | Users without active employment/assignment. |
| Delegation review | Before expiry and periodically. | Active delegations and evidence. |
| Service account review | Periodic and release-based. | Owner, purpose, permissions, token age. |
| Cross-border access review | Risk-based. | Access logs by location and client booking centre. |

## 12. Test scenarios

| Scenario | Expected result |
|---|---|
| RM assigned to client creates proposal. | Allowed if client/profile/product/channel all valid. |
| RM not assigned tries to view client. | Denied and logged. |
| Backup RM access after end date. | Denied. |
| Operations maker approves own payment. | Denied due to SoD. |
| Client delegate views unauthorized account. | Denied. |
| Supervisor accesses team client. | Allowed with supervisory reason and logged. |
| Break-glass user accesses portfolio. | Allowed only with emergency entitlement and review event. |
| Export includes client outside data scope. | Blocked or filtered at API level. |
