<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 06 - Secrets, Identity, Access, Policy and Zero-Trust Security

## 1. Core principle

Never trust a workload simply because it is inside the cluster.

Use identity, policy and telemetry at every layer:

```text
user identity
+ workload identity
+ service identity
+ data entitlement
+ network policy
+ secret access policy
+ API authorization
+ audit log
```

## 2. Identity layers

| Identity | Example |
|---|---|
| Human identity | developer, SRE, support analyst |
| Service identity | portfolio-api service account |
| Workload identity | pod identity mapped to cloud IAM |
| Client identity | authenticated client/advisor |
| Data identity | CIF, BR, account, portfolio entitlement |
| Machine identity | node, cluster, certificate |
| AI agent identity | named tool/action permission boundary |

## 3. Kubernetes RBAC

RBAC controls access to Kubernetes API resources.

Good practice:

- least privilege
- no direct prod admin except break-glass
- namespace-scoped roles where possible
- separate deployer, viewer, operator and admin roles
- service accounts per workload
- avoid sharing service accounts
- audit RBAC changes
- use groups rather than individual bindings

## 4. Application entitlements versus platform RBAC

Do not confuse:

| Control | Scope |
|---|---|
| Kubernetes RBAC | who can manage cluster resources |
| Cloud IAM | who/what can access cloud resources |
| Application entitlements | which client/account/portfolio/product a user can access |
| Data policy | what data fields are visible |
| Workflow authorization | who can approve or act |
| AI tool policy | which actions an agent can call |

A platform engineer may have Kubernetes access but must not automatically have client data access.

## 5. Secrets management

Secrets include:

- database passwords
- API keys
- certificates
- private keys
- JWT signing keys
- encryption keys
- vendor credentials
- model gateway keys
- service credentials

Good practice:

- store secrets in a managed vault
- mount or inject at runtime
- rotate periodically
- avoid environment variables for highly sensitive secrets where possible
- avoid logging secrets
- use short-lived credentials
- restrict secret access by workload identity
- audit secret access
- test rotation

## 6. Policy as code

Use policies to enforce:

- no privileged containers
- no root user
- allowed registries only
- required resource limits
- required labels
- required probes
- no public load balancer unless approved
- no hostPath
- read-only root filesystem where possible
- required network policies
- required annotations for owner/cost/app
- allowed namespaces
- required image digest/signature

Common tools include admission controllers, OPA/Gatekeeper, Kyverno, Azure Policy, OpenShift policies and platform-specific policy engines.

## 7. Network security

| Control | Purpose |
|---|---|
| Network policies | restrict pod-to-pod traffic |
| Private endpoints | avoid public data-service access |
| Egress control | restrict outbound calls |
| mTLS | service identity and encryption |
| WAF | protect internet-facing apps |
| API gateway | auth/rate/policy/audit |
| DNS control | prevent endpoint confusion |
| Firewall | enterprise boundary controls |

## 8. Data protection

For wealth systems:

- encrypt data at rest
- encrypt data in transit
- mask sensitive logs
- tokenize or redact where possible
- apply field-level access controls
- separate prod/non-prod data
- avoid copying client data into unapproved tools
- retain audit and consent evidence
- apply data residency rules

## 9. AI security

AI introduces additional risks:

- prompt injection
- data exfiltration through tool calls
- retrieval of unauthorized documents
- excessive agency
- hallucinated recommendations
- sensitive information disclosure
- model/provider boundary violations
- poisoned knowledge base content

Controls:

- retrieval must respect entitlements
- tools must require explicit permission
- high-risk actions need human approval
- answers should cite approved sources
- prompts/responses should be auditable
- PII should be redacted where appropriate
- model calls should go through approved gateways

## 10. Break-glass access

Break-glass should be:

- rare
- time-bound
- approved
- logged
- reviewed
- automatically revoked
- tied to incident/ticket
- never used for routine deployment

## 11. Security QA scenarios

| Scenario | Expected result |
|---|---|
| Pod tries to run privileged | blocked by policy |
| Service tries to access unauthorized secret | denied |
| User without entitlement requests portfolio | 403 / no data |
| AI agent tries unauthorized tool | blocked and logged |
| Public endpoint created without approval | policy violation |
| Expired certificate | alert before expiry |
| Debug logs include token | test fails |
| Cross-country data access attempted | blocked by data policy |

## 12. Practitioner summary

A strong answer:

> In Kubernetes wealth platforms, security must combine platform RBAC, workload identity, secrets management, network policy, application entitlements, data policy and audit. Zero trust means internal services are authenticated, authorized, encrypted and observed. AI agents require even stronger action-level governance because they can retrieve, summarize and act on sensitive client data.
