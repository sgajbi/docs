# AI Operating Model, Governance Roles, and Maturity Roadmap

## Purpose

This file explains how to organize people, roles, and maturity around enterprise AI capabilities.

AI governance should be proportionate to use-case risk and integrated with normal engineering delivery.

---

## Governance Roles

| Role | Responsibility |
|---|---|
| Product owner | Defines use case and user value. |
| Engineering owner | Owns implementation and runtime. |
| Architect | Reviews design, boundaries, and integration. |
| Data owner | Approves sources and data use. |
| Security owner | Reviews access, leakage, and abuse risk. |
| Model-risk owner | Reviews evaluation, risk, and approval posture. |
| QA owner | Defines evaluation and regression tests. |
| SRE owner | Defines observability and operations. |
| Legal/compliance | Reviews regulated or client-facing use where required. |

---

## Governance Flow

```text
use-case proposal
  -> risk classification
  -> architecture review
  -> data/source approval
  -> security review
  -> evaluation design
  -> implementation
  -> testing and red team
  -> certification
  -> controlled rollout
  -> monitoring
  -> periodic review
```

---

## Maturity Levels

| Level | Description |
|---|---|
| L0 | Ad hoc AI use. |
| L1 | Approved internal experiments. |
| L2 | Use-case inventory and basic guardrails. |
| L3 | RAG/source governance and evaluations. |
| L4 | Model-risk evidence, monitoring, and controlled rollout. |
| L5 | Enterprise AI platform with reusable governance, telemetry, and continuous improvement. |

---

## Review Cadence

Suggested cadence:

- use-case review before build
- security/model-risk review before pilot
- evaluation review before release
- monthly quality and usage review
- quarterly model/source review
- incident-driven review as needed

---

## AI Governance Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No use-case inventory | Shadow AI. |
| Governance only after implementation | Late rework. |
| One-size-fits-all approvals | Slow low-risk use or weak high-risk control. |
| No owner for prompt/model changes | Quality drift. |
| No monitoring after release | Risk grows silently. |
| No deprecation path | Old AI workflows persist. |

---

## Review Checklist

- Are roles defined?
- Is use case classified?
- Are sources approved?
- Is security review complete?
- Is model-risk review needed?
- Are evaluations defined?
- Is rollout controlled?
- Is monitoring defined?
- Is review cadence defined?
- Is maturity roadmap active?

---

## Summary

Enterprise AI needs an operating model.

Governance should enable safe adoption, not stop useful innovation.
