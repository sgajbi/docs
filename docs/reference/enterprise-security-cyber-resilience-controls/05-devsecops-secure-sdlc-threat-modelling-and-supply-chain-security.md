# DevSecOps, Secure SDLC, Threat Modelling and Supply-Chain Security

## 1. Secure SDLC objective

Secure SDLC ensures that vulnerabilities, design flaws and control gaps are prevented or detected before they reach production. In a bank-grade wealth platform, security must be embedded from requirements to operations.

## 2. Secure SDLC stages

| Stage | Security activities |
|---|---|
| Idea / epic | Security classification, regulatory impact, data sensitivity |
| Requirements | Abuse cases, access rules, audit evidence, privacy requirements |
| Architecture | Threat model, trust boundaries, control design, ADRs |
| Development | Secure coding, peer review, secrets scanning, dependency hygiene |
| Build | SAST, SCA, container scan, SBOM, image signing |
| Test | Security test cases, API authorization tests, DAST where applicable |
| Release | Risk acceptance, change approval, deployment evidence |
| Operate | Monitoring, vulnerability remediation, incident response |
| Improve | Post-incident learning and control maturity updates |

## 3. Threat modelling

Threat modelling identifies what can go wrong before the system is built or changed.

Common methods:

| Method | Use |
|---|---|
| STRIDE | Spoofing, tampering, repudiation, information disclosure, denial of service, elevation of privilege |
| LINDDUN | Privacy-focused threat modelling |
| Attack trees | Scenario-based attacker goals |
| Misuse cases | Business abuse paths |
| Data-flow diagrams | Trust boundaries and sensitive data movement |

## 4. Wealth platform threat model checklist

For each feature ask:

- Who are the actors?
- What client/account/portfolio data is touched?
- What authority is required?
- What could be tampered with?
- What could be disclosed?
- What action needs non-repudiation?
- What is the worst business impact?
- What happens if price, FX, NAV or collateral data is wrong?
- What happens if a client report is delivered to the wrong person?
- What happens if an AI assistant retrieves unauthorized context?
- What logs and evidence prove correct behavior?

## 5. Example threat model: AI portfolio review assistant

| Threat | Control |
|---|---|
| Advisor asks about unauthorized portfolio | Entitlement-filtered retrieval and API authorization |
| Prompt injection in uploaded document | Document sanitization, prompt-injection detection, instruction hierarchy |
| Model fabricates performance explanation | Source-grounded RAG with citations and calculation lineage |
| Assistant recommends unsuitable product | Suitability engine integration and human approval |
| Sensitive prompt stored in logs | Prompt redaction and retention policy |
| Tool executes trade directly | Tool access limited to draft proposal; order workflow requires approval |

## 6. Supply-chain security

Software supply-chain risk includes dependencies, containers, build tools, CI/CD pipelines, infrastructure modules, third-party services and AI models.

Controls:

- approved dependency repositories
- dependency pinning and lock files
- SCA scanning and vulnerability policy
- SBOM generation
- signed commits and signed artifacts where appropriate
- container image scanning
- base image governance
- provenance and build attestation
- isolated build runners
- restricted CI/CD secrets
- third-party package review
- vendor/product due diligence

## 7. CI/CD security gates

| Gate | Evidence |
|---|---|
| Formatting/linting | Consistent code hygiene |
| Unit tests | Functional correctness |
| Security unit tests | Authorization and validation checks |
| SAST | Static security scan |
| SCA | Dependency vulnerability scan |
| Secret scan | No secrets in repo/build logs |
| IaC scan | Cloud/Kubernetes misconfiguration detection |
| Container scan | Image vulnerability and configuration review |
| SBOM | Software bill of materials |
| Policy-as-code | Deployment meets control baseline |
| Approval gate | High-risk changes reviewed |

## 8. Secure coding patterns for wealth services

- Decimal math for financial values.
- Explicit currency fields on all monetary amounts.
- Idempotency keys for mutation APIs.
- Correlation IDs across services and events.
- Object-level authorization tests for every sensitive endpoint.
- No direct database calls from controllers.
- Strict DTO validation and domain rules.
- No secrets in config files or logs.
- Structured audit events for sensitive actions.
- Safe pagination for large holdings/transactions APIs.
- Maker-checker for operational corrections.

## 9. Vulnerability management lifecycle

| Step | Action |
|---|---|
| Discover | Scanner, vendor advisory, threat intel, pentest |
| Triage | Severity, exploitability, asset criticality, compensating controls |
| Assign | Control owner and remediation team |
| Fix | Patch, config change, dependency update or control workaround |
| Verify | Retest and evidence capture |
| Close | Risk acceptance or closure record |
| Learn | Root cause and preventive action |

## 10. Security exception management

Exceptions are sometimes needed, but they must be controlled:

- documented risk and business reason
- expiry date
- compensating controls
- accountable risk owner
- approval authority
- review cadence
- closure plan
- evidence in audit repository

## 11. Common DevSecOps anti-patterns

| Anti-pattern | Risk |
|---|---|
| Security added only before release | Late defects and rushed exceptions |
| SAST/SCA results ignored | Known vulnerabilities ship to production |
| Long-lived CI secrets | Build pipeline compromise |
| Manual deployment outside pipeline | No reproducibility or audit lineage |
| No test for object-level authorization | Horizontal data leakage |
| No threat model for AI features | Prompt/data/tool risks missed |
| No SBOM | Unknown dependency exposure |
