# 09 - Platform Implementation, Workflows, Reconciliation and Test Scenarios

## 1. Vendor onboarding workflow

```text
Create vendor record
-> classify service
-> assign business owner
-> assign technology owner
-> classify data
-> assess inherent risk
-> collect evidence
-> perform due diligence
-> raise findings
-> approve or reject
-> contract
-> onboard
-> monitor
-> review
-> renew or exit
```

## 2. Workflow states

| State | Meaning |
|---|---|
| Draft | Vendor/service created |
| Risk assessment pending | Business and technology inputs required |
| Evidence requested | Vendor must provide documents |
| Review in progress | Security/risk/legal/procurement reviewing |
| Findings open | Issues found |
| Approved | Production use allowed |
| Approved with conditions | Use allowed with remediation |
| Restricted | Only limited use allowed |
| Rejected | Use not allowed |
| Active | Vendor in production |
| Suspended | Temporarily blocked |
| Exiting | Exit plan in execution |
| Terminated | Relationship ended |

## 3. Controls implementation

A platform should support:

- vendor inventory
- service inventory
- risk tiering
- due diligence questionnaires
- evidence upload
- control mapping
- findings and remediation
- approval workflows
- contract metadata
- SLA monitoring
- incident tracking
- renewal reminders
- exit planning
- subcontractor tracking
- audit evidence
- dashboards

## 4. Reconciliation checks

| Check | Purpose |
|---|---|
| Vendor inventory vs contracts | Ensure all active vendors have contracts |
| Vendor inventory vs production integrations | Detect unregistered third parties |
| Data classification vs DPA | Ensure processing agreement matches data sensitivity |
| Critical service vs exit plan | Ensure critical vendors have exit plans |
| SLA breach vs incident records | Ensure breaches are tracked |
| Subcontractor list vs approved list | Detect unapproved fourth parties |
| Contract renewal vs review date | Avoid silent auto-renewal |
| AI tool inventory vs AI governance approval | Detect shadow AI |

## 5. Test scenarios

### Scenario 1 - New SaaS with client data

Expected:

- high inherent risk
- DPA required
- security review required
- access model required
- exit plan required
- production blocked until approval

### Scenario 2 - AI tool proposed for advisor copilot

Expected:

- AI governance review
- prompt/data retention review
- RAG grounding required
- human approval required for advice
- no autonomous order placement
- evaluation evidence required

### Scenario 3 - Vendor changes cloud region

Expected:

- material change event
- privacy review
- data residency review
- client/country impact assessment
- approval before migration

### Scenario 4 - Critical vendor outage

Expected:

- incident opened
- SLA breach measured
- business impact recorded
- client impact assessed
- workaround invoked
- post-incident review completed

### Scenario 5 - Contract termination

Expected:

- exit plan activated
- data export completed
- data deletion certificate received
- access revoked
- final reconciliation completed
- archive evidence retained

## 6. Dashboard metrics

| Metric | Why useful |
|---|---|
| Critical vendors count | Concentration and oversight |
| Vendors with overdue reviews | Governance gap |
| Open high-risk findings | Control risk |
| SLA breaches | Performance risk |
| Vendors without exit plan | Resilience gap |
| Expiring contracts | Procurement risk |
| Unapproved subcontractors | Fourth-party risk |
| AI vendors without evaluation | Model risk |
| Production integrations without vendor record | Shadow IT |

## 7. Key takeaway

Vendor risk needs workflow, evidence, reconciliation, metrics and audit trail - not spreadsheets alone.
