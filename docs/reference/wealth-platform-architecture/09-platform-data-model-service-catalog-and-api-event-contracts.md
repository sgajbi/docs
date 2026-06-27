# 09 - Platform Data Model, Service Catalog and API/Event Contracts

## 1. Purpose

This file provides a canonical catalogue of major services, entities, APIs and events in a wealth platform. It is intended as a starting point for architecture design, backlog structuring, service ownership and Codex/agentic implementation guidance.

## 2. Canonical entity model

| Entity | Meaning |
|---|---|
| Party | Person, company, trust, foundation or legal entity. |
| Client | Party that receives wealth services. |
| Beneficial owner | Natural/legal owner or controlling person behind structure. |
| Household/group | Reporting or relationship grouping across clients/entities. |
| Account | Legal/custody/banking account. |
| Portfolio | Investment view or managed portfolio under an account/client. |
| Mandate | Advisory/DPM/investment mandate attached to portfolio. |
| Instrument | Tradable or reportable product/security. |
| Product | Higher-level offering or product wrapper. |
| Issuer | Entity issuing security/product. |
| Counterparty | Entity on opposite side of trade/derivative/lending exposure. |
| Order | Instruction to transact. |
| Execution | Market/counterparty fill. |
| Transaction | Economic posting. |
| Transaction leg | Cash/security/income/fee/tax leg. |
| Position | Holding balance. |
| Lot | Cost/tax/performance lot. |
| Cash balance | Cash by account/currency/basis. |
| Price | Instrument valuation input. |
| FX rate | Currency translation input. |
| Benchmark | Reference index/composite/strategy. |
| Report snapshot | Frozen report input/output version. |
| Workflow case | Task/approval/exception process. |

## 3. Service catalog

| Service | Owns | Key APIs | Key events |
|---|---|---|---|
| Client Master | Parties, relationships, KYC profile | Get client, get hierarchy | ClientUpdated, RelationshipChanged |
| Account Portfolio Master | Accounts, portfolios, mandates, booking centre | Get accounts, get portfolio hierarchy | PortfolioOpened, MandateChanged |
| Product Master | Instruments, product taxonomy, terms | Get instrument, search products | InstrumentUpdated, ProductTermsChanged |
| Product Governance | APU, target market, risk rating, eligibility | Check product eligibility | ProductApproved, ProductSuspended |
| Market Data | Prices, FX, rates, curves, benchmarks | Get price, get FX, get curve | PriceUpdated, FXRateUpdated |
| Order Service | Orders and order state | Create order, get order | OrderPlaced, OrderCancelled |
| Execution Service | Executions/fills/allocations | Get executions | TradeExecuted, AllocationConfirmed |
| Transaction Service | Economic transactions/legs | Get transactions, book transaction | TransactionBooked, TransactionReversed |
| Position Service | Holdings and balances | Get positions | PositionUpdated, PositionRebuilt |
| Cash Service | Cash balances and projections | Get cash, get availability | CashBalanceUpdated |
| Accounting Service | Lots, accruals, P&L, fees/tax | Get P&L, get lots | LotCreated, AccrualPosted |
| Performance Service | Returns, contribution, attribution | Get performance | PerformanceCalculated |
| Risk Service | Risk, exposure, concentration | Get risk summary | RiskCalculated, ConcentrationBreachDetected |
| Mandate Service | Rules and breach monitoring | Check mandate | MandateBreachDetected, MandateBreachResolved |
| Reporting Service | Snapshots, reports, PDFs | Request report, get report | ReportSnapshotCreated, ReportGenerated |
| Entitlement Service | Access and data scope | Check access | EntitlementChanged |
| Workflow Service | Tasks, approvals, exceptions | Create task, approve task | TaskCreated, TaskCompleted |
| Reconciliation Service | Breaks and certification | Get breaks, certify recon | ReconciliationBreakRaised |

## 4. Command APIs

| API | Purpose | Idempotency required? |
|---|---|---|
| `POST /advisory-proposals` | Create proposal. | Yes |
| `POST /advisory-proposals/{id}/submit` | Submit for review/client consent. | Yes |
| `POST /orders` | Create order. | Yes |
| `POST /reports/portfolio-review-requests` | Request report generation. | Yes |
| `POST /analytics/recalculation-jobs` | Trigger recalculation. | Yes |
| `POST /workflow/tasks/{id}/complete` | Complete task. | Yes |
| `POST /exceptions/{id}/resolve` | Resolve exception. | Yes |

## 5. Query APIs

| API | Purpose |
|---|---|
| `GET /clients/{id}` | Client details. |
| `GET /clients/{id}/relationship-tree` | Household/entity hierarchy. |
| `GET /portfolios/{id}/summary` | Portfolio dashboard. |
| `GET /portfolios/{id}/positions` | Holdings. |
| `GET /accounts/{id}/transactions` | Transactions. |
| `GET /portfolios/{id}/performance` | Returns. |
| `GET /portfolios/{id}/risk-summary` | Risk view. |
| `GET /portfolios/{id}/mandate-status` | Mandate results. |
| `GET /reports/{id}` | Report metadata/output. |

## 6. Event catalog

| Event | Publisher | Key consumers |
|---|---|---|
| ClientUpdated | Client Master | Entitlements, Advisory, Reporting |
| PortfolioOpened | Account Master | Position, Reporting, Advisory |
| InstrumentUpdated | Product Master | Market Data, Risk, Reporting |
| ProductSuspended | Product Governance | Advisory, OMS, Mandate |
| PriceUpdated | Market Data | Valuation, Risk, Reporting |
| OrderPlaced | Order Service | OMS/Execution, Workflow |
| TradeExecuted | Execution Service | Transaction, Position, Reporting |
| TransactionBooked | Transaction Service | Position, Cash, Accounting, Performance |
| PositionUpdated | Position Service | Valuation, Risk, Reporting |
| CashBalanceUpdated | Cash Service | Buying Power, Reporting |
| PerformanceCalculated | Performance Service | Reporting, Advisor Dashboard |
| MandateBreachDetected | Mandate Service | Workflow, Advisor Workbench |
| ReportGenerated | Reporting Service | Archive, Client Portal |
| ReconciliationBreakRaised | Reconciliation Service | Operations Workflow |

## 7. Data model separation

```text
Order model
  != Trade execution model
  != Transaction model
  != Ledger model
  != Position model
  != Reporting model
```

Each model can reference the others through stable identifiers and lineage.

## 8. Identifier strategy

Use multiple identifiers where needed:

| ID | Purpose |
|---|---|
| internalId | Platform internal identity. |
| sourceSystemId | Source record linkage. |
| externalIdentifier | ISIN, CUSIP, FIGI, LEI, account number, etc. |
| businessKey | Natural key where safe, e.g. portfolio + instrument + asOfDate. |
| correlationId | Trace workflow across systems. |
| idempotencyKey | Deduplicate command requests. |
| snapshotId | Versioned report/calculation input set. |

## 9. Common data contract fields

Every business object should consider:

```text
id
sourceSystem
sourceRecordId
businessDate
effectiveDate
status
version
createdAt
updatedAt
createdBy
lineageId
correlationId
dataQualityStatus
```

## 10. Status modelling

Do not use free text for lifecycle states. Use controlled status enums.

Examples:

| Domain | Status examples |
|---|---|
| Order | Draft, pending approval, submitted, executed, cancelled, rejected. |
| Transaction | Pending, booked, settled, reversed, corrected. |
| Position | Active, closed, blocked, pledged, transferred. |
| Report | Requested, snapshot created, generating, generated, failed, archived. |
| Calculation job | Pending, running, completed, failed, cancelled. |
| Data quality | OK, warning, degraded, blocked, provisional, stale. |
| Mandate breach | Open, acknowledged, remediated, waived, closed. |

## 11. Service ownership template

For each service, document:

```text
Service name
Business capability
Owned entities
Consumed entities
Primary database/store
Inbound APIs/events/files
Outbound APIs/events
Calculation rules
Data-quality rules
Security model
Operational SLOs
Reconciliation controls
Runbooks
Downstream reporting impact
```

## 12. API/event contract review template

For each contract:

```text
Contract name
Domain owner
Consumers
Purpose
Schema version
Example payload
Security scope
Data-quality fields
Backward compatibility notes
Deprecation policy
Reconciliation/audit impact
Test cases
```
