# 05 - SaaS, Cloud, AI, Data Vendor and Open-Source Risk Deep Dive

## 1. SaaS risk

SaaS products reduce deployment effort but create dependency on vendor controls.

Assess:

- tenant isolation
- admin access
- SSO/MFA
- audit logs
- data export
- backup and restore
- data residency
- support model
- update control
- availability
- subcontractors
- security certifications
- incident notification
- API rate limits
- license lock-in
- exit feasibility

## 2. Cloud provider risk

Cloud is not risk-free. It uses a shared-responsibility model.

Evaluate:

- account/subscription governance
- identity and access management
- network segmentation
- private connectivity
- encryption and key management
- region and zone strategy
- backup and DR
- logging and monitoring
- cloud posture management
- cost controls
- resilience architecture
- concentration risk
- managed-service dependencies

## 3. AI and GenAI vendor risk

AI vendors introduce special risks:

| Risk | Example |
|---|---|
| Data leakage | Client data appears in model logs or training |
| Hallucination | AI invents portfolio explanation |
| Unsuitable recommendation | AI suggests product outside client target market |
| Prompt injection | Malicious document manipulates answer |
| Excessive agency | Agent sends email or places order without approval |
| Model drift | Output quality changes after model update |
| Unclear lineage | No evidence of why answer was produced |
| Bias | Recommendations systematically skewed |
| Regulatory risk | AI output perceived as regulated advice |
| IP risk | Generated content or training data concerns |

## 4. Data vendor risk

Market data and reference data vendors require special attention:

- data license rights
- redistribution rights
- derived data restrictions
- benchmarking rights
- audit of usage
- data-quality SLAs
- correction process
- delayed versus real-time terms
- source lineage
- holiday calendars
- pricing hierarchy
- stale data handling
- entitlement controls
- contractual penalties

## 5. Open-source risk

Open source is essential, but must be governed.

Controls:

- approved package repositories
- dependency scanning
- license scanning
- SBOM generation
- vulnerability remediation
- maintainer reputation review for critical dependencies
- pinned versions
- reproducible builds
- no unreviewed copy-paste code
- AI-generated dependency review
- transitive dependency visibility
- end-of-life component tracking

## 6. Fintech integration risk

Fintech vendors often move fast but may lack bank-grade controls.

Assess:

- maturity of SDLC
- financial runway
- founder dependency
- production support capacity
- incident history
- security certifications
- regulated client experience
- audit readiness
- integration stability
- data portability
- long-term roadmap

## 7. Wealth-specific high-risk vendors

| Vendor type | Why high risk |
|---|---|
| AI advisor copilot | Advice, suitability, data confidentiality |
| Portfolio analytics engine | Calculation correctness and client reporting |
| Performance engine | Client trust and regulatory reporting |
| Pricing vendor | Valuation and P&L impact |
| Product master vendor | Suitability and product eligibility impact |
| Cloud/SaaS platform | Systemic operational dependency |
| Reporting/PDF vendor | Client-facing data and archive impact |
| KYC/AML vendor | Regulatory and onboarding impact |

## 8. Key takeaway

Different vendor types require different controls. A generic questionnaire is not enough.
