# Client, Account, Portfolio, Household, Booking Centre and Relationship Master Data Reference Pack

## Purpose

This pack documents the client and account master-data layer behind a private-banking / wealth-management platform. It explains how clients, beneficial owners, accounts, portfolios, households, relationship managers, mandates, booking centres, service models, tax profiles, suitability profiles, reporting hierarchies and access-control structures should be understood and modelled.

The goal is to support future study, office knowledge sharing, portfolio analytics, advisory workflows, DPM/mandate management, client reporting, source ownership, migration, reconciliation and platform design.

## Curation Note

Curated from provided source material on 2026-06-27 and normalized for reusable practitioner, product, platform and implementation reference.

## Why this topic matters

Every product pack depends on the client/account hierarchy. A bond, note, fund, equity, loan, derivative or insurance policy does not exist in a reporting platform in isolation. It is held by an account or portfolio, owned or controlled by a legal client entity, serviced by a relationship team, booked in a specific jurisdiction, governed by product eligibility rules, constrained by suitability and mandate rules, and reported through a client or household consolidation structure.

If this layer is weak, downstream systems show wrong suitability results, wrong consolidated statements, wrong mandate breaches, wrong tax reporting, wrong CRM coverage, wrong access permissions and wrong analytics grouping.

## Audience

- Wealth-management architects and platform owners
- Product owners and business analysts
- Portfolio analytics and reporting teams
- Advisory and DPM technology teams
- Data governance, migration and reconciliation teams
- QA, support and operations teams
- Relationship-manager and client-experience teams

## Reading order

| Order | File | Purpose |
|---:|---|---|
| 1 | `01-client-account-portfolio-master-fundamentals-and-taxonomy.md` | Core concepts and vocabulary. |
| 2 | `02-client-cif-br-household-beneficial-owner-and-relationship-hierarchy.md` | Client identity, CIF/BR concepts, householding, beneficial ownership and relationships. |
| 3 | `03-account-portfolio-mandate-booking-centre-and-service-model-design.md` | Account, portfolio, mandate, booking centre and service-model design. |
| 4 | `04-onboarding-kyc-aml-tax-residency-suitability-and-lifecycle-events.md` | Onboarding, KYC/AML, tax residency, suitability and lifecycle events. |
| 5 | `05-data-model-entity-account-portfolio-hierarchy-and-access-control.md` | Canonical data model and access-control design. |
| 6 | `06-advisory-mandate-analytics-reporting-and-client-experience.md` | Advisory, mandate, analytics and reporting impact. |
| 7 | `07-cross-border-booking-centre-distribution-and-data-privacy-controls.md` | Cross-border, booking-centre and data privacy controls. |
| 8 | `08-platform-implementation-source-ownership-reconciliation-test-scenarios.md` | Implementation, source ownership, reconciliation and QA. |
| 9 | `09-glossary-practitioner-reference-and-review-checklists.md` | Glossary, practitioner explanations and review checklists. |
| 10 | `10-source-notes-and-further-reading.md` | Source notes and further reading. |
| 11 | `11-worked-examples-and-implementation-patterns.md` | Practical examples for client/account hierarchy, householding, portfolio membership, mandates, booking centres, access control, reporting and QA. |

## Recommended repo location

```text
docs/products/client-account-portfolio-master-data/
```

## Core design principle

Model the client/account layer as a governed hierarchy, not as a few flat fields copied into positions.

```text
Party / Client Entity
  -> Relationship / Role
  -> Household / Client Group
  -> Account / Contract
  -> Portfolio / Reporting or Investment Container
  -> Mandate / Advisory or DPM Rules
  -> Holdings, Transactions, Cash, Collateral, Analytics, Reports
```

## Disclaimer

This pack is for education, architecture, documentation, platform design, QA and internal knowledge sharing. It is not legal, tax, regulatory, investment or client advice. Jurisdiction-specific rules should be implemented through approved legal/compliance interpretation and configurable policy rules.
