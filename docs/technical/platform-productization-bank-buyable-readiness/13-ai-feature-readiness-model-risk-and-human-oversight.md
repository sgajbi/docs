# AI Feature Readiness, Model Risk, and Human Oversight

## Purpose

This file explains enterprise readiness requirements for AI features in bank-buyable software.

AI capabilities require additional clarity around data, grounding, model-risk, human oversight, and safe claims.

---

## AI Readiness Areas

| Area | Question |
|---|---|
| Use case | What task does AI support? |
| Risk tier | Is output internal, advisory, client-facing, or action-triggering? |
| Data | What sources and sensitive data are used? |
| Grounding | Is output source-backed? |
| Human review | Who approves output? |
| Model | Which model/provider is used? |
| Evaluation | How is quality tested? |
| Security | How are prompt injection and leakage controlled? |
| Audit | What evidence is retained? |
| Operations | How is cost, latency, and failure monitored? |

---

## AI Supported Status

Use precise language:

- prototype
- internal assistant
- human-reviewed draft
- evidence-grounded internal output
- client-ready under approval
- unsupported

---

## Human Oversight

Required for:

- client-facing reports
- advisory rationale
- investment recommendations
- suitability explanations
- workflow approvals
- regulated communications
- sensitive operational actions

---

## AI Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| AI feature sold as autonomous decision engine | Regulatory/model-risk concern. |
| No evaluation evidence | Quality untrusted. |
| No human review | Buyer risk. |
| Prompts use sensitive data without controls | Privacy issue. |
| AI output not grounded | Hallucination risk. |
| Model/provider not disclosed where needed | Governance concern. |

---

## Review Checklist

- Is AI use case classified?
- Is risk tier clear?
- Are sources approved?
- Is output grounded?
- Is human review required?
- Are evaluations available?
- Are prompts/versioning governed?
- Are logs safe?
- Is model-risk evidence available?
- Are buyer-facing claims safe?

---

## Summary

AI product readiness requires stronger evidence than a convincing demo.

Buyers need to know AI is bounded, governed, evaluated, and human-controlled where necessary.
