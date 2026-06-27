# 07 - Agentic Workflows, Tooling and Action Governance

## 1. What agentic AI means in wealth

Agentic AI means the AI system can plan multiple steps and use tools to complete a task. In wealth management, agentic AI must be tightly controlled because actions can affect clients, portfolios, reports, compliance records and financial outcomes.

Agentic AI should begin with assistive workflows before moving toward controlled actions.

## 2. Agent maturity ladder

| Level | Description | Example |
|---|---|---|
| L0 - Chat only | No tools, generic explanation. | Explain what duration means. |
| L1 - Retrieval | Retrieves approved documents. | Explain FCN using product KB. |
| L2 - Read-only tools | Reads portfolio/calculation data. | Summarize portfolio performance. |
| L3 - Draft actions | Creates drafts/tasks for human approval. | Draft proposal or report commentary. |
| L4 - Controlled actions | Executes low-risk actions with approval. | Create task, archive report, trigger recalculation. |
| L5 - High-risk actions | Trade/order/client-impacting actions. | Should require strict workflow and human authorization. |

## 3. Agent workflow examples

### 3.1 Portfolio review preparation agent

```text
Goal: Prepare portfolio review pack for advisor.
Steps:
1. Get client and portfolio context.
2. Retrieve latest report snapshot.
3. Retrieve performance, risk and attribution analytics.
4. Check mandate and data-quality exceptions.
5. Draft executive summary.
6. Draft discussion points.
7. Create advisor review task.
8. Save draft with citations and audit trail.
```

### 3.2 Reconciliation triage agent

```text
Goal: Investigate position break.
Steps:
1. Read break details.
2. Compare source position, transaction and corporate action data.
3. Check market data/reference data changes.
4. Search historical breaks.
5. Suggest likely cause.
6. Draft remediation steps.
7. Route to owner.
```

### 3.3 Product termsheet extraction agent

```text
Goal: Extract product attributes from a structured note termsheet.
Steps:
1. Parse document.
2. Extract issuer, currency, tenor, underlying, coupon, barrier, observations.
3. Validate against schema.
4. Compare with product master.
5. Highlight mismatches.
6. Draft product governance review checklist.
```

## 4. Tool design principles

| Principle | Requirement |
|---|---|
| Narrow tools | Each tool should do one business action clearly. |
| Typed inputs | Use strict schemas, enums and validation. |
| Permissioned | Tool access depends on user role and data scope. |
| Read before write | High-risk actions should read current state and show preview. |
| Idempotent | Avoid duplicate tasks, drafts or recalculations. |
| Audited | Record user, model, prompt, tool call, input, output and outcome. |
| Reversible | Prefer draft/cancel/reverse patterns. |
| Observable | Track latency, errors, refusals, policy blocks and approvals. |

## 5. Tool categories

| Category | Examples |
|---|---|
| Knowledge tools | Search product KB, retrieve policy, read methodology. |
| Analytics tools | Get performance, risk, attribution, exposure, mandate results. |
| Workflow tools | Create task, create case, route approval, update status. |
| Reporting tools | Generate draft narrative, prepare report snapshot, archive document. |
| Product tools | Extract termsheet, compare product, check APU. |
| Operations tools | Read break, search runbook, assign issue. |
| Engineering tools | Generate QA cases, update docs draft, create ADR draft. |

## 6. Action risk classes

| Action class | Examples | Approval |
|---|---|---|
| Read-only | Retrieve portfolio data, policy, report. | No approval beyond entitlement. |
| Draft-only | Draft commentary, proposal, email, task. | Human review before sending/publishing. |
| Low-risk write | Create internal task or note. | May allow with audit. |
| Medium-risk write | Update workflow status, trigger recalculation. | Role-based approval or confirmation. |
| High-risk write | Client communication, suitability record, order draft. | Explicit human approval. |
| Prohibited autonomous write | Execute trade, change client profile, approve suitability. | AI must not act alone. |

## 7. Human-in-the-loop patterns

| Pattern | Use |
|---|---|
| Review and approve | Client-facing output, proposal draft, report commentary. |
| Review and edit | Advisor email, meeting summary, product explanation. |
| Dual approval | High-risk cross-border or complex product recommendations. |
| Exception approval | Override or missing data handling. |
| Supervisor approval | Restricted product, suitability concern, mandate breach. |

## 8. Agent memory

Agent memory should be controlled and purpose-specific.

| Memory type | Example | Control |
|---|---|---|
| Session memory | Current conversation context. | Cleared or archived per policy. |
| User preference | Advisor prefers concise summaries. | Non-sensitive, user-controlled. |
| Client memory | Client objective or preference. | Must come from approved CRM/profile. |
| Workflow memory | Current task/case state. | Stored in workflow system. |
| Model memory | Training/fine-tuning. | Avoid client-private data unless approved. |

## 9. Agent safety controls

| Risk | Control |
|---|---|
| Excessive agency | Limit tool permissions; require approval. |
| Prompt injection | Treat retrieved content as untrusted; isolate instructions from data. |
| Data leakage | Entitlement-filter before retrieval/tool call. |
| Unauthorized action | Policy engine blocks tool. |
| Hallucinated tool result | Tool outputs must be structured and signed. |
| Duplicate action | Idempotency keys. |
| Unclear accountability | Log human approver and AI contribution. |

## 10. Agent test scenarios

- Prompt injection in a retrieved product document tries to override system policy.
- User asks AI to recommend restricted product to unsuitable client.
- AI tries to access client data outside advisor book.
- Tool output is stale or missing.
- Report narrative draft uses unsupported performance explanation.
- Agent loops through repeated tool calls.
- Agent creates duplicate task.
- Agent attempts write action without approval.
- Advisor edits AI narrative; final approved version is archived with lineage.
