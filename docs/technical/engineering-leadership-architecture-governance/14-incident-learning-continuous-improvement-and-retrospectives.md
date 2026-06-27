# Incident Learning, Continuous Improvement, and Retrospectives

## Purpose

This file explains how engineering leaders should turn incidents and retrospectives into system improvement.

A mature organization does not only recover from incidents. It learns from them.

---

## Incident Learning Questions

After an incident, ask:

- What happened?
- What was the impact?
- How was it detected?
- Why was it not prevented?
- Why was it not detected earlier?
- Did alerts work?
- Did runbooks work?
- Was rollback clear?
- What evidence was missing?
- What control should improve?
- What test should be added?

---

## Retrospective Categories

Review:

- requirements clarity
- design assumptions
- delivery process
- testing gaps
- CI/CD gaps
- security gaps
- observability gaps
- communication gaps
- dependency management
- production readiness

---

## Improvement Actions

Good actions are:

- specific
- owned
- time-bound
- tied to root cause
- measurable
- reviewed later

Poor action:

```text
Be more careful.
```

Better action:

```text
Add contract test for stale source supportability state and update runbook by Friday.
```

---

## Continuous Improvement Loop

```text
incident or delivery issue
  -> review
  -> root cause / contributing factors
  -> action items
  -> backlog
  -> implementation
  -> evidence
  -> standard/template/gate update
```

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Blame-focused review | People hide problems. |
| No action owners | Nothing improves. |
| Same incident repeats | Learning failure. |
| Runbook not updated | Future response weak. |
| Test gap not closed | Regression likely. |
| Only process action, no technical fix | Root cause remains. |

---

## Review Checklist

- Was incident reviewed?
- Was impact clear?
- Were contributing factors identified?
- Were actions specific?
- Were owners assigned?
- Were tests/gates/runbooks updated?
- Were standards improved?
- Were actions followed up?
- Did team learn?

---

## Summary

Incidents are expensive learning opportunities.

Do not waste them by only restoring service.
