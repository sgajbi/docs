# Entitlements, Digital Channel Fundamentals and Taxonomy

## 1. Executive summary

Entitlements are the rules and evidence that determine **who can do what, to whom, in which channel, under which conditions, and with what approval**. In wealth management, entitlements are more complicated than generic application roles because permissions are not only function-based. They are shaped by client relationship, booking centre, account type, portfolio mandate, advisor assignment, delegation, power of attorney, legal entity structure, product complexity, geography, channel, client consent and supervisory controls.

Digital channels and advisor workbenches are the user-facing surfaces where these entitlements become real. A client portal may show holdings and statements. A mobile channel may allow secure messaging and approvals. An advisor workbench may show client 360, portfolio review, proposals, suitability results, product ideas, alerts and tasks. Operations portals may process exceptions, statements, corporate actions, onboarding tasks and client instructions.

The central design principle is:

```text
Do not treat permissions, workflows and client interactions as UI-only logic.
They must be auditable domain objects with source ownership, lineage and point-in-time reconstruction.
```

## 2. Core concepts

| Concept | Meaning | Wealth-platform implication |
|---|---|---|
| Identity | The user, client, advisor, operations user, system, API client or service account. | A single human may have multiple roles and relationships. |
| Authentication | Proof that the user is who they claim to be. | MFA, device binding, step-up authentication and session risk controls matter. |
| Authorization | Decision that the authenticated user may perform an action. | Must consider function, data scope, channel and context. |
| Entitlement | A permission grant, restriction or access scope. | Can be role-based, relationship-based, account-based, mandate-based or product-based. |
| Data scope | Which clients/accounts/portfolios/documents a user may access. | Critical for RM teams, support teams, family office groups and cross-border controls. |
| Workflow | A controlled process with states, tasks, approvals, evidence and outcomes. | Advisory proposal, suitability override, order approval, statement approval, KYC refresh. |
| Interaction | A meeting, call, note, message, consent, disclosure, document view or client action. | Must be stored for audit, complaint handling and advisor continuity. |
| Channel | Advisor desktop, client portal, mobile app, API, operations portal, contact centre. | Capability may differ by channel and jurisdiction. |
| Client 360 | Unified client and relationship view. | Should combine relationship, accounts, holdings, analytics, alerts, tasks and interactions. |
| Audit trail | Immutable record of actions and decisions. | Required for control evidence and reconstruction. |

## 3. Why wealth entitlements are harder than normal access control

A normal enterprise application may ask: "Is the user an admin, maker or checker?"

A wealth platform must ask:

- Is the user the assigned RM, backup RM, investment counsellor, assistant, desk head, portfolio manager, compliance user or operations user?
- Does the user belong to the same booking centre as the client?
- Is this client in Singapore, Hong Kong, Japan, Australia, UAE, Switzerland or another location?
- Does cross-border policy permit this user to view, advise or transact?
- Is the portfolio advisory, execution-only, discretionary, trust-owned, company-owned or family-office consolidated?
- Is the product approved for this jurisdiction and client segment?
- Is the client's risk profile and KYC status active?
- Is the user acting under delegation, power of attorney, guardian authority or corporate signatory authority?
- Is the channel allowed to show full transaction details, suitability output or restricted products?
- Is step-up authentication required for a sensitive action?
- Is supervisor approval required before execution or report release?

## 4. Main user groups

| User group | Typical needs | Control focus |
|---|---|---|
| Client | View portfolio, statements, messages, documents, approvals, service requests. | Privacy, consent, authentication, document delivery, channel restrictions. |
| Advisor / RM | Client 360, portfolio review, proposals, suitability, order capture, alerts. | Data scope, advice evidence, conflict controls, cross-border controls. |
| Investment counsellor | Product ideas, portfolio construction, proposal support, client discussions. | Product eligibility, advisory rationale, restricted-list controls. |
| Portfolio manager | DPM mandate view, model drift, rebalance workflow, trade list, overrides. | Mandate compliance, pre-trade checks, allocation fairness. |
| Operations | Onboarding, instructions, settlement breaks, corporate actions, statements. | Maker-checker, segregation of duties, exception lifecycle. |
| Compliance / risk | Reviews, surveillance, suitability exceptions, access investigations. | Audit completeness, exception escalation, evidence capture. |
| Supervisor / desk head | Approval queue, activity oversight, RM coverage, risk exceptions. | Timely review, supervisory attestations, escalation. |
| Support / production | Incident triage, data-quality issues, entitlement breaks. | Break-glass controls, least privilege, audit trail. |
| System/API client | Batch, events, report generation, digital delivery. | Service identity, scoped token, non-human account governance. |

## 5. Major channel types

| Channel | Purpose | Typical controls |
|---|---|---|
| Advisor workbench | Internal front-office desktop. | Role, relationship, region, product, mandate and supervisor controls. |
| Client portal | Client self-service web. | Strong authentication, document access, consent, privacy, secure messaging. |
| Mobile app | On-the-go client access. | Device binding, biometric activation, push approval, session risk. |
| Operations portal | Back-office work management. | Maker-checker, queue access, exception controls. |
| Contact centre desktop | Assisted service. | Call authentication, partial masking, view-only/action separation. |
| Portfolio manager console | DPM/model portfolio operations. | Mandate-level access, rebalance approvals, allocation controls. |
| API channel | Machine-to-machine services and partners. | OAuth scopes, service accounts, API entitlements, consent records. |
| Reporting channel | Statement/PDF generation and delivery. | Report scope, recipient authority, delivery consent, archive evidence. |

## 6. Domain boundaries

| Domain | Owns | Does not own |
|---|---|---|
| Identity management | User identity, authentication factors, federation, session. | Business eligibility logic. |
| Entitlement service | Role, permission, data scope, relationship scope, policy decision. | Product pricing or performance calculation. |
| Client master | Client, account, portfolio, advisor assignment, legal role. | UI button visibility. |
| Workflow engine | Tasks, states, transitions, approvals, SLA, audit. | Business calculations unless delegated to domain services. |
| Interaction history | Meetings, notes, messages, consents, disclosures, evidence. | Position valuation. |
| Advisor workbench | User journey orchestration and presentation. | Core source-of-record data ownership. |
| Reporting platform | Report snapshot, generation, archive, delivery. | Client master ownership. |

## 7. Design principles

1. **Least privilege by default**: access must be granted explicitly and reviewed regularly.
2. **Point-in-time decisioning**: reconstruct what access, profile, mandate, disclosure and approval existed at decision time.
3. **Separate identity from authority**: being authenticated does not mean the user is allowed to act.
4. **Separate role from data scope**: a user may be an RM but only for specific clients/portfolios.
5. **Separate view from action**: viewing a portfolio is different from advising, approving or trading.
6. **Separate UI visibility from backend enforcement**: the API must enforce entitlements even if the UI hides actions.
7. **Capture evidence as structured data**: disclosures, consents, suitability results and overrides should not be stored only as PDF text.
8. **Make degraded states explicit**: missing KYC, expired suitability, stale mandate, missing consent or uncertain authority should block or degrade workflows.
9. **Treat audit as product functionality**: audit and lineage are not optional logging details.

## 8. Typical failure modes

| Failure | Impact |
|---|---|
| Advisor sees a client from the wrong booking centre. | Cross-border and privacy breach. |
| Backup RM access remains active after coverage ends. | Orphaned access and insider-risk issue. |
| Client portal shows consolidated trust/family-office data to an unauthorized beneficiary. | Confidentiality and fiduciary issue. |
| Suitability override stored as free text only. | Cannot reconstruct approval basis. |
| Report generated after data correction but archived without version evidence. | Client dispute and audit weakness. |
| Workflow state and transaction state diverge. | Order may execute without required approval evidence. |
| Operations user has both maker and checker roles. | Segregation-of-duties breach. |
| Digital consent captured after order timestamp. | Advice/process control failure. |

## 9. Practitioner Explanation

"Entitlements in a wealth platform are not just application roles. They combine identity, relationship coverage, account and portfolio scope, booking-centre policy, client authority, mandate restrictions, product eligibility, channel capabilities and workflow approvals. I would design them as a policy-driven domain with point-in-time decision evidence. Advisor workbench and client channels should consume the entitlement service, but backend APIs must enforce the same decisions. Every material interaction - advice, proposal, disclosure, consent, approval, order, report delivery or override - should create an auditable event so the bank can reconstruct what happened, who acted, what the client saw, and why the action was allowed."
