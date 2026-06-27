# Workflow Orchestration, Tasks, Approvals, Escalations and Case Management

## 1. Why workflow orchestration matters

Wealth-management platforms involve many controlled actions that cannot be implemented as simple CRUD updates. Advisory recommendations, DPM rebalances, product approvals, suitability exceptions, corporate-action elections, margin calls, onboarding refreshes, client instructions and report approvals all require state, ownership, evidence and escalation.

A workflow engine should manage:

- state transitions;
- task ownership;
- maker-checker controls;
- approvals and overrides;
- SLA tracking;
- escalation;
- audit evidence;
- integration with domain services;
- notifications;
- cancellation and reversal;
- exception resolution.

## 2. Workflow versus process versus case

| Term | Meaning |
|---|---|
| Workflow | Defined state machine or process path. |
| Task | Work item assigned to a user/team/system. |
| Approval | Formal decision that allows progress. |
| Case | Container for related tasks, documents, comments and decisions. |
| Exception | Condition requiring review, correction or override. |
| SLA | Expected response/resolution time. |
| Escalation | Movement to higher authority or priority due to time/risk. |

## 3. Common wealth workflows

| Workflow | Trigger | Key controls |
|---|---|---|
| Advisory proposal | Advisor starts recommendation. | Suitability, disclosure, consent, versioning. |
| DPM rebalance | Drift or PM instruction. | Mandate, allocation, pre-trade, PM approval. |
| Product approval | New product due diligence. | APU, risk rating, target market, disclosures. |
| Order approval | Order captured. | Pre-trade checks, limits, maker-checker, client consent. |
| Corporate action election | Custodian event received. | Eligibility, deadline, client instruction, default option. |
| Margin call | Collateral shortfall. | Notice delivery, remediation, liquidation escalation. |
| KYC refresh | Periodic or event-driven. | Client documentation, AML review, account restrictions. |
| Statement approval | Report generated. | Data quality, valuation completeness, disclosure. |
| Complaint case | Client complaint received. | Classification, response SLA, evidence preservation. |
| Data correction | Break identified. | Maker-checker, lineage, recalculation, notification. |

## 4. Workflow state model

A workflow should have explicit states.

```text
DRAFT
-> SUBMITTED
-> CHECKS_RUNNING
-> PENDING_APPROVAL
-> APPROVED
-> PENDING_CLIENT_CONSENT
-> CONSENTED
-> RELEASED_TO_EXECUTION
-> COMPLETED
```

Exception path:

```text
CHECKS_RUNNING
-> EXCEPTION_RAISED
-> PENDING_REVIEW
-> APPROVED_WITH_OVERRIDE / REJECTED / RETURNED_FOR_AMENDMENT
```

Cancellation path:

```text
DRAFT/SUBMITTED/PENDING_APPROVAL
-> CANCELLED
```

## 5. Workflow design rules

1. Workflow state is not the same as transaction state.
2. A workflow should never silently skip required approvals.
3. Every transition should have actor, timestamp, reason and source.
4. Manual override must require reason and authority.
5. Task assignment should be rule-driven and auditable.
6. Workflow should reference immutable versions of proposal/report/disclosure data.
7. Long-running workflows need SLA, reminders and escalation.
8. Integration failures should create retryable technical tasks, not hidden errors.
9. Workflow completion should emit domain events.
10. Cancelled workflows should remain searchable for audit.

## 6. Task model

| Field | Purpose |
|---|---|
| task_id | Unique task. |
| case_id / workflow_id | Parent container. |
| task_type | Review, approve, verify, contact client, upload document. |
| assigned_to_user / team | Owner. |
| candidate_roles | Who may claim task. |
| priority | Normal/high/urgent. |
| due_at | SLA deadline. |
| status | Open, claimed, completed, rejected, escalated. |
| required_action | Approve, reject, amend, provide evidence. |
| reason_code | Decision reason. |
| comments | Structured/narrative comments. |
| created_by / completed_by | Audit. |
| evidence_links | Documents, reports, calculations, consent records. |

## 7. Approval model

Approvals should be specific, not generic.

| Approval type | Example authority |
|---|---|
| Suitability override | Supervisor or compliance. |
| Mandate breach approval | PM head, risk, investment committee. |
| Product exception | Product governance committee. |
| Fee waiver | Desk head or pricing committee. |
| High-value order | Supervisor or credit/risk. |
| Cross-border exception | Legal/compliance. |
| Data correction | Data owner and operations checker. |
| Report release | Report owner / advisor / supervisor. |

Approval record fields:

| Field | Meaning |
|---|---|
| approval_id | Unique approval. |
| approval_type | Suitability, mandate, product, report, data correction. |
| approver_id | User granting approval. |
| approver_role_at_time | Supervisor, compliance, PM head, etc. |
| scope | Client/account/product/order/report. |
| approved_version | Exact object version. |
| decision | Approved, rejected, approved with conditions. |
| reason | Structured code and narrative. |
| conditions | Expiry, amount cap, product cap, monitoring requirement. |
| timestamp | Decision time. |
| evidence | Linked documents/check results. |

## 8. Escalation model

Escalation should be predictable.

| Escalation trigger | Example |
|---|---|
| SLA breach | KYC refresh not completed. |
| Risk severity | Margin shortfall above threshold. |
| Client segment | UHNW complaint escalated immediately. |
| Product complexity | Complex product exception requires compliance. |
| Financial amount | Large transfer/order. |
| Repeated failure | Multiple failed settlement retries. |
| Regulatory deadline | Corporate action deadline approaching. |

Escalation can change:

- priority;
- assignee;
- approver level;
- notifications;
- block/unblock state;
- client communication requirement.

## 9. Case management

A case is useful when multiple tasks and artifacts are related.

Examples:

- onboarding case;
- KYC refresh case;
- complaint case;
- corporate action case;
- margin call case;
- data correction case;
- failed settlement case;
- advisory exception case.

Case fields:

| Field | Meaning |
|---|---|
| case_id | Unique case. |
| case_type | KYC, complaint, margin, corporate action, etc. |
| client/account/portfolio | Context. |
| severity | Normal/high/critical. |
| status | Open, pending client, pending approval, resolved, closed. |
| owner_team | RM team, operations, compliance, risk. |
| due_at | SLA. |
| related_tasks | Tasks. |
| related_documents | Evidence. |
| related_events | Trigger events. |
| resolution_code | Outcome. |
| closure_evidence | Evidence for close. |

## 10. Workflow integration patterns

| Pattern | Use |
|---|---|
| Synchronous check | Fast validation: entitlement, active client, mandatory field. |
| Asynchronous check | Long calculation: suitability, risk simulation, report generation. |
| Event-driven workflow | Corporate action, margin call, price exception, KYC expiry. |
| Human task | Approval, review, client contact. |
| Timer event | SLA reminder, expiry, auto-escalation. |
| Compensation | Cancel order, reverse task, regenerate report. |
| External handoff | OMS, document service, digital signature, CRM. |

## 11. Idempotency and duplicate prevention

Workflow actions should be idempotent where possible.

| Risk | Control |
|---|---|
| Duplicate client consent event. | Idempotency key per proposal version and client. |
| Duplicate order release. | Workflow state transition lock and order reference. |
| Duplicate task creation. | Unique task type per case/state. |
| Duplicate report generation. | Snapshot ID and version key. |
| Duplicate notification. | Notification de-duplication and delivery state. |

## 12. QA scenarios

| Scenario | Expected result |
|---|---|
| Proposal submitted with missing KYC. | Workflow blocked with exception. |
| Supervisor rejects override. | Workflow cannot continue to order. |
| Client consents to old proposal version. | Consent invalid for amended version. |
| Approval expires before execution. | Reapproval required. |
| SLA breached on margin call. | Escalation triggered. |
| Operations maker tries to check own case. | Denied. |
| Report generation fails. | Technical retry task and failure state. |
| Duplicate click on release order. | Only one order is created. |

## 13. Practitioner Explanation

"I would treat workflows as auditable state machines. They should not be hidden in UI code. Each workflow should have explicit states, tasks, approvals, SLA, escalation, decision evidence and linked business objects such as proposal version, suitability result, consent record, order or report snapshot. This allows the platform to reconstruct who did what, when, under what authority, and why the process was allowed to continue."
