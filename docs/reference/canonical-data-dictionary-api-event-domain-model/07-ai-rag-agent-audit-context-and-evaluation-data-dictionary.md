# 07 - AI, RAG, Agent, Audit Context and Evaluation Data Dictionary

## 1. AI interaction

| Field | Type | Description |
|---|---|---|
| ai_interaction_id | string | AI interaction ID |
| user_id | string | user |
| user_role | string | advisor, PM, operations, client, admin |
| session_id | string | conversation/session |
| use_case | enum | portfolio_summary, product_explanation, suitability_assist, operations_triage |
| input_prompt | text | user prompt, protected by retention policy |
| normalized_intent | string | interpreted task |
| output_text | text | AI response |
| output_status | enum | draft, reviewed, approved, rejected, expired |
| model_provider | string | model provider |
| model_name | string | model |
| model_version | string | version |
| created_at | datetime | timestamp |
| retention_policy_id | string | retention rule |

## 2. Retrieval record

| Field | Type | Description |
|---|---|---|
| retrieval_id | string | retrieval record |
| ai_interaction_id | string | interaction |
| query_text | text | retrieval query |
| source_document_id | string | document |
| source_chunk_id | string | chunk |
| source_type | enum | policy, product_pack, report, calculation, termsheet, client_data |
| source_effective_date | date | source effective date |
| relevance_score | decimal | score |
| used_in_answer | boolean | cited/used |
| citation_label | string | citation label |

## 3. AI context scope

| Field | Type | Description |
|---|---|---|
| context_scope_id | string | scope |
| ai_interaction_id | string | interaction |
| party_id | string | client scope |
| account_id | string | account scope |
| portfolio_id | string | portfolio scope |
| entitlement_check_id | string | access check |
| data_classification | enum | public, internal, confidential, restricted |
| pii_present | boolean | PII indicator |
| client_data_used | boolean | client data used |
| approved_for_client_output | boolean | whether output may be shown to client |

## 4. Tool invocation

| Field | Type | Description |
|---|---|---|
| tool_invocation_id | string | tool call ID |
| ai_interaction_id | string | interaction |
| tool_name | string | tool |
| action_type | enum | read, calculate, draft, write, approve, send, trade |
| input_summary | text | summarized input |
| output_summary | text | summarized output |
| allowed_by_policy | boolean | policy allowed |
| human_approval_required | boolean | approval required |
| human_approval_id | string | approval link |
| status | enum | proposed, approved, executed, rejected, failed |

## 5. AI evaluation

| Field | Type | Description |
|---|---|---|
| evaluation_id | string | evaluation |
| ai_interaction_id | string | interaction |
| eval_suite_id | string | test suite |
| metric | enum | groundedness, correctness, refusal, safety, tone, citation_quality |
| score | decimal | score |
| pass_fail | enum | pass, fail, warning |
| evaluator | enum | human, automated, llm_judge |
| evaluation_notes | text | notes |
| evaluated_at | datetime | time |

## 6. AI governance approval

| Field | Type | Description |
|---|---|---|
| ai_use_case_id | string | use case |
| use_case_name | string | name |
| risk_tier | enum | low, medium, high, prohibited |
| approved_models | array | model list |
| approved_tools | array | tool list |
| approved_data_sources | array | sources |
| prohibited_actions | array | actions not allowed |
| human_approval_required | boolean | human oversight |
| evaluation_required | boolean | eval required |
| approval_status | enum | draft, approved, restricted, rejected, retired |
| approval_date | date | date |
| next_review_date | date | review |

## 7. Prompt template

| Field | Type | Description |
|---|---|---|
| prompt_template_id | string | template |
| use_case | string | use case |
| template_text | text | prompt template |
| input_variables | array | variables |
| guardrails | array | guardrail instructions |
| output_schema | string | expected output schema |
| version | string | version |
| status | enum | draft, approved, retired |
| owner | string | owner |

## 8. Validation rules

- AI access must use same entitlements as non-AI workflows.
- client data retrieval requires approved scope.
- tool writes/actions require policy and human approval if material.
- AI recommendation workflows must link to suitability/mandate checks.
- outputs used in client communication must be reviewed/approved.
- source documents must be effective and approved.
- model version must be recorded.
- evaluation failures must block or escalate use.

## 9. Key takeaway

AI data models must capture context, sources, tools, approvals, outputs, evaluations and lineage.
