# Report-Only to Blocking Gate Promotion and Maturity Roadmap

## Purpose

This file explains how to introduce new CI/CD quality gates without overwhelming teams.

A mature gate promotion model lets teams measure first, classify findings, then block regressions when the signal is stable.

---

## Report-Only Gates

Use report-only gates when:

- baseline is unknown
- tool is new
- findings may be noisy
- ownership is not clear
- threshold needs calibration
- cleanup backlog is large

Report-only does not mean optional forever.

---

## Promotion Criteria

Promote a gate to blocking when:

- findings are understood
- false positives are manageable
- owners are assigned
- remediation path is known
- threshold is agreed
- CI runtime is acceptable
- exceptions are controlled
- documentation exists

---

## No-New-Regression Mode

Before full blocking, use no-new-regression mode.

Example:

```text
Existing complexity findings allowed temporarily.
New complexity findings fail the gate.
```

This prevents worsening while backlog is cleaned.

---

## Exception Policy

Exceptions should be:

- explicit
- justified
- owner-assigned
- time-bound
- reviewed
- visible in reports
- not used to hide broad quality issues

---

## Gate Maturity Roadmap

| Level | Description |
|---|---|
| L0 | No gate. |
| L1 | Manual check. |
| L2 | Report-only gate. |
| L3 | No-new-regression gate. |
| L4 | Blocking gate with exceptions. |
| L5 | Blocking gate with trend reporting and ownership. |

---

## CI Maturity Roadmap

| Stage | Focus |
|---|---|
| Stage 1 | Basic build, lint, unit tests. |
| Stage 2 | Typecheck, coverage, contract tests. |
| Stage 3 | Integration, Docker, migration, security scans. |
| Stage 4 | Release evidence, SBOM, provenance, main releasability. |
| Stage 5 | Platform E2E, certification, SLO-aware gates, maturity scorecards. |

---

## Promotion Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| New noisy gate blocks immediately | Developer frustration. |
| Report-only ignored forever | No improvement. |
| Exceptions never expire | Gate loses meaning. |
| Threshold lowered to pass PR | Quality erosion. |
| Gate has no owner | Findings unmanaged. |
| Runtime too slow | Teams bypass checks. |
| Tool output not understandable | No remediation. |

---

## Review Checklist

- Why is this gate needed?
- Is it report-only or blocking?
- Who owns findings?
- What threshold applies?
- Are false positives understood?
- Are exceptions time-bound?
- Is runtime acceptable?
- Is remediation documented?
- Is promotion plan clear?
- Are trends reviewed?

---

## Summary

Good gate adoption is gradual but intentional.

Measure first, stabilize signal, prevent regressions, then block what matters.
