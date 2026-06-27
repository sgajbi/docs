# Production Support Operating Model

## Purpose

This file explains how to structure a production support operating model for enterprise systems.

A support operating model defines who supports what, how incidents are handled, how issues are escalated, how evidence is retained, and how services improve over time.

---

## Support Model Components

A support model should define:

- service owner
- production owner
- support tiers
- support hours
- severity model
- escalation path
- incident manager role
- communication process
- runbook ownership
- dashboard and alert ownership
- change/release support process
- post-incident improvement process

---

## Support Tiers

| Tier | Responsibility |
|---|---|
| L0 | Self-service docs, status pages, FAQs, automated diagnostics. |
| L1 | Initial triage, known issue checks, standard runbook execution. |
| L2 | Application support, data checks, configuration, controlled recovery. |
| L3 | Engineering/developer support, defect fixes, deep diagnosis. |
| Platform/SRE | Infrastructure, runtime, observability, deployment, resilience. |
| Security | Security/privacy incident handling and access control review. |
| Business Operations | Business validation, user communication, manual fallback, sign-off. |

---

## Support Scope

Each service should define:

```text
supported capabilities:
support hours:
business criticality:
severity criteria:
dependencies:
dashboards:
runbooks:
known limitations:
support contacts:
escalation path:
```

---

## Support Channels

Common channels:

- ticketing system
- incident channel
- on-call paging
- support mailbox
- status page
- command center bridge
- business communication channel
- post-incident review forum

Avoid relying only on informal chat.

---

## Operating Model Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Support depends on one developer | Key-person risk. |
| No L1/L2 runbooks | Every issue escalates to engineering. |
| No severity model | Response expectations unclear. |
| No service owner | Issues bounce between teams. |
| Chat is the only evidence | Audit and learning gap. |
| Business support not included | User impact unmanaged. |

---

## Review Checklist

- Is service owner clear?
- Are support tiers defined?
- Are support hours defined?
- Is severity model clear?
- Are channels defined?
- Are runbooks available?
- Are dashboards available?
- Is escalation path clear?
- Is post-incident improvement process defined?
- Is support knowledge current?

---

## Summary

A production support model turns operational chaos into repeatable response.

It is a product capability, not an afterthought.
