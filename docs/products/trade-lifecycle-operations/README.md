# Trade Lifecycle, Order Management, Settlement, Custody, Corporate Actions and Investment Operations Reference Pack

## Purpose

This pack documents the operational backbone that connects all investment-product families to real platform execution and reporting.

The earlier product packs explain what the products are. This pack explains how those products move through the wealth platform:

1. advisory decision and proposal
2. pre-trade controls
3. order capture
4. order routing and execution
5. trade capture
6. allocation
7. confirmation and affirmation
8. clearing
9. settlement
10. custody and safekeeping
11. income and corporate actions
12. reconciliation
13. exception management
14. position, cash, ledger, performance and reporting updates

The pack is designed for private-banking, wealth-management, DPM, advisory, analytics, reporting and enterprise-platform work.

## File map

| File | Purpose |
|---|---|
| `01-trade-lifecycle-fundamentals-and-operating-model.md` | End-to-end lifecycle, operating model, roles, states and control principles |
| `02-order-management-advisory-trade-capture-and-execution.md` | Advisory proposal to order capture, OMS/EMS, execution, order states, order types and product-specific order handling |
| `03-clearing-confirmation-affirmation-allocation-and-settlement.md` | Post-trade enrichment, matching, confirmation, settlement, DVP/PVP, settlement cycles, fails and settlement status |
| `04-custody-asset-servicing-corporate-actions-and-income-events.md` | Safekeeping, nominee/CDP/custody models, corporate actions, income events, proxy and entitlement processing |
| `05-reconciliation-exception-management-fails-breaks-and-controls.md` | Trade, cash, position, price, income, corporate-action and performance reconciliation |
| `06-data-model-order-trade-transaction-position-ledger-and-lineage.md` | Canonical data model across order, execution, trade, transaction, cash, position, ledger and lineage |
| `07-product-specific-operations-treatment-across-asset-classes.md` | Operational treatment by product family: equities, bonds, funds, notes, derivatives, FX, private markets, loans, insurance and real assets |
| `08-advisory-mandate-analytics-reporting-and-client-experience.md` | Advisory, DPM, mandate, analytics and client-reporting impact of operational lifecycle events |
| `09-platform-architecture-integration-patterns-controls-and-test-scenarios.md` | Platform architecture, integration patterns, idempotency, eventing, controls, QA and production scenarios |
| `10-glossary-practitioner-reference-and-source-notes.md` | Glossary, practitioner explanations, operating reference and source notes |
| `11-worked-examples-and-implementation-patterns.md` | Practical examples for partial fills, bond accrued interest, settlement fails, fund cut-offs, corporate-action revisions, allocation corrections, linked FX settlement, custody transfers, securities lending recalls, failed elections, nostro breaks, omnibus allocations, market claims, intraday settlement cut-offs, partial custody transfers, buy-in exposure, tax-lot transfer portability, cross-custodian SSI migration, operational incident communication, affirmation mismatch repair, corporate-action election escalation, failed FX compensation, omnibus rounding disputes, settlement penalty pass-through, post-incident control attestation, exchange fails in stressed markets, outsourced middle-office handoff breaks, partial cancel/correct chains, custody fee dispute resolution, market-claim ageing dashboards, same-day trade amendment approvals and lifecycle lineage |

## Core design principle

A mature wealth platform should not treat transactions as isolated rows. It should maintain a linked lifecycle:

```text
Proposal -> Order -> Execution -> Trade -> Allocation -> Confirmation -> Settlement -> Transaction -> Position/Cash/Ledger -> Reporting
```

The key platform requirement is lineage. A user, support analyst, auditor or portfolio manager should be able to start from a client position and trace back to the order, execution, settlement, custody booking, cash movement, corporate action, valuation and reporting impact.

## Audience

This pack is written for:

- wealth-platform architects
- portfolio analytics teams
- client-reporting teams
- product owners
- business analysts
- DPM and advisory technology teams
- operations and production-support teams
- QA teams
- engineering teams building order, transaction, position, reconciliation and reporting services

## How to use this pack

Read in this order:

1. `01` for the big picture
2. `02` and `03` for order/trade/settlement mechanics
3. `04` and `05` for post-settlement operations and controls
4. `06` for the canonical platform data model
5. `07` for product-specific implementation treatment
6. `08` for advisory, mandate, analytics and reporting usage
7. `09` for build-quality, controls and test scenarios
8. `10` for glossary, practitioner reference and source notes
9. `11` for practical implementation examples

## Important caveat

Market rules, settlement cycles, tax treatment, regulatory obligations and custody models vary by jurisdiction, booking centre, product, client type and custodian. The knowledge here should be used as professional platform guidance, not as legal, tax or regulatory advice.
