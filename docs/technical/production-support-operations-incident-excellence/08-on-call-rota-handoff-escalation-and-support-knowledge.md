# On-Call, Rota, Handoff, Escalation, and Support Knowledge

## Purpose

This file explains how to run an on-call and escalation model.

On-call should protect services without burning out people.

---

## On-Call Model

Define:

- primary on-call
- secondary on-call
- escalation manager
- coverage hours
- timezone model
- handoff process
- paging criteria
- expected response
- compensation or rotation policy where applicable
- backup coverage

---

## Handoff

Handoff should include:

- open incidents
- known issues
- recent deployments
- risky changes
- data-quality status
- batch/job status
- outstanding alerts
- planned maintenance
- escalation notes

---

## Escalation

Escalation should be based on:

- severity
- time since detection
- customer impact
- lack of progress
- security/privacy concern
- data integrity concern
- business decision needed
- vendor/platform dependency

---

## On-Call Health

Track:

- page volume
- off-hours pages
- noisy alerts
- incident duration
- repeated incidents
- toil hours
- escalation frequency
- burnout signals
- runbook gaps

---

## On-Call Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| One person always on-call | Burnout/key-person risk. |
| Alerts page for non-actionable events | Fatigue. |
| No handoff | Context lost. |
| Escalation socially awkward | Delays. |
| No secondary | Single point of failure. |
| On-call lacks access/runbooks | Slow response. |
| Incidents not reviewed | Same pages repeat. |

---

## Review Checklist

- Is rota defined?
- Is primary/secondary clear?
- Is handoff structured?
- Are paging criteria defined?
- Are runbooks available?
- Does on-call have safe access?
- Are noisy alerts reviewed?
- Is escalation path clear?
- Is burnout monitored?
- Are repeated incidents fixed?

---

## Summary

On-call is a reliability practice, not punishment.

Good on-call requires actionable alerts, useful runbooks, healthy rotation, and continuous improvement.
