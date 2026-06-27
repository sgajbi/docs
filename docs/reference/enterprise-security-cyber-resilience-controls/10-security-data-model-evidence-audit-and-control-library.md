# Security Data Model, Evidence, Audit and Control Library

## 1. Why a security data model matters

Security controls need evidence. In regulated wealth platforms, it is not enough to say a control exists; the platform should prove:

- who accessed what
- which policy allowed or denied access
- which data was used
- which approval occurred
- which source/version/calculation supported the output
- which exception was granted
- which control test passed or failed

## 2. Core entities

| Entity | Purpose |
|---|---|
| Identity | User, service, workload, client, delegate |
| Role | Business role or technical role |
| Permission | Action allowed on a resource |
| Policy | Rule set used to evaluate access or action |
| Resource | Client, account, portfolio, report, product, order |
| Entitlement assignment | Link between identity and scope |
| Access decision | Allow/deny decision evidence |
| Audit event | Immutable record of sensitive action |
| Control | Security requirement or testable safeguard |
| Evidence | Artifact proving control operation |
| Exception | Approved deviation with expiry |
| Incident | Security or resilience event |
| Vulnerability | Weakness requiring remediation |
| Secret | Managed credential/key metadata |
| AI interaction | Prompt/retrieval/output/tool evidence |

## 3. Policy data model

```json
{
  "policy_id": "wealth-access-policy",
  "version": "2026.06",
  "resource_type": "portfolio",
  "action": "view_performance",
  "conditions": [
    "actor.status == active",
    "actor.role in ['advisor','pm','compliance']",
    "portfolio.booking_centre in actor.allowed_booking_centres",
    "portfolio.client_id in actor.assigned_clients",
    "data_classification <= actor.max_data_classification"
  ],
  "decision_default": "deny"
}
```

## 4. Audit event model

| Field | Description |
|---|---|
| event_id | Unique immutable ID |
| event_type | Access, action, approval, export, AI response |
| actor_id | User/service/agent |
| actor_capacity | Advisor, delegate, service, AI agent |
| resource_type | Client, portfolio, report, order, model, secret |
| resource_id | Resource identifier |
| client/account/portfolio scope | Wealth-specific scope |
| action | View, export, update, approve, generate, execute |
| decision | Allow, deny, blocked, pending approval |
| policy_version | Rule version applied |
| timestamp | Event time |
| channel | API, UI, batch, AI, support |
| correlation_id | End-to-end trace |
| reason | Optional structured explanation |

## 5. Control library structure

| Field | Example |
|---|---|
| control_id | SEC-IAM-001 |
| control_name | MFA required for workforce access |
| domain | IAM |
| objective | Prevent unauthorized access |
| requirement | All workforce users must authenticate via SSO + MFA |
| evidence | IdP policy export, access logs, test results |
| frequency | Continuous / monthly / quarterly |
| owner | IAM platform owner |
| tester | Security assurance / QA |
| linked risks | Credential compromise |
| linked systems | Advisor workbench, APIs, admin consoles |
| maturity | Defined / implemented / measured / optimized |

## 6. Example controls

| Control ID | Control |
|---|---|
| SEC-IAM-001 | MFA required for workforce and privileged users |
| SEC-IAM-002 | Access to client/portfolio data must be scope-controlled |
| SEC-PAM-001 | Privileged production access requires just-in-time approval |
| SEC-DATA-001 | Restricted client data encrypted at rest and in transit |
| SEC-DATA-002 | Production data must not be copied to lower environments unmasked |
| SEC-API-001 | All APIs enforce object-level authorization server-side |
| SEC-APP-001 | Mutation APIs require idempotency and audit logging |
| SEC-AI-001 | RAG retrieval must enforce source-level entitlements |
| SEC-AI-002 | AI-generated recommendations require human approval |
| SEC-OPS-001 | Security incidents follow approved response playbook |
| SEC-DR-001 | Critical backups must be restore-tested periodically |
| SEC-SDLC-001 | High-risk features require threat model before build |

## 7. Evidence examples

| Control | Evidence |
|---|---|
| API authorization | Automated test results with deny/allow cases |
| Report delivery control | Report-recipient reconciliation log |
| PAM | JIT request, approval, session record, expiry evidence |
| Secrets management | Vault rotation log and secret-scan report |
| RAG entitlement | Test showing unauthorized document not retrieved |
| DR | Restore test report and RTO/RPO outcome |
| Vulnerability remediation | Scan result before/after fix |
| Access review | Certification output and removal evidence |

## 8. Audit-ready lineage

Security lineage should connect:

```text
business requirement -> risk -> control -> policy -> implementation -> test -> runtime event -> evidence -> review
```

For example:

```text
Client report confidentiality -> report access control -> entitlement policy -> API authorization middleware -> integration test -> report download event -> monthly review evidence
```

## 9. Exception model

```json
{
  "exception_id": "EXC-2026-001",
  "control_id": "SEC-SDLC-003",
  "system": "portfolio-reporting-api",
  "risk_description": "DAST scan deferred due to emergency fix",
  "compensating_controls": ["WAF enabled", "manual code review", "targeted API tests"],
  "owner": "engineering-lead",
  "approver": "security-risk-owner",
  "expiry_date": "2026-07-31",
  "status": "APPROVED"
}
```

## 10. Security metrics

| Metric | Why it matters |
|---|---|
| MFA coverage | Credential compromise resistance |
| Privileged access duration | Excessive access control |
| Open critical vulnerabilities | Attack surface |
| Mean time to remediate | Operational discipline |
| Secrets found in scans | Secure engineering hygiene |
| Unauthorized access attempts | Attack/abuse signals |
| Access recertification completion | Entitlement governance |
| Security incidents by severity | Control health |
| Restore test success rate | Cyber resilience |
| AI safety block rate | AI control effectiveness |
