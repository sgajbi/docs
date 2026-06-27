# 11 - Architecture Governance, ADRs, Quality Rubric and Test Scenarios

## 1. Why architecture governance matters

A wealth platform evolves across products, countries, teams and systems. Without governance, architecture becomes inconsistent: APIs diverge, events lack contracts, product models fragment, reports become non-reproducible and controls are added manually.

Architecture governance should not be bureaucracy. It should make decisions explicit, reusable and testable.

## 2. Architecture Decision Records

Create an ADR when a decision is material, hard to reverse or affects multiple teams.

Examples:

- trade-date vs settlement-date position basis
- transaction pagination strategy
- event envelope standard
- report snapshot model
- source-of-truth decision for positions
- valuation price source hierarchy
- performance recalculation policy
- product taxonomy structure
- service boundary decision
- country-specific rule configuration strategy

## 3. ADR template

```markdown
# ADR-XXXX: Title

## Status
Proposed / Accepted / Superseded / Deprecated

## Date
YYYY-MM-DD

## Context
What problem are we solving? What constraints matter?

## Decision
What did we decide?

## Alternatives considered
What options were considered and why not chosen?

## Consequences
Positive and negative implications.

## Impacted domains
APIs, events, data model, reporting, migration, operations, security.

## Controls and tests
How will the decision be validated?

## Review date
When should this be revisited?
```

## 4. Architecture review checklist

| Area | Review questions |
|---|---|
| Domain boundary | Is ownership clear? Is the service doing too much? |
| API contract | Is the OpenAPI contract complete, versioned and tested? |
| Event contract | Are events versioned, idempotent and replayable? |
| Data model | Are identifiers, states and lineage explicit? |
| Source ownership | Is source of truth clear? |
| Data quality | Are missing/stale/provisional states handled? |
| Security | Are authentication, authorization, PII and audit covered? |
| Resilience | Are failure modes, retries and degraded states defined? |
| Observability | Are metrics, logs and traces sufficient? |
| Reporting | Can outputs be reproduced and explained? |
| Migration | Is cutover/parallel-run impact considered? |
| Operations | Are runbooks, dashboards and support ownership defined? |

## 5. API quality rubric

| Score | Description |
|---|---|
| 1 | API exposes internal tables, no contract, weak errors. |
| 2 | API works for happy path but lacks versioning, pagination or entitlement design. |
| 3 | API has contract, errors, security and basic tests. |
| 4 | API has full governance, examples, contract tests, lineage and degraded-state handling. |
| 5 | API is productized, well-documented, observable, backward-compatible and reusable across channels. |

## 6. Event quality rubric

| Score | Description |
|---|---|
| 1 | Unversioned payload, unclear name, no schema. |
| 2 | Basic event but weak idempotency/replay. |
| 3 | Versioned schema, examples, topic design and consumer list. |
| 4 | Replay, lineage, deduplication, quality flags and monitoring included. |
| 5 | Fully governed event product with AsyncAPI-style contract, tests and lifecycle policy. |

## 7. Data-quality rubric

| Score | Description |
|---|---|
| 1 | Data issues found manually downstream. |
| 2 | Basic validation but no ownership or SLA. |
| 3 | Rule-based validation and error reporting. |
| 4 | DQ scorecards, lineage, workflow and reconciliation. |
| 5 | Proactive controls, impact analysis, self-service dashboards and release gates. |

## 8. Reporting quality rubric

| Score | Description |
|---|---|
| 1 | Report pulls live mutable data, no reproducibility. |
| 2 | Report generates but weak snapshot/versioning. |
| 3 | Snapshot and template version exist. |
| 4 | Full input/calculation/disclosure lineage and exception handling. |
| 5 | Report is fully governed, archived, explainable, testable and operationally monitored. |

## 9. Regression test scenarios

### Scenario 1 - Large transaction history

Given an account with 100,000 transactions, the transaction API must return stable paginated results with no duplicates or missing records while new transactions are booked.

Expected:

- cursor pagination
- deterministic sorting
- latency within SLO
- no inconsistent page boundary

### Scenario 2 - Stale price blocks official report

Given a portfolio containing an illiquid bond with no approved price for the valuation date, official report generation should block or show configured degraded state.

Expected:

- data-quality flag
- report exception
- workflow task
- no silent zero valuation

### Scenario 3 - Backdated transaction recalculates performance

Given a corrected backdated cashflow, affected periods should be detected and recalculated.

Expected:

- recalculation job created
- impacted portfolios/periods identified
- new calculation version stored
- reporting impact assessed

### Scenario 4 - Duplicate source event

Given the same transaction event arrives twice, the consumer must not duplicate cash/position movement.

Expected:

- deduplication by source event key
- idempotent processing
- audit record of duplicate ignored

### Scenario 5 - Mandate breach after market movement

Given market price movement increases issuer concentration above limit, post-EOD mandate monitoring should raise a breach.

Expected:

- breach event
- advisor/workflow notification
- report/dashboard visibility according to policy

### Scenario 6 - Entitlement change

Given advisor assignment changes, old advisor must no longer access client data and new advisor must have appropriate scope.

Expected:

- API access denied for old advisor
- audit trail of entitlement change
- report access updated

### Scenario 7 - Migration parallel run mismatch

Given target positions differ from source positions beyond tolerance, migration sign-off should block.

Expected:

- reconciliation break
- root cause category
- sign-off gate failure
- exception workflow

### Scenario 8 - Report regeneration

Given a client report must be regenerated for the same period, the system should use the original snapshot unless restatement is explicitly approved.

Expected:

- same input snapshot by default
- same values and checksum if template unchanged
- explicit new version if restated

## 10. Definition of done for architecture packs

For every new platform/domain pack:

- Purpose and scope are clear.
- Domain vocabulary is defined.
- Business lifecycle is documented.
- Data model is included.
- APIs/events are described.
- Source ownership is explicit.
- Advisory/mandate/reporting implications are included.
- Platform controls and QA scenarios are included.
- Edge cases and degraded states are documented.
- Sources and further reading are included.

## 11. Interview-ready architecture answer

A strong answer:

> A wealth platform should be decomposed around stable business capabilities such as client master, product master, order management, transactions, positions, accounting, analytics, mandates, reporting and entitlements. Each domain should have clear ownership, API and event contracts, source-of-truth rules, data-quality controls and audit lineage. APIs should serve query/command interactions with versioned OpenAPI contracts, while events should propagate lifecycle changes through versioned event contracts with idempotency and replay support. For reporting and analytics, the platform must use versioned input snapshots and calculation lineage so client-facing outputs are reproducible and explainable. Architecture quality should be enforced through ADRs, contract tests, data-quality gates, reconciliation, observability and operational runbooks.
