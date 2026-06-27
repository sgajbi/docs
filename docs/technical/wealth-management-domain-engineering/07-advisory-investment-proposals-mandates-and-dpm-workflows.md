# Advisory, Investment Proposals, Mandates, and DPM Workflows

## Purpose

This file explains advisory and discretionary portfolio management workflow concepts.

Advisory systems connect portfolio data, client profile, product eligibility, suitability, recommendations, proposals, approvals, and evidence.

---

## Advisory Workflow

Common flow:

```text
client context
  -> portfolio review
  -> opportunity or recommendation
  -> proposal draft
  -> suitability and policy checks
  -> advisor review
  -> approval workflow
  -> client communication
  -> execution handoff where applicable
  -> evidence archive
```

---

## Investment Proposal

A proposal may include:

- recommended buy/sell/hold actions
- target allocation
- rationale
- expected impact
- suitability checks
- risk impact
- cost/fee impact
- benchmark or mandate context
- disclosures
- approval status
- evidence

---

## Mandates

A mandate defines investment rules and constraints.

Examples:

- discretionary mandate
- advisory mandate
- model portfolio
- risk profile
- asset allocation range
- restricted instruments
- ESG or preference constraints
- leverage limits
- concentration limits
- benchmark assignment

---

## DPM and Rebalancing

Discretionary portfolio management may include:

- model portfolio alignment
- drift detection
- rebalance proposal
- trade list generation
- compliance checks
- approval workflow
- execution handoff
- post-trade monitoring

---

## Advisory Evidence

Evidence may include:

- client profile used
- portfolio state used
- recommendation rationale
- suitability check result
- advisor edits
- approvals
- disclosure version
- generated document
- audit trail

---

## Advisory Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Proposal generated without source portfolio evidence | Weak auditability. |
| Suitability only in frontend | Control bypass. |
| AI-generated rationale published without review | Governance risk. |
| Mandate rules hardcoded per screen | Inconsistent control. |
| No lifecycle state | Workflow ambiguity. |
| No audit trail | Review and compliance gap. |
| Recommendation not linked to portfolio state | Hard to explain. |

---

## Review Checklist

- What advisory workflow is supported?
- Is client profile source known?
- Is portfolio state version captured?
- Are suitability rules server-side?
- Are mandate restrictions explicit?
- Is proposal lifecycle defined?
- Is approval/audit captured?
- Is AI output reviewed?
- Is report/archive evidence linked?

---

## Summary

Advisory platforms are workflow and evidence systems, not only recommendation screens.

Human accountability, suitability, and auditability must be built into the design.
