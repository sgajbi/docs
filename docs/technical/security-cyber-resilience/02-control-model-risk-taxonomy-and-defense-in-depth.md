# Control Model, Risk Taxonomy, and Defense in Depth

## Purpose

This file explains how to think about security controls, risk categories, and layered defense.

Enterprise security is strongest when multiple controls reduce the same risk at different layers.

---

## Control Types

| Control Type | Purpose |
|---|---|
| Preventive | Stop bad action before it happens. |
| Detective | Detect suspicious or failed behavior. |
| Corrective | Restore safe state after issue. |
| Compensating | Reduce risk when primary control is incomplete. |
| Deterrent | Discourage misuse through visibility/audit. |
| Directive | Define expected behavior through policy/standard. |

---

## Defense In Depth

Defense in depth means controls exist at multiple layers.

Example for protected document download:

```text
UI hides unauthorized action
  -> BFF enforces caller context
  -> archive service validates entitlement
  -> storage access uses short-lived controlled access
  -> logs record safe audit event
  -> metrics track permission-blocked state
  -> alerts detect unusual denial or download spikes
```

No single control carries all trust.

---

## Risk Taxonomy

Common risk categories:

| Risk | Example |
|---|---|
| Unauthorized access | User sees restricted portfolio. |
| Data leakage | Logs expose account details. |
| Privilege escalation | Workflow token writes to protected branch. |
| Integrity failure | Calculation result modified without trace. |
| Availability failure | Dependency outage blocks critical service. |
| Supply-chain compromise | Malicious dependency or workflow action. |
| Misconfiguration | Production service points to wrong dependency. |
| Audit gap | No evidence for who approved or ran action. |
| AI misuse | Generated content treated as authoritative. |
| Operational abuse | Replay action run without authorization. |

---

## Control Evidence

Controls should produce evidence.

Evidence examples:

- test results
- scan reports
- audit logs
- policy validation output
- access-denied event
- workflow permission check
- SBOM
- image digest
- dashboard screenshot where approved
- machine-readable certification output
- runbook link
- incident review

---

## Control Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| One control relied on for everything | Single point of failure. |
| Policy without validation | Paper control. |
| Evidence manually assembled every time | Inconsistent audit proof. |
| Controls not mapped to risks | Gaps hidden. |
| Detection without response | Alerts ignored. |
| Exceptions never expire | Permanent control bypass. |

---

## Review Checklist

- What risk is being addressed?
- Is the control preventive, detective, corrective, or compensating?
- Which layer implements it?
- What evidence proves it works?
- Who owns it?
- How is failure detected?
- What happens when control fails?
- Are exceptions time-bound?
- Is the control tested?
- Is the residual risk documented?

---

## Summary

Security maturity comes from layered controls tied to specific risks and supported by evidence.
