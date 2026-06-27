# Pagination, Filtering, Sorting, and Large Result Sets

## Purpose

This file explains how to design APIs that return large collections such as transactions, holdings, report jobs, events, audit records, work items, and data-product catalog entries.

Unbounded collection APIs are a common cause of slow UI, database pressure, memory growth, and production incidents.

## Core Rule

Never expose an unbounded list API for data that can grow.

Every collection API should define:

- maximum page size
- default page size
- pagination model
- sort order
- filter rules
- stable ordering
- response metadata
- performance expectations

## Pagination Patterns

### Offset Pagination

Example:

```text
GET /transactions?offset=100&limit=50
```

Pros:

- simple
- familiar
- easy for small datasets

Cons:

- slow for large offsets
- unstable when data changes
- duplicate or missing rows possible under inserts/deletes

Best for:

- small stable administrative lists
- low-volume reference lists

### Cursor Pagination

Example:

```text
GET /transactions?cursor=abc123&pageSize=50
```

Pros:

- stable for changing datasets
- hides internal implementation
- efficient when implemented well

Cons:

- more complex
- cursor needs encoding and validation
- harder to jump to arbitrary page

Best for:

- large transaction lists
- event logs
- activity feeds
- user-facing scrolling

### Keyset Pagination

Example:

```text
GET /transactions?afterTradeDate=2026-04-10&afterId=txn123&limit=50
```

Pros:

- efficient
- stable
- database-friendly with correct indexes

Cons:

- requires strict sort key
- not ideal for arbitrary sorting

Best for:

- ordered financial records
- transaction timelines
- audit events

### Time-Window Pagination

Example:

```text
GET /transactions?fromDate=2026-01-01&toDate=2026-03-31&cursor=...
```

Pros:

- natural for financial and operational records
- aligns with reporting and audit windows
- improves query selectivity

Cons:

- boundary semantics must be clear
- time zones/business dates matter

Best for:

- transactions
- cashflows
- events
- reports
- operational logs

## Response Shape

Good response:

```json
{
  "items": [],
  "page": {
    "pageSize": 50,
    "nextCursor": "synthetic-cursor",
    "hasMore": true,
    "sort": "tradeDate:desc,id:desc"
  },
  "filters": {
    "fromDate": "2026-01-01",
    "toDate": "2026-03-31"
  }
}
```

## Sorting

Sorting must be deterministic.

A stable sort often needs a tie-breaker:

```text
tradeDate desc, transactionId desc
```

Avoid sorting only by non-unique fields when paginating.

## Filtering

Filters should be:

- documented
- indexed where needed
- validated
- bounded
- compatible with pagination
- safe for authorization

Common filters:

- date range
- status
- type
- portfolio scope
- booking center
- source system
- supportability state
- created/updated range

## Large Transaction API Guidance

For transaction APIs:

- require date window or cursor
- use stable order
- use keyset/cursor pagination for deep scrolling
- avoid offset for very large datasets
- index portfolio/date/id paths
- return lightweight list DTO first
- provide detail route for full record
- document max page size
- expose next cursor
- test realistic volume

## Filtering And Authorization

Filtering must not bypass authorization.

Bad pattern:

```text
filter by portfolio id only in UI
```

Good pattern:

```text
backend validates caller scope and applies authorization before returning results
```

## Pagination Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No limit | Slow queries and memory pressure. |
| Very high max page size | Accidental bulk export and performance risk. |
| Offset on huge table | Slow and unstable. |
| Sort without tie-breaker | Duplicate/missing records across pages. |
| Filters not indexed | Database load and timeouts. |
| Cursor exposes raw SQL details | Security and compatibility risk. |
| UI downloads all data | Poor user experience and backend load. |

## Review Checklist

- Is the list bounded?
- Is default page size reasonable?
- Is max page size enforced?
- Is ordering stable?
- Is a cursor or keyset needed?
- Are filters validated?
- Are filters indexed?
- Are authorization rules applied server-side?
- Is response metadata clear?
- Are large-volume tests present?
- Are examples included in OpenAPI?
- Are unsupported sort/filter combinations rejected?

## Summary

Pagination is API design, database design, and UX design working together.

Large result sets must be treated as first-class design problems, not as simple arrays.
