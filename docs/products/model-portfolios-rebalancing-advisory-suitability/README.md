# Model Portfolios, Rebalancing, Advisory Proposals and Suitability Workflows Reference Pack

## Purpose

This pack is part of a professional financial-products and wealth-platform knowledge base. It connects product knowledge, portfolio analytics and client advisory into the practical workflows used by wealth managers, discretionary portfolio managers, advisors, relationship managers, investment counsellors, portfolio specialists, product specialists, operations teams and technology platforms.

Earlier packs explain the instruments: notes, bonds, funds, equities, derivatives, structured products, private markets, cash/FX, lending/collateral, insurance, commodities, real estate, wealth structuring, tax reporting and portfolio analytics. This pack explains how those instruments are converted into **client action**:

- model portfolios;
- strategic and tactical asset allocation;
- advisory proposals;
- discretionary portfolio management mandates;
- portfolio drift and rebalancing;
- suitability and appropriateness checks;
- investment restrictions;
- pre-trade controls;
- proposal documents;
- client consent and approvals;
- advisor dashboards;
- order generation;
- analytics and reporting after execution.

## How to use this pack

Use this pack for:

1. wealth-management product study;
2. advisory, DPM and mandate design;
3. portfolio analytics and reporting design;
4. technology architecture and domain modelling;
5. BA/PO requirement writing;
6. QA scenario creation;
7. reusable practitioner fluency for wealth-platform, advisory, portfolio-management and investment-suitability work;
8. knowledge sharing with office teams.

## Files

| File | Focus |
|---|---|
| `01-model-portfolios-and-advisory-fundamentals.md` | Concepts, taxonomy and wealth-management context |
| `02-asset-allocation-model-portfolio-and-mandate-design.md` | Strategic/tactical allocation, model portfolios, model governance and mandates |
| `03-rebalancing-drift-trade-generation-and-optimization.md` | Drift, tolerance bands, calendar/threshold/hybrid rebalancing, trade-list generation and optimization |
| `04-advisory-proposal-client-consent-and-order-workflows.md` | Proposal lifecycle, advisor journey, client approval, order placement and post-trade tracking |
| `05-suitability-appropriateness-pre-trade-checks-and-restrictions.md` | KYC, risk profile, product suitability, mandate restrictions, concentration, liquidity, leverage and regulatory safeguards |
| `06-data-model-portfolio-targets-rules-events-and-lineage.md` | Canonical data model for model portfolios, client portfolios, proposals, checks, orders and lineage |
| `07-analytics-reporting-dashboard-and-client-experience.md` | Advisory analytics, mandate reporting, DPM dashboards, portfolio review and client reporting |
| `08-platform-implementation-controls-reconciliation-and-test-scenarios.md` | Architecture, controls, reconciliation, QA scenarios and operating model |
| `09-glossary-practitioner-reference-and-review-checklists.md` | Glossary, practitioner explanations, review prompts and implementation checklists |
| `10-source-notes-and-further-reading.md` | Source notes and further reading |

## Core design principle

A high-quality advisory and mandate platform should not treat a proposal as a static PDF or a simple order basket. It should model the entire chain:

```text
Client Profile
  -> Suitability / Eligibility
  -> Mandate / Advisory Model
  -> Current Portfolio Analytics
  -> Target Portfolio / Model Portfolio
  -> Drift and Gap Analysis
  -> Proposed Actions
  -> Pre-trade Checks
  -> Client / Manager Approval
  -> Orders
  -> Execution / Settlement
  -> Post-trade Portfolio State
  -> Ongoing Monitoring and Reporting
```

The key is full traceability: every recommendation, restriction breach, override, order and client approval should be explainable later.

## Important note

This pack is for product, platform and educational purposes. It is not legal, tax, regulatory or investment advice. Regulatory treatment should be parameterised by jurisdiction, client type, product type, service model and firm policy.
