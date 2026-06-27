# CI/CD as Evidence Production System

## Purpose

This file explains the most important CI/CD mindset: pipelines are evidence-production systems.

Automation is valuable only when it produces evidence that helps engineers, reviewers, release managers, security teams, operators, and auditors trust a change.

---

## CI/CD Is More Than Automation

A weak pipeline answers:

```text
Did the script finish?
```

A mature pipeline answers:

```text
What was tested?
What contracts were protected?
What artifact was built?
What security posture changed?
What runtime behavior was proved?
What evidence can be reused during release or support?
```

---

## Evidence Produced By CI/CD

A strong CI/CD system can produce:

- test results
- coverage reports
- OpenAPI contract snapshots
- event schema validation
- data-product contract validation
- dependency audit output
- SBOM
- build provenance
- container image digest
- Docker smoke results
- migration validation
- quality baseline
- code complexity report
- repository hygiene report
- security scan results
- release package
- deployment summary
- post-deploy verification
- artifacts for troubleshooting

---

## CI/CD Control Questions

Every pipeline should answer:

| Question | Why It Matters |
|---|---|
| What changed? | Traceability. |
| Who approved it? | Accountability. |
| What tests passed? | Behavior confidence. |
| What contracts changed? | Consumer impact. |
| What security checks ran? | Risk reduction. |
| What artifact was produced? | Reproducibility. |
| Where was it deployed? | Operational traceability. |
| How can it be rolled back? | Resilience. |
| What evidence remains? | Audit and support. |

---

## Evidence Quality

Not all evidence is equal.

Good evidence is:

- current
- reproducible
- machine-readable where useful
- linked to commit/PR
- safe from sensitive data
- understandable by humans
- aligned with implementation
- retained for appropriate period

Poor evidence:

- screenshots only
- manual statement without command
- stale artifact
- output from wrong branch
- local-only proof without reproducibility
- report with no owner
- false production-ready claim

---

## Pipeline As A Risk Filter

Pipelines reduce risk by catching:

- syntax errors
- type errors
- broken tests
- contract drift
- unsafe dependencies
- secrets
- insecure workflows
- broken migrations
- container startup failure
- documentation drift
- unbounded metrics
- unsupported API claims

No pipeline catches everything. CI/CD should reduce preventable risk and make residual risk visible.

---

## CI/CD Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Green badge with weak checks | False confidence. |
| Manual release proof only | Hard to audit and repeat. |
| Report-only checks ignored forever | Quality debt normalized. |
| Blocking noisy checks too early | Developers lose trust in CI. |
| Pipeline hides failure details | Slow triage. |
| Artifacts contain sensitive data | Security incident. |
| Build not tied to commit | Artifact provenance weak. |
| Release evidence not retained | Support and audit gap. |

---

## Review Questions

- What evidence does this pipeline produce?
- Who consumes that evidence?
- Which checks block merge?
- Which checks are report-only?
- Are report-only findings tracked?
- Are artifacts safe?
- Is evidence tied to commit and branch?
- Can release managers use this output?
- Can support teams use this output?
- Is the evidence enough for the risk of the change?

---

## Summary

A pipeline should not merely run commands. It should create trustworthy evidence that the change is safe to review, merge, release, and operate.
