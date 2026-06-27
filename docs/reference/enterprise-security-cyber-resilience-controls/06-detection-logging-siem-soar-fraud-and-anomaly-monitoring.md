# Detection, Logging, SIEM/SOAR, Fraud and Anomaly Monitoring

## 1. Detection objective

Detection controls identify suspicious activity, control failures and operational anomalies early enough to contain impact. In wealth platforms, detection must cover cyber, fraud, operational, data-quality and AI misuse signals.

## 2. Logging principles

Logs should be:

- complete enough for investigation
- structured and machine searchable
- correlated across services
- protected from tampering
- retained according to policy
- free of unnecessary sensitive data
- usable for audit and control evidence

## 3. Audit event categories

| Category | Examples |
|---|---|
| Authentication | Login success/failure, MFA, device change |
| Authorization | Access allowed/denied, policy decision |
| Data access | Client/portfolio/report viewed, export performed |
| Mutation | Order entered, proposal approved, mandate changed |
| Admin action | Role change, entitlement override, config change |
| Privileged access | Production access, break-glass, database query |
| Data pipeline | Feed received, validation failed, reconciliation break |
| Security event | WAF alert, suspicious request, malware detection |
| AI event | Prompt, retrieval set, model output, tool action, safety block |
| Incident event | Alert, triage, escalation, containment, closure |

## 4. Minimum audit event fields

```json
{
  "event_id": "evt-123",
  "timestamp": "2026-06-27T10:15:30Z",
  "event_type": "REPORT_DOWNLOAD",
  "actor_id": "user-123",
  "actor_role": "advisor",
  "client_id": "cif-456",
  "account_id": "acc-789",
  "portfolio_id": "pf-101",
  "resource_id": "report-abc",
  "action": "DOWNLOAD",
  "decision": "ALLOW",
  "policy_version": "access-policy-2026.06",
  "source_ip": "10.0.0.5",
  "device_id": "device-555",
  "channel": "ADVISOR_WORKBENCH",
  "correlation_id": "corr-xyz"
}
```

## 5. SIEM detection use cases

| Use case | Detection signal |
|---|---|
| Unusual client data access | Advisor views many non-assigned clients |
| Bulk export | High report/transaction download volume |
| Privilege misuse | Admin role granted and used quickly |
| Impossible travel | Login from distant geographies in short time |
| Brute force | Repeated failed logins |
| API scraping | High-volume paginated calls |
| Prompt injection attempt | Model input contains instruction override patterns |
| RAG leakage | Retrieval includes documents outside user scope |
| Fraudulent transaction | Order pattern deviates from client profile |
| Data tampering | Price/FX/reference data changed outside source process |

## 6. SOAR automation

SOAR playbooks can automate response but must be carefully governed.

| Alert | Automated action | Human approval? |
|---|---|---|
| Credential stuffing | Rate limit and force MFA step-up | Usually no |
| Suspicious export | Temporarily suspend export capability | Maybe |
| Privileged access anomaly | Notify SOC and owner, require review | Yes |
| Malware in uploaded file | Quarantine file | No |
| AI prompt injection attempt | Block response and log event | No |
| Possible account takeover | Lock session and require re-authentication | Maybe |
| Production database export | Immediate escalation | Yes |

## 7. Fraud and financial-crime monitoring intersection

Cyber events and fraud events overlap. Examples:

- account takeover leading to transfer instruction
- compromised advisor credentials leading to order placement
- social engineering leading to beneficiary or contact change
- fraudulent document upload
- suspicious transaction instruction near credential reset
- unusual cash withdrawal after digital profile change

The platform should correlate:

```text
identity event + client profile change + transaction instruction + device/IP anomaly + approval trail
```

## 8. Data-quality anomaly detection

Not all incidents are malicious. Analytics/reporting errors can come from data failures.

Examples:

| Signal | Possible issue |
|---|---|
| Sudden portfolio value drop | Missing price, wrong FX, corporate action not applied |
| Negative cash unexpected | Settlement/fee/accrual issue |
| Performance outlier | Bad valuation or cashflow classification |
| Buying power spike | Collateral LTV or exposure feed stale |
| Missing report section | Upstream service failure |
| NAV unchanged too long | Stale fund price |

## 9. Detection for AI systems

AI-specific telemetry:

- prompt category and risk score
- retrieved documents and entitlement filters used
- model version and prompt template version
- output safety checks
- hallucination/grounding evaluation score where available
- tool call request and approval status
- blocked prompt injection attempts
- user feedback and correction events
- sensitive data redaction events

## 10. Alert quality management

Too many false positives create alert fatigue. Good alert design should define:

- severity
- owner
- business impact
- triage steps
- suppression logic
- correlation rules
- escalation path
- SLA
- evidence required for closure

## 11. Investigation workflow

A security investigation should be able to reconstruct:

1. actor identity and session
2. device/network context
3. authorization decision
4. data accessed or modified
5. workflow state before and after action
6. approvals and overrides
7. source system data involved
8. logs across API, service, database and AI/tool layer
9. business impact
10. remediation and control improvement
