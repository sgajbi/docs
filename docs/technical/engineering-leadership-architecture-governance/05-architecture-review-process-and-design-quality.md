# Architecture Review Process and Design Quality

## Purpose

This file explains how to run architecture reviews that improve design quality without becoming heavy bureaucracy.

Architecture review should help teams make better decisions earlier.

---

## When To Review

Architecture review is useful for:

- new service
- major API contract
- data model change
- cross-team integration
- security boundary
- data-product publication
- async workflow
- migration/cutover
- production-critical change
- new technology choice
- major refactor
- deprecation decision

---

## Review Inputs

A design review should include:

- problem statement
- goals
- non-goals
- current state
- proposed design
- alternatives
- data flow
- service ownership
- API/event/data contracts
- security model
- operational model
- testing strategy
- migration/rollback
- risks and open questions

---

## Review Dimensions

| Dimension | Questions |
|---|---|
| Capability | What business or platform capability is affected? |
| Ownership | Who owns the truth? |
| Boundary | Is logic in the right layer? |
| Contracts | What APIs/events/data products change? |
| Data | What is source, lineage, freshness, quality? |
| Security | Who can access and what is sensitive? |
| Resilience | What fails and how does it degrade? |
| Operations | Can it be monitored and supported? |
| Testing | What evidence proves behavior? |
| Delivery | How will it be rolled out and rolled back? |

---

## Review Outcome

Architecture review should produce:

- approved decision
- requested changes
- open risks
- ADR/RFC updates
- follow-up owners
- review date
- implementation evidence expectations

---

## Review Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Review after implementation only | Too late to change. |
| Review focuses only diagram aesthetics | Real risk missed. |
| No decision recorded | Debate repeats. |
| No owner for actions | Follow-up lost. |
| Review board as gatekeeping theater | Teams avoid it. |
| No production view | Operability gaps. |

---

## Review Checklist

- Is problem clear?
- Are goals/non-goals clear?
- Are alternatives considered?
- Are boundaries correct?
- Are contracts clear?
- Are security and data risks covered?
- Are failure modes covered?
- Is rollout/rollback covered?
- Is testing evidence defined?
- Is decision recorded?

---

## Summary

Good architecture review improves decisions.

It should be practical, evidence-oriented, and focused on risk, ownership, and future maintainability.
