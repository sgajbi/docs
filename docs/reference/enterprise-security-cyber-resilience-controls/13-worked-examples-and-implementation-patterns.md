# Worked Examples and Implementation Patterns

This file converts enterprise security, cyber resilience, IAM, data protection and audit-control guidance into practical patterns for regulated wealth-platform work.

## Example 1. Entitlement decision for a portfolio view

**Scenario:** An advisor opens a portfolio dashboard for a client household.

| Decision input | Example | Why it matters |
|---|---|---|
| Actor | Advisor identity, role, booking centre and employment status. | Confirms who is acting. |
| Resource | Client, account, portfolio, report and document ids. | Confirms what is being accessed. |
| Relationship | Advisor-client assignment or team coverage. | Prevents broad role-only access. |
| Action | View, export, recommend, approve or execute. | Different actions carry different risk. |
| Context | Channel, location, device posture, time and risk signal. | Supports adaptive control. |
| Policy version | Entitlement rule version and source. | Makes decisions auditable and reproducible. |

**Pattern:** Authorization should be server-side, attribute-aware and audit-ready. The UI may hide actions, but the API must enforce the policy.

## Example 2. Report delivery data-protection control

**Scenario:** Monthly client statements are generated and delivered through a client portal.

Minimum controls:

1. immutable report snapshot and document id,
2. entitlement check at generation and retrieval time,
3. encrypted document storage,
4. delivery-channel allowlist,
5. watermarking or traceable download evidence,
6. no client-sensitive fields in logs,
7. misdelivery prevention test,
8. revocation and retention handling,
9. audit event for generation, view, download and delivery.

**Pattern:** Report security must cover data assembly, rendering, storage, delivery and subsequent access, not only the download endpoint.

## Example 3. API security contract

**Scenario:** A portfolio API exposes holdings, performance and risk data.

| Control | Expected implementation |
|---|---|
| Authentication | Strong token validation, issuer/audience checks and expiry handling. |
| Authorization | Portfolio-scope entitlement check for every request. |
| Input validation | Strict identifiers, pagination, date and currency validation. |
| Error handling | No account, client or entitlement details leaked in errors. |
| Rate limiting | Protects sensitive and expensive endpoints. |
| Audit | Actor, action, resource, decision, reason and correlation id. |
| Data minimization | Return only fields required for the caller and use case. |

**Pattern:** A secure financial API returns the right data to the right actor for the right action with a traceable decision.

## Example 4. Threat model for an AI portfolio assistant

**Scenario:** An AI assistant drafts portfolio-review explanations using product notes, client portfolio data and report history.

Threat model focus:

| Threat | Mitigation |
|---|---|
| Retrieval crosses client boundary | Entitlement-aware retrieval and client/account filters. |
| Prompt injection in source document | Source trust scoring, tool restrictions and instruction hierarchy. |
| Unsupported recommendation | Suitability, mandate and product-governance checks before user action. |
| Sensitive data leakage | Redacted logs and no cross-session context reuse. |
| Excessive agency | Draft-only mode unless an approved workflow authorizes action. |
| Untraceable output | Store source references, model version, prompt template and review status where policy allows. |

**Pattern:** AI security is both data-access security and action-authority security.

## Example 5. Security detection for suspicious access

**Scenario:** A user downloads many client reports outside normal behavior.

Detection signal should combine:

1. user identity and role,
2. client and portfolio scope,
3. number of reports downloaded,
4. time and location,
5. device and network context,
6. previous behavior baseline,
7. privileged access status,
8. case or approved business reason,
9. downstream action such as export or email.

**Pattern:** Wealth-platform detection needs business context. A raw download count is weaker than an entitlement-aware anomaly.

## Example 6. Cyber incident response for data exposure

**Scenario:** A misconfigured policy exposes documents to an unauthorized internal group.

Response flow:

1. contain access by disabling the policy or feature flag,
2. preserve audit logs and affected artifact versions,
3. identify impacted clients, documents, accounts and users,
4. validate whether data was viewed, exported or delivered,
5. correct the policy and add regression tests,
6. run access recertification for impacted roles,
7. prepare legal, compliance and client communication inputs as required,
8. record root cause, control failure and preventive actions.

**Pattern:** Cyber response must preserve business-impact evidence, not only infrastructure logs.

## Example 7. Control evidence model

**Scenario:** Audit asks how production access and AI retrieval are governed.

Evidence package:

| Evidence | Purpose |
|---|---|
| Policy definition | Shows the intended control. |
| Approval workflow | Shows who approved access or exceptions. |
| Enforcement logs | Shows the control operated at runtime. |
| Test evidence | Shows the control blocks unauthorized behavior. |
| Exception register | Shows residual risk ownership and expiry. |
| Recertification record | Shows periodic review. |
| Incident/problem links | Shows control failures and remediation. |

**Pattern:** Audit-ready evidence connects policy, enforcement, testing, exceptions and operations.

## Example 8. Security release gate

Use a security release gate when a change affects identity, entitlements, client data, transactions, reporting, AI context, secrets or production access.

Minimum gate:

1. threat model updated,
2. entitlement tests added,
3. sensitive-data logging check passed,
4. dependency and container scans passed or exceptions approved,
5. secrets and key-management review complete,
6. API security tests complete,
7. detection and audit events defined,
8. rollback and containment plan documented,
9. production support runbook updated.

**Pattern:** Security gates should be risk-based and evidence-driven, not generic checklist theater.

## Security review checklist

- Does the design identify actors, resources, actions and policy decisions?
- Are entitlement decisions enforced server-side and logged?
- Is sensitive client data minimized, encrypted and masked where needed?
- Are secrets excluded from source, images, logs and prompts?
- Are API errors and logs safe from data leakage?
- Is AI retrieval entitlement-aware and traceable?
- Are privileged actions separated, approved and monitored?
- Are detection rules tied to business context?
- Is incident response evidence sufficient for client, audit and regulatory review?
- Are control exceptions time-bound with accountable owners?

## Disclaimer

This material is for education, architecture, platform design, engineering, security-control analysis and documentation work. It is not legal, regulatory, audit, cybersecurity certification, tax, investment or client advice.
