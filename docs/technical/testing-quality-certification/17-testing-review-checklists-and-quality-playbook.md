# Testing Review Checklists and Quality Playbook

## Purpose

This file provides practical checklists for testing strategy, PR review, release readiness, certification, and quality maturity.

Use these checklists during design reviews, PR reviews, QA planning, release preparation, and engineering leadership reviews.

---

## Test Strategy Checklist

- [ ] Capability risk identified.
- [ ] Test levels selected intentionally.
- [ ] Domain rules covered.
- [ ] API contracts covered.
- [ ] Integration seams covered.
- [ ] E2E/product flows covered where needed.
- [ ] Security boundaries covered.
- [ ] Observability/supportability covered.
- [ ] Test data synthetic.
- [ ] CI lane placement defined.

---

## Unit and Domain Checklist

- [ ] Value objects tested.
- [ ] Policies tested.
- [ ] Calculations tested.
- [ ] Lifecycle transitions tested.
- [ ] Invalid transitions tested.
- [ ] Edge cases covered.
- [ ] Reason codes asserted.
- [ ] No infrastructure required.
- [ ] Test names describe behavior.

---

## API and Contract Checklist

- [ ] Request validation tested.
- [ ] Response schema tested.
- [ ] Error model tested.
- [ ] OpenAPI metadata validated.
- [ ] Idempotency tested.
- [ ] Pagination tested.
- [ ] Caller context tested.
- [ ] Supportability states tested.
- [ ] Backward-compatible aliases tested.

---

## Data and Event Checklist

- [ ] Producer contract tested.
- [ ] Consumer contract tested.
- [ ] Schema tested.
- [ ] Vocabulary tested.
- [ ] Event schema tested.
- [ ] Lineage tested.
- [ ] Freshness tested.
- [ ] Trust metadata tested.
- [ ] Access/SLO/evidence policy tested.

---

## Integration Checklist

- [ ] Repository/database tested.
- [ ] Migration tested.
- [ ] HTTP clients tested.
- [ ] Queue/stream tested.
- [ ] Cache tested.
- [ ] Object storage tested.
- [ ] Timeout/retry tested.
- [ ] Dependency error mapping tested.
- [ ] Tests isolated and deterministic.

---

## E2E and Browser Checklist

- [ ] Critical product journey selected.
- [ ] Canonical runtime documented.
- [ ] Data seeded deterministically.
- [ ] Assertions meaningful.
- [ ] Degraded states covered.
- [ ] Permission-blocked state covered.
- [ ] Screenshots safe.
- [ ] Evidence machine-readable.
- [ ] Failures diagnosable.

---

## Security Testing Checklist

- [ ] Allowed path tested.
- [ ] Denied path tested.
- [ ] Write permission tested.
- [ ] Operator action tested.
- [ ] Logs safe.
- [ ] Metrics safe.
- [ ] Traces safe.
- [ ] Diagnostics safe.
- [ ] Secrets scan exists.
- [ ] Artifacts checked.

---

## Observability Testing Checklist

- [ ] Structured logs tested.
- [ ] Metrics tested.
- [ ] Forbidden labels blocked.
- [ ] Trace propagation tested.
- [ ] Readiness tested.
- [ ] Supportability states tested.
- [ ] Dashboard metric references validated.
- [ ] Alert rules validated.
- [ ] Runbook links valid.

---

## Migration and Runtime Checklist

- [ ] Migration apply tested.
- [ ] Rollback/compensation documented.
- [ ] Docker build tested.
- [ ] Container smoke tested.
- [ ] Health/readiness tested.
- [ ] Config validation tested.
- [ ] Deployment manifests validated.
- [ ] Runtime evidence retained.

---

## Certification Checklist

- [ ] Scope defined.
- [ ] Supported claims listed.
- [ ] Unsupported claims excluded.
- [ ] Synthetic data used.
- [ ] Expected values asserted.
- [ ] Health/readiness checked.
- [ ] Evidence generated.
- [ ] Evidence safe.
- [ ] Gaps documented.
- [ ] Certification recency tracked.

---

## PR Review Checklist

- [ ] Tests match risk.
- [ ] Test names meaningful.
- [ ] Assertions meaningful.
- [ ] No unnecessary E2E usage.
- [ ] No sensitive test data.
- [ ] CI lane appropriate.
- [ ] Regression added for bug fix.
- [ ] Docs updated if testing posture changed.
- [ ] Residual risk documented.

---

## Quality Maturity Checklist

- [ ] Test pyramid balanced.
- [ ] Contract gates active.
- [ ] Integration tests reliable.
- [ ] E2E tests focused.
- [ ] Certification evidence repeatable.
- [ ] Flaky tests tracked.
- [ ] Coverage trends reviewed.
- [ ] Escaped defects reviewed.
- [ ] Incident learnings converted to tests.
- [ ] Quality roadmap maintained.

---

## Engineering Playbook

### Before Build

1. Identify risk.
2. Define acceptance criteria.
3. Choose test levels.
4. Design synthetic data.
5. Plan contract/security/observability tests.

### During Build

1. Add domain/use-case tests first.
2. Add API and contract tests.
3. Add integration tests for real seams.
4. Add degraded/security tests.
5. Add runtime evidence if runtime changed.

### Before Merge

1. Run local checks.
2. Confirm CI lanes pass.
3. Review test evidence.
4. Confirm no sensitive data.
5. Document gaps.

### Before Release

1. Run certification or release gates.
2. Confirm runtime proof.
3. Confirm regression coverage.
4. Confirm supportability evidence.
5. Confirm known risks.

---

## Summary

Quality engineering is repeatable risk reduction.

Use these checklists to make test strategy clear, evidence strong, and release confidence honest.
