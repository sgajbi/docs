# DevSecOps, CI/CD, Secure Supply Chain and Release Engineering

## 1. Executive summary

DevSecOps integrates security, quality and operations into the normal engineering flow. For wealth platforms, DevSecOps is not only about scanning code. It is about ensuring that every change to product, data, calculation, reporting, advisory or operational capability is:

- reviewed
- tested
- scanned
- traceable
- reproducible
- deployable
- observable
- reversible
- supportable

## 2. DevSecOps lifecycle

```text
Plan
  -> code
  -> review
  -> build
  -> test
  -> scan
  -> package
  -> sign
  -> deploy
  -> observe
  -> learn
```

Security and quality controls should occur throughout this flow, not only at the end.

## 3. CI/CD pipeline stages

| Stage | Purpose | Example controls |
|---|---|---|
| Checkout | Retrieve controlled source | branch protection, signed commits |
| Static checks | Validate style and maintainability | lint, formatting, type checks |
| Unit tests | Verify local logic | coverage threshold |
| Domain tests | Verify product/calculation rules | golden examples |
| Contract tests | Protect APIs/events | OpenAPI/AsyncAPI compatibility |
| Integration tests | Validate service interaction | containerized dependencies |
| Security scans | Identify vulnerabilities | SAST, SCA, secrets, IaC scan |
| Build | Create artifact | reproducible build |
| SBOM/provenance | Record artifact ingredients | SBOM, build attestation |
| Package/sign | Secure artifact | image signing, digest pinning |
| Deploy to lower env | Validate runtime config | smoke tests |
| Release gate | Approve risk and evidence | automated + human approval |
| Production deploy | Controlled release | blue-green/canary/rolling |
| Post-deploy verification | Confirm health | synthetic checks, dashboards |

## 4. Secure software development practices

Secure development should include:

- secure design review
- threat modelling
- input validation
- authentication/authorization checks
- secrets management
- dependency management
- secure logging
- data protection
- vulnerability management
- secure build and release
- incident response readiness

## 5. Branch and PR governance

Recommended controls:

| Control | Purpose |
|---|---|
| Protected main branch | Prevent unreviewed changes. |
| Required PR review | Ensure peer accountability. |
| Required CI status checks | Prevent broken code merging. |
| CODEOWNERS | Route domain-sensitive files to owners. |
| Small PRs | Improve review quality and reduce risk. |
| Signed commits/tags | Improve provenance. |
| No direct production hotfix without record | Preserve auditability. |
| PR template | Force risk, test, migration and documentation thinking. |

PR template example:

```markdown
## Business outcome

## Product/domain impact

## Data model / API / event impact

## Tests added

## Security/privacy impact

## Operational impact

## Rollback plan

## Evidence links
```

## 6. Secure supply chain

A secure supply chain answers:

| Question | Why it matters |
|---|---|
| What source produced this artifact? | Prevents unknown code from entering production. |
| Who approved the change? | Establishes accountability. |
| What dependencies are included? | Supports vulnerability response. |
| How was the artifact built? | Supports reproducibility and provenance. |
| Was the artifact modified after build? | Prevents tampering. |
| What environment received the artifact? | Supports deployment audit. |

## 7. SBOM and dependency control

A Software Bill of Materials helps identify what components are present in an application or image.

Minimum dependency controls:

- lockfile required
- dependency updates reviewed
- vulnerable dependencies triaged
- transitive dependencies monitored
- license policy checked
- no unapproved packages
- SBOM generated for production artifact
- dependency exceptions time-bound

## 8. Container and Kubernetes controls

For containerized wealth apps:

| Control | Purpose |
|---|---|
| Minimal base image | Reduce attack surface. |
| Non-root runtime | Reduce privilege risk. |
| Image scanning | Detect CVEs. |
| Image signing | Validate artifact identity. |
| Resource requests/limits | Prevent noisy-neighbour issues. |
| Health probes | Support safe rollout. |
| Network policies | Restrict lateral movement. |
| Secret mounts | Avoid hardcoded secrets. |
| Immutable config | Improve reproducibility. |
| Deployment strategy | Support rollback/canary. |

## 9. Release engineering

Release engineering is the discipline of making deployments predictable.

Release package should include:

- release scope
- changed services
- API/event contract changes
- database migrations
- feature flags
- configuration changes
- dependencies
- test evidence
- vulnerability status
- known issues
- rollback plan
- post-deploy checks
- communications
- support contacts

## 10. Feature flags

Feature flags support:

- progressive rollout
- dark launch
- country-specific activation
- booking-centre activation
- client-segment activation
- controlled rollback
- A/B experimentation where permitted

Controls:

- flags must have owners
- flags must have expiry dates
- production flag changes must be audited
- flags must be included in test matrix
- stale flags must be removed

## 11. Database migration practices

Migration rules:

| Rule | Reason |
|---|---|
| Backward-compatible first | Allows rolling deploy. |
| Expand-migrate-contract | Safer schema evolution. |
| Idempotent scripts | Avoid partial failure issues. |
| Reversible or compensating plan | Supports rollback. |
| Data checks before/after | Confirms correctness. |
| Performance tested | Avoid production slowdown. |
| Migration owner defined | Supports incident response. |

## 12. API and event contract gates

CI should validate:

- OpenAPI schema compatibility
- AsyncAPI / event schema compatibility
- required fields
- enum changes
- backward compatibility
- pagination semantics
- error semantics
- idempotency requirements
- correlation ID propagation
- schema versioning

## 13. AI-generated code controls

For AI-assisted engineering:

- AI output must go through PR review
- tests are mandatory
- generated code must not bypass architecture boundaries
- security scans must run
- generated dependencies must be justified
- generated docs must be checked for false claims
- sensitive data must not be pasted into prompts
- coding agents should operate on feature branches
- commit history should remain reviewable

## 14. DevSecOps anti-patterns

| Anti-pattern | Why it is risky |
|---|---|
| Security scan only at release | Late discovery creates pressure to accept risk. |
| One giant release branch | Hard to review and rollback. |
| Manual deployment steps | Error-prone and hard to audit. |
| "It passed locally" | Not reproducible. |
| No artifact provenance | Cannot prove what was deployed. |
| No feature flag cleanup | Complexity accumulates. |
| No dependency policy | Vulnerability response becomes chaotic. |
| AI-generated code merged blindly | Hidden logic and security errors enter production. |

## 15. Practitioner checklist

Before production release:

- [ ] PRs reviewed and linked to stories
- [ ] CI green
- [ ] tests and regression evidence complete
- [ ] contract compatibility checked
- [ ] security scans triaged
- [ ] SBOM/provenance generated where required
- [ ] database migration validated
- [ ] feature flags documented
- [ ] deployment plan approved
- [ ] rollback plan ready
- [ ] monitoring dashboards updated
- [ ] runbook updated
- [ ] support team briefed
- [ ] release notes published
