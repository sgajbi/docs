# Platform Implementation Checklists, Test Scenarios and Maturity Roadmap

## 1. Implementation checklist

### Governance

- Security policy mapped to platform domains.
- Control owners assigned.
- Risk appetite and exception process documented.
- System criticality and data classification recorded.
- Security requirements included in epics/features.
- Architecture decisions capture security impact.

### IAM and entitlements

- SSO and MFA enforced.
- User roles mapped to business capabilities.
- Client/account/portfolio-level authorization enforced server-side.
- Privileged access is just-in-time and audited.
- Service/workload identities are managed and rotated.
- Access recertification process exists.

### Data protection

- Data classification implemented.
- Encryption at rest and in transit enabled.
- Secrets stored in vault/KMS.
- Production data masked in lower environments.
- Sensitive fields redacted from logs.
- Report delivery is secure and audited.

### API/application

- API schema validation enabled.
- Object-level authorization tested.
- Rate limiting and pagination controls implemented.
- Idempotency for mutations.
- Structured error handling without sensitive leakage.
- Audit events captured for sensitive actions.

### Cloud/Kubernetes

- Private networking and network policies configured.
- Image scanning and signing policy implemented.
- Pods run as non-root with least privilege.
- Secrets not stored in manifests.
- Admission control/policy-as-code enabled.
- Observability and audit logging configured.

### AI

- RAG sources approved and classified.
- Vector retrieval entitlement-aware.
- Prompt injection controls tested.
- Tool access limited and audited.
- Human approval for recommendations/actions.
- Model/prompt/template versions recorded.

### Resilience

- Critical business services mapped.
- RTO/RPO defined.
- Backups encrypted and isolated.
- Restore tests performed.
- Incident playbooks documented and exercised.
- Post-incident review process defined.

## 2. Security QA scenarios

### Authorization

| Scenario | Expected result |
|---|---|
| Advisor accesses assigned client portfolio | Allow |
| Advisor accesses unassigned client portfolio | Deny |
| Assistant exports report without export permission | Deny |
| PM edits mandate without approval role | Deny |
| Compliance views exception evidence | Allow if scoped |

### Data leakage

| Scenario | Expected result |
|---|---|
| API error occurs | No stack trace or sensitive data in response |
| Report generated | Report contains only entitled accounts |
| Lower environment uses production-like data | Data is masked/tokenized |
| Logs inspected | No secrets or full PII |

### AI security

| Scenario | Expected result |
|---|---|
| Prompt asks model to ignore policy | Block or safe refusal |
| User asks about unauthorized client | Deny / no retrieval |
| Uploaded document contains malicious instruction | Instruction ignored and event logged |
| AI drafts trade recommendation | Human approval required |
| AI answer cites unavailable source | Fails grounding check |

### Resilience

| Scenario | Expected result |
|---|---|
| Market data feed delayed | Stale-data flag and degraded reporting behavior |
| Reporting database unavailable | Failover or use last certified snapshot with disclosure |
| Secret compromised | Secret revoked/rotated and services redeployed |
| Region outage | DR procedure executed and evidence captured |
| Ransomware exercise | Backup restore validated |

## 3. Release gates

| Gate | Required evidence |
|---|---|
| Design gate | Threat model, data classification, security ADR |
| Build gate | SAST/SCA/secrets/container scan clean or accepted |
| Test gate | Authorization, API, negative and abuse tests passed |
| AI gate | Grounding, entitlement, prompt-injection and tool-action tests passed |
| Ops gate | Runbook, alerts, dashboards and rollback plan ready |
| Risk gate | Exceptions approved and residual risks documented |
| Go-live gate | Production access, monitoring, DR and support readiness confirmed |

## 4. Maturity roadmap

### Level 1: Basic

- SSO/MFA enabled.
- Basic API authentication.
- Manual access reviews.
- Basic vulnerability scanning.
- Logs available but limited correlation.

### Level 2: Defined

- Documented security controls.
- Standard authorization model.
- Threat modelling for high-risk features.
- Secure SDLC gates.
- Centralized logging and alerting.
- Data classification and masking policies.

### Level 3: Managed

- Policy-as-code and automated evidence.
- Object-level authorization tests in CI.
- PAM and JIT privileged access.
- SIEM correlation rules.
- Tested incident playbooks and DR.
- AI security controls and evaluations.

### Level 4: Optimized

- Continuous control monitoring.
- Runtime security and anomaly detection.
- Automated access certification workflows.
- Resilience chaos/cyber exercises.
- AI red-teaming and continuous evals.
- Security metrics used in product governance.

## 5. Security backlog themes

- Entitlement engine consolidation.
- Access decision audit events.
- Report delivery and watermarking controls.
- RAG source entitlement metadata.
- Secrets rotation automation.
- Policy-as-code for Kubernetes and cloud.
- Secure-by-default API gateway templates.
- Incident response tabletop exercises.
- Product-specific data leakage tests.
- Security evidence dashboard.

## 6. Practitioner framing

> "For a wealth platform, security must be designed across identity, entitlement, product governance, advisory workflows, transaction authority, client reporting, data lineage, AI retrieval and operational resilience. I would not treat it as only infrastructure hardening. I would define controls at the business capability level, enforce them server-side, capture audit evidence, automate security testing in CI/CD and build resilience so the platform can continue and recover safely even during cyber events."
