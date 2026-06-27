# Quality Engineering, Test Strategy, Non-Functional Testing and Automation

## 1. Quality mindset

Quality engineering is not a phase after development. It is a design discipline.

In wealth platforms, quality means:

- calculations are correct
- product lifecycle events are represented accurately
- transactions and positions reconcile
- reports are explainable
- APIs are stable and performant
- security and entitlements are enforced
- operations can support failures
- changes can be regression-tested repeatably

## 2. Test pyramid for wealth systems

```text
Manual exploratory / UAT
Contract and E2E tests
Integration tests
Domain/golden tests
Unit tests
Static analysis
```

The strongest base is unit, domain and contract tests. E2E tests are useful but should not be the only quality control.

## 3. Test categories

| Category | Purpose | Example |
|---|---|---|
| Unit | Verify small logic | Coupon accrual formula. |
| Domain/golden | Verify business scenario | Bond amortization schedule. |
| Contract | Protect APIs/events | Transaction API schema compatibility. |
| Integration | Verify systems together | Position ingestion to valuation. |
| Component | Test service with dependencies mocked or containerized | Performance engine API. |
| End-to-end | Validate critical workflow | Advisory proposal to order. |
| Regression | Prevent previous defects | Corporate-action cost-basis bug. |
| UAT | Business acceptance | Advisor validates report output. |
| NFR | Non-functional quality | Performance, resilience, security. |
| Production synthetic | Detect live issues | Sample portfolio review generation. |

## 4. Wealth-domain golden test packs

Golden tests should exist for:

| Product/domain | Golden scenarios |
|---|---|
| Bonds | coupon accrual, clean/dirty price, maturity, call, amortization. |
| Notes | coupon, barrier, autocall, physical settlement, issuer default. |
| Funds | subscription, redemption, switch, NAV lag, distribution, reinvestment. |
| Equities | buy/sell, dividend, split, rights, spin-off, merger. |
| Derivatives | option exercise, futures margin, FX forward settlement, swap reset. |
| Private markets | capital calls, distributions, NAV lag, IRR. |
| Loans/collateral | LTV, haircut, pledge, cross-pledge, margin call. |
| Performance | TWR, MWR, cashflow classification, contribution. |
| Reporting | holdings, transactions, performance, disclosures, exceptions. |

Golden tests should include expected numbers, explanation and source rationale.

## 5. Acceptance criteria to test cases

Acceptance criterion:

```text
Given an equity split 2-for-1 with pre-event quantity 100 and cost 10,000
When the corporate action is applied
Then quantity becomes 200
And total cost remains 10,000
And average cost per share becomes 50
And no realized P&L is created
```

Test cases should be directly generated from these criteria.

## 6. Non-functional requirements

| NFR | Wealth-platform example |
|---|---|
| Performance | Transaction API returns first page under target latency. |
| Scalability | Valuation job handles all portfolios before report cut-off. |
| Availability | Portfolio review API meets uptime target. |
| Resilience | Recalculation job resumes after failure. |
| Security | Entitlement prevents cross-booking-centre access. |
| Privacy | Client PII is masked in logs. |
| Observability | Errors have correlation IDs and business context. |
| Recoverability | Failed corporate action can be reversed/replayed. |
| Auditability | Suitability override has user, time, reason and rule version. |
| Maintainability | Code complexity and dependency checks pass. |

## 7. Performance testing

Performance test dimensions:

| Dimension | Example |
|---|---|
| API latency | p50/p95/p99 for portfolio holdings API. |
| Batch duration | EOD valuation completion time. |
| Throughput | transactions processed per minute. |
| Concurrency | advisors generating reports at market open. |
| Pagination | stable response for large transaction histories. |
| Cache effectiveness | hit ratio and stale-data behaviour. |
| Database query plan | index usage, sort spill, sequential scan risk. |
| Memory/CPU | pod resource behaviour under load. |

## 8. Resilience testing

Resilience scenarios:

- upstream pricing feed delayed
- FX rate missing
- duplicate transaction event
- out-of-order corporate action
- database connection failure
- Kafka consumer restart
- recalculation job crash
- downstream report renderer unavailable
- partial batch failure
- stale cache
- feature flag misconfiguration

Each scenario should define expected degraded behaviour.

## 9. Security testing

Security tests should cover:

- authentication
- authorization
- object-level access control
- booking-centre restrictions
- data masking
- input validation
- injection prevention
- secrets exposure
- dependency vulnerabilities
- API rate limits
- audit trail
- prompt injection if AI features exist

## 10. Data quality testing

Data quality rules:

| Dimension | Example |
|---|---|
| Completeness | instrument must have currency and asset class. |
| Validity | coupon rate cannot be negative unless product supports it. |
| Consistency | position quantity equals prior quantity plus movements. |
| Timeliness | price must be within allowed freshness window. |
| Uniqueness | transaction ID must be unique by source. |
| Accuracy | market value equals quantity x price x FX. |
| Lineage | value source must be recorded. |

## 11. Test data strategy

Test data should include:

- small deterministic portfolios
- large production-like portfolios
- multi-currency portfolios
- cross-booking-centre accounts
- clients with different risk profiles
- stale/missing market data
- complex product holdings
- corporate actions
- backdated transactions
- reversals and corrections
- migrated historical data

Avoid using live client data in non-production unless properly masked and approved.

## 12. Regression pack governance

Regression pack should be:

- versioned
- tagged by product/domain
- linked to defects and incidents
- runnable in CI where possible
- executable before country release
- reviewed after production incidents
- extended whenever a new edge case is found

## 13. UAT design

UAT should validate business outcomes, not repeat technical tests.

UAT checklist:

- business workflow works
- numbers match expected outputs
- labels are understandable
- exceptions are explainable
- report output is acceptable
- role/access behaviour is correct
- downstream process is not broken
- support team understands failure modes

## 14. AI-assisted testing

AI can help create:

- edge-case inventories
- test data variations
- contract tests
- regression scenarios
- report QA checklists
- log triage summaries
- defect reproduction steps

But AI-generated tests must be reviewed by domain SMEs and must not become unverified theatre.

## 15. Quality dashboard

Useful quality metrics:

| Metric | Purpose |
|---|---|
| Escaped defects | Measures production quality. |
| Regression pass rate | Measures release confidence. |
| Flaky test rate | Measures test trust. |
| Code coverage | Limited but useful baseline. |
| Mutation score | Measures test strength where used. |
| NFR pass/fail | Measures production readiness. |
| Data-quality score | Measures source reliability. |
| Incident recurrence | Measures problem-management effectiveness. |
| Mean time to detect | Measures observability. |
| Mean time to restore | Measures operational resilience. |

## 16. Practitioner explanation

> "For wealth platforms, quality engineering has to be domain-aware. I would build golden test packs for product lifecycle, cashflows, positions, valuation, performance and reporting; combine them with API/event contract tests, non-functional tests, security checks and data-quality rules; and ensure every production incident results in new regression evidence. The goal is not only to test code, but to prove that the business outcome is correct and supportable."
