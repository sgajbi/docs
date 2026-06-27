# Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| ABAC | Attribute-based access control; policies evaluate actor, resource, action and context attributes. |
| RBAC | Role-based access control; permissions are assigned through roles. |
| PAM | Privileged access management; controls admin/high-risk access. |
| MFA | Multi-factor authentication. |
| Zero trust | Security model that continuously verifies identity, device, resource, action and context. |
| Least privilege | Users/services receive only the access needed for their role/action. |
| Data classification | Categorizing data sensitivity to drive controls. |
| Tokenization | Replacing sensitive data with controlled tokens. |
| KMS | Key management service for cryptographic keys. |
| HSM | Hardware security module for protected key operations. |
| SIEM | Security information and event management. |
| SOAR | Security orchestration, automation and response. |
| EDR | Endpoint detection and response. |
| DLP | Data loss prevention. |
| WAF | Web application firewall. |
| SAST | Static application security testing. |
| DAST | Dynamic application security testing. |
| SCA | Software composition analysis. |
| SBOM | Software bill of materials. |
| Threat modelling | Structured analysis of threats and mitigations during design. |
| RTO | Recovery time objective. |
| RPO | Recovery point objective. |
| RAG | Retrieval-augmented generation. |
| Prompt injection | Attack that tries to override or manipulate AI instructions. |
| Excessive agency | AI/agent has more autonomy/tool power than safe or intended. |
| Control evidence | Artifact proving that a control exists and operated effectively. |

## 2. Practitioner answer: cybersecurity in wealth platforms

> "Cybersecurity in wealth management is not just about infrastructure. It protects client confidentiality, transaction authority, advisory evidence, product suitability, mandate controls, reporting accuracy and operational resilience. I would design security across identity, entitlements, data protection, API controls, secure SDLC, detection, incident response and audit evidence. For AI-enabled platforms, I would also enforce retrieval entitlements, prompt-injection controls, human approval for actions and full lineage of prompt, source, model and output."

## 3. Practitioner answer: zero trust

> "In a private-banking platform, zero trust means we do not trust a request just because it comes from an internal network. We verify the user, service, device, resource, client/account scope, action and context. The entitlement decision must be server-side and auditable. This is especially important where advisors, portfolio managers, operations users, APIs and AI copilots interact with client portfolios and reports."

## 4. Practitioner answer: AI security

> "AI security requires controlling both data access and action authority. A GenAI assistant should only retrieve sources the user is entitled to access, should cite approved knowledge, should not bypass suitability or mandate checks, and should not execute actions without a controlled workflow. Every AI response should have lineage: user, prompt template, retrieved sources, model version, output checks, tool calls and human approval where required."

## 5. Practitioner checklist

| Question | Good answer |
|---|---|
| Who can see client data? | Only entitled users/services under client/account/portfolio scope. |
| Who can recommend? | Advisor/PM roles under product governance and suitability constraints. |
| Who can approve? | Separate approval role with evidence and policy decision. |
| Who can execute? | OMS/order workflow only after required checks. |
| Who can export? | Explicit export permission, watermarking and audit. |
| Who can admin? | JIT privileged access with approval and monitoring. |
| Can AI access data? | Only through entitlement-aware tools/retrieval. |
| Can AI act? | Draft/assist by default; high-risk action requires human approval. |

## 6. Source notes and further reading

Use these sources to validate and enrich implementation decisions:

- NIST Cybersecurity Framework 2.0 Resource Center: https://www.nist.gov/cyberframework
- NIST SP 800-53 Rev. 5, Security and Privacy Controls: https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final
- NIST SP 800-61 Rev. 3, Incident Response Recommendations and Considerations: https://csrc.nist.gov/pubs/sp/800/61/r3/final
- NIST SP 800-63B Digital Identity Guidelines: https://pages.nist.gov/800-63-4/sp800-63b.html
- NIST SP 800-218 Secure Software Development Framework: https://csrc.nist.gov/pubs/sp/800/218/final
- NIST SP 800-30 Rev. 1 Guide for Conducting Risk Assessments: https://csrc.nist.gov/pubs/sp/800/30/r1/final
- OWASP Application Security Verification Standard: https://owasp.org/www-project-application-security-verification-standard/
- OWASP Top 10 Web Application Security Risks: https://owasp.org/www-project-top-ten/
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- CIS Critical Security Controls v8.1: https://www.cisecurity.org/controls/cis-controls-list
- CISA Zero Trust Maturity Model: https://www.cisa.gov/zero-trust-maturity-model

## 7. Suggested repo path

```text
docs/reference/enterprise-security-cyber-resilience-controls/
```
