# AI Security, GenAI, RAG, Agent and Model-Risk Controls

## 1. AI changes the security model

AI adds new assets, attack surfaces and governance requirements:

- prompts and conversation history
- retrieved context from knowledge bases
- vector embeddings and vector databases
- model outputs and explanations
- tool/action calls made by agents
- prompt templates and system instructions
- model versions and evaluation results
- human approvals and overrides

A wealth AI system must be secure, privacy-preserving, grounded, auditable and constrained by advisory/mandate rules.

## 2. AI threat taxonomy

| Threat | Wealth example | Control |
|---|---|---|
| Prompt injection | Uploaded document tells model to ignore policy | Instruction hierarchy, document sanitization, detection |
| Data leakage | Model reveals another client's holdings | Entitlement-aware retrieval and output filters |
| Excessive agency | Agent executes order without approval | Tool scoping, human-in-loop, workflow gates |
| Hallucination | Model invents performance reason | Grounded RAG, source citations, calculation lineage |
| Model inversion/extraction | Attacker probes model for training data | Do not train on confidential data without governance |
| Vector leakage | Unauthorized document appears in retrieval | Metadata-based access filtering |
| Poisoned knowledge | Bad product rule added to knowledge base | Knowledge approval workflow and versioning |
| Unsafe recommendation | Product suggestion violates suitability | Suitability and product governance integration |
| Secret leakage | Prompt includes API key or token | Secret scanning/redaction and prompt policy |
| Bias/unfair treatment | Segment-specific inappropriate suggestions | Evaluation and governance controls |

## 3. RAG security controls

| RAG component | Control |
|---|---|
| Source documents | Approved source list, classification, owner |
| Ingestion | Malware scan, content validation, metadata extraction |
| Chunking | Preserve entitlement and source metadata |
| Embeddings | Protect vector store; avoid leaking sensitive text in logs |
| Retrieval | Filter by user/client/product entitlement |
| Reranking | Do not promote unauthorized content |
| Prompt assembly | Redact unnecessary PII; cite sources |
| Output | Grounding check, policy check, client-safe phrasing |
| Audit | Store source IDs, prompt template version, model version |

## 4. Agentic workflow controls

AI agents should operate under explicit capability boundaries.

| Agent action | Control pattern |
|---|---|
| Read portfolio | User-entitled API only |
| Summarize report | Use existing report snapshot and lineage |
| Draft proposal | Draft only; human advisor approval |
| Generate trade list | Pre-trade suitability and mandate checks; no direct execution |
| Send client message | Human review and channel approval |
| Create ticket | Allowed with audit trail |
| Update production data | Usually prohibited; require controlled workflow |
| Access secrets | Prohibited |

## 5. Human-in-the-loop patterns

| Pattern | Use |
|---|---|
| Human review | Output visible before use |
| Human approval | Required before client/advisor action |
| Dual approval | High-risk transactions or overrides |
| Human override | User can correct AI result with reason |
| Human escalation | Compliance/security review for restricted cases |
| Human attestation | Advisor confirms generated content before delivery |

## 6. AI output controls

AI output should be checked for:

- unsupported factual claims
- missing source references
- suitability issues
- product-governance violations
- client confidentiality issues
- prohibited instructions
- overly certain language
- outdated data
- calculation mismatch
- cross-border distribution breach

## 7. AI audit event

```json
{
  "event_type": "AI_RESPONSE_GENERATED",
  "actor_id": "advisor-123",
  "client_id": "cif-456",
  "use_case": "PORTFOLIO_REVIEW_SUMMARY",
  "model": "approved-model-vX",
  "prompt_template_version": "portfolio-review-v3",
  "retrieved_source_ids": ["report-2026Q2", "mandate-policy-v5"],
  "entitlement_policy_version": "wealth-access-2026.06",
  "safety_result": "PASS",
  "human_approval_required": true,
  "tool_calls": [],
  "correlation_id": "corr-ai-789"
}
```

## 8. AI evaluation dimensions

| Dimension | Test question |
|---|---|
| Grounding | Is answer supported by retrieved sources? |
| Entitlement | Does retrieval respect user/client scope? |
| Accuracy | Are calculations and facts correct? |
| Suitability | Does recommendation respect client profile? |
| Safety | Does output avoid disallowed instructions? |
| Privacy | Does output avoid unnecessary sensitive data? |
| Robustness | Does prompt injection fail safely? |
| Consistency | Does same scenario produce stable answer? |
| Explainability | Can user understand basis and limitations? |
| Auditability | Can we reconstruct source, prompt, model and approval? |

## 9. AI model risk governance

Governance should define:

- use-case classification by risk
- model/provider approval
- data-use approval
- evaluation benchmarks
- drift monitoring
- incident process
- retraining/re-indexing governance
- human oversight requirements
- client disclosure rules where applicable
- retirement/deprecation process

## 10. Wealth AI guardrail principles

- AI may assist; it should not silently decide.
- AI should not access more data than the user can access.
- AI should not recommend products outside approved universe.
- AI should not bypass suitability or mandate checks.
- AI should not execute trade, payment or account changes without controlled workflow.
- AI should cite approved sources for factual/product explanations.
- AI outputs should be auditable and reproducible enough for governance.
