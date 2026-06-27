# 04 - Contracting, SLAs, Exit Plans, Data Rights and Audit Clauses

## 1. Why contracts matter

The contract converts vendor promises into enforceable obligations.

For bank-buyable software, the contract must cover more than price and licenses. It must define:

- services and responsibilities
- security obligations
- data rights
- privacy obligations
- audit rights
- SLA commitments
- incident notification
- regulatory cooperation
- subcontractor control
- business continuity
- exit support
- liability and indemnity
- intellectual property
- confidentiality
- termination rights

## 2. Core contract areas

| Area | Contract question |
|---|---|
| Scope | What exactly is being provided? |
| Service levels | What performance is guaranteed? |
| Support | How are incidents handled? |
| Data | Who owns, accesses and processes data? |
| Security | What controls must vendor maintain? |
| Privacy | What legal/data obligations apply? |
| Audit | Can bank/regulator inspect controls? |
| Subcontracting | Can vendor use fourth parties? |
| Resilience | What DR/BCP commitments exist? |
| Exit | What happens when relationship ends? |

## 3. SLA design

SLAs should cover:

- availability
- response time
- resolution time
- support hours
- escalation path
- incident severity definitions
- data delivery timeliness
- reporting generation timelines
- batch/API completion
- recovery objectives
- planned maintenance windows
- vulnerability remediation timelines

Example:

| Severity | Example | Response | Resolution target |
|---|---|---:|---:|
| Sev 1 | Client reporting unavailable before regulatory deadline | 15 min | 4 hours / workaround |
| Sev 2 | Advisory proposal workflow degraded | 1 hour | 1 business day |
| Sev 3 | Non-critical defect | 1 business day | Next release |
| Sev 4 | Query / enhancement | 3 business days | Planned |

## 4. Exit plan

An exit plan should be written before production use.

It should define:

- termination triggers
- data export format
- data deletion process
- transition assistance
- migration support
- knowledge transfer
- fallback operating model
- replacement provider approach
- parallel-run period
- client impact management
- regulatory notification if needed
- archive and audit evidence retention
- escrow arrangements if applicable

## 5. Data rights

A bank should clarify:

- bank owns client and portfolio data
- vendor cannot use data for unrelated purposes
- vendor cannot train models on bank/client data unless explicitly approved
- vendor cannot resell or redistribute data
- vendor must delete/return data at termination
- data access must be logged
- data residency and transfer conditions are defined
- subprocessors cannot access data without approved basis

## 6. Audit rights

Audit clauses should allow:

- bank audit review
- regulator access where required
- access to control evidence
- review of subcontractor controls
- security testing rights where applicable
- penetration test summaries
- SOC / ISO evidence
- incident records
- DR test evidence
- control remediation tracking

## 7. Change and notification rights

Vendor should notify the bank about:

- material architecture changes
- cloud region changes
- data-location changes
- subprocessors
- ownership change
- major incidents
- material control failures
- severe vulnerabilities
- end-of-life components
- major product roadmap changes
- regulatory issues affecting service

## 8. AI-specific contract clauses

For AI vendors, add:

- no training on client data without permission
- prompt and output logging controls
- model version change notification
- evaluation and red-team evidence
- human oversight requirements
- prohibited autonomous actions
- explainability and citation expectations
- hallucination and unsafe-output monitoring
- data retention for prompts and completions
- retrieval-source governance
- tool-use restrictions

## 9. Key takeaway

A good vendor contract should support safe operations, audit, resilience and exit - not just purchase.
