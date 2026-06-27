# 03 - API Design, Contracts, Versioning, Pagination and Error Semantics

## 1. Role of APIs in a wealth platform

APIs provide controlled access to business capabilities. In a wealth platform they are used for:

- advisor and client UI queries
- portfolio review generation
- advisory proposal workflows
- order capture and pre-trade checks
- position, cash, transaction and performance retrieval
- market/reference data access
- reporting snapshot creation
- operational exception handling
- integration with enterprise and external systems

APIs should not be random database query endpoints. They should expose stable business contracts.

## 2. API types

| API type | Purpose | Example |
|---|---|---|
| Experience API / BFF | Tailored to a channel or journey | Client 360 dashboard, advisor workbench summary |
| Domain API | Stable business capability | Position API, transaction API, performance API |
| Command API | State-changing action | Create order, approve proposal, request report |
| Query API | Read-only retrieval | Get holdings, get performance, get report snapshot |
| Admin/operations API | Controlled operational action | Reprocess event, resolve break, mark report regenerated |
| Integration API | Enterprise-to-enterprise integration | Product master lookup, KYC status, custodian position feed |

## 3. Contract-first API design

API contracts should be designed and reviewed before implementation. The contract should describe:

- endpoint purpose
- resource model
- request parameters
- response schema
- status codes
- error payloads
- security scopes
- idempotency rules
- pagination and sorting
- versioning
- examples
- backward compatibility rules
- data-quality/degraded-state fields

For HTTP APIs, OpenAPI is the standard contract format. A strong API governance model should require OpenAPI contracts, schema validation, examples and contract tests.

## 4. Resource naming

Prefer business resources:

```text
GET /clients/{clientId}/portfolios
GET /portfolios/{portfolioId}/positions
GET /portfolios/{portfolioId}/performance
GET /portfolios/{portfolioId}/risk-summary
GET /accounts/{accountId}/transactions
POST /advisory-proposals
POST /orders
POST /reports/portfolio-review-requests
```

Avoid implementation-centric resources:

```text
GET /position_rows
GET /query?table=portfolio_txn
POST /runCalculationJob
```

## 5. API response design

A wealth API response should include:

| Field group | Purpose |
|---|---|
| Business data | The requested positions, transactions, analytics or report result. |
| As-of metadata | Valuation date, business date, snapshot date, source timestamp. |
| Currency metadata | Portfolio currency, instrument currency, reporting currency, FX rate date. |
| Source metadata | Source system, data version, source confidence where needed. |
| Calculation metadata | Calculation version, method, input snapshot identifier. |
| Quality flags | Stale price, missing FX, provisional NAV, estimated value, incomplete data. |
| Entitlement metadata | Optional: data redaction/scope reason where relevant. |
| Traceability | Correlation ID, request ID, lineage ID. |

Example shape:

```json
{
  "portfolioId": "P123",
  "asOfDate": "2026-06-30",
  "basis": "TRADE_DATE",
  "currency": "USD",
  "positions": [],
  "dataQuality": {
    "status": "DEGRADED",
    "flags": ["STALE_PRICE", "PROVISIONAL_NAV"]
  },
  "lineage": {
    "snapshotId": "SNAP-20260630-001",
    "calculationVersion": "risk-engine-2.4.1"
  }
}
```

## 6. Versioning strategy

### 6.1 Version what matters

Version these explicitly:

- API contract
- event schema
- calculation methodology
- product taxonomy
- reporting template
- mandate rules
- price source hierarchy
- data-quality rule set

### 6.2 API version options

| Strategy | Example | When to use |
|---|---|---|
| URI versioning | `/v1/portfolios/{id}` | Simple, visible, common for external APIs. |
| Header versioning | `Accept: application/vnd.bank.v1+json` | More flexible, but less visible. |
| Compatibility-based | Same path, backward-compatible changes only | Useful for internal APIs with strict governance. |

Avoid frequent major versions. Design for backward compatibility.

### 6.3 Breaking vs non-breaking changes

| Change | Type |
|---|---|
| Add optional response field | Usually non-breaking |
| Add optional request parameter | Usually non-breaking |
| Remove field | Breaking |
| Rename field | Breaking |
| Change enum meaning | Breaking |
| Change calculation formula without version | Breaking in reporting/analytics context |
| Change default sorting/pagination | Potentially breaking |

## 7. Pagination patterns

Large transaction and holdings APIs must be paginated.

| Pattern | Strength | Weakness | Best use |
|---|---|---|---|
| Offset/limit | Simple | Slow and inconsistent for large mutable data | Small admin lists |
| Page number | User-friendly | Not stable under changes | Static result sets |
| Cursor/keyset | Fast and stable | More complex | Transaction feeds, holdings lists |
| Seek by date/id | Efficient | Requires stable ordering | Time-series transactions |
| Snapshot + cursor | Reproducible | Requires snapshot management | Reports, audit queries |

For transaction APIs, prefer cursor or snapshot cursor:

```text
GET /accounts/{accountId}/transactions?fromDate=2026-01-01&toDate=2026-06-30&limit=500&cursor=abc
```

A cursor should encode stable ordering such as:

```text
bookingDate DESC, transactionId DESC
```

For report generation, prefer snapshot queries so the result does not change while pages are retrieved.

## 8. Filtering and sorting

Transaction and portfolio APIs should support controlled filters:

| API | Common filters |
|---|---|
| Transactions | date range, instrument, transaction type, status, source, cash/security, booking basis |
| Positions | as-of date, basis, product family, currency, asset class, custodian, status |
| Performance | period, currency, method, breakdown, benchmark, gross/net |
| Risk | risk date, method, exposure basis, look-through flag |
| Reports | report type, period, version, status, language, channel |

Sorting should be explicit and deterministic.

## 9. Error semantics

Use clear, machine-readable errors. RFC 7807-style problem details are useful for consistent error payloads.

Example:

```json
{
  "type": "https://example.com/problems/stale-price",
  "title": "Stale price prevents official report generation",
  "status": 422,
  "detail": "Instrument ABC has no valid price for 2026-06-30.",
  "instance": "/reports/requests/RPT123",
  "correlationId": "corr-123",
  "errorCode": "REPORT_STALE_PRICE_BLOCK"
}
```

### Recommended status usage

| Status | Use |
|---|---|
| 200 | Successful query. |
| 201 | Resource created. |
| 202 | Accepted for asynchronous processing. |
| 400 | Invalid request syntax or parameter combination. |
| 401 | Unauthenticated. |
| 403 | Authenticated but not entitled. |
| 404 | Resource not found or not visible under entitlement rules. |
| 409 | Conflict, duplicate idempotency key conflict, state conflict. |
| 422 | Valid syntax but business validation failed. |
| 429 | Rate limit. |
| 500 | Unexpected server failure. |
| 503 | Dependency unavailable. |

## 10. Idempotency

State-changing APIs must support idempotency where duplicate requests are possible.

Examples:

- create order
- submit advisory proposal
- approve consent
- request report generation
- trigger recalculation
- ingest external transaction

Recommended approach:

```text
Idempotency-Key: client-generated-stable-key
```

Store:

- key
- caller identity
- request hash
- resulting resource ID
- status
- expiry

If the same key is replayed with a different request hash, return conflict.

## 11. API security

Every API should define:

- authentication method
- authorization scopes
- portfolio/account data scoping
- client/advisor access rules
- data classification
- audit logging requirement
- PII redaction rules
- rate limit and abuse controls
- mTLS or service-to-service identity where required

Avoid relying only on UI hiding. Enforce entitlements at the API layer.

## 12. API observability

Log and trace:

- correlation ID
- caller application
- user/service identity
- client/account/portfolio scope where allowed
- request path and method
- latency
- dependency latency
- result status
- error code
- data-quality status
- snapshot/calculation version for analytics/reporting APIs

## 13. API governance checklist

Before publishing an API:

- OpenAPI contract exists.
- Examples are realistic.
- Error responses are documented.
- Pagination is safe for expected volumes.
- Versioning policy is defined.
- Data-quality flags are included where relevant.
- Entitlement model is enforced.
- Idempotency is implemented for mutations.
- Contract tests exist.
- Performance tests exist for large portfolios.
- Backward compatibility impact is reviewed.
- API owner and support model are defined.
