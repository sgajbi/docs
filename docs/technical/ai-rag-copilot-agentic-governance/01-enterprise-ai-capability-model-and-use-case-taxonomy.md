# Enterprise AI Capability Model and Use-Case Taxonomy

## Purpose

This file explains how to classify AI capabilities and use cases in an enterprise platform.

AI use cases should be classified by purpose, risk, data sensitivity, user impact, and required control level.

---

## AI Capability Types

| Capability Type | Description |
|---|---|
| Search assistance | Helps users find information. |
| Summarization | Condenses documents, messages, reports, incidents, or records. |
| Explanation | Explains domain concepts, calculations, or operational states. |
| Draft generation | Drafts emails, reports, notes, tickets, or documentation. |
| Decision support | Provides suggestions or analysis for human review. |
| Workflow assistance | Helps complete multi-step internal tasks. |
| Copilot | Interactive assistant embedded in a product or engineering workflow. |
| Agent | Uses tools to perform bounded actions. |
| RAG assistant | Answers using approved retrieved knowledge sources. |
| AI evaluator | Assesses quality, risk, similarity, or compliance of content. |
| AI engineering assistant | Helps write, refactor, test, or document code. |

---

## Use-Case Risk Tiers

| Tier | Description | Example |
|---|---|---|
| Low | Internal productivity with no sensitive data and no action. | Summarize public engineering docs. |
| Medium | Internal use with controlled sensitive context and human review. | Draft internal KT notes from approved docs. |
| High | Decision support in regulated workflow. | Suggest advisory rationale for human review. |
| Critical | Client-facing, financial, legal, or action-triggering output. | Generate client-ready investment explanation or execute workflow action. |
| Blocked | Not permitted due to policy, data, or control gap. | AI independently approves trade or advice. |

---

## Use-Case Classification Template

```text
Use case:
User:
Business purpose:
Data accessed:
Output type:
Action capability:
Human review:
Risk tier:
Allowed sources:
Disallowed sources:
Security controls:
Evaluation method:
Evidence required:
Supported status:
```

---

## AI Boundaries

AI should not become authoritative for:

- official portfolio accounting
- official performance calculation
- official risk calculation
- suitability decision
- mandate breach decision
- regulatory report
- trade execution
- entitlement decision
- client-ready advice

unless the institution explicitly approves the AI capability and its control framework.

---

## Use-Case Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| All AI use cases treated the same | Controls too weak for high-risk use. |
| Prototype shown as supported product | Trust and governance risk. |
| AI output treated as calculation truth | Domain authority violation. |
| Client-ready output without approval | Regulatory and reputational risk. |
| Sensitive data used without classification | Data leakage. |
| No use-case inventory | Governance blind spot. |

---

## Review Checklist

- Is use case defined?
- Is risk tier assigned?
- Is data sensitivity known?
- Is output type clear?
- Is human review required?
- Are allowed sources listed?
- Are blocked uses listed?
- Are evaluation criteria defined?
- Is supported status honest?
- Is evidence required before release?

---

## Summary

Enterprise AI begins with use-case classification.

The same AI technology can be low risk or critical risk depending on data, user, output, and action capability.
