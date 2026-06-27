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
| 9 | Lab 6 | Sensitive-data and access-control review for document retrieval. |
| 10 | Lab 7 | Test strategy redesign for fragile end-to-end coverage. |
| 11 | Lab 9 | Runtime deployment readiness for a containerized API service. |
| 12 | Lab 10 | Git, release and documentation truth review for a completed feature. |
| 13 | Lab 13 | Reporting and archive evidence review for client statements. |
| 14 | Lab 14 | Platform productization readiness for enterprise buyer review. |

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

## 9. Sensitive-Data And Access-Control Review

Lab: sensitive-data and access-control review
System or artifact reviewed: document retrieval API and advisor-facing document list
Guides used: identity and entitlements, sensitive-data handling, security testing

Evidence inspected:

| Evidence | Observation |
|---|---|
| API contract | Document list exposes document id, title, type, generated date, owner account and download link. |
| Authorization policy | Role-level policy exists; object-level account and relationship checks are partially documented. |
| UI behavior | UI hides download actions when entitlement is missing but still calls the list endpoint. |
| Logs | Access-denied events are logged with document id and actor id; no document content is logged. |
| Tests | Happy-path access tests exist; cross-account denial and expired-delegation tests are missing. |

Current-state assessment:

The design is close but not yet strong enough for document access. Document retrieval needs object-level authorization on both list and download endpoints, not only UI hiding or role-level checks.

Authorization matrix:

| Actor | Scope | Allowed action | Denial behavior |
|---|---|---|---|
| Relationship manager | Assigned client accounts only | List metadata, download permitted documents | Return empty list or `403` without confirming document existence. |
| Assistant | Delegated accounts within active delegation window | List metadata; download only if delegated scope allows it | Return `403` with safe problem detail. |
| Operations user | Operational support scope | View metadata needed for support; no content unless break-glass approved | Audit event and restricted access workflow. |
| Client user | Own accounts only | Download client-visible documents | Return `404` or safe `403` for inaccessible document ids. |

Risk or gap:

The list endpoint can reveal document metadata across accounts if object-level filtering is incomplete. Metadata such as document type and generated date can still be sensitive, especially for advisory, tax, credit or regulatory documents.

Smallest useful improvement:

Enforce object-level authorization inside the API service before returning document metadata. Add negative tests for cross-account access, expired delegation, disabled account access and direct download by document id.

Validation evidence expected:

1. API test proves a user cannot list or download another account's document.
2. Delegation-expiry test proves access stops immediately after expiry.
3. Log-safety test proves document titles, contents and account-sensitive narrative fields are not logged.
4. UI test proves hidden actions are backed by API denial, not used as the only control.
5. Access-denied events include enough safe fields for audit and support.

Follow-up trigger:

If document retrieval is exposed to AI, reporting export or bulk download workflows, recertify the same authorization rules across those entry points.

## 10. Test Strategy Redesign

Lab: test strategy redesign
System or artifact reviewed: portfolio review workflow with high end-to-end test reliance
Guides used: test pyramid, golden examples, certification sweeps

Evidence inspected:

| Evidence | Observation |
|---|---|
| Test inventory | 42 browser tests cover portfolio review; only 8 domain tests cover calculations and rules. |
| Recent failures | Browser tests fail intermittently due to timing and seed-data drift. |
| Escaped defects | Two recent defects involved advisory suitability state and stale benchmark data. |
| Contract tests | API schema tests exist but do not validate degraded states or supportability metadata. |
| Test data | Synthetic portfolios exist, but golden cases are not named or owned. |

Current-state assessment:

The test suite is broad but top-heavy. Too much business behavior is proven only through browser tests. This makes feedback slow and brittle while still missing lower-level rule regressions.

Recommended test pyramid:

| Behavior | Better test level | Reason |
|---|---|---|
| Suitability rule classification | Domain/unit test | Deterministic rule with many edge cases. |
| Benchmark stale-state mapping | API/service contract test | Consumer-visible state and error semantics. |
| Portfolio review page renders degraded badge | Browser test | UI-specific rendering and accessibility. |
| Report payload contains advisory status | Integration or contract test | Cross-service response composition. |
| Full review workflow | E2E smoke | One or two golden happy/degraded paths, not every rule. |

Golden-case catalog:

| Case | Purpose | Owner |
|---|---|---|
| Balanced advisory portfolio | Ready-state baseline for holdings, risk, suitability and report summary. | Product/QA owner |
| Stale benchmark portfolio | Proves stale analytics state and report warning behavior. | Analytics owner |
| Mandate breach portfolio | Proves suitability and DPM control messaging. | Advisory/control owner |
| Restricted-document account | Proves entitlement and document-access denial. | Security owner |

Smallest useful improvement:

Extract suitability and stale-benchmark assertions from browser tests into service/API tests. Keep one browser smoke test for visible degraded-state rendering.

Validation evidence expected:

1. New domain tests cover suitability ready, warning and blocked states.
2. API contract tests cover stale benchmark response shape and problem details.
3. Browser test count drops for duplicated rule checks while one end-to-end smoke remains.
4. Golden-case catalog names owner and review cadence.
5. Flaky-test dashboard shows reduced timing-related failures after redesign.

Follow-up trigger:

If a browser test fails twice for timing rather than product behavior, review whether the assertion belongs at a lower test level.

## 11. Runtime Deployment Readiness

Lab: runtime deployment readiness
System or artifact reviewed: containerized account-summary API service
Guides used: runtime infrastructure, container deployment, health/readiness design

Evidence inspected:

| Evidence | Observation |
|---|---|
| Container image | Uses pinned runtime base image and non-root user; build stage includes dev tooling that is not copied to runtime image. |
| Configuration | Required environment variables are documented; default timeout is implicit. |
| Secrets | Secrets are loaded from runtime secret provider; no secrets found in image or docs. |
| Health checks | Liveness endpoint returns process health; readiness checks database but not upstream pricing dependency. |
| Deployment notes | Rollback steps exist; migration compatibility is not stated. |

Current-state assessment:

The service is deployable, but readiness is incomplete. It can receive traffic when a mandatory upstream pricing dependency is unavailable, which can produce partial or stale account summaries.

Configuration map:

| Setting | Requirement | Review note |
|---|---|---|
| `DATABASE_URL` | Required secret reference | Validate at startup and redact from logs. |
| `PRICING_API_BASE_URL` | Required service endpoint | Include in readiness dependency check if mandatory. |
| `REQUEST_TIMEOUT_MS` | Required bounded timeout | Document default and maximum. |
| `LOG_LEVEL` | Environment-specific | Avoid debug in production. |
| `FEATURE_PARTIAL_SUMMARY` | Explicit true/false | Controls whether degraded response is allowed. |

Probe recommendation:

| Probe | Expected behavior |
|---|---|
| Liveness | Process is running and event loop is responsive. |
| Readiness | Database reachable, schema compatible and mandatory upstream dependencies available or intentionally degraded. |
| Startup | Configuration validated and migrations not blocking startup unexpectedly. |

Smallest useful improvement:

Add pricing dependency state to readiness when the account summary cannot be safely produced without prices. If partial summaries are allowed, readiness should stay healthy but the API must return explicit degraded supportability.

Validation evidence expected:

1. Startup test fails fast when required configuration is missing.
2. Readiness test fails or reports degraded when mandatory pricing dependency is unavailable.
3. API test proves partial summary includes supportability state when partial mode is enabled.
4. Deployment smoke validates liveness, readiness and one synthetic account summary.
5. Rollback note states whether schema changes are backward compatible.

Follow-up trigger:

If autoscaling or high load is introduced, add resource-limit review, queue-depth or saturation metrics and capacity test evidence.

## 12. Git, Release And Documentation Truth

Lab: Git, release and documentation truth
System or artifact reviewed: completed feature adding supportability metadata to account balances
Guides used: Git workflow, documentation governance, implementation evidence reading

Evidence inspected:

| Evidence | Observation |
|---|---|
| Pull request | PR description lists API change and tests but does not link the updated runbook. |
| OpenAPI contract | Contract includes optional `supportability` object and examples. |
| Tests | API compatibility and UI degraded-state tests passed. |
| README | README still describes balances as always ready when endpoint succeeds. |
| Release note | Mentions UI badge but not API metadata for consumers. |

Current-state assessment:

Implementation and tests are in better shape than documentation. Durable truth is inconsistent: API docs describe supportability, but README and release notes under-explain behavior for consumers and support teams.

Truth map:

| Truth source | Status | Action |
|---|---|---|
| Code | Current | No action. |
| OpenAPI | Current | Keep examples. |
| Tests | Current | Add command evidence to PR. |
| README | Stale | Explain ready, stale, partial and unavailable balance states. |
| Runbook | Partially current | Link stale-source diagnosis and recovery. |
| Release note | Too narrow | Add consumer-facing API metadata note. |

Smallest useful improvement:

Update README and release note to match the implemented supportability contract. Add a PR checklist item requiring docs and runbook links when supportability behavior changes.

Validation evidence expected:

1. README describes new response metadata and degraded states.
2. Release note states the change is additive and optional for API consumers.
3. PR evidence links tests, OpenAPI diff and runbook update.
4. Stale phrase scan confirms old always-ready claim is removed.
5. Documentation link check passes.

Follow-up trigger:

When supportability metadata appears in three or more APIs, add a shared vocabulary page and require contract consistency checks.

## 13. Reporting And Archive Evidence

Lab: reporting and archive evidence
System or artifact reviewed: monthly client statement generation and archive workflow
Guides used: reporting/document evidence, snapshots and lineage, integrity controls

Evidence inspected:

| Evidence | Observation |
|---|---|
| Report job payload | Includes report type, account group, as-of date and language. |
| Snapshot metadata | Captures holdings and cash data versions; template version is missing. |
| Render output | PDF hash is stored after generation. |
| Archive metadata | Retention class and document owner are present; entitlement rule is referenced by role only. |
| Regression tests | Golden report visual diff exists for ready state; stale section warning is not covered. |

Current-state assessment:

The archive has a good foundation, especially hash capture, but reproducibility is incomplete without template version and calculation methodology version. Entitlement rules need object-level scope, not only role labels.

Snapshot rule:

| Snapshot component | Required evidence |
|---|---|
| Data inputs | Holdings, cash, prices, transactions and benchmark versions. |
| Calculation | Methodology version and calculation run id. |
| Template | Template id, version and localization bundle version. |
| Rendering | Renderer version, output format and hash. |
| Delivery | Recipient scope, channel, timestamp and delivery status. |

Degraded-report behavior:

If a mandatory section is stale, client-ready publication should be blocked. If a non-mandatory section is stale and policy allows publication, the report should include a clear warning, evidence metadata and support runbook link.

Smallest useful improvement:

Add template version and methodology version to report snapshot metadata, then add a regression test for a stale non-mandatory section warning.

Validation evidence expected:

1. Regenerated report can be traced to data, calculation, template and renderer versions.
2. Archive metadata includes object-level entitlement scope.
3. Hash validation test detects modified output.
4. Golden report test covers ready state and stale-warning state.
5. Retrieval audit records actor, document id, action, decision and safe reason code.

Follow-up trigger:

If reports are migrated to a new renderer or archive store, run an archive-continuity test proving old documents remain retrievable, authorized and hash-verifiable.

## 14. Platform Productization Readiness

Lab: platform productization readiness
System or artifact reviewed: enterprise portfolio reporting capability prepared for buyer or internal platform review
Guides used: platform productization, reference architecture, readiness scorecards

Evidence inspected:

| Evidence | Observation |
|---|---|
| Capability catalog | Lists report generation, scheduling, archive retrieval and entitlement checks. |
| Reference architecture | Shows API service, worker, renderer, archive store and notification channel. |
| Deployment guide | Covers container runtime and environment variables; sizing assumptions are light. |
| Security evidence | Entitlement and document-access tests exist; vendor due-diligence answers are incomplete. |
| Support model | Runbook exists for failed report jobs; customer onboarding checklist is missing. |

Current-state assessment:

The capability is credible for internal adoption but not fully ready for enterprise buyer review. Strongest evidence is architecture and test coverage. Weakest evidence is implementation packaging: onboarding, sizing, support boundaries and procurement-style responses.

Supported-feature matrix:

| Capability | Status | Evidence |
|---|---|---|
| On-demand report generation | Implemented | API contract, worker tests, golden report. |
| Scheduled batch reporting | Configurable | Scheduler docs and batch runbook. |
| Archive retrieval | Implemented | Archive API, entitlement tests, hash validation. |
| Multi-tenant customer isolation | Integration-dependent | Requires deployment and identity model decision. |
| Custom templates | Configurable with governance | Template versioning and approval workflow needed. |
| Regulatory delivery workflows | Planned | Not claimable without jurisdiction-specific controls. |

Readiness scorecard:

| Area | Score | Main gap |
|---|---:|---|
| Capability clarity | 4/5 | Unsupported regulatory workflows need clearer boundaries. |
| Architecture evidence | 4/5 | Add sizing and dependency assumptions. |
| Security and privacy | 3/5 | Complete buyer due-diligence answers and data-flow diagrams. |
| Deployment readiness | 3/5 | Add environment sizing, backup and DR expectations. |
| Support model | 3/5 | Add onboarding and escalation playbook. |
| Buyer evidence pack | 2/5 | Consolidate proofs into one reviewable package. |

Smallest useful improvement:

Create a buyer evidence pack that includes capability matrix, reference architecture, deployment assumptions, security controls, data-flow diagram, support model, test evidence and known limitations.

Validation evidence expected:

1. Capability matrix separates implemented, configurable, integration-dependent, planned and unsupported items.
2. Evidence pack links architecture, API contracts, security controls, tests, runbooks and deployment guide.
3. Unsupported or planned capabilities are not marketed as available.
4. Onboarding checklist includes environment setup, roles, source data, template configuration and validation commands.
5. Readiness scorecard has owners and dates for all material gaps.

Follow-up trigger:

Before a pilot or procurement review, run a due-diligence simulation covering security, support, data governance, deployment, resilience, audit, exit plan and operating model.

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
