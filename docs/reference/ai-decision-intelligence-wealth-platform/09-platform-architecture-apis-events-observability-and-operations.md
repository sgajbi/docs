# 09 - Platform Architecture, APIs, Events, Observability and Operations

## 1. Target AI platform architecture

```text
Client / advisor / operations UI
  -> AI experience layer
  -> AI orchestration service
  -> policy and entitlement gateway
  -> retrieval service
  -> calculation and rules services
  -> model gateway
  -> tool gateway
  -> audit and evaluation store
  -> monitoring and incident management
```

The architecture should be modular so each capability can evolve independently.

## 2. Major services

| Service | Responsibility |
|---|---|
| AI orchestration service | Intent classification, prompt assembly, tool planning and response composition. |
| Model gateway | Central access to approved LLMs/embedding models with logging and policy. |
| Retrieval service | Entitlement-aware search over approved knowledge and documents. |
| Tool gateway | Controlled access to APIs, calculation services and workflow actions. |
| Policy guardrail service | Use-case rules, refusals, output checks and action permissions. |
| Evaluation service | Offline and online quality/safety testing. |
| Audit service | Stores evidence, prompts, context, outputs, tool calls and approvals. |
| Feedback service | Captures corrections, ratings, reviewer notes and improvement tickets. |
| Monitoring service | Latency, cost, usage, quality, safety and incident metrics. |

## 3. API patterns

### Ask API

```http
POST /ai/ask
```

Purpose: answer a question with grounding and controls.

Important request fields:

| Field | Purpose |
|---|---|
| user_id | Entitlement and audit. |
| use_case_id | Risk tier and prompt selection. |
| scope | Client/account/portfolio/product scope. |
| question | User request. |
| requested_output_type | Summary, explanation, draft, table, JSON. |
| source_requirements | Product docs, policy, portfolio snapshot, calculation. |

### Draft API

```http
POST /ai/draft
```

Purpose: create controlled draft for review, not send.

Examples: client email, proposal rationale, meeting note, report commentary.

### Agent Run API

```http
POST /ai/agent-runs
```

Purpose: start a governed multi-step workflow.

Key controls: allowed tools, max steps, max cost, required approvals, rollback policy.

## 4. Events

AI should participate in event-driven architecture.

| Event | Purpose |
|---|---|
| ai.requested | AI request submitted. |
| ai.context.retrieved | Sources and data snapshots selected. |
| ai.tool.called | Tool invocation recorded. |
| ai.output.generated | Draft or answer created. |
| ai.output.reviewed | Human reviewed and accepted/rejected. |
| ai.action.approved | Workflow approved action. |
| ai.action.executed | Action completed. |
| ai.feedback.received | Feedback captured. |
| ai.incident.detected | AI issue detected. |
| ai.evaluation.completed | Evaluation run completed. |

Events must not contain unnecessary client-sensitive payloads. Use identifiers and secure retrieval.

## 5. Integration with existing wealth services

| Existing domain | AI integration |
|---|---|
| Client master | Profile, household, entitlements, tax residency, suitability. |
| Product master | Product taxonomy, risk rating, target market, terms. |
| Portfolio engine | Holdings, transactions, cash, positions, valuations. |
| Performance engine | TWR, MWR, contribution, attribution, benchmarks. |
| Risk engine | VaR, stress, drawdown, concentration, exposure. |
| Suitability engine | Rule results and failure reasons. |
| Mandate engine | Drift, breaches and restrictions. |
| Reporting engine | Report snapshots and narratives. |
| OMS/workflow | Proposal, approval, order staging and execution lifecycle. |
| CRM | Notes, tasks, interactions and preferences. |
| Document archive | Statements, contracts, approvals and source evidence. |

## 6. Observability

| Metric | Example |
|---|---|
| Usage | Requests by use case, user group and channel. |
| Quality | Reviewer acceptance rate, correction rate, citation accuracy. |
| Safety | Refusal correctness, policy violations, prompt injection attempts. |
| Grounding | Retrieval precision, missing-source rate, unsupported-claim rate. |
| Tool behavior | Tool call success/failure, unauthorized-call blocks. |
| Cost | Token usage, model cost, retrieval cost, agent run cost. |
| Performance | Latency, timeout, queue time. |
| Reliability | Error rate, fallback rate, degraded-state frequency. |
| Drift | Changes after model/prompt/source updates. |

## 7. Deployment environments

| Environment | Purpose |
|---|---|
| Sandbox | Experiment with fake/anonymized data. |
| Development | Build prompts, RAG and integrations. |
| Validation | Run golden tests and red-team suites. |
| Pilot | Limited users, real workflows, strong monitoring. |
| Production | Approved use cases only. |
| Break-glass / disabled state | AI off, standard platform continues. |

## 8. Prompt and model release process

```text
Change proposal
  -> prompt/model update
  -> offline tests
  -> security tests
  -> business sample review
  -> model-risk/compliance approval if required
  -> canary deployment
  -> monitoring
  -> full rollout or rollback
```

## 9. Production support runbook

Support should be able to answer:

- Which user asked the question?
- Which client/portfolio/product was in scope?
- Which model and prompt version were used?
- Which sources were retrieved?
- Which calculations and tools were called?
- What output was generated?
- Was output reviewed?
- Was any action executed?
- Did monitoring detect an issue?
- Was there a similar historical incident?

## 10. Architecture anti-patterns

| Anti-pattern | Why dangerous |
|---|---|
| Direct UI-to-LLM calls | No governance, lineage, entitlement or control. |
| LLM decides suitability | Non-deterministic and unauditable. |
| One vector index for all clients | Cross-client leakage risk. |
| Prompt contains secrets | Prompt leakage risk. |
| AI sends client messages directly | Conduct and disclosure risk. |
| No golden tests | Model/prompt changes break silently. |
| No fallback | Business workflow depends on AI availability. |
| No audit trail | Cannot investigate client-impacting output. |

## 11. Platform principle

Build AI as an enterprise platform service with controlled APIs, not as one-off copilots embedded separately in each application.
