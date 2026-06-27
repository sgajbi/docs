# Pull Request Structure, Evidence, and Description Quality

## Purpose

This file explains how to write pull requests that are easy to review and useful as delivery evidence.

A PR is the review and evidence container for a change.

---

## Good PR Structure

A good PR includes:

- summary
- why the change is needed
- what changed
- architecture impact
- API/event/data impact
- tests and validation
- security impact
- observability impact
- migration/config impact
- documentation impact
- rollback or mitigation
- known gaps and follow-up

---

## PR Template

```markdown
## Summary

## Why this change is needed

## What changed

## Architecture / boundary impact

## API / event / data-product impact

## Tests and validation

## Security and privacy impact

## Observability and operations impact

## Migration / configuration impact

## Documentation updated

## Rollback / mitigation plan

## Known gaps and follow-up
```

---

## Evidence Quality

Good evidence includes:

- commands run
- test results
- CI links
- generated artifact references
- screenshots where meaningful and safe
- before/after notes
- known limitations
- residual risks

Poor evidence:

- "tested locally" with no command
- stale screenshots
- unrelated CI output
- no failure/degraded testing
- no validation for contract changes

---

## PR Size

A reviewable PR should be small enough that reviewers can understand:

- intent
- behavior
- risk
- evidence
- documentation impact

If not, split it.

---

## PR Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Empty description | Reviewer lacks context. |
| Massive diff | Poor review quality. |
| No test evidence | Weak confidence. |
| Docs claim more than code | False truth. |
| Security impact omitted | Hidden risk. |
| "No tests" without reason | Review gap. |
| Follow-up risks not listed | Problems forgotten. |

---

## Review Checklist

- Is summary clear?
- Is motivation clear?
- Is scope reviewable?
- Is evidence listed?
- Are contract changes explained?
- Are security/observability impacts covered?
- Are docs updated?
- Are gaps listed?
- Is rollback/mitigation described?
- Would future support understand this PR?

---

## Summary

A good PR explains change, proof, and risk.

It should help reviewers now and future engineers later.
