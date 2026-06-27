# 05 - Agentic Workflows, Tool Use, Actions and Human Oversight

## 1. What is an AI agent?

An AI agent is a system that can plan and perform multi-step tasks using tools. In a wealth platform, a tool may be an API, database query, document retrieval service, calculation service, workflow engine, order system, case system or message service.

Agents are powerful because they can orchestrate work. They are risky because they can take action.

## 2. Wealth agent examples

| Agent type | Example task |
|---|---|
| Advisor preparation agent | Prepare meeting pack from portfolio, research, tasks and client profile. |
| Portfolio-review agent | Generate report narrative and exception explanation. |
| Proposal agent | Draft investment proposal and route to suitability checks. |
| Rebalance agent | Detect drift and prepare trade list for approval. |
| Operations agent | Investigate reconciliation break and create case. |
| Data-quality agent | Detect stale prices, missing benchmarks or inconsistent classifications. |
| Engineering agent | Generate test cases, ADRs and documentation updates. |
| Compliance agent | Review draft client message against disclosure and policy requirements. |

## 3. Agent action taxonomy

| Action type | Example | Risk |
|---|---|---|
| Read-only retrieval | Fetch product docs, portfolio facts, client profile | Low to medium |
| Calculation request | Run performance/risk/suitability check | Medium |
| Draft creation | Draft email/proposal/report note | Medium |
| Case creation | Open support/compliance case | Medium |
| Workflow routing | Submit for approval | Medium/high |
| Data update | Update client preference or task status | High |
| Order staging | Create draft order | High |
| Order submission | Submit trade to OMS | Very high |
| Client communication | Send message to client | Very high |
| Autonomous execution | Trade/rebalance without human approval | Highest |

## 4. Human oversight tiers

| Tier | Description | Example |
|---|---|---|
| Human-in-the-loop | Human must approve before action. | Send client message, submit order, override breach. |
| Human-on-the-loop | AI acts within limited scope; human monitors and can intervene. | Batch classify low-risk operations cases. |
| Human-over-the-loop | Humans review metrics, incidents and samples. | Internal summarization assistant. |
| No autonomous action | AI may only explain or draft. | Product learning assistant. |

For regulated wealth decisions, the default should be human-in-the-loop.

## 5. Tool permission model

Agents need least-privilege tool access.

| Permission | Example |
|---|---|
| portfolio.read | Read positions, transactions and analytics for entitled portfolios. |
| product.read | Retrieve product master and documents. |
| policy.read | Retrieve suitability, mandate and cross-border rules. |
| calculation.run | Run performance, risk, suitability or buying-power checks. |
| proposal.create_draft | Create draft proposal only. |
| workflow.submit | Submit draft for approval. |
| order.stage | Stage order but not submit. |
| order.submit | Submit order, restricted to controlled workflows. |
| message.draft | Draft client message. |
| message.send | Send message only after approval and confirmation. |

Permissions should be scoped by user role, client relationship, booking centre, data classification and workflow state.

## 6. Agent workflow design

A safe agent workflow should include:

1. Intent classification.
2. User entitlement check.
3. Data/source retrieval.
4. Deterministic calculation/rule execution where applicable.
5. AI reasoning and drafting.
6. Guardrail validation.
7. Human review/approval.
8. Action execution.
9. Audit logging.
10. Post-action monitoring.

## 7. Example: AI-assisted rebalance workflow

```text
Trigger: portfolio drift above threshold

1. Retrieve portfolio, model target and mandate rules.
2. Run drift, risk, liquidity, concentration and tax/cost checks.
3. Generate candidate trades using rebalancing engine.
4. Run pre-trade suitability, mandate and buying-power checks.
5. AI drafts rationale and client/advisor explanation.
6. Advisor reviews proposal.
7. IC/compliance approves if required.
8. Orders are staged in OMS.
9. Human submits orders.
10. Post-trade report compares expected vs actual outcome.
```

The AI agent does not replace the rebalancing engine, suitability engine or OMS. It orchestrates and explains them.

## 8. Agent safety constraints

| Constraint | Purpose |
|---|---|
| No hidden actions | User must know when action is taken. |
| No tool access without authorization | Prevent data leakage and unauthorized trades. |
| No order submission without explicit approval | Prevent accidental execution. |
| No client message without review | Prevent inappropriate advice or disclosure. |
| No override of deterministic controls | Prevent mandate/suitability bypass. |
| No use of unapproved sources for advice | Prevent misinformation. |
| No memory reuse across clients unless permitted | Prevent confidentiality breach. |
| No self-modification of policies | Prevent governance bypass. |

## 9. Agent audit log

Each agent run should record:

| Field | Purpose |
|---|---|
| agent_run_id | Traceability. |
| user_id | Who initiated. |
| role/entitlements | Authorization context. |
| client/account/portfolio scope | Data boundary. |
| intent | Why agent ran. |
| retrieved_sources | Evidence. |
| tools_called | Operational lineage. |
| calculations_run | Deterministic outputs. |
| prompt_version | Reproducibility. |
| model_version | Model lineage. |
| output | Draft/explanation/action. |
| approvals | Human oversight evidence. |
| final_action | What changed. |
| rollback_reference | Recovery path. |

## 10. Agent incident scenarios

| Incident | Example | Control response |
|---|---|---|
| Excessive agency | Agent sends message without review. | Kill switch, incident, tool permission review. |
| Wrong client scope | Agent uses household member data incorrectly. | Entitlement audit and data scoping fix. |
| Prompt injection | Malicious document instructs agent to ignore policy. | Retrieval sanitization and instruction hierarchy. |
| Tool misuse | Agent calls order API outside workflow. | Tool gateway policy enforcement. |
| Poor output | Agent misstates risk. | Evaluation gap, prompt/model update, reviewer training. |
| Cost runaway | Agent loops through expensive model calls. | Budget limit and run timeout. |

## 11. Agent design principle

Agents should be designed around **bounded autonomy**:

```text
AI may plan, retrieve, summarize, explain and draft.
AI may request deterministic checks.
AI may prepare actions.
Humans and governed workflow decide and approve consequential actions.
```
