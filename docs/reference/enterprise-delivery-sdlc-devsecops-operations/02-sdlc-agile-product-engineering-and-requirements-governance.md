# SDLC, Agile Product Engineering and Requirements Governance

## 1. Why SDLC matters in wealth platforms

The software development lifecycle is the operating system for controlled change.

In wealth platforms, poor SDLC shows up as:

- incorrect product treatment
- missed corporate-action edge cases
- broken performance returns
- unclear transaction classifications
- inconsistent reporting labels
- untraceable source-data changes
- production incidents after migration
- audit gaps
- advisory or suitability workflow risk

Good SDLC makes requirements, design, code, tests, controls and operations fit together.

## 2. SDLC stages

| Stage | Outcome |
|---|---|
| Demand intake | Clear business problem and priority. |
| Discovery | Problem, users, process, data and edge cases understood. |
| Requirements | Stories, rules, examples and acceptance criteria defined. |
| Design | Architecture, API/event/data model and control approach agreed. |
| Build | Code, tests and documentation implemented. |
| Verify | Functional, regression, NFR, security and business validation complete. |
| Release | Deployment, rollback, evidence and communications ready. |
| Operate | Monitoring, support, incident response and feedback loop active. |
| Improve | Defects, incidents and metrics drive backlog improvements. |

## 3. Product discovery for wealth software

Discovery should answer:

| Question | Example |
|---|---|
| Who is the user? | Advisor, PM, client, operations, risk, support, report consumer. |
| What is the decision or action? | Recommend, rebalance, approve, report, investigate, explain. |
| What product/data is involved? | Notes, bonds, funds, cash, loans, transactions, valuations. |
| What is the source of truth? | Custodian, product master, order system, pricing feed, analytics engine. |
| What is the control boundary? | Suitability, entitlement, mandate, booking centre, data privacy. |
| What is the failure mode? | Missing price, stale NAV, late transaction, wrong FX, duplicate event. |
| What evidence is required? | Test case, report sample, reconciliation, approval, audit trail. |

## 4. Requirements hierarchy

```text
Business outcome
  -> capability
  -> workflow
  -> user story
  -> rule / scenario
  -> acceptance criterion
  -> test case
  -> implementation task
  -> release evidence
```

Avoid jumping from business outcome directly to implementation task.

## 5. User story pattern

A good wealth-platform story:

```text
As a <role>
I need <capability>
So that <business outcome>

Acceptance criteria:
- Given <portfolio/account/client/product state>
- When <event/action/calculation/report is triggered>
- Then <observable result>
- And <control/reporting/audit behaviour>
```

Example:

```text
As an advisor
I need to see whether a proposed structured note is suitable for the client
So that I can avoid recommending a product outside the client's risk profile and approved product universe.

Acceptance criteria:
- Given a client with risk profile "balanced"
- And a structured note with product risk rating "high"
- And the product is not approved for the client's booking centre
- When the advisor creates a proposal
- Then the proposal is blocked before order submission
- And the block reason is shown to the advisor
- And the decision is stored with rule version and product data snapshot
```

## 6. Business-rule documentation

Rules should be written in decision-table form where possible.

| Condition | Case 1 | Case 2 | Case 3 |
|---|---:|---:|---:|
| Product approved in APU? | Yes | No | Yes |
| Client risk profile sufficient? | Yes | Yes | No |
| Booking centre allowed? | Yes | Yes | Yes |
| Mandate allows product? | Yes | Yes | Yes |
| Outcome | Pass | Block | Block |

This is better than a paragraph because it is easier to test.

## 7. Definition of ready

A story is ready when:

- business value is clear
- upstream/downstream dependencies are known
- product/data model impact is understood
- acceptance criteria are testable
- edge cases are listed
- NFRs are identified
- security/privacy impact is assessed
- reporting and operations impact are known
- test data is available or planned
- open questions have owners

## 8. Definition of done

A story is done when:

- acceptance criteria pass
- code is reviewed
- tests are automated where appropriate
- documentation is updated
- API/event contracts are updated
- data migrations are handled
- observability is added
- errors are structured and actionable
- runbook impact is addressed
- release notes are updated
- evidence is linked

## 9. Architecture Decision Records

Use ADRs for decisions such as:

- API pagination pattern
- event-driven vs request/response integration
- position model design
- transaction type taxonomy
- source-of-truth selection
- cache strategy
- idempotency approach
- data-retention policy
- pricing hierarchy
- AI guardrail pattern

ADR format:

```text
# ADR-0001: Decision title

## Status
Proposed / Accepted / Superseded

## Context
What problem are we solving?

## Decision
What did we choose?

## Options considered
Option A, B, C.

## Consequences
Benefits, risks, tradeoffs.

## Controls
How will we test/monitor/operate it?

## Review date
When should this be revisited?
```

## 10. Requirements traceability

Traceability matrix:

| Requirement | Story | Design | Code | Test | Release evidence |
|---|---|---|---|---|---|
| Transaction API pagination | ST-101 | ADR-012 | PR-456 | TC-900 | Release 2026.07 |
| Structured note coupon event | ST-211 | API v2 | PR-512 | TC-1150 | Release 2026.08 |

Traceability matters for audit, migration and production issue investigation.

## 11. Backlog governance

Backlog should separate:

| Item type | Example |
|---|---|
| Feature | Add portfolio review risk section. |
| Enabler | Create canonical transaction type taxonomy. |
| Defect | Wrong realized P&L after split. |
| Technical debt | Refactor valuation adapter. |
| Control debt | Missing audit lineage for suitability override. |
| NFR | Improve transaction API latency. |
| Data quality | Missing issuer LEI for bonds. |
| Operations | Add runbook for stuck recalculation jobs. |

## 12. Agile ceremonies with enterprise value

| Ceremony | Wealth-platform purpose |
|---|---|
| Refinement | Clarify product rules, data, edge cases and testability. |
| Planning | Commit to achievable, dependency-aware scope. |
| Daily stand-up | Surface blockers, not status theatre. |
| Review/demo | Validate business outcome and controls. |
| Retrospective | Improve delivery, quality, support and collaboration. |
| Architecture sync | Resolve cross-domain decisions. |
| Release readiness | Confirm evidence, dependencies, rollback and operations readiness. |

## 13. Common requirements mistakes

| Mistake | Better practice |
|---|---|
| "Display performance" | Specify period, method, cashflow timing, currency, benchmark and exceptions. |
| "Support bonds" | Define bond subtypes, coupons, accruals, amortization, calls, prices and reporting. |
| "Add AI explanation" | Define approved inputs, grounding, guardrails, review, audit and blocked outputs. |
| "Use pagination" | Define cursor/keyset/offset, sorting, consistency and backward compatibility. |
| "Show cash" | Define available, settled, book, blocked, pledged, margin and projected cash. |

## 14. AI-assisted requirements work

AI can help draft:

- user stories
- acceptance criteria
- edge-case lists
- test scenarios
- decision tables
- API examples
- documentation
- migration checklists

But AI should not be treated as the authority. The authority remains:

- business owner
- data owner
- product terms
- source-system contract
- approved policies
- regulatory/control framework
- production evidence

## 15. Practitioner explanation

> "I treat SDLC as a controlled value chain from business outcome to production evidence. In wealth platforms, a requirement is not complete until product treatment, data ownership, calculation logic, reporting impact, suitability/mandate controls, test scenarios and operational support are understood. Agile helps us move fast, but architecture, traceability, DevSecOps, quality engineering and release readiness make the change safe."
