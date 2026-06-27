# API, Application, Cloud and Kubernetes Security Controls

## 1. API security in wealth platforms

APIs expose high-value capabilities: positions, transactions, performance, reports, orders, proposals, client data, entitlements, product master, pricing and AI tools. API security must be built into every layer.

## 2. API control checklist

| Control | Requirement |
|---|---|
| Authentication | OAuth2/OIDC, mTLS for service-to-service where appropriate |
| Authorization | Enforce user, client, account, portfolio and action scope server-side |
| Input validation | Strict schemas, type validation, business validation |
| Output filtering | Return only fields allowed by entitlement and purpose |
| Rate limiting | Protect against abuse, scraping and accidental overload |
| Idempotency | Required for order, payment and mutation APIs |
| Replay protection | Token expiry, nonce where needed, request signing for sensitive flows |
| Audit logging | Actor, action, resource, decision, correlation ID |
| Error handling | No sensitive internals; use consistent problem details |
| Versioning | Controlled breaking changes and deprecation windows |
| Pagination | Avoid bulk exfiltration and performance issues |
| Download protection | Statement/report exports require authorization, watermarking and logging |

## 3. Application security controls

| Area | Examples |
|---|---|
| Injection prevention | Parameterized queries, input validation, output encoding |
| Session management | Secure cookies, token expiry, session revocation |
| Business logic | Server-side checks for suitability, mandate, approvals |
| Authorization | Object-level and field-level controls |
| File handling | Malware scan, content-type validation, size limits |
| Deserialization | Avoid unsafe deserialization; validate schemas |
| Error handling | No stack traces to users; structured diagnostics internally |
| Auditability | Immutable action logs for sensitive workflows |
| Secure defaults | Deny by default, least privilege, safe config |

## 4. Cloud security domains

| Domain | Control focus |
|---|---|
| Landing zone | Account/subscription structure, policies, network segmentation |
| Identity | Federated identity, workload identity, privileged role management |
| Network | Private endpoints, egress control, firewalls, service mesh |
| Compute | Hardened nodes, image scanning, patching, runtime security |
| Storage | Encryption, private access, backups, retention |
| Database | Network isolation, encryption, least privilege, audit logging |
| Secrets | Vault/KMS integration, no static secrets in config |
| Monitoring | Central logs, metrics, security alerts, drift detection |
| Policy | Infrastructure-as-code and policy-as-code gates |
| Cost/resilience | Scaling, quotas, capacity and DR design |

## 5. Kubernetes security controls

| Layer | Controls |
|---|---|
| Cluster | Private cluster where possible, secure API server, RBAC, audit logs |
| Namespace | Isolation by environment/domain/team, resource quotas |
| Workload | Non-root containers, read-only filesystems, dropped Linux capabilities |
| Network | Network policies, service mesh, mTLS, egress restrictions |
| Image | Signed images, vulnerability scanning, minimal base images |
| Secrets | External secret store, no secrets in manifests, rotation |
| Policy | Admission control, OPA/Gatekeeper/Kyverno policies |
| Runtime | Runtime threat detection, pod security standards |
| Observability | Logs, metrics, traces, security events |
| Recovery | Backup manifests, stateful data strategy, restore tests |

## 6. API gateway and service mesh

The API gateway typically handles north-south traffic:

- authentication delegation
- rate limiting
- request validation
- routing
- WAF integration
- API analytics
- threat detection

The service mesh or internal platform controls east-west traffic:

- mTLS
- service identity
- traffic policy
- retries/timeouts
- telemetry
- service authorization

## 7. Wealth-specific API examples

| API | Key security controls |
|---|---|
| `/positions` | Client/account/portfolio scope, pagination, stale-data label |
| `/transactions` | Date range limits, pagination, export controls |
| `/portfolio-review` | Report entitlement, watermarking, generated snapshot lineage |
| `/proposal` | Suitability and product eligibility checks |
| `/orders` | Idempotency, dual approval, non-repudiation |
| `/credit-lines` | Borrowing authority and collateral sensitivity |
| `/ai/chat` | RAG entitlement filtering, prompt injection checks, tool scope |
| `/admin/reprice` | Privileged access and maker-checker |

## 8. Secure error handling

Error responses should not leak implementation details.

Good pattern:

```json
{
  "type": "https://docs.example.com/errors/access-denied",
  "title": "Access denied",
  "status": 403,
  "detail": "You are not permitted to access this portfolio.",
  "correlation_id": "corr-123"
}
```

Avoid:

```text
SQL error in portfolio_positions table for client_id=123...
```

## 9. Data exfiltration controls

Bulk data APIs and report exports need extra control:

- maximum page sizes
- date-range limits
- throttling
- export approval for restricted reports
- data-loss prevention checks
- watermark and recipient binding
- anomaly detection for unusual downloads
- purpose code for sensitive access

## 10. Application security evidence

For every wealth platform service, maintain evidence of:

- threat model
- API contract and security scheme
- authentication and authorization tests
- dependency scan
- container scan
- secrets scan
- static analysis
- dynamic/API security test
- penetration test where applicable
- production access review
- incident and logging configuration
- backup and restore validation
