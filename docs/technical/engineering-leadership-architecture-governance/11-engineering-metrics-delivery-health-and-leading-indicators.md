# Engineering Metrics, Delivery Health, and Leading Indicators

## Purpose

This file explains how engineering leaders should use metrics without creating harmful incentives.

Metrics should help teams learn, improve, and manage risk. They should not be used as shallow productivity theatre.

---

## Useful Metric Categories

| Category | Examples |
|---|---|
| Flow | lead time, cycle time, PR age, deployment frequency. |
| Quality | escaped defects, regression count, test health, coverage trend. |
| Reliability | incidents, SLOs, MTTR, alert noise, error budget. |
| Delivery predictability | planned vs delivered, dependency blockers, carryover. |
| Review health | PR size, review time, rework rate. |
| Technical debt | debt backlog, complexity trend, deprecated surface count. |
| Operations | runbook coverage, readiness, supportability gaps. |
| Security | vulnerability age, secrets findings, exception count. |
| Documentation | stale docs, broken links, onboarding feedback. |

---

## Leading Versus Lagging Indicators

| Leading Indicator | Lagging Indicator |
|---|---|
| PR size | Production defect |
| Missing tests | Escaped bug |
| Alert noise | Slow incident response |
| Stale runbook | Incident confusion |
| Dependency blocker | Release delay |
| Security finding age | Audit issue |
| High complexity | Future defect risk |

Leaders should pay attention to leading indicators.

---

## DORA Metrics

Common delivery metrics:

- deployment frequency
- lead time for changes
- change failure rate
- time to restore service

Use carefully and contextually.

---

## Anti-Metrics

Avoid using metrics that create bad behavior.

Examples:

- lines of code written
- number of commits
- number of PRs without considering value
- raw story points across teams
- test count without quality
- coverage target without behavior review

---

## Metrics Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Metrics used to blame people | Teams game numbers. |
| Only output metrics | Quality and risk ignored. |
| No context | Wrong decisions. |
| Too many metrics | No focus. |
| Metrics not reviewed | No improvement. |
| Vanity dashboards | False maturity. |

---

## Review Checklist

- What decision will this metric support?
- Is it leading or lagging?
- Can it be gamed?
- Is context available?
- Is owner defined?
- Is target useful?
- Does it improve behavior?
- Is it reviewed regularly?
- Are qualitative signals included?

---

## Summary

Metrics should improve judgment, not replace it.

A good engineering metric helps teams see risk earlier and improve deliberately.
