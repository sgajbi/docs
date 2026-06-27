# 09 - AI Governance, Risk, Security, Evaluation and Model Ops

## 1. Governance objective

AI governance ensures AI systems are useful, safe, compliant, explainable, secure, monitored and accountable.

For wealth platforms, AI governance must cover:

- client-data privacy
- suitability and advisory risk
- cross-border constraints
- product-governance restrictions
- hallucination and unsupported claims
- model bias and unfair treatment
- prompt injection and data leakage
- excessive agency and unauthorized tool use
- audit trail and human accountability
- third-party/model-provider risk

## 2. AI governance operating model

| Role | Responsibility |
|---|---|
| Business owner | Owns use case, value, workflow and user acceptance. |
| Product owner | Defines requirements, backlog and user experience. |
| Domain SME | Validates wealth/product/analytics correctness. |
| Data owner | Approves data use, quality and lineage. |
| Compliance/legal | Reviews suitability, disclosure, cross-border and client communications. |
| Model risk | Reviews model selection, limitations, evaluation and monitoring. |
| Security | Reviews access, prompt injection, data leakage and tool risk. |
| Engineering | Builds, integrates, tests and operates AI services. |
| Operations | Monitors production issues and incidents. |
| Human approver | Approves high-risk outputs/actions. |

## 3. Use-case approval checklist

Before production:

- business purpose documented
- data sources approved
- entitlement model defined
- risk classification assigned
- model/provider approved
- prompts versioned
- evaluation dataset created
- guardrails implemented
- human approval workflow defined
- audit trail implemented
- fallback behavior defined
- production monitoring configured
- incident process defined

## 4. Model risk controls

| Control | Description |
|---|---|
| Model inventory | Track model/provider/version/use case. |
| Use-case classification | Low, medium, high, very high risk. |
| Limitations statement | Document what the model cannot do. |
| Evaluation suite | Accuracy, grounding, safety, bias, robustness. |
| Change control | Prompt/model/index changes require testing. |
| Monitoring | Drift, degradation, hallucination, policy violations. |
| Human oversight | Required for high-risk outputs. |
| Decommissioning | Retire model/prompt safely. |

## 5. Evaluation dimensions

| Dimension | Example test |
|---|---|
| Factual accuracy | Does the answer match source document? |
| Citation quality | Does citation support each claim? |
| Numeric accuracy | Does narrative correctly use supplied analytics? |
| Completeness | Did it answer required sections? |
| Suitability safety | Does it avoid unsuitable recommendations? |
| Mandate safety | Does it avoid breach-causing actions? |
| Privacy | Does it avoid revealing unauthorized data? |
| Robustness | Does it handle adversarial or ambiguous input? |
| Consistency | Same input produces materially consistent answer. |
| Tone | Professional, clear, not overconfident. |

## 6. Evaluation dataset design

Create golden test sets by use case:

| Use case | Test cases |
|---|---|
| Product answer generation | Product terms, lifecycle, risks, unsuitable claims. |
| Portfolio narrative | Known performance/attribution scenarios. |
| Suitability summary | Suitable/unsuitable client-product examples. |
| Mandate explanation | Breach/no-breach scenarios. |
| Operations triage | Known reconciliation break root causes. |
| RAG retrieval | Exact-source and conflicting-source tests. |
| Agent tool use | Tool permission and approval tests. |
| Security | Prompt injection and data leakage tests. |

## 7. Security controls

| Risk | Control |
|---|---|
| Prompt injection | Treat external content as data, not instruction; use retrieval isolation and output validation. |
| Sensitive information disclosure | Entitlement filtering, masking, logging controls. |
| Excessive agency | Tool scoping, approval workflows, deny-by-default. |
| Vector-store leakage | Metadata-based access controls and tenant isolation. |
| Supply chain risk | Model/provider due diligence, SBOM/AIBOM where applicable. |
| Data poisoning | Source validation, ingestion controls, content review. |
| Unbounded consumption | Rate limits, token limits, budget controls. |
| Output misuse | Labels, disclaimers, approval states and channel controls. |

## 8. Prompt governance

Prompts are production artifacts and should be governed like code/configuration.

| Requirement | Description |
|---|---|
| Versioning | Prompt changes are versioned. |
| Owner | Each prompt has business and technical owner. |
| Approval | High-risk prompts require review. |
| Test coverage | Regression dataset linked. |
| Rollback | Previous version recoverable. |
| Monitoring | Prompt performance tracked in production. |
| Change log | Reason and impact documented. |

## 9. RAG governance

| Control | Description |
|---|---|
| Source approval | Only approved sources enter production index. |
| Effective dating | Policy/product validity is respected. |
| Access control | Retrieval respects client/user entitlements. |
| Freshness | Stale documents flagged or excluded. |
| Deletion | Removed/superseded content is not retrieved. |
| Citation verification | Claims trace to chunks. |
| Index versioning | Output can be traced to index version. |

## 10. AI audit record

Every material AI interaction should record:

- user and role
- client/account/portfolio scope
- request timestamp
- use case
- model and version
- prompt template and version
- retrieved source IDs
- structured context IDs
- tool calls and results
- policy checks
- generated response
- human edits
- approval outcome
- published output ID
- feedback and issue flags

## 11. Production monitoring

| Metric | Purpose |
|---|---|
| Hallucination/unsupported claim rate | Quality and trust. |
| Citation coverage | Grounding quality. |
| Retrieval miss rate | Knowledge-base health. |
| Human edit distance | Output usefulness. |
| Policy block rate | Control effectiveness. |
| Prompt injection attempts | Security monitoring. |
| Tool failure rate | Operational reliability. |
| Cost per use case | Commercial sustainability. |
| Latency | User experience. |
| Complaint/incident linkage | Risk monitoring. |

## 12. Governance principle

AI should not reduce accountability. It should increase transparency by making evidence, assumptions, limitations and approval paths more visible.
