# Advisory, Mandate, Reporting and Client Experience Security

## 1. Security in advisory workflows

Advisory workflows are sensitive because they combine client profile, product knowledge, market view, recommendation, suitability and client consent.

Security controls must protect:

- client profile and risk appetite
- product eligibility and target market
- proposal generation and approval
- suitability evidence
- disclosure documents
- client consent and acknowledgement
- advisor notes and rationale
- AI-assisted explanations

## 2. Advisory action controls

| Action | Security/control requirements |
|---|---|
| View client | Relationship/entitlement check |
| View product | APU and booking-centre eligibility |
| Generate idea | Product governance and client profile filters |
| Draft recommendation | Suitability pre-check and evidence capture |
| Approve proposal | Role-based approval and segregation |
| Send to client | Secure delivery and versioned document |
| Capture consent | Non-repudiation, timestamp, channel evidence |
| Convert to order | Pre-trade checks and idempotent order flow |

## 3. DPM and mandate security

DPM workflows require stronger separation between authority and control.

Controls:

- mandate-level permissions
- permitted instrument universe
- target allocation and drift limits
- concentration and issuer limits
- leverage and derivative limits
- cash/liquidity limits
- restricted securities list
- pre-trade and post-trade compliance
- maker-checker for mandate changes
- audit trail for model changes and rebalancing decisions

## 4. Reporting security

Client reports are high-risk assets because they contain consolidated wealth information.

Controls:

| Report action | Control |
|---|---|
| Generate report | Use certified data snapshot and entitlement check |
| Preview report | Advisor must be authorized for all included portfolios |
| Download report | Watermark, log, expiry, access control |
| Email/share report | Secure channel and recipient validation |
| Archive report | Immutable storage and retention policy |
| Regenerate report | Versioning and difference tracking |
| AI summarize report | Same entitlements and source lineage |

## 5. Client experience security

Client-facing channels need both strong security and usability.

Key features:

- secure login and MFA
- device/session management
- secure messaging
- digital document delivery
- consent capture
- transaction confirmation
- trusted notifications
- self-service preference management
- suspicious activity alerts
- delegation management

## 6. Cross-border and booking-centre controls

Security is not only technical; it includes jurisdictional restrictions.

Examples:

- advisor in one country may not view/advise client booked in another jurisdiction unless permitted
- products may not be distributable in client residence country
- reports may need local disclaimer/disclosure
- client data may not be exported to certain locations
- AI/copilot may need to avoid cross-border recommendation generation

## 7. Client-report misdelivery scenario

Risk:

```text
Client A receives Client B's portfolio statement.
```

Controls:

- report generation binds report to client/account/recipient metadata
- recipient validation before delivery
- watermark with recipient and generated timestamp
- dual control for bulk report distribution
- automated reconciliation of report-to-recipient mapping
- delivery logs and bounce/error monitoring
- incident playbook for misdelivery

## 8. Mandate breach explanation scenario

An AI assistant explains a mandate breach.

Required controls:

- access to mandate rules and certified holdings only
- current data timestamp displayed
- distinction between rule breach, warning and data-quality exception
- no recommendation to trade unless advisory workflow invoked
- source references to mandate rule and position data
- audit event with prompt, sources, output and reviewer

## 9. Security reporting dashboards

Security dashboards for wealth stakeholders should show:

- entitlement exceptions
- privileged access usage
- report downloads by user/client
- failed access attempts
- stale access recertifications
- open security exceptions
- high-risk vulnerabilities
- unresolved reconciliation breaks with security impact
- AI safety blocks and prompt-injection attempts
- cyber incident status

## 10. Security language for stakeholders

Good stakeholder explanation:

> "We separate who can view data, who can recommend, who can approve and who can execute. Every sensitive action is enforced server-side, captured with audit evidence, and checked against client/account scope, product eligibility, suitability and mandate restrictions."
