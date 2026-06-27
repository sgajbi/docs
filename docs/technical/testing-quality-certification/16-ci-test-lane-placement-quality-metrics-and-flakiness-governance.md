# CI Test Lane Placement, Quality Metrics, and Flakiness Governance

## Purpose

This file explains how to place tests in CI/CD lanes and govern quality metrics and flaky tests.

Good CI placement balances fast feedback with release confidence.

---

## Lane Placement

| Test Type | Local | Feature Lane | PR Gate | Main Gate | Platform E2E |
|---|---:|---:|---:|---:|---:|
| Unit/domain | Yes | Yes | Yes | Optional | No |
| Application | Yes | Yes | Yes | Optional | No |
| API/contract | Focused | Yes | Yes | Yes | Conditional |
| Integration | Focused | Optional | Yes | Yes | Conditional |
| Migration | Optional | Optional | Yes | Yes | No |
| Docker smoke | Optional | Optional | Yes | Yes | Conditional |
| E2E/browser | Optional | No | Conditional | Yes | Yes |
| Certification | No | No | Optional | Yes | Yes |
| Performance | Optional | No | Conditional | Scheduled | No |
| Security | Optional | Yes | Yes | Yes | No |

---

## Coverage

Coverage is a signal.

Use coverage to:

- find untested areas
- prevent large drops
- support refactoring
- identify dead zones

Do not use coverage to replace behavior review.

---

## Quality Metrics

Useful metrics:

- test pass rate
- coverage trend
- flaky test count
- test duration
- escaped defects
- regression test count
- contract test coverage
- CI failure reason categories
- time to fix broken main
- certification pass rate

---

## Flaky Tests

A flaky test sometimes passes and sometimes fails without code change.

Flaky tests are defects.

Governance:

- identify owner
- quarantine only with time limit
- track reason
- fix root cause
- avoid ignoring indefinitely
- review frequently

---

## Slow Tests

Slow tests should be:

- optimized
- split
- moved to correct lane
- run in parallel where safe
- scheduled if not needed for PR
- kept meaningful

---

## Report-Only Testing Gates

Use report-only for:

- new performance baseline
- new mutation testing
- new complexity gate
- new browser coverage
- new security check
- new contract drift report

Promote only after signal is stable.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Every test in PR gate | Slow delivery. |
| Heavy tests never run | Release risk. |
| Flaky tests ignored | CI loses trust. |
| Coverage target gamed | Weak evidence. |
| Slow tests have no owner | CI worsens. |
| Report-only findings ignored | No maturity improvement. |
| Failed main not prioritized | Release line weak. |

---

## Review Checklist

- Are tests in the right lane?
- Is PR feedback fast enough?
- Are release gates strong enough?
- Are flaky tests tracked?
- Are slow tests reviewed?
- Are coverage trends meaningful?
- Are report-only gates owned?
- Are quality metrics reviewed regularly?
- Are CI failures actionable?

---

## Summary

Test governance keeps CI useful.

The goal is fast, trustworthy feedback and strong release evidence without drowning teams in slow or flaky checks.
