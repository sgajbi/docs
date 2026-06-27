# Incident Response, Cyber Resilience, BCP, DR and Crisis Management

## 1. Incident response objective

Incident response is the ability to prepare for, detect, analyze, contain, eradicate and recover from cybersecurity and operational incidents. For wealth platforms, it must protect client trust, regulatory obligations, transaction authority, data integrity and business continuity.

## 2. Incident categories

| Category | Example |
|---|---|
| Credential compromise | Advisor account takeover |
| Data leakage | Client statement sent to wrong recipient |
| Unauthorized transaction | Suspicious order or payment instruction |
| Malware/ransomware | File upload or endpoint compromise |
| API abuse | Scraping positions/transactions |
| Production outage | Reporting or valuation unavailable |
| Data integrity incident | Wrong prices used in reports |
| AI incident | Model discloses restricted information |
| Vendor incident | Custodian/market data provider breach |
| Insider threat | Privileged export of client data |

## 3. Incident lifecycle

| Phase | Activities |
|---|---|
| Prepare | Playbooks, contacts, access, backups, exercises |
| Detect | Monitoring, alerts, user reports, vendor notifications |
| Analyze | Scope, root cause, impact, severity, evidence |
| Contain | Account lock, revoke token, isolate service, block export |
| Eradicate | Patch, remove malicious artifact, rotate secrets |
| Recover | Restore service, validate data, resume operations |
| Communicate | Stakeholder, regulator, client and internal communications as required |
| Learn | Post-incident review, action items, control improvements |

## 4. Severity model

| Severity | Example | Response |
|---|---|---|
| Sev 1 | Confirmed client data breach or unauthorized trade | Crisis bridge, executive escalation, legal/compliance, rapid containment |
| Sev 2 | Major outage affecting reporting/order capture | Incident manager, business comms, recovery plan |
| Sev 3 | Limited security alert or failed control | Team response, evidence and remediation |
| Sev 4 | Low-risk suspicious event | Monitor, triage and close with evidence |

## 5. Wealth-specific incident playbooks

Recommended playbooks:

- compromised advisor account
- compromised client account
- unauthorized order instruction
- client data/report misdelivery
- ransomware affecting reporting/valuation
- market data corruption
- entitlements failure
- API scraping/data exfiltration
- production database unauthorized access
- AI prompt injection and data leakage
- vendor/custodian cyber incident
- failed DR or backup restoration

## 6. Cyber-resilience design

Resilience requires more than failover. It requires:

- critical business service mapping
- dependency mapping
- RTO and RPO by capability
- immutable/isolated backups
- restore testing
- alternate operation procedure
- crisis communication plan
- manual fallback for critical processes
- data integrity verification after recovery
- lessons learned and control improvement

## 7. RTO/RPO examples

| Capability | Possible RTO | Possible RPO | Notes |
|---|---:|---:|---|
| Authentication/entitlements | Minutes | Near zero | Critical access layer |
| Order capture | Minutes to hours | Near zero | Depends on business hours and product |
| Portfolio positions | Hours | Last good snapshot | Must reconcile after recovery |
| Market data pricing | Hours | Last valid EOD/intraday | Need stale-price flags |
| Client reporting | Hours to day | Last certified snapshot | Can use prior snapshot with disclosure |
| Performance analytics | Day | Last completed calculation | Recalculate after feed restore |
| AI copilot | Hours/day | Last approved knowledge index | Can disable AI while core ops continue |

## 8. Ransomware resilience

Controls:

- endpoint protection and EDR
- network segmentation
- least privilege and PAM
- immutable backups
- offline/isolated recovery copies
- restore testing
- golden images and rebuild automation
- incident playbook
- external communications plan
- evidence preservation

## 9. Data integrity recovery

After recovery, confirm:

- source feed completeness
- cash/position reconciliation
- price/FX/NAV validity
- transaction sequence and idempotency
- report snapshot consistency
- performance recalculation status
- collateral and buying power recalculated
- advisory/proposal state restored correctly
- audit trail preserved

## 10. Crisis governance

Crisis roles:

| Role | Responsibility |
|---|---|
| Incident commander | Overall coordination |
| Security lead | Technical investigation and containment |
| Business owner | Business impact and prioritization |
| Operations lead | Client/advisor operational continuity |
| Legal/compliance | Regulatory and legal obligations |
| Communications lead | Internal/external messaging |
| Technology lead | Recovery and service restoration |
| Data owner | Data integrity validation |
| Vendor manager | Third-party coordination |

## 11. Post-incident review

The review should capture:

- what happened
- timeline
- detection source
- blast radius
- client/business impact
- control gaps
- response effectiveness
- recovery evidence
- root cause
- corrective actions
- owner and due date
- whether similar systems are affected

## 12. Cyber exercise scenarios

- Advisor credentials compromised before large client report download.
- Product pricing feed corrupted before monthly statements.
- Ransomware blocks access to reporting database before quarter-end reporting.
- AI assistant retrieves a restricted trust document for unauthorized user.
- Kubernetes secret leaked from CI pipeline.
- Privileged support user exports production client table.
- Primary cloud region unavailable during order window.
