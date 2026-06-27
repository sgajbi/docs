# Worked Examples and Implementation Patterns

This file converts enterprise delivery, SDLC, DevSecOps and production-operations concepts into practical patterns for regulated wealth-platform project work.

## Example 1. Requirement-to-release traceability

**Scenario:** A team adds a new portfolio-review calculation and needs controlled delivery evidence.

| Layer | Evidence to keep | Review question |
|---|---|---|
| Business outcome | Product brief, user journey, reporting impact | What decision or client output changes? |
| Requirement | Story, acceptance criteria, business rules | Are edge cases and degraded states explicit? |
| Design | ADR, API/event contract, data-flow diagram | Which domain owns the truth? |
| Build | Pull request, code review, feature flag | Is the change small and reversible? |
| Test | Unit, contract, integration, golden scenarios | Does the test prove financial behavior, not only code paths? |
| Release | Deployment plan, rollback, release note | Can production safely recover? |
| Operate | Dashboard, alert, runbook, support owner | Can support explain failures and data changes? |

**Pattern:** Treat release evidence as a product artifact. It should explain what changed, why it is correct, how it was tested, how it is monitored and how it can be reversed.

## Example 2. CI/CD gate design for a financial API

**Scenario:** A transaction API change affects positions, cash movements, performance and reporting.

Minimum gated checks:

1. formatting and static analysis,
2. unit tests for domain rules,
3. API schema and backward-compatibility checks,
4. idempotency and duplicate-submission tests,
5. integration tests for downstream position and ledger updates,
6. golden portfolio regression,
7. security and dependency scans,
8. migration validation where persistence changes,
9. OpenAPI or contract documentation update,
10. release note and rollback evidence.

**Anti-pattern:** A pipeline that passes because the service builds while downstream business outputs silently change.

## Example 3. Secure supply-chain release

**Scenario:** A service is containerized and deployed through GitOps.

| Control | Expected implementation |
|---|---|
| Dependency control | Lockfiles, software composition scan and upgrade policy. |
| Build provenance | Build metadata, source commit, artifact digest and CI identity. |
| Image security | Minimal base image, vulnerability scan and approved registry. |
| Secret handling | No secrets in source, image, logs or pipeline output. |
| Deployment authorization | Environment-specific approval and policy checks. |
| Runtime policy | Least privilege, resource limits, network policy and observability. |

**Pattern:** Release approval should refer to immutable artifact identity, not only a branch name or ticket number.

## Example 4. Production-readiness review

**Scenario:** A new reporting workflow will generate monthly client statements.

Readiness evidence should include:

| Area | Required proof |
|---|---|
| Functional correctness | Golden statement output and reconciliation against source data. |
| Performance | Peak report-volume test, queue behavior and capacity model. |
| Resilience | Retry, idempotency, partial failure and duplicate-delivery tests. |
| Observability | Correlation ID, report snapshot ID, dependency status and dashboard. |
| Support | Runbook, exception taxonomy, operator actions and escalation path. |
| Audit | Dataset version, calculation version, rendered document hash and delivery evidence. |

**Pattern:** The workflow is not ready until support can diagnose a failed or changed report without reverse-engineering raw logs.

## Example 5. Incident-to-problem loop

**Scenario:** A stale price causes incorrect portfolio values in an advisor dashboard.

Expected flow:

1. detect through freshness alert or user report,
2. classify impact by portfolios, reports, clients and channels,
3. apply immediate containment, such as degraded-state banner or data-source failover,
4. correct source data with lineage,
5. replay impacted calculations,
6. compare old and corrected outputs,
7. decide whether restatement or client communication is required,
8. record root cause and preventive action,
9. add regression test for the failure mode.

**Pattern:** Problem management closes only when recurrence risk is reduced, not when the dashboard turns green.

## Example 6. Migration cutover control room

**Scenario:** A booking centre migrates client, account, position and transaction history to a new platform.

Minimum control-room boards:

| Board | Purpose |
|---|---|
| Data-load progress | Confirms all batches and source extracts arrived. |
| Reconciliation breaks | Tracks position, cash, ledger, valuation and reporting mismatches. |
| Defect triage | Separates mapping defects, source defects, platform defects and acceptable differences. |
| Business sign-off | Captures product, operations, advisory, reporting and technology approval. |
| Rollback readiness | Maintains fallback trigger, decision owner and last safe point. |
| Post-cutover monitoring | Tracks early production exceptions, latency, report failures and client-impact issues. |

**Pattern:** Cutover success is measured by reconciled business capability, not by successful file transfer.

## Example 7. AI-assisted engineering control pattern

**Scenario:** A delivery team uses AI tools to draft tests, refactor code and update documentation.

Minimum controls:

1. approved source context only,
2. no client secrets, personal data or production credentials in prompts,
3. small pull requests with human ownership,
4. generated code reviewed like human-written code,
5. test and security gates unchanged,
6. dependency additions justified,
7. documentation checked against implementation truth,
8. prompt or agent output retained only where policy requires it.

**Pattern:** AI can accelerate work, but accountability remains with the engineering owner and the delivery governance flow.

## Example 8. Operating-rhythm scorecard

Use a balanced scorecard instead of one-dimensional velocity reporting.

| Dimension | Example metric | Why it matters |
|---|---|---|
| Flow | Lead time, deployment frequency | Shows delivery speed and batch size. |
| Stability | Change failure rate, restore time | Shows production safety. |
| Quality | Escaped defects, regression failure rate | Shows defect prevention. |
| Domain correctness | Golden scenario pass rate, reconciliation breaks | Shows wealth-platform correctness. |
| Security | Critical vulnerability age, policy exceptions | Shows residual risk. |
| Operations | Alert noise, runbook coverage, toil | Shows supportability. |

**Pattern:** Metrics should drive better engineering decisions, not performative reporting.

## Review checklist

- Is the change connected to a clear business capability?
- Are source ownership and data lineage explicit?
- Are API, event, data and reporting contracts updated?
- Are financial calculations covered by deterministic scenarios?
- Are degraded states visible to users and APIs?
- Is release evidence complete and easy to audit?
- Can the change be rolled back or disabled safely?
- Can support operate the feature without tribal knowledge?
- Are security, privacy and access-control impacts reviewed?
- Are AI-assisted changes subject to the same quality bar?

## Disclaimer

This material is for education, architecture, product management, engineering, delivery and operating-model design. It is not legal, regulatory, audit, tax, security-certification, investment, client or compliance advice.
