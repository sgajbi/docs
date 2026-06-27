# Request, Response, DTO, and Domain Boundaries

## Purpose

This file explains how to design request and response models while keeping transport, domain, and persistence concerns separate.

A good DTO protects the API contract. It should not become the domain model or persistence model.

## Three Different Models

| Model | Purpose |
|---|---|
| DTO | API request/response shape. |
| Domain model | Business behavior and invariants. |
| Persistence model | Database storage shape. |

Confusing these creates coupling.

## Request DTOs

A request DTO should:

- validate input shape
- use clear field names
- document required and optional fields
- use canonical enums
- avoid ambiguous booleans
- reject unsupported combinations
- provide examples
- map to application command/query

Request DTOs should not:

- contain business methods
- expose database field names accidentally
- accept arbitrary nested dictionaries without contract
- include internal-only fields
- mix UI display concerns with domain semantics

## Response DTOs

A response DTO should:

- expose stable public fields
- include metadata where useful
- include supportability where needed
- include warnings where needed
- include lineage or evidence refs where relevant
- avoid internal identifiers unless contractually required
- avoid raw domain exceptions
- avoid sensitive data

## Mapping Pattern

```text
RequestDTO
  -> Command/Query
  -> Domain/Application Result
  -> ResponseDTO
```

Mapping is where contract compatibility can be handled.

## DTO Validation

Validate:

- required fields
- enums
- date formats
- numeric ranges
- currency formats
- pagination limits
- mutually exclusive fields
- unsupported field combinations
- idempotency requirements
- source selector compatibility

## Avoid Boolean Traps

Poor:

```json
{
  "includeAll": true,
  "safe": false
}
```

Better:

```json
{
  "selectionMode": "ALL_ELIGIBLE",
  "riskTreatment": "EXCLUDE_UNSUPPORTED"
}
```

Explicit enums are easier to govern.

## Response Metadata

Useful metadata:

```json
{
  "data": {},
  "metadata": {
    "asOfDate": "2026-04-10",
    "generatedAt": "2026-04-10T12:00:00Z",
    "source": "portfolio-source",
    "correlationId": "synthetic-correlation-id"
  },
  "supportability": {
    "state": "READY",
    "reason": "SOURCE_READY"
  },
  "warnings": []
}
```

## DTO Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| DTO reused as domain entity | Transport concerns leak into business logic. |
| ORM returned as response | Database schema becomes public API. |
| Request accepts `dict[str, Any]` everywhere | Weak contract and validation. |
| Response contains raw upstream payload | Coupling and sensitive-data risk. |
| Compatibility aliases undocumented | Consumers depend on accidents. |
| Field names mix styles randomly | Poor developer experience. |
| Optional field meaning unclear | Consumers misuse response. |

## Review Checklist

- Are DTOs explicit?
- Are domain models separate?
- Are persistence models hidden?
- Is mapping intentional?
- Are enums canonical?
- Are unsupported combinations rejected?
- Are response metadata and supportability included where useful?
- Are sensitive fields excluded?
- Are examples current?
- Are DTO tests present?

## Summary

DTOs are the API's public language.

Design them deliberately and map them explicitly so domain and persistence models stay clean.
