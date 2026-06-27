# 09 - Canonical Event Schemas, Lifecycle Events and Integration Patterns

## 1. Event design principles

Events should represent meaningful business facts.

Good events are:

- named in business language
- immutable
- versioned
- timestamped
- idempotent
- traceable
- schema-governed
- minimal but sufficient
- linked to source system
- safe for replay

## 2. Event envelope

```json
{
  "eventId": "EVT-001",
  "eventType": "transaction.booked",
  "eventVersion": "1.0",
  "occurredAt": "2026-06-27T10:00:00Z",
  "publishedAt": "2026-06-27T10:00:02Z",
  "sourceSystem": "order-service",
  "correlationId": "CORR-123",
  "causationId": "CMD-456",
  "entityType": "transaction",
  "entityId": "TXN-001",
  "payload": {}
}
```

## 3. Common event types

| Event | Meaning |
|---|---|
| client.created | new client record |
| account.opened | account opened |
| portfolio.created | portfolio created |
| instrument.created | instrument created |
| price.updated | price received/updated |
| fx_rate.updated | FX rate received |
| transaction.booked | transaction booked |
| transaction.settled | transaction settled |
| position.updated | position updated |
| valuation.completed | valuation completed |
| performance.calculated | performance run completed |
| report.generated | report created |
| report.delivered | report delivered |
| mandate.breach_detected | mandate breach detected |
| suitability.check_completed | suitability completed |
| ai.output_generated | AI output generated |
| workflow.task_created | task created |

## 4. Transaction booked event

```json
{
  "eventType": "transaction.booked",
  "eventVersion": "1.0",
  "payload": {
    "transactionId": "TXN-001",
    "transactionGroupId": "TXG-001",
    "portfolioId": "P-100",
    "accountId": "A-100",
    "transactionType": "BOND_BUY",
    "instrumentId": "BOND-ABC",
    "tradeDate": "2026-06-27",
    "settlementDate": "2026-07-01",
    "status": "booked"
  }
}
```

## 5. Position updated event

```json
{
  "eventType": "position.updated",
  "eventVersion": "1.0",
  "payload": {
    "portfolioId": "P-100",
    "instrumentId": "BOND-ABC",
    "quantity": "0",
    "nominalAmount": "100000.00",
    "asOfDate": "2026-06-27",
    "reason": "transaction.booked",
    "sourceTransactionId": "TXN-001"
  }
}
```

## 6. Corporate action event

```json
{
  "eventType": "corporate_action.announced",
  "eventVersion": "1.0",
  "payload": {
    "corporateActionId": "CA-001",
    "instrumentId": "EQ-ABC",
    "eventType": "STOCK_SPLIT",
    "ratio": "2:1",
    "exDate": "2026-07-10",
    "recordDate": "2026-07-11",
    "paymentDate": "2026-07-15",
    "status": "announced"
  }
}
```

## 7. Structured note observation event

```json
{
  "eventType": "structured_note.observation_completed",
  "eventVersion": "1.0",
  "payload": {
    "instrumentId": "NOTE-123",
    "observationDate": "2026-06-27",
    "observationType": "AUTOCALL",
    "observedLevelPct": "105.00",
    "triggerLevelPct": "100.00",
    "result": "TRIGGERED",
    "expectedPaymentDate": "2026-07-02"
  }
}
```

## 8. Mandate breach event

```json
{
  "eventType": "mandate.breach_detected",
  "eventVersion": "1.0",
  "payload": {
    "breachId": "BRCH-001",
    "portfolioId": "P-100",
    "mandateId": "M-100",
    "ruleId": "RULE-EQ-MAX",
    "dimension": "asset_class",
    "dimensionValue": "equity",
    "currentValue": "0.75",
    "limitValue": "0.70",
    "severity": "high"
  }
}
```

## 9. AI output generated event

```json
{
  "eventType": "ai.output_generated",
  "eventVersion": "1.0",
  "payload": {
    "aiInteractionId": "AI-001",
    "useCase": "portfolio_review_explanation",
    "portfolioId": "P-100",
    "outputStatus": "draft",
    "humanApprovalRequired": true,
    "modelName": "approved-llm",
    "evaluationStatus": "pending"
  }
}
```

## 10. Event implementation rules

- event schemas must be versioned.
- consumers must tolerate additive fields.
- events should carry IDs, not full sensitive payloads unless necessary.
- sensitive data in events must be minimized.
- event replay must be safe.
- idempotency must be enforced by event ID and business key.
- event ordering assumptions must be explicit.
- DLQ and retry strategy must exist.
- schema registry should be used for critical events.

## 11. Key takeaway

Events are the nervous system of the wealth platform. They must be business-meaningful, versioned and controlled.
