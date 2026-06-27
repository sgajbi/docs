# Technical Practice Lab Sample Answers

Use these examples with [`technical-practice-labs-and-case-studies.md`](technical-practice-labs-and-case-studies.md). They show the level of practical, evidence-backed output expected from a lab.

The examples are synthetic and source-safe. They avoid product names, client identifiers, local paths and private repository labels, while still showing the kind of reasoning, evidence and recommendation a strong engineer should produce.

## How To Read These Examples

Each sample answer follows this pattern:

1. scenario and artifact reviewed,
2. evidence inspected,
3. current-state assessment,
4. risks or gaps,
5. recommendation,
6. smallest useful improvement,
7. validation evidence expected,
8. follow-up trigger.

The point is not to copy the wording. The point is to learn how to turn technical reading into a defensible engineering judgement.

## Sample Answer Index

| Sample | Lab | Focus |
|---:|---|---|
| 1 | Lab 1 | Service boundary review for portfolio summary ownership. |
| 2 | Lab 2 | Backward-compatible API change for adding supportability metadata. |
| 3 | Lab 3 | Data-product certification for daily performance returns. |
| 4 | Lab 4 | CI gate promotion from report-only to blocking. |
| 5 | Lab 5 | Incident and observability review for stale report output. |
| 6 | Lab 8 | Async workflow resilience for batch report generation. |
| 7 | Lab 11 | AI/RAG governance for an internal knowledge assistant. |
| 8 | Lab 12 | Migration and cutover readiness for portfolio holdings migration. |

## 1. Service Boundary Review

Lab: service boundary review  
System or artifact reviewed: portfolio summary capability across domain service, experience API and UI panel  
Guides used: architecture boundaries, backend service design, wealth domain boundaries  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Domain API contract | Domain service exposes holdings, cash, market value, valuation date and source freshness. |
| Experience API contract | Experience API composes portfolio summary, risk indicator and latest activity for a UI panel. |
| UI view model | UI receives a summary object and renders degraded-state badges. |
| Test suite | Unit tests exist for domain valuation mapping; browser tests only verify ready state. |
| Runbook | Runbook explains source delay but does not name the owning service for each summary field. |

Current state:

The boundary is mostly correct. The domain service owns portfolio valuation, holding aggregation and source freshness. The experience API composes the domain summary with risk and activity snapshots. The UI renders the response and should not calculate business values.

Risk or gap:

The experience API currently derives a `has_concentration_warning` flag from holding weights. That is a domain or risk-service rule, not an experience-layer rule. If the threshold changes in the risk service but not in the experience API, the UI can show a warning that disagrees with risk reporting.

Decision or recommendation:

Move concentration-warning ownership into the risk or portfolio analytics owner. The experience API should consume the warning and preserve the supportability state instead of recalculating it.

Smallest useful improvement:

Add a `concentration_warnings` field to the authoritative upstream response or remove the derived flag until the owning service provides it. Add a contract test proving the experience API passes through warning state without recomputation.

Validation evidence expected:

| Evidence | Acceptance signal |
|---|---|
| Contract update | Authoritative service contract includes warning semantics or explicitly marks the field unsupported. |
| Experience API test | Test proves pass-through mapping and stale/partial state handling. |
| Browser test | UI renders ready, stale and unavailable warning states from the API response. |
| Documentation update | Boundary note names the owner of valuation, exposure, warning and UI composition. |

Follow-up trigger:

Revisit the boundary if a second UI or report starts consuming the same warning. That is the signal that the rule should become a formal domain contract, not a local panel helper.

## 2. Backward-Compatible API Change

Lab: backward-compatible API change  
System or artifact reviewed: `GET /api/v1/accounts/{account_id}/balances` response contract  
Guides used: API-as-contract, versioning and deprecation, API contract testing  

Evidence inspected:

| Evidence | Observation |
|---|---|
| OpenAPI response schema | Current response includes `available_cash`, `settled_cash` and `as_of_date`. |
| UI consumer | UI displays balances and disables trading actions when cash is unavailable. |
| Reporting consumer | Daily account report stores available cash but does not consume supportability fields. |
| Error model | API uses structured problem details for unavailable account and permission-blocked states. |
| Tests | Contract tests verify required fields but do not verify additive compatibility. |

Current state:

The API can add supportability metadata without breaking existing consumers if the new field is optional and examples are updated. The current response does not tell consumers whether a balance is fresh, stale or partially available.

Proposed contract delta:

```json
{
  "account_id": "SYNTH_ACC_001",
  "available_cash": 125000.00,
  "settled_cash": 118500.00,
  "as_of_date": "2026-06-26",
  "supportability": {
    "state": "ready",
    "source_timestamp": "2026-06-26T17:30:00Z",
    "source": "cash-ledger",
    "warnings": []
  }
}
```

Compatibility assessment:

| Consumer | Risk | Mitigation |
|---|---|---|
| UI | Low | Add optional rendering for supportability badges; old response remains parseable. |
| Reporting | Low | Ignore unknown field; add report test to confirm no layout change. |
| API clients | Medium if strict deserializers reject unknown fields | Publish compatibility note and generated-client smoke test. |

Deprecation plan:

No immediate field removal. The existing `as_of_date` remains supported. If `as_of_date` later becomes redundant, mark it deprecated only after all consumers use `supportability.source_timestamp` and reporting output is unchanged.

Smallest useful improvement:

Add optional `supportability` metadata with OpenAPI examples, contract tests and one generated-client compatibility check.

Validation evidence expected:

1. OpenAPI diff shows only additive optional fields.
2. Existing response example remains valid.
3. Contract test verifies old minimal response and new enriched response.
4. UI smoke test shows ready and stale states.
5. Release note states the field is additive and optional.

Follow-up trigger:

If supportability metadata is added to more than one API family, standardize the field shape and error-state vocabulary across APIs.

## 3. Data-Product Certification

Lab: data-product certification  
System or artifact reviewed: daily portfolio performance returns data product  
Guides used: data-product engineering, SLO/access/evidence policy, trust telemetry  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Data contract | Defines `portfolio_id`, `period_start`, `period_end`, `return_type`, `return_value`, `currency`, `methodology_version`. |
| Lineage note | Inputs include holdings snapshot, cashflows, market prices and benchmark calendar. |
| Quality rules | Completeness and reconciliation rules exist; freshness SLO is not explicit. |
| Consumer list | Performance dashboard and report generation consume the dataset. |
| Telemetry | Freshness metric exists at job level but not product/version level. |

Current state:

The data product is close to certifiable for internal analytics and reporting use. The main gap is trust evidence at the product level. Consumers can see that the job completed, but cannot reliably tell whether this specific product version is fresh and complete.

Certification decision:

Conditionally certify as `visible` but not `certified` until product-level trust telemetry is added.

Required trust rules:

| Rule | Threshold | Failure behavior |
|---|---:|---|
| Freshness | Published by 07:00 local business time for prior business day. | Mark product `stale`; block client-ready report publication. |
| Completeness | 100% of in-scope portfolios either calculated or explicitly excluded with reason. | Mark product `partial`; expose excluded portfolio count. |
| Reconciliation | Aggregate return source counts match holdings/cashflow input coverage. | Mark product `quality_blocked`; require operator review. |
| Methodology version | Output stores methodology version used for each row. | Block certification if missing. |

Smallest useful improvement:

Add product-level `freshness_state`, `last_certified_at`, `completeness_rate`, `failed_portfolio_count` and `methodology_version` telemetry. Update the consumer guide to explain `ready`, `stale`, `partial` and `quality_blocked`.

Validation evidence expected:

1. Data-product contract includes trust fields.
2. Certification test simulates stale input and expects `stale` state.
3. Report-generation test blocks client-ready output for `quality_blocked`.
4. Dashboard shows product state without exposing portfolio-sensitive details.
5. Certification result records owner, date, scope and expiry.

Follow-up trigger:

If AI retrieval or advisor commentary consumes this product, add a consumer-specific trust rule requiring citation to methodology version and freshness state.

## 4. CI Gate Promotion

Lab: CI gate promotion  
System or artifact reviewed: API contract drift check currently running as report-only  
Guides used: CI/CD evidence production, gate promotion, green main discipline  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Report-only history | Last 30 runs: 29 passed, 1 failed due to missing generated contract update. |
| Runtime | Median 42 seconds, p95 58 seconds. |
| Failure output | Names changed endpoint and expected regeneration command. |
| Owner | API platform owner listed in quality scorecard. |
| Bypass policy | Not documented. |

Current state:

The check is deterministic, fast enough for PR lane and directly protects consumer safety. It is a good candidate for blocking promotion after documenting bypass and exception handling.

Gate purpose:

Prevent code and OpenAPI contract drift before merge. This protects generated clients, documentation, contract tests and consumer impact analysis.

Promotion decision:

Promote from report-only to blocking in the PR merge lane after one final week of warning mode and a documented exception path.

Rollout plan:

| Step | Action | Exit criterion |
|---:|---|---|
| 1 | Keep report-only and announce intended promotion. | No unresolved false-positive issue. |
| 2 | Run warning mode for one week. | Failures are actionable and resolved by owning teams. |
| 3 | Make blocking for API-affecting PRs. | Required check configured and documented. |
| 4 | Review after first 20 blocking runs. | Runtime and false-positive rate remain acceptable. |

Smallest useful improvement:

Add a short gate runbook with purpose, owner, expected failure output, regeneration command, bypass approval rule and scorecard location.

Validation evidence expected:

1. CI workflow marks the check required for API contract changes.
2. Failure output includes changed endpoint and remediation command.
3. PR template asks for contract-drift evidence when APIs change.
4. Quality scorecard records promotion date and owner.
5. Bypass requires named owner approval and follow-up issue.

Follow-up trigger:

If the check exceeds two minutes p95 or produces two false positives in a month, demote to warning and open a gate-quality improvement item.

## 5. Incident And Observability Review

Lab: incident and observability review  
System or artifact reviewed: stale report output after delayed market-data load  
Guides used: observability/SRE, incident command, problem management  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Alert timeline | Batch delay alert fired 18 minutes after expected completion. |
| Dashboard | Job status and report-generation panels were separate; no panel showed affected reports. |
| Report API response | Response included `generated_at` but not source freshness state. |
| Runbook | Market-data replay steps exist; report-cache invalidation steps are missing. |
| Incident notes | Impact was identified manually from report ids generated during delay window. |

Current-state assessment:

Detection existed, but it was too indirect. Operators knew the upstream batch was late, but not which reports were unsafe to publish. The report capability lacked a clear stale-state contract and recovery runbook.

Severity decision:

Severity should be based on client or business output exposure, not only system failure. In this case, classify as high severity if client-ready reports were published with stale values; otherwise classify as medium severity with publication block and expedited recovery.

Root-cause summary:

The triggering event was delayed market-data ingestion. The contributing control gap was that report generation did not bind each report to source freshness state, so stale-source output was not automatically blocked.

Corrective actions:

| Action | Owner | Expected evidence |
|---|---|---|
| Add source freshness to report snapshot metadata. | Reporting owner | Snapshot schema and regression test. |
| Block client-ready publication when required source state is stale. | Reporting owner | API and workflow tests. |
| Add affected-report panel to dashboard. | SRE owner | Dashboard screenshot or exported panel definition. |
| Extend runbook with cache invalidation and report regeneration. | Operations owner | Runbook update and drill result. |

Smallest useful improvement:

Add `source_freshness_state` and `source_as_of` to report snapshot metadata, then block publication when mandatory sources are stale.

Validation evidence expected:

1. Synthetic stale-source test blocks client-ready report publication.
2. Dashboard shows number of affected reports by report type and source.
3. Runbook includes replay, cache invalidation, regeneration and validation steps.
4. Incident follow-up confirms no sensitive client data appears in logs or screenshots.

Follow-up trigger:

Run a quarterly stale-source drill for reporting flows that depend on market data, positions, cashflows or benchmark files.

## 6. Async Workflow Resilience

Lab: async workflow resilience  
System or artifact reviewed: batch report-generation workflow  
Guides used: async execution, worker queues, observability for workers  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Job API | Caller creates report job and polls status endpoint. |
| Worker behavior | Worker retries transient rendering failures but does not persist retry reason history. |
| Idempotency | Duplicate submission can create a second job if submitted after 10 minutes. |
| Dead-letter handling | Dead-letter queue exists but replay is manual and not audited. |
| Metrics | Counts succeeded and failed jobs; no metric for retry age or stuck jobs. |

Current-state assessment:

The workflow is usable but not fully resilient. The main risks are duplicate report generation, weak replay auditability and limited stuck-job detection.

Recommended job state model:

```text
accepted -> queued -> running -> succeeded
                            -> retrying -> running
                            -> failed_retryable -> retrying
                            -> failed_terminal
                            -> cancelled
                            -> expired
```

Idempotency rule:

Use a caller-supplied idempotency key or deterministic request hash covering report type, portfolio scope, as-of date, template version and requested output format. A duplicate request with the same hash returns the existing job. A duplicate key with a different hash returns conflict.

Recovery playbook:

| Recovery action | Control |
|---|---|
| Retry transient failure | Automatic retry with bounded backoff and retry reason recorded. |
| Replay dead-lettered job | Operator action requiring reason, job id, prior failure and approval. |
| Cancel unsafe job | Operator action audited with caller and reason. |
| Expire old result | Retention job records expiry and prevents stale download. |

Smallest useful improvement:

Persist retry history and add a `stuck_job_age_seconds` metric with alert threshold for jobs in `queued`, `running` or `retrying` longer than expected.

Validation evidence expected:

1. Duplicate request test returns existing job for same request hash.
2. Conflict test rejects same idempotency key with different request hash.
3. Worker restart test resumes or safely retries in-flight job.
4. Dead-letter replay records operator, reason and replay result.
5. Dashboard shows queued, running, retrying, failed and stuck-job counts.

Follow-up trigger:

If report volumes increase or rendering time becomes variable, add capacity planning for worker concurrency, queue depth, template complexity and output size.

## 7. AI/RAG Or Copilot Governance

Lab: AI/RAG or copilot governance  
System or artifact reviewed: internal engineering knowledge assistant  
Guides used: AI governance, entitlement-aware retrieval, model-risk evaluation  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Source list | Approved docs include technical guides, runbooks and architecture notes. |
| Retrieval policy | Retrieval excludes secrets but does not yet enforce document-level entitlement metadata. |
| Prompt template | System prompt requires citations but does not define refusal behavior for unsupported claims. |
| Evaluation set | Small factuality set exists; no prompt-injection or sensitive-data tests. |
| Tool access | Assistant is read-only today; future write tools are proposed. |

Current-state assessment:

The assistant is acceptable for low-risk read-only knowledge lookup if source quality and entitlement controls are improved before broader rollout. It is not ready for action-taking workflows.

Source policy:

| Source type | Allowed use | Required control |
|---|---|---|
| Published technical docs | General engineering guidance | Owner and review date. |
| Runbooks | Operational guidance | Environment scope and sensitive-data review. |
| Architecture decisions | Design rationale | Decision status: proposed, accepted, superseded or retired. |
| Incident records | Learning only | Redaction and restricted access. |

Entitlement model:

Retrieval must filter documents before generation based on user role, document classification and repository or platform area. The model should not receive text the user is not allowed to see.

Evaluation set:

| Test class | Example expectation |
|---|---|
| Citation quality | Answer includes traceable source references for factual claims. |
| Unsupported question | Assistant says evidence is missing instead of inventing. |
| Prompt injection | Retrieved text cannot override system policy or expose restricted content. |
| Sensitive data | Assistant refuses to reveal secrets, tokens, client data or restricted evidence. |
| Freshness | Stale source is identified when review date is expired. |

Smallest useful improvement:

Add document-level metadata for classification, owner, review date and allowed audience; enforce pre-generation retrieval filtering; add prompt-injection and sensitive-data regression tests.

Validation evidence expected:

1. Retrieval test proves restricted docs are not returned for unauthorized users.
2. Evaluation set includes factuality, citation, refusal and prompt-injection cases.
3. Assistant logs contain source ids, policy decisions and no sensitive prompt payloads.
4. Tool access remains disabled until action authorization and approval flow are designed.

Follow-up trigger:

Before any write-capable tool is added, require a tool-authorization design, human approval model, audit trail and rollback behavior.

## 8. Migration And Cutover Readiness

Lab: migration and cutover readiness  
System or artifact reviewed: migration of portfolio holdings from legacy store to target domain service  
Guides used: migration engineering, reconciliation strategy, cutover governance  

Evidence inspected:

| Evidence | Observation |
|---|---|
| Inventory | Portfolio, account, holding, price, cash and restriction records are listed. |
| Mapping rules | Security identifier and quantity mappings are clear; cost basis rules need sign-off. |
| Parallel run | Two weeks of comparison exists for holdings count and market value. |
| Reconciliation | Breaks are categorized, but tolerances are not approved by business owner. |
| Cutover runbook | Sequencing exists; rollback trigger and command-center roles are incomplete. |

Current-state assessment:

The migration is not ready for production cutover. Data mapping is strong for holdings identity and quantity, but financial value, cost basis and exception governance need stronger approval evidence.

Readiness decision:

Proceed with another mock cutover, not production cutover.

Reconciliation rules:

| Measure | Proposed tolerance | Owner sign-off required |
|---|---:|---|
| Position count | Exact match | Operations owner |
| Quantity | Exact match for standard instruments | Operations owner |
| Market value | Within configured currency rounding tolerance | Product/control owner |
| Cost basis | Exact where source exists; explicit exception otherwise | Tax/reporting owner |
| Restricted holdings | Exact match | Compliance/control owner |

Rollback decision tree:

```text
If pre-cutover reconciliation fails -> do not cut over.
If post-cutover read-only validation fails before client publication -> rollback target reads to legacy source.
If client output has been published -> freeze affected workflows, communicate impact, reconcile, then decide correction or rollback with incident owner.
```

Smallest useful improvement:

Create a signed reconciliation-tolerance note and update the mock-cutover runbook with rollback triggers, command-center roles and hypercare dashboard checks.

Validation evidence expected:

1. Mock cutover completes with approved tolerances.
2. Break report includes owner, category, materiality and disposition.
3. Rollback drill proves read path can return to legacy source before publication.
4. Hypercare dashboard shows migrated portfolio count, reconciliation breaks, failed API reads and report-generation exceptions.
5. Business sign-off is captured before production cutover.

Follow-up trigger:

If cost-basis exceptions remain unresolved near cutover, split the migration into holdings-read cutover and cost-basis reporting cutover with explicit report warnings and owner approval.

## How To Use These Sample Answers In KT

For a KT session:

1. assign one lab to each participant,
2. ask them to produce a short answer using the same structure,
3. compare their answer to the relevant sample,
4. discuss differences in evidence quality, risk framing and smallest useful improvement,
5. convert one finding into a backlog item or documentation improvement.

## Quality Bar For Lab Answers

Good lab answers:

1. name the system or artifact reviewed,
2. cite evidence categories without exposing restricted details,
3. separate current state from risk,
4. make a clear recommendation,
5. define the smallest useful improvement,
6. name validation evidence,
7. state a follow-up trigger.

Weak lab answers:

1. summarize the guide without inspecting evidence,
2. make broad claims without artifacts,
3. recommend large refactors without a small first step,
4. ignore failure states,
5. omit security, supportability or validation,
6. leave ownership unclear.

## Curation Note

Curated as synthetic, source-safe sample output for the technical practice labs. The examples are designed for self-learning, KT, architecture review, PR review and engineering-lead coaching across enterprise banking and wealth-management technology without product branding or private implementation claims.
