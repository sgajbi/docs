# AI Review Checklists and Engineering Playbook

## Purpose

This file provides practical checklists for AI/RAG/copilot/agent design, review, testing, security, operations, and release readiness.

Use these checklists during architecture review, PR review, AI governance review, model-risk review, release readiness, and production support planning.

---

## AI Use-Case Checklist

- [ ] Use case defined.
- [ ] User group defined.
- [ ] Business purpose clear.
- [ ] Risk tier assigned.
- [ ] Output type defined.
- [ ] Data sensitivity classified.
- [ ] Human review requirement defined.
- [ ] Unsupported uses listed.
- [ ] Evidence requirement defined.
- [ ] Owner assigned.

---

## RAG Checklist

- [ ] Sources approved.
- [ ] Sources classified.
- [ ] Source owners defined.
- [ ] Entitlement-aware retrieval enforced.
- [ ] Chunking strategy defined.
- [ ] Metadata complete.
- [ ] Freshness tracked.
- [ ] Archived/stale docs handled.
- [ ] Citations preserved.
- [ ] Retrieval quality tested.

---

## Prompt Checklist

- [ ] Prompt template versioned.
- [ ] Owner defined.
- [ ] Purpose clear.
- [ ] Boundaries explicit.
- [ ] Output schema defined.
- [ ] Refusal conditions included.
- [ ] Source requirements included.
- [ ] Prompt-injection considered.
- [ ] Evaluation dataset linked.
- [ ] Change history recorded.

---

## Copilot Checklist

- [ ] AI output labelled.
- [ ] Human control preserved.
- [ ] Edit/approve/reject available.
- [ ] Sources shown where required.
- [ ] Limitations shown.
- [ ] Sensitive output requires approval.
- [ ] Feedback captured.
- [ ] Audit captured where needed.
- [ ] Permission-blocked state safe.
- [ ] UX trust signals tested.

---

## Agent and Tool Checklist

- [ ] Tools allowlisted.
- [ ] Tool risk classified.
- [ ] Tool input schema defined.
- [ ] Authorization enforced.
- [ ] Human approval required for sensitive actions.
- [ ] Idempotency defined.
- [ ] Audit emitted.
- [ ] Rate limits defined.
- [ ] Unsafe actions blocked.
- [ ] Tool failure handled safely.

---

## Model Selection Checklist

- [ ] Model choice justified.
- [ ] Use-case fit assessed.
- [ ] Data policy compatible.
- [ ] Evaluation results available.
- [ ] Latency acceptable.
- [ ] Cost acceptable.
- [ ] Provider risk considered.
- [ ] Fallback defined.
- [ ] Routing policy defined.
- [ ] Model change review required.

---

## Evaluation Checklist

- [ ] Evaluation dataset defined.
- [ ] Normal cases included.
- [ ] Edge cases included.
- [ ] Unsupported/refusal cases included.
- [ ] Prompt-injection cases included.
- [ ] Entitlement cases included.
- [ ] Citation accuracy tested.
- [ ] Structured output tested.
- [ ] Results recorded.
- [ ] Regression cadence defined.

---

## Security Checklist

- [ ] Data classification complete.
- [ ] Retrieval scoped by entitlement.
- [ ] Prompts minimized.
- [ ] Prompts/responses not logged raw.
- [ ] Prompt injection tested.
- [ ] Tool authorization tested.
- [ ] Secrets excluded.
- [ ] Evidence safe.
- [ ] Abuse cases documented.
- [ ] Human approval enforced where needed.

---

## Observability Checklist

- [ ] Use-case metrics defined.
- [ ] Model/prompt version captured.
- [ ] Retrieval metadata captured safely.
- [ ] Tool calls audited.
- [ ] Safety checks logged safely.
- [ ] Latency tracked.
- [ ] Token/cost tracked.
- [ ] Refusal/grounding metrics tracked.
- [ ] Dashboards available.
- [ ] Runbook available.

---

## Release Readiness Checklist

- [ ] Use case approved.
- [ ] Risk tier accepted.
- [ ] Sources approved.
- [ ] Prompt version approved.
- [ ] Evaluation passed.
- [ ] Security review complete.
- [ ] Model-risk review complete where required.
- [ ] Human review workflow ready.
- [ ] Observability ready.
- [ ] User guidance ready.
- [ ] Rollback plan defined.
- [ ] Known limitations documented.

---

## Engineering Playbook

### Before Build

1. Classify use case.
2. Define data and source boundaries.
3. Define human review and allowed actions.
4. Select AI architecture pattern.
5. Define evaluation criteria.
6. Identify security/model-risk reviews.

### During Build

1. Version prompts.
2. Implement entitlement-aware retrieval.
3. Add output schema.
4. Add tests and evaluations.
5. Add audit and observability.
6. Add documentation and user guidance.

### Before Release

1. Run evaluations.
2. Run red-team/security tests.
3. Confirm evidence.
4. Confirm human review workflow.
5. Confirm monitoring and rollback.
6. Communicate limitations.

### After Release

1. Monitor usage and quality.
2. Review feedback.
3. Track cost and latency.
4. Investigate unsafe outputs.
5. Improve prompts, sources, and evaluations.
6. Recertify when material changes occur.

---

## Summary

AI governance becomes practical when it is translated into reviewable engineering checklists.

Use these checklists to build AI that is useful, safe, explainable, and supportable.
