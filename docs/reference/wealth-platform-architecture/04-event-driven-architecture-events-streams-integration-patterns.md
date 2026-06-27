# 04 - Event-Driven Architecture, Events, Streams and Integration Patterns

## 1. Why event-driven architecture matters in wealth platforms

Wealth platforms are lifecycle-heavy. A single portfolio view depends on many changes:

- client updated
- account opened
- product approved
- order placed
- trade executed
- transaction booked
- settlement completed
- position changed
- price updated
- corporate action announced
- coupon paid
- NAV published
- benchmark updated
- performance recalculated
- report generated
- mandate breached

Events allow systems to react to business changes asynchronously without tight point-to-point coupling.

## 2. Event vs command vs query

| Concept | Meaning | Example |
|---|---|---|
| Command | Request to do something | `CreateOrder`, `GeneratePortfolioReview` |
| Event | Fact that happened | `OrderPlaced`, `PositionUpdated`, `CouponPaid` |
| Query | Request for current/derived state | `GetPositions`, `GetPerformance` |

Events should be named in past tense because they represent facts.

## 3. Event categories

| Event category | Examples |
|---|---|
| Business lifecycle events | `AccountOpened`, `TradeExecuted`, `CouponPaid`, `ReportGenerated` |
| Data-update events | `PriceUpdated`, `FXRateUpdated`, `InstrumentUpdated` |
| Calculation events | `PerformanceCalculated`, `RiskCalculated`, `MandateEvaluated` |
| Workflow events | `ProposalApproved`, `ClientConsentCaptured`, `TaskEscalated` |
| Exception events | `ReconciliationBreakRaised`, `PriceStaleDetected`, `ReportGenerationFailed` |
| Control events | `MandateBreachDetected`, `SuitabilityCheckFailed`, `EntitlementChanged` |

## 4. Event contract design

A good event contract includes:

| Field | Purpose |
|---|---|
| eventId | Unique event identity. |
| eventType | Business event name. |
| eventVersion | Schema version. |
| occurredAt | When business event occurred. |
| publishedAt | When event was published. |
| sourceSystem | Publishing system. |
| correlationId | Trace across workflow. |
| causationId | Parent command/event that caused this event. |
| businessDate | Relevant business date. |
| entityType/entityId | Main affected business entity. |
| payload | Business data. |
| lineage | Source record, batch, API, feed or calculation metadata. |
| dataQuality | Optional quality/degraded-state flags. |

Example:

```json
{
  "eventId": "evt-001",
  "eventType": "PositionUpdated",
  "eventVersion": "1.0",
  "occurredAt": "2026-06-30T18:30:00Z",
  "publishedAt": "2026-06-30T18:31:00Z",
  "sourceSystem": "position-service",
  "correlationId": "corr-789",
  "businessDate": "2026-06-30",
  "entityType": "PORTFOLIO",
  "entityId": "P123",
  "payload": {
    "instrumentId": "ABC",
    "quantity": "1000",
    "basis": "TRADE_DATE"
  }
}
```

## 5. CloudEvents-style envelope

CloudEvents provides a common event metadata envelope. Even if a platform does not fully adopt CloudEvents, the same principle is useful: separate event metadata from business payload.

Recommended event-envelope fields:

```text
id
type
source
specversion
time
subject
datacontenttype
data
```

Add domain extensions carefully, such as:

```text
businessDate
correlationId
causationId
portfolioId
sourceRecordId
schemaVersion
```

## 6. AsyncAPI for event contracts

AsyncAPI is useful for documenting event-driven interfaces, including channels/topics, messages, payload schemas, security, bindings and examples.

Use AsyncAPI-style contract governance for:

- position events
- transaction events
- market-data events
- product events
- corporate-action events
- report events
- workflow events
- reconciliation events

## 7. Topics and stream design

### Topic naming pattern

```text
<domain>.<entity>.<event-family>.<version>
```

Examples:

```text
positions.position.updated.v1
transactions.transaction.booked.v1
marketdata.price.updated.v1
products.instrument.updated.v1
reports.report.generated.v1
mandates.breach.detected.v1
```

### Partition keys

Choose partition keys based on ordering requirements.

| Domain | Candidate partition key |
|---|---|
| Transactions | accountId or portfolioId |
| Positions | portfolioId |
| Prices | instrumentId |
| FX rates | currencyPair |
| Corporate actions | instrumentId |
| Reports | reportRequestId or portfolioId |
| Workflow | caseId or proposalId |

Ordering should be explicit. Do not assume global ordering across all topics.

## 8. Idempotency and deduplication

Consumers must be idempotent.

Recommended deduplication keys:

| Event | Dedup key |
|---|---|
| Transaction booked | sourceSystem + sourceTransactionId + version |
| Price updated | instrumentId + priceDate + priceType + source |
| FX rate updated | currencyPair + rateDate + rateType + source |
| Corporate action | eventId + optionId + version |
| Report generated | reportRequestId + reportVersion |
| Performance calculated | portfolioId + period + method + calculationVersion |

## 9. Replay and backfill

Replay is required for:

- rebuilding positions
- recalculating performance after corrected transactions/prices
- migration parallel runs
- recovering failed consumers
- regenerating reporting snapshots
- auditing historical results

Replay rules:

1. Replay must use deterministic event ordering within the relevant aggregate.
2. Consumers must distinguish live processing from replay processing.
3. Replay should not trigger duplicate client notifications unless explicitly allowed.
4. Reprocessing should create lineage to the replay batch or job.
5. Replayed results should be reconciled before publication to official outputs.

## 10. Event-driven integration patterns

| Pattern | Use | Risk |
|---|---|---|
| Publish/subscribe | Notify multiple consumers of business change | Consumers may diverge if contracts weak |
| Event-carried state transfer | Event includes enough data for consumers | Payload can grow and become duplicated truth |
| Event notification + API pull | Event notifies, consumer calls API for details | More dependency on source API availability |
| Outbox pattern | Reliable publishing from transactional store | Requires outbox processing controls |
| Inbox pattern | Consumer deduplication and processing status | Extra persistence required |
| Saga/process manager | Multi-step workflow orchestration | Complex failure compensation |
| CQRS projection | Read model built from events | Projection lag and rebuild controls needed |
| Dead-letter queue | Isolate poison events | Needs operational triage workflow |

## 11. Event sourcing vs event-driven integration

Do not confuse these.

| Concept | Meaning |
|---|---|
| Event-driven integration | Systems publish facts to integrate asynchronously. |
| Event sourcing | The domain state is stored as a sequence of events and rebuilt from them. |

Most wealth platforms need event-driven integration. Only selected domains may benefit from true event sourcing, such as workflow, order state or calculation job state.

Transactions and accounting ledgers can be append-only without being fully event-sourced.

## 12. Common event anti-patterns

| Anti-pattern | Problem |
|---|---|
| Event named as command | `UpdatePosition` instead of `PositionUpdated` creates ambiguity. |
| No schema version | Consumers break silently. |
| No correlation ID | Workflow tracing becomes impossible. |
| No business date | Reporting and reconciliation become ambiguous. |
| No source lineage | Cannot prove where data came from. |
| Reusing one generic event for everything | Consumers need custom parsing and assumptions. |
| Events carry UI-specific data | Domain event becomes channel-specific. |
| Publishing before transaction commit | Consumers process facts that may roll back. |
| No replay strategy | Recovery and recalculation are fragile. |

## 13. Wealth-specific event examples

### Transaction booked

```text
Event: TransactionBooked
Consumers:
  - position service
  - cash balance service
  - accounting service
  - performance engine
  - reporting snapshot service
  - reconciliation service
```

### Price updated

```text
Event: PriceUpdated
Consumers:
  - valuation service
  - risk engine
  - performance engine
  - report readiness monitor
  - stale price control dashboard
```

### Mandate breach detected

```text
Event: MandateBreachDetected
Consumers:
  - advisor workbench
  - operations workflow
  - reporting dashboard
  - portfolio manager alerting
```

### Report generated

```text
Event: ReportGenerated
Consumers:
  - document archive
  - client portal
  - advisor notification
  - audit trail
```

## 14. Event governance checklist

Before publishing an event:

- Event name is past tense and domain meaningful.
- Schema is versioned.
- Payload has realistic examples.
- Partition key is defined.
- Ordering requirement is documented.
- Replay behavior is defined.
- Idempotency key is defined.
- Consumer impact is known.
- Data-quality flags are present where relevant.
- Sensitive data is minimized.
- Retention policy is defined.
- Dead-letter and retry strategy exists.
- Contract tests exist.
