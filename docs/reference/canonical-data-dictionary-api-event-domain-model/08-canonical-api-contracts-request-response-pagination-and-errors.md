# 08 - Canonical API Contracts, Request/Response, Pagination and Errors

## 1. API design principles

Wealth APIs should be:

- consistent
- versioned
- entitlement-aware
- idempotent for mutations
- explicit on dates and currency
- precise on status
- traceable
- paginated
- filterable
- observable
- documented with examples
- clear on degraded data

## 2. Standard headers

| Header | Purpose |
|---|---|
| Authorization | bearer token / auth |
| X-Correlation-Id | trace request across systems |
| Idempotency-Key | safe retry for mutations |
| X-Client-Channel | advisor, client_portal, batch, api |
| X-Request-Source | calling app |
| Accept-Language | localization |
| X-As-Of-Date | optional as-of date for read APIs |

## 3. Standard response envelope

```json
{
  "data": {},
  "meta": {
    "asOfDate": "2026-06-27",
    "baseCurrency": "USD",
    "dataQualityStatus": "complete",
    "calculationVersion": "perf-v1.2.0",
    "sourceSnapshotId": "SNAP-001"
  },
  "links": {
    "self": "/v1/portfolios/P-100/summary"
  }
}
```

## 4. Error model

Use RFC 7807-style problem details.

```json
{
  "type": "https://docs.example.com/errors/stale-price",
  "title": "Stale price prevents final report generation",
  "status": 422,
  "detail": "Instrument EQ-ABC has no approved reporting price within the stale-price threshold.",
  "instance": "/v1/reports/R-100",
  "correlationId": "CORR-123",
  "errors": [
    {
      "field": "priceId",
      "code": "STALE_PRICE",
      "message": "Latest approved price is older than threshold."
    }
  ]
}
```

## 5. Pagination pattern

For large lists, prefer cursor/keyset pagination.

Request:

```http
GET /v1/portfolios/P-100/transactions?limit=100&cursor=eyJ...
```

Response:

```json
{
  "data": [],
  "page": {
    "limit": 100,
    "nextCursor": "eyJ...",
    "hasMore": true
  }
}
```

Use offset pagination only for small/admin lists where performance and consistency concerns are low.

## 6. Filtering examples

Transactions:

```http
GET /v1/portfolios/P-100/transactions?fromDate=2026-01-01&toDate=2026-06-27&type=BOND_COUPON
```

Positions:

```http
GET /v1/portfolios/P-100/positions?assetClass=fixed_income&asOfDate=2026-06-27
```

Performance:

```http
GET /v1/portfolios/P-100/performance?period=YTD&breakdown=asset_class&method=TWR
```

## 7. Portfolio summary API

```http
GET /v1/portfolios/{portfolio_id}/summary?asOfDate=2026-06-27
```

Response:

```json
{
  "data": {
    "portfolioId": "P-100",
    "baseCurrency": "USD",
    "marketValue": "1250000.00",
    "cashValue": "50000.00",
    "investedValue": "1200000.00",
    "performanceYtd": "0.0825",
    "riskProfile": "balanced"
  },
  "meta": {
    "asOfDate": "2026-06-27",
    "dataQualityStatus": "complete"
  }
}
```

## 8. Transaction API

```http
POST /v1/transactions
Idempotency-Key: abc-123
```

Request:

```json
{
  "portfolioId": "P-100",
  "transactionType": "BOND_BUY",
  "instrumentId": "BOND-ABC",
  "tradeDate": "2026-06-27",
  "settlementDate": "2026-07-01",
  "nominalAmount": "100000.00",
  "price": "98.50",
  "priceBasis": "PERCENT_OF_PAR",
  "cashCurrency": "USD"
}
```

## 9. Report generation API

```http
POST /v1/reports
```

Request:

```json
{
  "portfolioId": "P-100",
  "reportType": "PORTFOLIO_REVIEW",
  "asOfDate": "2026-06-27",
  "sections": ["overview", "holdings", "performance", "risk", "transactions"],
  "outputFormat": "PDF"
}
```

## 10. AI explanation API

```http
POST /v1/ai/portfolio-explanation
```

Request:

```json
{
  "portfolioId": "P-100",
  "question": "Why did the portfolio underperform this quarter?",
  "audience": "advisor",
  "requireCitations": true
}
```

Response:

```json
{
  "data": {
    "answer": "The portfolio underperformed mainly due to equity selection...",
    "status": "draft",
    "citations": [
      {"sourceType": "performance_attribution", "sourceId": "ATTR-100"}
    ],
    "humanApprovalRequired": true
  }
}
```

## 11. API validation rules

- all read APIs enforce entitlements.
- all mutation APIs require idempotency key.
- all financial amounts use decimal strings.
- all dates use ISO 8601.
- all responses include correlation metadata.
- degraded data status is explicit.
- errors use consistent problem format.
- APIs should not silently exclude unsupported data.

## 12. Key takeaway

Canonical APIs should be predictable, traceable, secure and reporting-grade.
