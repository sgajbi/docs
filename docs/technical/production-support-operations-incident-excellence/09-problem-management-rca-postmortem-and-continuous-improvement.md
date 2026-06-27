# Problem Management, RCA, Postmortem, and Continuous Improvement

## Purpose

This file explains how to turn incidents into durable improvement.

Problem management prevents recurrence by addressing root causes and contributing factors.

---

## Incident Versus Problem

| Term | Meaning |
|---|---|
| Incident | An event that disrupts service. |
| Problem | Underlying cause or recurring condition that leads to incidents. |
| RCA | Root-cause analysis. |
| Postmortem | Structured review of incident, impact, response, and actions. |
| Corrective action | Work item that reduces recurrence or impact. |

---

## Postmortem Structure

```text
Summary
Impact
Timeline
Detection
Response
Root cause
Contributing factors
What went well
What did not go well
Corrective actions
Owners
Due dates
Evidence
Follow-up review
```

---

## Root Cause Thinking

Avoid stopping at the first technical cause.

Ask:

- Why did it happen?
- Why was it not prevented?
- Why was it not detected earlier?
- Why was impact high?
- Why was recovery slow?
- What process/control/test/runbook was missing?

---

## Corrective Actions

Good actions are:

- specific
- owned
- time-bound
- measurable
- tied to cause
- reviewed later

Examples:

- add regression test
- add alert
- update runbook
- add retry/back-pressure
- improve dashboard
- fix data-quality validation
- change release gate
- improve rollback automation

---

## RCA Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Blame-focused postmortem | People hide issues. |
| Action says “be careful” | No real improvement. |
| Actions have no owners | Nothing changes. |
| Same issue repeats | Problem management failure. |
| RCA only technical | Process/control gaps missed. |
| Postmortem not shared | Learning limited. |
| No follow-up review | Actions forgotten. |

---

## Review Checklist

- Is postmortem completed?
- Is timeline accurate?
- Is impact clear?
- Are contributing factors identified?
- Are actions specific?
- Are owners assigned?
- Are due dates set?
- Are tests/runbooks/dashboards improved?
- Is follow-up scheduled?
- Is learning shared?

---

## Summary

Incidents are expensive learning events.

A mature team converts every serious incident into better systems, tests, runbooks, and controls.
