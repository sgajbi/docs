# Wealth Management Technology Domain Engineering

## Purpose

This technical guide explains the core business and technology domain concepts needed to design, build, operate, and govern enterprise-grade wealth-management technology platforms.

It is written for engineering leads, architects, backend engineers, data engineers, QA engineers, product technologists, business analysts, SREs, platform engineers, and new joiners working on portfolio, advisory, analytics, reporting, and wealth-platform systems.

The guide covers:

- wealth-management technology platform mental model
- client, account, portfolio, relationship, and booking hierarchy
- portfolio holdings, positions, transactions, cashflows, and valuation
- instrument, market data, FX, prices, and benchmark reference data
- performance analytics: TWR, MWR, contribution, attribution, benchmarks, composites
- risk analytics: volatility, drawdown, VaR, concentration, suitability, exposure
- advisory, investment proposal, suitability, mandates, and discretionary management workflows
- reporting, document generation, archive, evidence, and client communication
- data sourcing, ingestion, curation, lineage, reconciliation, and restatement
- intraday versus EOD/BOD data patterns
- API, BFF, domain-service, and data-product boundaries
- event, batch, file, and API integration patterns
- operational supportability for wealth platforms
- security, entitlements, client confidentiality, and jurisdiction controls
- migration, replatforming, cutover, and continuity patterns
- testing, certification, and golden examples for wealth domain systems
- leadership checklists and architecture review questions

The content is application-neutral. It explains reusable patterns for enterprise wealth technology without relying on any one vendor, bank, or implementation brand.

---

## Core Thesis

A wealth-management technology platform is not only a set of screens and APIs. It is a controlled ecosystem that turns source-owned client, account, portfolio, instrument, market, transaction, position, and advisory data into trusted analytics, workflows, reports, and evidence.

A mature platform must answer:

```text
Who is the client?
What accounts and portfolios are in scope?
What holdings and transactions are source-owned?
What valuations, prices, FX, and reference data were used?
What methodology produced performance and risk analytics?
What advisory or mandate rule applies?
What is visible to the user under entitlement?
What is current, stale, partial, or reconciled?
What report or document was generated?
What evidence proves the output?
```

The highest-value engineering skill in this domain is connecting business meaning, data ownership, calculation methodology, API contracts, runtime supportability, and client confidentiality.

---

## Audience

| Audience | Use |
|---|---|
| Developer | Learn domain concepts and how they map to services, APIs, data products, and tests. |
| Senior engineer | Review calculations, boundaries, lineage, APIs, and supportability more confidently. |
| Architect | Use for domain/service boundary, integration, migration, and platform design reviews. |
| Engineering lead | Use for KT, roadmap shaping, delivery risk, and cross-team technical leadership. |
| Business analyst | Use to connect portfolio/advisory/reporting language with technology design. |
| Data engineer | Use for source ownership, ingestion, curation, reconciliation, lineage, and data products. |
| QA engineer | Use for golden test scenarios, calculation evidence, migration, and certification planning. |
| SRE / support | Use for operational states, runbooks, data freshness, reconciliation, and incident diagnosis. |
| Product technologist | Use for capability framing, supported-feature status, and stakeholder communication. |

---

## Reading Order

| Step | File | Purpose |
|---:|---|---|
| 1 | `README.md` | Pack purpose, thesis, audience, and navigation. |
| 2 | `01-wealth-platform-mental-model-and-capability-map.md` | End-to-end wealth-platform capability map and technology mental model. |
| 3 | `02-client-account-portfolio-relationship-and-booking-hierarchy.md` | Client, relationship, account, portfolio, booking center, legal entity, and entitlement hierarchy. |
| 4 | `03-holdings-positions-transactions-cashflows-and-valuation.md` | Core portfolio data concepts and how they drive analytics and reporting. |
| 5 | `04-instrument-reference-market-data-price-fx-and-benchmark-data.md` | Instrument master, prices, FX, benchmarks, reference data, and governance. |
| 6 | `05-performance-analytics-twr-mwr-contribution-attribution-and-composites.md` | Performance domain concepts and engineering methodology boundaries. |
| 7 | `06-risk-analytics-exposure-concentration-var-volatility-and-suitability.md` | Risk analytics, exposure, concentration, suitability, and risk-supportability patterns. |
| 8 | `07-advisory-investment-proposals-mandates-and-dpm-workflows.md` | Advisory, proposals, mandates, model portfolios, rebalancing, and discretionary workflow concepts. |
| 9 | `08-reporting-document-generation-archive-and-client-evidence.md` | Reporting lifecycle, document rendering, archive, evidence, and client communication controls. |
| 10 | `09-data-sourcing-ingestion-curation-lineage-and-reconciliation.md` | Source systems, ingestion, curation, lineage, reconciliation, restatement, and supportability. |
| 11 | `10-intraday-eod-bod-snapshots-and-temporal-data-patterns.md` | Intraday/EOD/BOD, business date, snapshots, valuation timing, and stale data patterns. |
| 12 | `11-api-bff-domain-service-and-data-product-boundaries.md` | Service ownership, BFF composition, domain APIs, data products, and anti-patterns. |
| 13 | `12-events-files-batches-streams-and-integration-patterns.md` | Wealth-platform integration through APIs, events, files, batches, and streams. |
| 14 | `13-security-entitlements-confidentiality-and-jurisdiction-controls.md` | Entitlements, client confidentiality, data residency, jurisdiction, and sensitive-data controls. |
| 15 | `14-operational-supportability-runbooks-and-data-quality-states.md` | Supportability states, operations, runbooks, dashboards, and incident diagnosis for wealth systems. |
| 16 | `15-migration-replatforming-cutover-and-continuity-engineering.md` | Migration, replatforming, cutover, reconciliation, continuity, and parallel-run patterns. |
| 17 | `16-testing-golden-scenarios-certification-and-domain-evidence.md` | Domain test scenarios, golden calculations, regression, certification, and evidence. |
| 18 | `17-architecture-review-checklists-and-kt-playbook.md` | Architecture review, KT, and engineering leadership checklists for wealth technology. |

---

## Design Principles

1. **Separate source truth from analytics output.**
2. **Preserve client, account, portfolio, and jurisdiction boundaries.**
3. **Make performance and risk methodology explicit and testable.**
4. **Treat market data, prices, FX, and benchmark data as governed platform inputs.**
5. **Represent business date, valuation date, generated timestamp, and source timestamp separately.**
6. **Expose stale, partial, degraded, permission-blocked, and unsupported states honestly.**
7. **Do not let UI or BFF layers become domain authorities.**
8. **Reports and documents require reproducible evidence, not just rendered output.**
9. **Migration and replatforming need reconciliation, continuity, and cutover evidence.**
10. **Use synthetic golden examples for calculations, reporting, and migration tests.**
11. **Security and entitlements are domain design concerns, not only middleware.**
12. **A wealth platform should be explainable to business, engineering, support, risk, and audit.**

---

## What Good Looks Like

A mature wealth-management technology platform has:

- clear domain ownership
- canonical portfolio/account/client model
- governed instrument, price, FX, and benchmark data
- source-owned holdings and transaction data
- reproducible performance and risk analytics
- transparent advisory and mandate workflows
- data lineage and reconciliation posture
- explicit temporal model
- BFF and UI layers that preserve upstream truth
- data products with freshness, quality, access, SLO, and evidence
- supportability states in APIs and product surfaces
- entitlement-aware APIs and reports
- report generation and archive evidence
- golden regression scenarios
- migration/replatforming control evidence
- operational runbooks and dashboards
- implementation-backed documentation

---

## Disclaimer

This material is for technical education, architecture, domain learning, implementation planning, KT, and engineering leadership development.

It is not legal, regulatory, audit, cybersecurity certification, tax, investment, suitability, financial advice, or client advice.

## Curation Note

Curated into the technical knowledge base on 2026-06-27 and normalized for reusable wealth-management technology, portfolio analytics, advisory, reporting, data, API, migration, supportability and domain-engineering guidance. Local source paths and package provenance are intentionally not retained.
