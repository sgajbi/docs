# Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| ABAC | Attribute-based access control. Access is decided using subject, resource, action and environment attributes. |
| RBAC | Role-based access control. Access is granted through roles. |
| Entitlement | Permission, restriction or access scope that determines what a user can do. |
| Data scope | The set of clients/accounts/portfolios/documents a user can access. |
| Delegation | Temporary or legal authority for one party to act for another. |
| Power of Attorney | Legal authority granted by a person to another person to act on their behalf. |
| Maker-checker | Control where one user prepares and another approves. |
| Segregation of duties | Principle preventing conflicting roles from being performed by the same user. |
| Step-up authentication | Stronger authentication required for sensitive actions. |
| Consent | Client agreement to a specific action, disclosure or data use. |
| Acknowledgement | Confirmation that information was received or understood. |
| Interaction history | Record of meetings, calls, messages, notes, consents and client actions. |
| Client 360 | Unified view of client relationship, portfolio, products, tasks and interactions. |
| Workflow case | Container for tasks, evidence and decisions. |
| Audit trail | Durable record of actions and outcomes. |
| Break-glass access | Emergency elevated access with strict monitoring. |
| Document delivery | Controlled distribution and archive evidence for client documents. |
| Secure messaging | Authenticated communication channel between client and bank. |
| Policy decision point | Component that evaluates authorization policy. |
| Policy enforcement point | Component that enforces authorization result. |
| Service account | Non-human identity used by systems or APIs. |

## 2. Practitioner Explanations

### What are entitlements in a wealth platform?

Entitlements define who can view or act on clients, accounts, portfolios, documents, workflows and products. In wealth management, entitlements combine role, relationship, data scope, booking centre, product eligibility, mandate, client authority, channel and workflow state. They should be enforced by backend APIs, not only by UI button visibility.

### Why is RBAC not enough?

RBAC is useful for broad functional permissions, but wealth platforms require relationship and attribute-based decisions. A user may be an RM, but only for assigned clients and only for certain booking centres. The same user may view a portfolio but not advise, trade, approve, export or deliver documents. ABAC and relationship-based access are needed to express these rules.

### What is the difference between consent and acknowledgement?

Acknowledgement means the client confirms they received or understood information. Consent means the client agrees to a specific action, such as approving an advisory proposal. Consent should link to the exact version of the proposal, disclosure and terms that the client saw.

### How should digital consent be modelled?

Digital consent should capture consenting party, authority basis, object version, disclosure version, channel, authentication context, timestamp, device/session metadata and status. It should be immutable and linked to downstream order or service action.

### What is an advisor workbench?

An advisor workbench is a front-office control tower that combines client 360, portfolio review, product ideas, proposal generation, suitability, mandate checks, pre-trade checks, client consent, tasks, alerts, document delivery and interaction history.

### What is the purpose of workflow orchestration?

Workflow orchestration manages long-running, auditable business processes with states, tasks, approvals, SLA, escalation and evidence. It prevents critical actions like order release, suitability override or report delivery from happening without required controls.

### Why is interaction history important?

Interaction history preserves evidence of meetings, advice, disclosures, consents, instructions, complaints and service actions. It supports advisor continuity, audit, conduct supervision, complaint handling and client explanations.

### How should reports be delivered digitally?

Reports should be delivered only to authorized recipients, linked to a report version and data snapshot, and stored with delivery status, viewed/downloaded timestamp, recipient authority and archive reference.

### What is break-glass access?

Break-glass access is emergency elevated access used during critical support or incident handling. It must be time-limited, justified, logged, reviewed and automatically revoked.

## 3. Practitioner Review Checklist

| Question | Strong platform answer |
|---|---|
| Who can see this client? | Evaluate role, relationship, booking centre, legal authority and data scope. |
| Who can advise? | Evaluate service model, advisor assignment, suitability/profile status and product eligibility. |
| Who can approve? | Evaluate approval type, approver role, SoD and policy version. |
| Who can receive report? | Evaluate recipient authority and report scope. |
| What did client approve? | Show exact proposal/disclosure version and consent evidence. |
| Why was action blocked? | Return safe reason codes and remediation path. |
| What happened in sequence? | Reconstruct audit timeline. |
| Is a note enough? | No, material advice/actions need structured evidence and linked objects. |
| Can UI hide controls only? | No, APIs and data services must enforce. |

## 4. Source notes and further reading

This pack uses the following sources as reference anchors. They should be treated as conceptual and control references; bank policy and local legal/regulatory interpretation must always prevail.

### Digital identity and authentication

- NIST SP 800-63B, *Digital Identity Guidelines: Authentication and Authenticator Management*. The guidance defines authentication assurance levels and technical requirements for remote user authentication. It describes AAL1, AAL2 and AAL3, including stronger requirements such as two-factor authentication and phishing-resistant authentication for higher assurance contexts.
  - https://pages.nist.gov/800-63-4/sp800-63b.html

### Application security

- OWASP Application Security Verification Standard. OWASP describes ASVS as a basis for testing web application technical security controls and as a list of requirements for secure development.
  - https://owasp.org/www-project-application-security-verification-standard/

### Information security management

- ISO/IEC 27001, *Information security management systems*. Useful as a management-system reference for security governance, control frameworks and continuous improvement.
  - https://www.iso.org/standard/27001

### Singapore data protection

- Personal Data Protection Commission, PDPA Overview. Useful for Singapore personal-data protection obligations and privacy considerations in digital channels, document delivery and interaction records.
  - https://www.pdpc.gov.sg/about/the-legislation/pdpa-overview

### AML/KYC and client authority context

- FATF Recommendations. Useful background for client due diligence, beneficial ownership, AML/CFT controls and risk-based governance.
  - https://www.fatf-gafi.org/en/publications/Fatfrecommendations/Fatf-recommendations.html

### Wealth-platform context

- Use together with related knowledge packs in this repository:
  - client/account/portfolio master data;
  - product governance and APU;
  - model portfolios and suitability;
  - portfolio reporting and client experience;
  - data governance and lineage;
  - trade lifecycle and operations;
  - investment accounting and ledger.

## 5. Suggested future extensions

- Add bank-style UX mock content for advisor workbench screens.
- Add detailed API contract examples for authorization, consent and workflow services.
- Add sequence diagrams for proposal, consent, order and report delivery.
- Add policy decision tables for RM, PM, operations, supervisor, client and delegate personas.
- Add migration guide for legacy CRM notes and portal consent history.
- Add detailed test-data fixtures for entitlement regression.
