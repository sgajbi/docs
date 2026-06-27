# 11 - Service Boundary, Contract and Implementation Guide

## 1. Purpose

Canonical data dictionaries are only useful when they become enforceable service boundaries, API contracts, event schemas, QA scenarios and operating evidence.

This guide translates the canonical dictionary into implementation patterns that can be reused across wealth platforms without depending on a specific application or vendor architecture.

## 2. Domain Ownership Model

| Domain | Typical Owner | Contract Surface |
|---|---|---|
| Party, relationship and household | Client master / CRM domain | Party APIs, relationship events, entitlement-scoped read models. |
| Account and custody container | Account master / custody integration domain | Account APIs, account status events, booking-centre references. |
| Portfolio and mandate grouping | Portfolio master / advisory or discretionary domain | Portfolio APIs, mandate events, target allocation contracts. |
| Instrument and product master | Instrument master / product governance domain | Instrument APIs, product terms, eligibility and risk-classification events. |
| Market data, FX and pricing | Market-data domain | Price APIs, valuation source events, stale-price controls. |
| Transaction and order lifecycle | Trading / operations domain | Order, trade, transaction and settlement APIs/events. |
| Position, cash, lot and ledger | Accounting / book-of-record domain | Position APIs, ledger posting events, cash balance snapshots. |
| Performance and attribution | Performance analytics domain | Performance APIs, calculation result events, methodology versions. |
| Risk and exposure | Risk analytics domain | Risk metric APIs, exposure events, stress and concentration outputs. |
| Reporting and archive | Reporting / document domain | Report snapshot APIs, generation events, immutable archive references. |
| Advisory, suitability and workflow | Advisory workflow domain | Proposal, suitability, consent, task and approval APIs/events. |
| AI context and evaluation | AI platform / governance domain | Retrieval context, prompt, response, evaluation and audit evidence contracts. |

## 3. Boundary Rules

| Rule | Meaning |
|---|---|
| One authoritative owner | Every canonical entity has one owner for mutation and source-of-truth semantics. |
| APIs for queries | Other domains consume stable APIs or governed read models instead of reading internal storage. |
| Events for state changes | Important lifecycle changes publish versioned events with lineage and idempotency keys. |
| No shared database coupling | Database tables are implementation details, not cross-domain contracts. |
| Shared schema library | Common money, identifier, date, lineage, status and error shapes are defined once. |
| Versioned calculations | Performance, risk, valuation and suitability outputs carry methodology and calculation version. |
| Idempotent ingestion | Feeds, events and commands can be safely retried without duplicate economic impact. |
| Audit by default | Mutations, overrides, corrections and generated outputs produce durable evidence. |

## 4. Suggested Contract Repository Shape

```text
contracts/
  openapi/
    account-api.yaml
    instrument-api.yaml
    portfolio-api.yaml
    performance-api.yaml
    risk-api.yaml
    report-api.yaml
  asyncapi/
    wealth-events.yaml
  schemas/
    common/
      money.schema.json
      identifier.schema.json
      lineage.schema.json
      problem-detail.schema.json
    transactions/
      transaction-booked.schema.json
      transaction-corrected.schema.json
    positions/
      position-updated.schema.json
      cash-balance-updated.schema.json
    reporting/
      report-snapshot-created.schema.json
    ai/
      ai-interaction-recorded.schema.json
      ai-evaluation-recorded.schema.json
```

## 5. Shared Schema: Money

```json
{
  "$id": "https://schemas.example.com/common/money.schema.json",
  "type": "object",
  "required": ["amount", "currency"],
  "properties": {
    "amount": {
      "type": "string",
      "pattern": "^-?\\d+(\\.\\d+)?$"
    },
    "currency": {
      "type": "string",
      "minLength": 3,
      "maxLength": 3
    }
  }
}
```

## 6. Shared Schema: Lineage

```json
{
  "$id": "https://schemas.example.com/common/lineage.schema.json",
  "type": "object",
  "required": ["lineageId", "sourceSystem", "processingTimestamp"],
  "properties": {
    "lineageId": {"type": "string"},
    "sourceSystem": {"type": "string"},
    "sourceRecordId": {"type": "string"},
    "calculationVersion": {"type": "string"},
    "processingTimestamp": {"type": "string", "format": "date-time"},
    "qualityStatus": {
      "type": "string",
      "enum": ["COMPLETE", "PARTIAL", "DEGRADED", "BLOCKED", "ESTIMATED", "OVERRIDDEN"]
    }
  }
}
```

## 7. API Ownership Examples

| API | Owning Domain | Notes |
|---|---|---|
| `GET /accounts/{accountId}` | Account master | Legal container, booking centre, status and restrictions. |
| `GET /portfolios/{portfolioId}` | Portfolio master | Reporting grouping, mandate links and portfolio hierarchy. |
| `GET /instruments/{instrumentId}` | Instrument master | Product terms, classification, identifiers and lifecycle terms. |
| `GET /prices/{instrumentId}` | Market data | Price, FX, source, timestamp, stale state and valuation basis. |
| `GET /portfolios/{portfolioId}/positions` | Book of record | Current holdings, cash balances, lots and source lineage. |
| `GET /portfolios/{portfolioId}/performance` | Performance analytics | Methodology, return basis, cashflow treatment and calculation version. |
| `GET /portfolios/{portfolioId}/risk-metrics` | Risk analytics | Exposure, concentration, stress and model provenance. |
| `POST /reports` | Reporting | Snapshot request, template, disclosure set and archive policy. |
| `POST /advisory-proposals` | Advisory workflow | Proposal content, suitability checks, consent and lifecycle status. |
| `POST /ai/portfolio-summary` | AI platform | Retrieval context, generated output, citations, safety status and evaluation evidence. |

## 8. Event Ownership Examples

| Event | Owning Domain | Meaning |
|---|---|---|
| `wealth.account.status-changed.v1` | Account master | Account status, restriction or booking attribute changed. |
| `wealth.instrument.updated.v1` | Instrument master | Instrument terms or classification changed. |
| `wealth.price.validated.v1` | Market data | Price passed validation and can be consumed downstream. |
| `wealth.transaction.booked.v1` | Book of record | Economic transaction was booked. |
| `wealth.position.updated.v1` | Book of record | Position or cash state changed after transaction, correction or valuation event. |
| `wealth.performance.calculated.v1` | Performance analytics | Performance result was calculated for a scope and period. |
| `wealth.risk.calculated.v1` | Risk analytics | Risk result was calculated for a scope and as-of date. |
| `wealth.report.snapshot-created.v1` | Reporting | Reproducible report snapshot was created. |
| `wealth.mandate.breach-detected.v1` | Advisory / mandate | Mandate or suitability breach was detected. |
| `wealth.ai.response-generated.v1` | AI platform | AI output was generated with traceable context and evaluation hooks. |

## 9. Implementation Guardrails

| Guardrail | Required Evidence |
|---|---|
| Canonical field naming | Data dictionary entry, source owner and API/event mapping. |
| Contract-first delivery | OpenAPI, AsyncAPI or JSON Schema updated before implementation is considered complete. |
| Explicit degraded states | Missing, stale, partial, estimated, restricted and overridden data states are represented. |
| Idempotent mutation paths | Commands, events and ingestion jobs use idempotency keys or deterministic natural keys. |
| Versioned methodology | Calculation outputs include methodology version, input set and effective date. |
| Audit and lineage | Every material output can trace source records, calculation version and processing timestamp. |
| Backward compatibility | Breaking changes require a new version or migration plan. |
| Contract tests | Provider and consumer tests verify API/event compatibility. |
| Reporting reproducibility | Report snapshots can be regenerated or explained from stored inputs and versions. |
| AI answer grounding | AI outputs reference retrieval sources, policy state and evaluation status. |

## 10. Implementation Prompt Pattern

When turning this reference into delivery work, include:

1. the canonical entity or event name,
2. source owner and consumer domains,
3. required fields and degraded-state behavior,
4. API or event contract path,
5. validation and lineage requirements,
6. UI/reporting labels,
7. migration and backward-compatibility constraints,
8. contract tests and regression scenarios.

## 11. Summary

The data dictionary should be treated as a product asset. It should shape API design, event contracts, source ownership, QA scenarios, report reproducibility, AI grounding and platform governance.
