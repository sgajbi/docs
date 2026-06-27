# 03 - API Contract Governance

APIs are durable contracts, not just function calls over HTTP. In enterprise banking and wealth platforms, API quality affects downstream UI behavior, integration risk, auditability, supportability and release confidence.

For deeper API contract design and governance guidance, use [`api-contract-engineering/`](api-contract-engineering/README.md).

## Good API Contract Characteristics

| Characteristic | What it means |
|---|---|
| Domain-correct | Names match business vocabulary and source ownership. |
| Explicit | Request, response, errors, pagination and support states are documented. |
| Stable | Breaking changes are intentional, versioned or coordinated. |
| Observable | Calls can be traced, measured and diagnosed. |
| Secure | Authorization, data classification and sensitive fields are understood. |
| Testable | OpenAPI, examples, vocabulary and response behavior are covered by gates. |

## Endpoint Design

Use resource and workflow names that reflect business meaning:

```text
GET /api/v1/portfolios/{portfolio_id}/holdings
POST /api/v1/rebalance/simulations
GET /api/v1/reports/{report_id}/status
```

Avoid vague endpoints:

```text
POST /api/v1/process
GET /api/v1/data
POST /api/v1/run
```

## Request And Response Governance

Every material endpoint should define:

1. request model,
2. response model,
3. summary and description,
4. examples,
5. tags,
6. error responses,
7. authorization assumptions,
8. stale, partial, unavailable or permission-blocked states where relevant.

## Supportability In Responses

Do not return only data. Return enough supportability for consumers to avoid false certainty.

Example:

```json
{
  "portfolio_id": "DEMO_PORTFOLIO_001",
  "as_of_date": "2026-06-27",
  "state": "partial",
  "positions": [],
  "supportability": {
    "reason": "market_data_stale",
    "source_date": "2026-06-25",
    "blocked_for_client_reporting": true
  }
}
```

## Error Semantics

Good errors are bounded and safe.

| Case | Preferred behavior |
|---|---|
| Invalid input | `400` with field-level validation details. |
| Unauthorized | `401` without sensitive internal detail. |
| Forbidden | `403` with product-safe denial reason. |
| Missing resource | `404` when the caller may know the resource exists. |
| Conflict | `409` for idempotency, state transition or version conflict. |
| Upstream unavailable | `503` or degraded response with supportability state. |
| Validation passed but source incomplete | `200` with `partial`/`blocked` state when the business workflow supports partial reads. |

Avoid leaking:

1. stack traces,
2. raw SQL,
3. internal hostnames,
4. credentials,
5. account numbers,
6. entitlement engine internals,
7. source payloads.

## Pagination, Filtering And Sorting

Collection endpoints need guardrails:

1. default limit,
2. maximum limit,
3. stable sort,
4. cursor or page token where useful,
5. explicit filters,
6. total count only when affordable and meaningful.

Anti-pattern:

```text
GET /api/v1/transactions
```

Better:

```text
GET /api/v1/portfolios/{portfolio_id}/transactions?from_date=2026-01-01&to_date=2026-06-27&limit=100&cursor=...
```

## API Vocabulary

Shared vocabulary matters. If one service says `portfolio_id`, another says `accountPortfolioId`, and another says `pf`, integration cost rises.

Govern:

1. field names,
2. lifecycle states,
3. supportability states,
4. period names,
5. error codes,
6. source lineage fields,
7. pagination fields,
8. date and currency conventions.

## OpenAPI Gate

An OpenAPI gate should check:

1. schema generation succeeds,
2. public endpoints have summaries and tags,
3. request and response models are explicit,
4. examples exist for important workflows,
5. undocumented aliases are blocked,
6. vocabulary rules pass,
7. deprecated routes are visible and governed.

## Versioning And Aliases

Use versioning for true contract changes. Avoid keeping many aliases alive during pre-live development unless they protect real consumers.

Alias rule:

1. an alias must have a documented consumer or migration reason,
2. it must be tested,
3. it must have a retirement path,
4. it must not hide a better replacement contract.

## API Review Checklist

Before approving a new endpoint, ask:

1. Who owns the business truth?
2. Is the endpoint a domain API or a composition API?
3. Are request and response models typed and documented?
4. Are partial, stale and unavailable states explicit?
5. Are errors product-safe?
6. Is pagination bounded?
7. Does OpenAPI show the current implementation?
8. Are tests protecting schema, behavior and failure semantics?
