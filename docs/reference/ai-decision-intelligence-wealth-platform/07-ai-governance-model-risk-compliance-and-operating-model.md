# 07 - AI Governance, Model Risk, Compliance and Operating Model

## 1. Why AI governance matters

AI governance ensures that AI systems are designed, deployed and monitored in a way that is lawful, ethical, robust, secure, explainable, auditable and aligned with business purpose.

In wealth platforms, governance must address:

| Risk area | Example |
|---|---|
| Conduct risk | AI creates unsuitable recommendation. |
| Model risk | AI produces inaccurate or biased output. |
| Data risk | AI uses stale, incorrect or unauthorized data. |
| Privacy risk | AI exposes client or personal data. |
| Security risk | Prompt injection or tool abuse. |
| Operational risk | Agent triggers wrong workflow or creates bad case. |
| Regulatory risk | Missing disclosures, cross-border breach or audit gap. |
| Reputation risk | Client receives misleading or inappropriate explanation. |

## 2. Governance lifecycle

```text
Use-case intake
  -> risk classification
  -> data/source review
  -> model/vendor review
  -> design and control review
  -> evaluation and validation
  -> pilot approval
  -> production release
  -> monitoring and periodic review
  -> incident management
  -> change management
  -> retirement
```

## 3. AI use-case risk tiering

| Tier | Description | Example | Controls |
|---|---|---|---|
| Low | Internal productivity, no client data or decisions | Draft internal meeting agenda | Basic review and logging. |
| Medium | Uses confidential data but no client-facing output | Summarize portfolio exceptions for support | Entitlements, monitoring, human review. |
| High | Influences advisor/client decisions | Draft proposal rationale | Evidence, suitability checks, approvals, evaluation. |
| Very high | Client-facing or action-taking | Client AI assistant, order-staging agent | Strong governance, human-in-loop, red-team testing, audit. |
| Prohibited unless approved | Autonomous advice/execution | Auto trade based on AI reasoning | Explicit policy, legal/compliance approval and strict controls. |

## 4. AI inventory

Maintain a complete inventory:

| Field | Purpose |
|---|---|
| use_case_id | Unique record. |
| owner | Business and technology accountability. |
| model/provider | Model lineage and vendor risk. |
| data domains | Privacy and entitlement review. |
| user population | RM, client, operations, developer. |
| decision impact | Informational, advisory, execution, regulatory. |
| risk tier | Control intensity. |
| approval status | Draft, pilot, production, retired. |
| evaluation status | Current validation evidence. |
| incidents | Linked operational issues. |
| periodic review date | Lifecycle control. |

## 5. Model risk management for GenAI

Traditional model validation is necessary but not sufficient for GenAI. GenAI validation must test both model behavior and application behavior.

| Validation dimension | Example test |
|---|---|
| Factual grounding | Does answer match retrieved source? |
| Hallucination | Does model invent unsupported facts? |
| Numerical correctness | Does it copy/calculate values correctly? |
| Prompt robustness | Does it follow instructions under adversarial input? |
| Bias/fairness | Does NBA ranking systematically disadvantage groups? |
| Safety/refusal | Does it refuse prohibited requests? |
| Privacy | Does it leak sensitive information? |
| Tool control | Does it call only authorized tools? |
| Drift | Does behavior change after model upgrade? |
| Explainability | Can the output be explained with evidence? |

## 6. Governance roles

| Role | Responsibility |
|---|---|
| Business owner | Defines purpose, approves business use, owns client impact. |
| Product/platform owner | Owns workflow, features and production outcome. |
| AI platform team | Provides model gateway, RAG, guardrails, monitoring. |
| Data owner | Certifies data source, quality, lineage and usage rights. |
| Model risk / validation | Reviews model behavior, evaluation, limitations and monitoring. |
| Compliance/legal | Reviews suitability, disclosure, cross-border and conduct risk. |
| Security | Reviews prompt injection, data exfiltration, access control and vendor risk. |
| Operations | Owns runbooks, incident triage and support. |
| Human reviewer | Approves client-impacting output or action. |

## 7. Control library

| Control | Description |
|---|---|
| Use-case approval | No AI use case goes live without inventory and risk tier. |
| Source approval | Only approved documents/data used for grounded answers. |
| Entitlement enforcement | Retrieval and tool calls respect user/client scope. |
| Prompt/version control | Prompts are versioned, reviewed and change-controlled. |
| Model gateway | Centralized model access with policy enforcement and logging. |
| Output validation | Schema, citation, toxicity, policy and numeric checks. |
| Human review | Mandatory for recommendations, client messages and orders. |
| Evaluation suite | Golden tests and regression checks before release. |
| Monitoring | Quality, safety, cost, latency, drift and incident metrics. |
| Kill switch | Disable use case/model/tool quickly if unsafe. |
| Incident management | Log, triage, remediate and report AI failures. |
| Periodic review | Reassess use case after changes in model, data, law or business. |

## 8. Governance artifacts

| Artifact | Purpose |
|---|---|
| Use-case assessment | Purpose, users, data, risks and controls. |
| AI impact assessment | Harm assessment, affected parties, mitigation and oversight. |
| Model card | Model capabilities, limits, training/evaluation context. |
| Data card | Data source, quality, lineage, restrictions and privacy. |
| Prompt card | Prompt purpose, version, owner and guardrails. |
| Evaluation report | Test results and known limitations. |
| Deployment approval | Sign-off evidence. |
| Monitoring dashboard | Production health and risk indicators. |
| Incident report | Issue, impact, root cause and corrective action. |

## 9. Change management

Trigger re-approval when:

- model provider or model version changes;
- prompt template changes materially;
- retrieval corpus changes materially;
- data source changes;
- tool permissions expand;
- use case becomes client-facing;
- use case starts influencing recommendations or orders;
- laws, regulations or firm policy change;
- monitoring shows quality degradation;
- incident reveals control weakness.

## 10. AI governance mapped to wealth decisions

| Wealth activity | AI governance need |
|---|---|
| Product explanation | Source grounding and disclosure controls. |
| Portfolio review | Official calculation lineage and numeric validation. |
| Suitability | Deterministic rules, evidence, human approval. |
| Rebalancing | Mandate rules, trade approvals and action limits. |
| Client messaging | Approved language, review, archiving and audit. |
| Operations triage | Clear handoff and no unauthorized data updates. |
| Engineering assistant | Code review, security scan and test evidence. |

## 11. Governance principle

AI governance should be proportionate. Low-risk productivity uses should not be overburdened, but anything that touches client advice, client data, trade execution, suitability, reporting or regulatory outcomes needs strong controls.
