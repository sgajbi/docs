# IAM, Entitlements, Zero Trust and Privileged Access

## 1. Why IAM is central in wealth platforms

Identity and access management is the foundation of client confidentiality, advisor productivity, regulatory compliance and operational control. In wealth platforms, access is not only application-level. It is also scoped by:

- legal entity / booking centre
- country and cross-border rules
- branch, market, desk and team
- client relationship manager assignment
- CIF / BR / household / account / portfolio
- product family and complexity
- service model: advisory, execution-only, DPM, brokerage, trust/fiduciary
- role: advisor, assistant, portfolio manager, investment counsellor, operations, compliance, support
- action: view, explain, recommend, approve, trade, override, export, report, administer

## 2. Identity types

| Identity type | Example | Control priority |
|---|---|---|
| Workforce user | Advisor, PM, ops user | SSO, MFA, role, portfolio scope, audit |
| Client user | Individual client portal user | MFA, device trust, consent, delegated access |
| Delegate | Family member, assistant, trustee | Authority scope and expiry |
| Service account | API-to-API identity | Non-human identity lifecycle and secret rotation |
| Workload identity | Kubernetes service identity | Short-lived credentials and policy enforcement |
| Break-glass user | Emergency admin | Strong approval, monitoring and expiry |
| Vendor user | Support / managed service | Just-in-time access and session recording |
| AI agent identity | Copilot/tool executor | Tool-scoped, client-scoped, action-scoped permissions |

## 3. Authentication patterns

| Pattern | Use | Key control |
|---|---|---|
| SSO | Workforce access | Central lifecycle and policy enforcement |
| MFA | Workforce/client access | Reduce credential-theft risk |
| Step-up authentication | Sensitive action | Required for orders, document download, beneficiary changes |
| Device binding | Client mobile and workforce laptops | Limit access from untrusted devices |
| Certificate / mTLS | Service-to-service | Strong service identity |
| OIDC/OAuth2 | APIs and channels | Token claims, scopes and expiry |
| Passwordless / FIDO | High assurance | Phishing-resistant authentication |

## 4. Authorization models

### RBAC

Role-based access control maps permissions to roles.

Example:

| Role | Capabilities |
|---|---|
| Advisor | View assigned clients, prepare proposal, submit for approval |
| Portfolio manager | View DPM portfolios, rebalance within mandate |
| Operations | Correct transactions with maker-checker |
| Compliance | View suitability evidence and exceptions |
| Support | Limited diagnostic access, no client export |

RBAC is simple but insufficient alone because wealth access depends heavily on client/account scope.

### ABAC

Attribute-based access control evaluates attributes:

```text
allow if user.market == client.market
and user.role includes "advisor"
and client.relationship_manager_id == user.id
and product.booking_centre in user.allowed_booking_centres
and action in role.allowed_actions
```

ABAC is useful for cross-border, booking-centre, product and client-scope rules.

### Relationship-based access

Relationship-based access links user access to relationship hierarchy:

- advisor to client
- team to advisor book
- portfolio manager to mandate
- trustee to trust account
- family-office staff to entity group
- operations queue to booking centre

## 5. Recommended entitlement dimensions

| Dimension | Examples |
|---|---|
| User role | advisor, assistant, PM, IC, compliance, ops, admin |
| Actor capacity | self, delegate, trustee, RM, temporary cover |
| Client scope | CIF, BR, household, legal entity, beneficial owner |
| Account scope | custody account, cash account, credit line, trust account |
| Portfolio scope | advisory portfolio, DPM portfolio, reporting group |
| Geography | country, booking centre, servicing location |
| Product scope | equities, bonds, structured products, alternatives, derivatives |
| Action scope | view, export, explain, propose, approve, order, override |
| Data sensitivity | public, internal, confidential, restricted, highly restricted |
| Channel | advisor workbench, client portal, API, batch, AI copilot |
| Time | working hours, temporary delegation, emergency access window |

## 6. Zero trust for wealth platforms

Zero trust means every access request is evaluated continuously based on identity, device, workload, data sensitivity, context and risk.

Wealth-platform zero trust should include:

- strong identity for users, services and workloads
- least-privilege authorization at API and data layer
- device posture and session risk checks
- micro-segmentation of services and data domains
- encrypted communications by default
- continuous logging and anomaly detection
- policy-as-code for infrastructure and APIs
- just-in-time privileged access
- no implicit trust based on network location alone

## 7. Privileged access management

Privileged access is especially risky because production systems may contain client financial data and transaction authority.

| Control | Requirement |
|---|---|
| Separate admin accounts | Do not use normal user IDs for admin activity |
| Just-in-time access | Time-boxed elevation with approval |
| Break-glass process | Emergency only; monitored and reviewed |
| Session recording | For sensitive infrastructure/admin operations |
| Command restrictions | Limit destructive commands in production |
| Production data access | Prefer masked views; require reason and ticket |
| Dual control | Maker-checker for overrides and data corrections |
| Access recertification | Periodic review by owner and manager |
| Orphan account detection | Remove leavers and stale service accounts |

## 8. Access patterns for AI assistants

AI assistants should not bypass entitlement systems. They need explicit identity and action scope.

| AI capability | Required access control |
|---|---|
| Portfolio question answering | Same client/portfolio scope as requesting user |
| Product explanation | Approved product knowledge only |
| Proposal drafting | Suitability-aware, recommendation not final until human approval |
| Trade generation | Generate draft trade list only, cannot execute without controlled workflow |
| Report summarization | Only reports user can already access |
| Operations triage | Masked data unless ops role and ticket permit details |
| Tool action | Tool-specific permission, audit event and approval where required |

## 9. Access review and certification

Access review should validate:

- user still employed / onboarded
- role still appropriate
- market/book assignment still current
- client/account access still justified
- privileged access still required
- service accounts still active and owned
- AI tools and APIs still correctly scoped
- emergency access not used as normal access

## 10. Example entitlement decision event

```json
{
  "event_type": "ENTITLEMENT_DECISION",
  "actor_id": "user-123",
  "actor_role": "advisor",
  "client_id": "cif-456",
  "portfolio_id": "pf-789",
  "action": "GENERATE_PROPOSAL",
  "channel": "ADVISOR_WORKBENCH",
  "decision": "ALLOW",
  "policy_version": "wealth-access-policy-2026.06",
  "decision_reason": "assigned_rm_and_product_scope_allowed",
  "correlation_id": "corr-abc"
}
```

## 11. Common IAM failures

| Failure | Impact |
|---|---|
| UI-only access control | API can still expose data |
| Role explosion | Permissions become unmanageable |
| No account/portfolio scoping | Advisor sees wrong client data |
| Shared admin accounts | No non-repudiation |
| Long-lived service secrets | High blast radius if leaked |
| Manual entitlement override without expiry | Permanent excessive access |
| AI tool not entitlement-aware | Model can summarize unauthorized data |
