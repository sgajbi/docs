# 10 - Glossary, practitioner reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| Vendor | External supplier of product or service |
| Third party | External organization supporting business activity |
| Fourth party | Vendor's subcontractor |
| Outsourcing | Third party performs activity that institution could perform itself |
| Critical service | Service whose failure materially affects business, clients or compliance |
| Due diligence | Risk assessment and evidence review before approval |
| Residual risk | Risk remaining after controls |
| SLA | Service-level agreement |
| RTO | Recovery time objective |
| RPO | Recovery point objective |
| DPA | Data processing agreement |
| Exit plan | Plan to terminate and transition service safely |
| Subprocessor | Third party that processes data for vendor |
| Shadow IT | Unapproved technology use |
| Concentration risk | Excessive dependency on one provider |
| Audit rights | Contractual right to review controls and evidence |
| Data residency | Where data is stored or processed |
| Bank-buyable | Mature enough for regulated financial institution adoption |

## 2. practitioner-ready explanation

Vendor management in a bank is not just procurement. It is a full lifecycle control framework for selecting, approving, contracting, onboarding, monitoring and exiting third-party relationships. For wealth platforms, vendor risk directly affects client data, portfolio valuations, advisory workflows, suitability, reporting, DPM mandates and operational resilience. A strong framework needs risk tiering, due diligence, security and privacy review, contractual protections, SLA monitoring, subcontractor oversight, audit evidence, exit planning and ongoing control testing. For AI and SaaS vendors, additional controls are required for data retention, model governance, prompt injection, explainability, human oversight and tool-action restrictions.

## 3. Practical architect answer

As an architect, I would ensure every third-party integration has:

- clear business owner
- clear technology owner
- data classification
- integration architecture
- access model
- security controls
- API/event contract
- monitoring and alerting
- SLA and support model
- resilience plan
- fallback/exit approach
- audit logging
- source ownership
- reconciliation controls
- regulatory and privacy approval

## 4. wealth-platform suite / startup answer

For a startup selling wealth software to banks, technical functionality is necessary but not sufficient. The product must be supported by enterprise evidence: security documentation, architecture, SDLC, test evidence, support model, incident process, data protection, audit logs, release controls, DR, privacy, contractual readiness and exit support. AI capabilities require additional governance around data use, model changes, retrieval sources, evaluation and human approval.

## 5. Source notes and further reading

Key references used for this pack:

1. Monetary Authority of Singapore - Guidelines on Outsourcing.
   - https://www.mas.gov.sg/regulation/guidelines/guidelines-on-outsourcing

2. OCC / Federal Reserve / FDIC - Interagency Guidance on Third-Party Relationships: Risk Management, OCC Bulletin 2023-17.
   - https://www.occ.gov/news-issuances/bulletins/2023/bulletin-2023-17.html

3. European Banking Authority - Guidelines on Outsourcing Arrangements.
   - https://www.eba.europa.eu/regulation-and-policy/internal-governance/guidelines-on-outsourcing-arrangements

4. EU Digital Operational Resilience Act - Regulation (EU) 2022/2554.
   - https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32022R2554

5. NIST Cybersecurity Framework 2.0.
   - https://www.nist.gov/cyberframework

6. NIST Secure Software Development Framework, SP 800-218.
   - https://csrc.nist.gov/pubs/sp/800/218/final

7. ISO/IEC 27001 information security management.
   - https://www.iso.org/standard/27001

8. SOC 2 / AICPA trust services criteria.
   - https://www.aicpa-cima.com/topic/audit-assurance/service-organization-controls-soc-suite-of-services

9. OWASP Application Security Verification Standard.
   - https://owasp.org/www-project-application-security-verification-standard/

10. OWASP Top 10 for Large Language Model Applications.
   - https://owasp.org/www-project-top-10-for-large-language-model-applications/

## 6. Disclaimer

This document is for education, product strategy, architecture and documentation work only. It is not legal, regulatory, procurement, risk, compliance or outsourcing advice.
