# 03 - Due Diligence, Security, Privacy, Resilience and Control Evidence

## 1. Due diligence purpose

Due diligence determines whether a vendor can safely support the bank's activity.

It should answer:

- What risks does the vendor introduce?
- Are controls designed appropriately?
- Are controls operating effectively?
- Are gaps acceptable, remediable or blocking?
- Who owns residual risk?
- What evidence supports the decision?

## 2. Due diligence domains

| Domain | Evidence |
|---|---|
| Corporate and financial | Incorporation, ownership, financial stability, insurance |
| Security | IAM, encryption, SDLC, pen test, vulnerability management |
| Privacy | DPA, data processing, retention, deletion, cross-border transfer |
| Resilience | BCP, DR, RTO/RPO, incident process, test evidence |
| Technology | Architecture, APIs, hosting, dependency map |
| Operations | Support model, SLAs, monitoring, escalation |
| Compliance | Certifications, policies, audit reports |
| Legal | Contract terms, liability, IP, confidentiality |
| AI/model risk | Model cards, evaluations, guardrails, human oversight |
| Subcontractors | Fourth-party list, controls, notification rights |
| Exit | Data portability, deletion, transition assistance |

## 3. Security due diligence

Assess:

- SSO and MFA support
- RBAC / ABAC / least privilege
- privileged access management
- encryption in transit and at rest
- key ownership and rotation
- tenant isolation
- secure SDLC
- SAST / DAST / dependency scanning
- SBOM availability
- vulnerability remediation SLA
- penetration test scope and findings
- logging and auditability
- incident detection
- backup security
- secrets management
- secure admin access
- endpoint and infrastructure hardening

## 4. Privacy due diligence

Assess:

- personal data processed
- sensitive data processed
- client identifiers and portfolio data
- purpose limitation
- consent basis or contractual basis
- data residency
- cross-border transfers
- subprocessors
- retention periods
- deletion process
- data subject request support
- breach notification process
- anonymization or pseudonymization
- training data controls for AI systems

## 5. Operational resilience

Assess:

| Control | Question |
|---|---|
| Availability | What uptime is committed? |
| RTO | How quickly can service be restored? |
| RPO | How much data loss is tolerable? |
| DR testing | Is it regularly tested? |
| Incident response | Who responds, when and how? |
| Major incident communication | How quickly is the bank notified? |
| Capacity | Can it handle volume spikes? |
| Change control | Are changes communicated and tested? |
| Concentration | Are critical dependencies concentrated? |
| Exit | Can the bank continue service if vendor fails? |

## 6. Evidence quality

Good evidence is:

- recent
- specific to the service
- independently validated where possible
- linked to control requirements
- not only marketing material
- traceable to responsible owner
- reviewable by bank audit / risk teams

Weak evidence includes:

- generic security brochures
- expired certifications
- incomplete architecture diagrams
- no production incident examples
- no subcontractor transparency
- no DR test report
- no vulnerability remediation timeline
- "trust us" answers without artifacts

## 7. Due diligence outcomes

| Outcome | Meaning |
|---|---|
| Approved | Controls adequate |
| Approved with conditions | Use allowed with remediation plan |
| Restricted approval | Limited users/data/countries/use cases |
| Pilot only | No production / no sensitive data |
| Rejected | Risk unacceptable |
| Deferred | Information incomplete |

## 8. Wealth platform examples

| Vendor type | Key due diligence focus |
|---|---|
| Market data provider | Data rights, pricing quality, SLA, redistribution |
| AI copilot provider | Data privacy, hallucination control, prompt injection, audit |
| PDF/report vendor | Data confidentiality, document archive, delivery controls |
| Portfolio analytics SaaS | Calculation methodology, data access, model governance |
| Cloud provider | resilience, data residency, IAM, shared-responsibility model |
| Custody/execution connector | settlement, reconciliation, uptime, transaction integrity |

## 9. Key takeaway

Due diligence should produce a defensible risk decision, not merely a completed questionnaire.
