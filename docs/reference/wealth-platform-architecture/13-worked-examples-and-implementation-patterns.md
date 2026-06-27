# Worked Examples and Implementation Patterns

This file turns wealth-platform architecture principles into practical examples for domain design, API governance, event contracts, source ownership, migration, observability, reporting, advisory workflows and implementation review.

## Example 1. Domain ownership for a portfolio review

**Scenario:** An advisor opens a portfolio review for a client household.

| Capability | Owner | What it owns | What it should not own |
|---|---|---|---|
| Client master | Client identity domain | Parties, accounts, relationships, household links | Portfolio valuation |
| Portfolio domain | Portfolio domain | Portfolio membership, base currency, mandate link | Product terms |
| Transaction domain | Trade/lifecycle domain | Trades, cashflows, bookings, corrections | Performance methodology |
| Position domain | Position domain | Holdings and position snapshots | Product eligibility |
| Market data | Data domain | Prices, FX, curves, stale flags | Client-specific suitability |
| Analytics | Analytics domain | Performance, risk, attribution, exposure | Source transaction corrections |
| Reporting | Reporting domain | Report snapshot, rendering, delivery evidence | Recalculating core analytics locally |

**Architecture rule:** A portfolio review should compose domain-owned facts. It should not create a private shadow data model inside the UI or reporting layer.

## Example 2. Command/query split for advisory proposals

**Scenario:** An advisor creates a proposal, simulates impact and sends it for client approval.

| Interaction | Pattern | Rationale |
|---|---|---|
| Search eligible products | Query | No state change. |
| Simulate portfolio impact | Query with reproducible calculation inputs | May be expensive but should not mutate proposal state. |
| Create draft proposal | Command | Creates auditable business object. |
| Submit suitability check | Command | Changes workflow state and stores evidence. |
| Retrieve proposal status | Query | Reads official workflow state. |
| Capture client consent | Command | Creates immutable consent evidence. |

**Anti-pattern:** A `GET` endpoint that creates proposal records because it is convenient for the UI.

## Example 3. Event contract for a booked transaction

**Scenario:** A trade lifecycle service books a transaction and publishes an event for downstream position, performance and reporting consumers.

Minimum event contract:

```text
event_type: TransactionBooked
event_id: globally unique event id
event_time: source publication timestamp
producer: transaction service
subject: transaction id
portfolio_id: affected portfolio
account_id: affected account
instrument_id: affected instrument
effective_date: business date
source_version: source transaction version
correction_of: optional prior transaction id
lineage_ref: source and ingestion reference
```

**Consumer behavior:** Position and analytics services should use idempotency keys, preserve event version, and handle correction events through reversal or amendment logic.

## Example 4. Degraded market-data state

**Scenario:** A fund NAV is stale and FX is current.

Expected architecture behavior:

| Layer | Behavior |
|---|---|
| Market-data service | Publishes price with stale flag and valuation date. |
| Position valuation | Calculates value but marks source-limited. |
| Performance engine | Marks affected return as partial or stale. |
| Reporting service | Shows valuation date and methodology disclosure. |
| Advisor workbench | Shows remediation or caution banner. |
| API response | Includes machine-readable degraded-state code. |

**Architecture rule:** Degraded state should travel with the data. It should not be lost when values are transformed.

## Example 5. Portfolio analytics calculation workflow

**Scenario:** A backdated transaction arrives after a monthly report was generated.

Expected workflow:

1. detect earliest impacted business date,
2. invalidate affected position snapshots,
3. recalculate valuation, cash, income, performance, contribution and exposure,
4. compare old and new report datasets,
5. apply materiality policy,
6. create restatement workflow if required,
7. preserve old and new lineage.

**Implementation pattern:** Analytics should be reproducible from versioned inputs. Published report snapshots should remain retrievable even after recalculation.

## Example 6. Migration parallel-run evidence

**Scenario:** A country rollout migrates client portfolios from a legacy core to a new platform.

Parallel-run evidence should include:

| Evidence | Purpose |
|---|---|
| Client/account count reconciliation | Confirms migration completeness. |
| Position value reconciliation | Confirms holdings and valuation migration. |
| Cash and ledger reconciliation | Confirms accounting consistency. |
| Performance sample replay | Confirms methodology and history. |
| Report output comparison | Confirms client-facing truth. |
| Exception inventory | Tracks unresolved breaks and sign-off. |
| Rollback plan | Defines operational fallback. |

**Architecture rule:** Do not treat migration as complete because data loaded successfully. Completion requires reconciled business outputs.

## Example 7. Resilience and observability for report generation

**Scenario:** End-of-month report generation depends on positions, prices, FX, performance and document rendering.

Minimum observability:

1. correlation ID across services,
2. report snapshot ID,
3. source dataset versions,
4. calculation version,
5. rendering version,
6. failed dependency and retry status,
7. degraded-state reason codes,
8. publication and delivery status,
9. archive reference,
10. operator action log.

**QA assertion:** Support should be able to explain why a report failed or why a report number changed without reading raw logs manually.

## Example 8. Architecture decision record

Use an ADR when a decision changes platform truth.

Minimum ADR fields:

| Field | Example |
|---|---|
| Decision | Performance engine owns TWR and MWR methodology. |
| Context | Reporting, advisor dashboards and APIs need consistent return numbers. |
| Options considered | Reporting-local calculation, portfolio-service calculation, dedicated analytics engine. |
| Decision rationale | Centralized analytics avoids inconsistent returns. |
| Consequences | Reporting consumes analytics outputs and lineage instead of recalculating. |
| Validation | Golden portfolios, regression tests, report comparisons. |

**Architecture rule:** ADRs should capture tradeoffs, not just the final answer.
