# Canonical Data Dictionary, API, Event and Domain Model Reference

**Source provenance:** Curated from `C:\Users\Sandeep\Downloads\canonical_data_dictionary_api_event_domain_model_reference_pack.zip\canonical_data_dictionary_api_event_domain_model_reference_pack` on 2026-06-27.

**Local treatment:** Imported as shared reference material, deduplicated from two overlapping source sequences, normalized into application-neutral terminology, and cleaned for reusable wealth-platform architecture work.

## Purpose

This pack defines a reusable reference for canonical wealth-platform domain models, data dictionaries, API contracts, event schemas, validation rules, lineage controls and implementation evidence.

Use it when designing or reviewing:

1. canonical entity models,
2. portfolio, account, instrument, transaction and position data contracts,
3. API request and response standards,
4. event schemas and lifecycle events,
5. source ownership, lineage and data-quality rules,
6. QA and contract-testing scenarios,
7. AI/RAG grounding context and audit evidence.

## Reading Order

| Step | File | Use |
|---:|---|---|
| 1 | [`01-domain-model-catalog-and-design-principles.md`](01-domain-model-catalog-and-design-principles.md) | Canonical domains, identifiers and modelling principles. |
| 2 | [`02-client-account-portfolio-relationship-data-dictionary.md`](02-client-account-portfolio-relationship-data-dictionary.md) | Client, account, portfolio and relationship fields. |
| 3 | [`03-instrument-product-market-data-and-reference-data-dictionary.md`](03-instrument-product-market-data-and-reference-data-dictionary.md) | Instrument, product, pricing and reference-data fields. |
| 4 | [`04-transaction-position-cash-lot-and-ledger-data-dictionary.md`](04-transaction-position-cash-lot-and-ledger-data-dictionary.md) | Transaction, position, cash, lot and ledger fields. |
| 5 | [`05-valuation-performance-risk-benchmark-and-reporting-data-dictionary.md`](05-valuation-performance-risk-benchmark-and-reporting-data-dictionary.md) | Valuation, performance, risk, benchmark and reporting fields. |
| 6 | [`06-advisory-suitability-mandate-rebalancing-and-workflow-data-dictionary.md`](06-advisory-suitability-mandate-rebalancing-and-workflow-data-dictionary.md) | Advisory, suitability, mandate, rebalance and workflow fields. |
| 7 | [`07-ai-rag-agent-audit-context-and-evaluation-data-dictionary.md`](07-ai-rag-agent-audit-context-and-evaluation-data-dictionary.md) | AI context, retrieval, agent action and evaluation fields. |
| 8 | [`08-canonical-api-contracts-request-response-pagination-and-errors.md`](08-canonical-api-contracts-request-response-pagination-and-errors.md) | API request, response, pagination, error and idempotency standards. |
| 9 | [`09-canonical-event-schemas-lifecycle-events-and-integration-patterns.md`](09-canonical-event-schemas-lifecycle-events-and-integration-patterns.md) | Event schemas, lifecycle events and integration patterns. |
| 10 | [`10-validation-rules-lineage-versioning-and-data-quality-controls.md`](10-validation-rules-lineage-versioning-and-data-quality-controls.md) | Validation, lineage, versioning and data-quality controls. |
| 11 | [`11-service-boundary-contract-implementation-guide.md`](11-service-boundary-contract-implementation-guide.md) | Service ownership, contract layout, schema governance and implementation guardrails. |
| 12 | [`12-qa-regression-contract-testing-and-source-notes.md`](12-qa-regression-contract-testing-and-source-notes.md) | QA scenarios, regression design, contract testing and source notes. |

## Core Principle

A wealth platform should not treat every screen, report, integration or API as a separate data model. It should use stable canonical concepts such as party, account, portfolio, instrument, transaction, position, valuation, performance, risk, mandate, workflow, report snapshot, AI context and audit evidence.

## Related Packs

| Related Material | Use |
|---|---|
| [`../wealth-platform-architecture/`](../wealth-platform-architecture/README.md) | Broader platform architecture, APIs, events, resilience and migration patterns. |
| [`../wealth-product-framework/`](../wealth-product-framework/README.md) | Cross-product position, transaction, advisory, mandate, analytics and reporting framework. |
| [`../../products/data-governance-quality-lineage-controls/`](../../products/data-governance-quality-lineage-controls/README.md) | Data governance, lineage, reconciliation and operating controls. |
| [`../../products/market-data-reference-data-pricing-instrument-master/`](../../products/market-data-reference-data-pricing-instrument-master/README.md) | Market data, reference data, pricing and instrument master details. |
| [`../../products/investment-accounting-ledger/`](../../products/investment-accounting-ledger/README.md) | Ledger, accounting, fees, tax, P&L, accruals and book-of-record patterns. |

## Maintenance Standard

When extending this pack:

1. keep field names canonical and source-owned,
2. separate legal holding from analytical exposure,
3. separate lifecycle event from accounting transaction,
4. define degraded-state behavior for missing, stale, restricted or disputed data,
5. include API, event, QA and reporting implications where the model affects implementation,
6. avoid application-specific service names unless documenting a concrete implementation elsewhere.

## Disclaimer

This is reference architecture and implementation guidance. It is not legal, tax, accounting, regulatory or investment advice.
