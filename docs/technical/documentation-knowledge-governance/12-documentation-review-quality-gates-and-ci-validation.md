# Documentation Review, Quality Gates, and CI Validation

## Purpose

This file explains how to review and validate documentation through engineering workflows.

Docs should be reviewed and tested when they affect engineering truth.

---

## Documentation Review Areas

Review:

- accuracy
- current status
- implementation evidence
- command validity
- link validity
- examples
- sensitive data
- duplication
- ownership
- lifecycle status
- readability

---

## Documentation CI Gates

Possible gates:

- markdown lint
- link check
- required README check
- stale date check
- forbidden phrase scan
- sensitive-content scan
- generated docs consistency
- OpenAPI/docs alignment
- runbook metric/link validation
- supported-feature evidence validation
- wiki source structure check

---

## Report-Only Versus Blocking

Start new docs gates in report-only mode when:

- baseline unknown
- false positives likely
- cleanup required
- ownership not clear

Promote to blocking after signal is stable.

---

## Documentation Quality Signals

Track:

- broken links
- stale docs
- missing owners
- unsupported claims
- docs updated with code
- runbook coverage
- ADR coverage
- onboarding completion feedback
- docs PR review comments

---

## Docs Review Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Docs PRs not reviewed | Stale or unsafe claims. |
| Link checks absent | Broken navigation. |
| Future claims not flagged | False truth. |
| Sensitive examples not scanned | Data leakage. |
| CI gate too noisy | Developers ignore docs checks. |
| Docs quality only manual | Regression. |

---

## Review Checklist

- Are docs accurate?
- Are examples current?
- Are links valid?
- Are claims evidence-backed?
- Is status clear?
- Are sensitive details removed?
- Is duplication avoided?
- Are docs gates passing?
- Is owner defined?
- Is review cadence defined?

---

## Summary

Documentation quality can be engineered.

Use review and CI gates to keep docs useful and trustworthy.
