# Enterprise Security, Cyber Resilience, IAM, Secrets, Threat Modelling and Audit Controls Reference Pack

## Purpose

This reference pack documents enterprise security and cyber-resilience knowledge for wealth-management platforms. It is designed for product architects, engineering leads, business analysts, QA, operations, security teams, platform owners, auditors and AI/product teams working on private-banking, advisory, portfolio analytics, reporting and digital wealth systems.

The pack treats security as a **business capability**, not only a technical control. In a wealth platform, security protects client trust, assets, portfolio data, suitability records, mandate decisions, reports, AI context, operations evidence and regulatory accountability.

## Audience

- Wealth platform architects and engineering leads
- Product owners and business analysts
- Security architects and cyber-risk teams
- DevSecOps, SRE and production support teams
- QA and control testers
- Data, reporting, analytics and AI platform teams
- Advisory, DPM, compliance and operations stakeholders

## Curation Note

Curated from provided source material on 2026-06-27 and normalized for reusable practitioner, product, platform and implementation reference.

## Reading order

| File | Purpose |
|---|---|
| `01-security-fundamentals-cyber-resilience-and-wealth-context.md` | Security principles, risk taxonomy and wealth-platform context. |
| `02-iam-entitlements-zero-trust-and-privileged-access.md` | Identity, authentication, authorization, RBAC/ABAC, PAM and zero trust. |
| `03-data-protection-privacy-encryption-tokenization-and-key-management.md` | Client data, confidentiality, encryption, masking, tokenization and key/secrets control. |
| `04-api-application-cloud-and-kubernetes-security-controls.md` | API, application, cloud, Kubernetes and runtime security patterns. |
| `05-devsecops-secure-sdlc-threat-modelling-and-supply-chain-security.md` | Secure engineering, threat modelling, CI/CD, dependency and software supply-chain controls. |
| `06-detection-logging-siem-soar-fraud-and-anomaly-monitoring.md` | Logging, monitoring, SIEM/SOAR, fraud and anomaly detection. |
| `07-incident-response-cyber-resilience-bcp-dr-and-crisis-management.md` | Incident response, cyber resilience, BCP, DR, ransomware and crisis playbooks. |
| `08-ai-security-genai-rag-agent-and-model-risk-controls.md` | AI/GenAI-specific security and governance controls. |
| `09-advisory-mandate-reporting-and-client-experience-security.md` | Security implications for advisory, mandates, reporting, client channels and operations. |
| `10-security-data-model-evidence-audit-and-control-library.md` | Security data model, evidence, control catalog and audit-ready lineage. |
| `11-platform-implementation-checklists-test-scenarios-and-maturity-roadmap.md` | Implementation checklist, QA scenarios, maturity roadmap and release gates. |
| `12-glossary-practitioner-reference-and-source-notes.md` | Glossary, practitioner questions and answers and source notes. |
| `13-worked-examples-and-implementation-patterns.md` | Practical security examples for entitlement decisions, data protection, API controls, threat modelling, detection, incident response, AI security and audit evidence. |

## Core design principle

Security should be designed as a **control fabric** across:

```text
client identity -> account hierarchy -> entitlements -> product access -> advisory workflow -> trade action -> reporting -> audit evidence
```

For AI-enabled wealth platforms, the security boundary also includes:

```text
approved knowledge -> retrieval context -> prompt/input -> model/tool call -> generated output -> human approval -> audit trail
```

## Recommended repo path

```text
docs/reference/enterprise-security-cyber-resilience-controls/
```

## Disclaimer

This pack is for education, product analysis, architecture, documentation and platform-design work. It is not legal, regulatory, audit, cybersecurity certification or client advice. Always validate implementation against the applicable institution policy, local regulation, legal counsel, risk appetite and control standards.
