# Security Fundamentals, Cyber Resilience and Wealth Platform Context

## 1. Security in wealth management

In wealth management, cybersecurity is not only about preventing unauthorized system access. It protects:

- client assets and transaction authority
- personally identifiable information and financial data
- portfolio holdings, valuations, performance and risk analytics
- suitability, mandate and advisory evidence
- product governance and approved product universe controls
- confidential reports, tax documents and statements
- digital consent, client interaction history and audit trails
- AI context, prompts, retrieval results and generated recommendations
- production availability for trading, reporting, valuations and operations

A wealth platform must be built on **confidentiality, integrity, availability, authenticity, accountability, non-repudiation and recoverability**.

## 2. Security objectives by capability

| Capability | Primary security concern | Example failure |
|---|---|---|
| Client onboarding | Identity proofing, KYC, AML and privacy | Wrong tax residency or client profile visible to unauthorized user |
| Entitlements | Access scoping and delegation | Advisor sees clients outside assigned book |
| Advisory | Suitability evidence and approval trail | Recommendation generated without required disclosure or profile check |
| DPM / mandate management | Authority, restrictions and segregation | Portfolio manager trades outside mandate limit |
| Order management | Authorization, non-repudiation and fraud control | Unauthorized trade or changed order instruction |
| Reporting | Data confidentiality and report correctness | Client receives another client's statement |
| Analytics | Data integrity and calculation lineage | Tampered price or holdings cause wrong performance |
| Lending / collateral | Pledge authority and exposure control | Buying power calculated using ineligible collateral |
| AI copilot | Prompt/data leakage and excessive agency | Model reveals restricted client data or triggers an action without approval |
| Operations | Incident response and audit evidence | Break cannot be traced back to source and decision history |

## 3. Cyber-risk taxonomy

| Risk type | Meaning | Wealth-platform example |
|---|---|---|
| Confidentiality breach | Data seen by unauthorized party | Client holdings leaked through report or AI context |
| Integrity breach | Data changed incorrectly or maliciously | Price, FX rate, mandate flag or beneficiary changed |
| Availability outage | Service unavailable | Portfolio review, order capture or valuation service down |
| Authentication failure | User identity not strongly verified | Stolen advisor credentials used to view accounts |
| Authorization failure | User can do more than allowed | Assistant can approve proposal despite view-only role |
| Privilege abuse | Excessive/admin access misused | Production database exports taken by support user |
| Supply-chain compromise | Dependency, package, image or vendor compromised | Malicious package in portfolio calculator image |
| Data exfiltration | Sensitive data extracted | Bulk client export from reporting API |
| Model/prompt compromise | AI system manipulated | Prompt injection causes RAG system to ignore policy |
| Operational resilience failure | Organization cannot recover quickly | Ransomware affects valuation and statements beyond RTO |

## 4. Security control layers

A robust wealth platform uses layered controls:

| Layer | Controls |
|---|---|
| Governance | Security policy, risk appetite, control owners, exceptions, audit evidence |
| Identity | MFA, SSO, device trust, privileged access, service identity |
| Entitlements | RBAC, ABAC, portfolio/account scoping, purpose-based access |
| Data | Classification, encryption, masking, tokenization, retention, DLP |
| Application | Secure coding, input validation, output encoding, session control, audit trail |
| API | OAuth/OIDC, scopes, rate limits, schema validation, idempotency, threat protection |
| Platform | Network segmentation, Kubernetes policy, secrets, image scanning, runtime protection |
| Observability | Security logs, SIEM, alerts, anomaly detection, immutable evidence |
| Resilience | Backup, restore, DR, incident response, crisis comms, cyber recovery |
| AI | RAG grounding, prompt-injection defense, tool permissions, output checks, human-in-loop |

## 5. Cyber resilience versus cybersecurity

Cybersecurity focuses on protection and prevention. Cyber resilience adds the ability to **continue, contain, recover and prove**.

| Dimension | Cybersecurity | Cyber resilience |
|---|---|---|
| Goal | Prevent compromise | Continue and recover despite compromise |
| Focus | Controls and prevention | End-to-end business continuity and recovery |
| Evidence | Policies, scans, tests | RTO/RPO proof, restore evidence, incident exercises |
| Example | MFA and WAF | Isolated backups and tested recovery of portfolio/reporting data |

## 6. Wealth-specific resilience priorities

For private banking and wealth management, priority systems include:

1. client/account master and entitlements
2. cash, positions and transactions book-of-record
3. pricing, FX and valuation feeds
4. advisory and order workflows
5. collateral, exposure and buying power
6. client reporting and statement generation
7. performance, risk and analytics services
8. AI/RAG knowledge and approval systems
9. audit/evidence store
10. operations dashboards and reconciliation tools

## 7. Security design principles

- Assume breach.
- Least privilege by default.
- Deny by default; explicitly allow.
- Separate read, recommend, approve and execute permissions.
- Treat client/account scope as a first-class authorization dimension.
- Make every sensitive action auditable.
- Never rely only on UI controls; enforce on API/service side.
- Separate product eligibility from recommendation suitability from order authorization.
- Keep secrets out of code, logs, prompts and documentation.
- Build recovery evidence, not just recovery design.
- Treat AI as a privileged assistant that must be scoped, monitored and governed.

## 8. Security as architecture quality

Security must appear in architecture decisions, not only test reports. Each major capability should document:

- data classification
- actors and access paths
- trust boundaries
- threat model
- authentication and authorization rules
- secrets and key handling
- audit events
- logging and retention
- exception handling
- operational recovery
- residual risks
- control ownership
- validation evidence

## 9. Practitioner mental model

A secure wealth platform answers six questions consistently:

```text
Who is acting?
On whose behalf?
On which client/account/portfolio?
For what purpose?
Using which approved capability?
With what audit evidence and recovery path?
```
