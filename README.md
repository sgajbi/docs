# Docs

Personal documentation library for product knowledge, platform design notes, prompt material, and reference packs.

This repo is organized so new document sets can be added without turning the root into a loose file dump.

## Repository Map

| Area | Purpose |
|---|---|
| [`docs/products/`](docs/products/README.md) | Product and domain knowledge packs. |
| [`docs/technical/`](docs/technical/README.md) | Technical knowledge base for enterprise architecture, backend structure, API governance, data products, CI/CD, observability, SRE, security, testing, resilience and engineering practices. |
| [`docs/prompts/`](docs/prompts/README.md) | Prompt libraries, AI workflows, reusable instructions, and evaluation notes. |
| [`docs/reference/`](docs/reference/README.md) | External references, glossaries, citations, and source lists. |
| [`docs/operations/`](docs/operations/documentation-governance.md) | How to add, classify, and maintain docs in this repo. |
| [`templates/`](templates/README.md) | Reusable templates for future document packs. |

## Reusable Knowledge Maps

| Map | Purpose |
|---|---|
| [`docs/products/cash-deposits-money-market-and-fx.md`](docs/products/cash-deposits-money-market-and-fx.md) | Product and platform-design map for cash, deposits, money market instruments, money market funds, FX, liquidity, buying power, settlement, and reporting. |
| [`docs/products/client-reporting-and-portfolio-review-guide.md`](docs/products/client-reporting-and-portfolio-review-guide.md) | Cross-product guide for client statements, portfolio reviews, report labels, exception panels, client explanations, and report QA. |
| [`docs/products/advisory-mandate-reporting-decision-guide.md`](docs/products/advisory-mandate-reporting-decision-guide.md) | Cross-product decision guide for advisory explanation, mandate controls, reporting requirements, QA evidence, and common review failures. |
| [`docs/products/commodities-precious-metals-and-real-assets.md`](docs/products/commodities-precious-metals-and-real-assets.md) | Product and platform-design map for commodities, precious metals, real-asset-linked wrappers, unit controls, exposure, collateral, and reporting. |
| [`docs/products/cross-product-worked-examples-calculations-case-studies/`](docs/products/cross-product-worked-examples-calculations-case-studies/README.md) | Deeper cross-product case-study library for calculations, transaction impacts, reporting outputs, QA scenarios, golden cases, AI workflow tests, and implementation acceptance criteria. |
| [`docs/products/cross-product-worked-examples.md`](docs/products/cross-product-worked-examples.md) | Worked examples across cash, bonds, commodities, equities, funds, insurance, lending, private markets, real estate, derivatives, structured products, and rebalance funding constraints. |
| [`docs/products/derivatives-and-overlay-products.md`](docs/products/derivatives-and-overlay-products.md) | Product and platform-design map for derivatives, hedging overlays, exposure, Greeks, margin, collateral, lifecycle events, and mandate controls. |
| [`docs/products/direct-equities-and-market-operations.md`](docs/products/direct-equities-and-market-operations.md) | Product and platform-design map for direct equities, trading, settlement, corporate actions, P&L, risk, and market operations. |
| [`docs/products/fixed-income-and-structured-products.md`](docs/products/fixed-income-and-structured-products.md) | Product and platform-design comparison for bonds and structured notes. |
| [`docs/products/insurance-and-annuities.md`](docs/products/insurance-and-annuities.md) | Product and platform-design map for policy contracts, protection benefits, ILPs, annuities, cash values, surrender, claims, and reporting. |
| [`docs/products/implementation-backed-capability-map.md`](docs/products/implementation-backed-capability-map.md) | Neutral product-capability map that separates implementation-backed support, source-backed product extensions, support-limited states, and future candidates. |
| [`docs/products/product-implementation-evidence-review-guide.md`](docs/products/product-implementation-evidence-review-guide.md) | Guide for translating app docs, API contracts, validation evidence, UI proof, report payloads, and runbooks into neutral product-capability documentation. |
| [`docs/products/lending-collateral-and-buying-power.md`](docs/products/lending-collateral-and-buying-power.md) | Product and platform-design map for credit lines, Lombard lending, margin lending, collateral, pledges, LTV, buying power, and margin calls. |
| [`docs/products/private-markets-and-alternatives.md`](docs/products/private-markets-and-alternatives.md) | Product and platform-design map for private equity, private credit, real assets, hedge funds, commitments, cashflows, NAV lag, liquidity, controls, and reporting. |
| [`docs/products/pooled-investment-products.md`](docs/products/pooled-investment-products.md) | Product and platform-design map for funds, ETFs, fund of funds, hedge funds, and private market funds. |
| [`docs/products/portfolio-construction-and-rebalancing-guide.md`](docs/products/portfolio-construction-and-rebalancing-guide.md) | Cross-product portfolio construction and rebalancing guide for target allocation, drift, funding, liquidity, tax/cost impact, mandates, execution readiness, reporting, and QA. |
| [`docs/products/portfolio-exposure-modelling-guide.md`](docs/products/portfolio-exposure-modelling-guide.md) | Cross-product portfolio exposure guide for legal holdings, market value, look-through, notional, sensitivity, issuer, counterparty, currency, liquidity, collateral, commitment, mandate, and reporting exposure. |
| [`docs/products/product-capability-boundary-matrix.md`](docs/products/product-capability-boundary-matrix.md) | Cross-product support-boundary matrix for generic baseline capabilities, source-backed extensions, support-limited states, future candidates, reporting claims, and QA evidence. |
| [`docs/products/product-calculation-example-catalog.md`](docs/products/product-calculation-example-catalog.md) | Cross-product calculation catalog with formulas, reporting labels, degraded-state behavior, and QA assertions across covered product families. |
| [`docs/products/product-documentation-review-rubric.md`](docs/products/product-documentation-review-rubric.md) | Professional product-documentation review rubric for improving clarity, depth, examples, analytics, advisory, DPM, reporting, implementation boundaries, supported capabilities, and future candidates. |
| [`docs/products/product-library-polish-dashboard.md`](docs/products/product-library-polish-dashboard.md) | Compact dashboard for product-family polish posture, strongest coverage, and next enrichment slices. |
| [`docs/products/product-library-role-guide.md`](docs/products/product-library-role-guide.md) | Role-based navigation for business users, BAs, developers, architects, QA, operations, advisory, DPM, reporting, analytics, and product development. |
| [`docs/products/product-lifecycle-cashflow-and-event-guide.md`](docs/products/product-lifecycle-cashflow-and-event-guide.md) | Cross-product lifecycle, cashflow, event, transaction, accrual, entitlement, obligation, reporting, implementation, and QA guide. |
| [`docs/products/product-payoff-scenario-and-stress-guide.md`](docs/products/product-payoff-scenario-and-stress-guide.md) | Cross-product payoff, scenario, stress, downside, income reliability, liquidity, advisory, reporting, and QA guide. |
| [`docs/products/product-performance-attribution-risk-guide.md`](docs/products/product-performance-attribution-risk-guide.md) | Cross-product guide for return components, contribution, attribution, risk denominators, degraded analytics states, and analytics QA. |
| [`docs/products/product-worked-example-index.md`](docs/products/product-worked-example-index.md) | Central navigation index for worked examples by product family and by implementation workflow. |
| [`docs/products/practical-product-implementation-review.md`](docs/products/practical-product-implementation-review.md) | Cross-product review layer for examples, cashflows, valuation, analytics, advisory, mandate, reporting, QA, and implementation boundaries. |
| [`docs/products/product-qa-regression-matrix.md`](docs/products/product-qa-regression-matrix.md) | Cross-product QA and regression matrix for normal, degraded, reporting, reconciliation, migration, and release-gate evidence. |
| [`docs/products/product-source-ownership-deep-dive.md`](docs/products/product-source-ownership-deep-dive.md) | Product-family source-ownership deep dive covering issuer credit events, corporate actions, fund liquidity, derivatives margin, structured-product observations, private-market notices, insurance values, collateral pledges, and wealth-structuring authority. |
| [`docs/products/product-source-intake-and-migration-guide.md`](docs/products/product-source-intake-and-migration-guide.md) | Cross-product source-intake and migration guide for field profiling, source ownership, reconciliation, degraded states, and sign-off evidence. |
| [`docs/products/product-taxonomy-and-vocabulary-guide.md`](docs/products/product-taxonomy-and-vocabulary-guide.md) | Cross-product taxonomy and vocabulary guide for product family, subtype, wrapper, legal holding, exposure, lifecycle, valuation, liquidity, reporting, and supportability states. |
| [`docs/products/real-estate-reits-and-infrastructure.md`](docs/products/real-estate-reits-and-infrastructure.md) | Product and platform-design map for listed REITs, property securities, private real estate, infrastructure, income, valuation, liquidity, and reporting. |
| [`docs/products/source-ownership-calculation-reporting-matrix.md`](docs/products/source-ownership-calculation-reporting-matrix.md) | Cross-product source-ownership, calculation dependency, reporting, QA, and implementation-boundary matrix. |
| [`docs/products/tax-regulatory-and-reporting.md`](docs/products/tax-regulatory-and-reporting.md) | Tax-aware and regulatory-reporting map for tax classification, withholding, tax lots, cross-border documentation, report outputs, controls, and QA. |
| [`docs/products/wealth-structuring-trusts-and-family-office.md`](docs/products/wealth-structuring-trusts-and-family-office.md) | Wealth-structuring map for trusts, estates, family offices, holding vehicles, beneficial ownership, authority, access control, reporting groups, and QA. |
| [`docs/technical/`](docs/technical/README.md) | Practical technical learning path for architecture boundaries, backend structure, API governance, data products, CI/CD, observability, SRE, security, testing, performance, resilience and engineering review. |
| [`docs/reference/private-banking-platform-knowledge-map.md`](docs/reference/private-banking-platform-knowledge-map.md) | Reusable checklist for applying product knowledge to APIs, UI, docs, QA, and source-ownership decisions. |
| [`docs/reference/private-banking-operating-model/`](docs/reference/private-banking-operating-model/README.md) | Migrated and consolidated private-banking operating-model wiki material for performance, advisory, DPM, suitability, execution, custody, lending, controls, and QA. |
| [`docs/reference/ai-decision-intelligence-wealth-platform/`](docs/reference/ai-decision-intelligence-wealth-platform/README.md) | AI, RAG, advisor-copilot, decision-intelligence, governance, security, evaluation, and agentic-workflow reference for wealth platforms. |
| [`docs/reference/canonical-data-dictionary-api-event-domain-model/`](docs/reference/canonical-data-dictionary-api-event-domain-model/README.md) | Canonical domain model, data dictionary, API contract, event schema, validation, lineage, QA and service-boundary reference. |
| [`docs/reference/commercial-saas-pricing-enterprise-sales/`](docs/reference/commercial-saas-pricing-enterprise-sales/README.md) | Commercial SaaS pricing, enterprise sales, contracts, support, customer success, revenue operations and value-realization reference. |
| [`docs/reference/enterprise-cloud-kubernetes-platform-engineering/`](docs/reference/enterprise-cloud-kubernetes-platform-engineering/README.md) | Cloud, Kubernetes, runtime, platform-engineering, observability, GitOps, resilience, cost-control and regulated-workload reference. |
| [`docs/reference/enterprise-delivery-sdlc-devsecops-operations/`](docs/reference/enterprise-delivery-sdlc-devsecops-operations/README.md) | Enterprise delivery, SDLC, DevSecOps, quality engineering, release, production-readiness, SRE, incident, migration and operating-governance reference. |
| [`docs/reference/enterprise-platform-reference-map.md`](docs/reference/enterprise-platform-reference-map.md) | Enterprise platform map for routing architecture, product framework, runtime, delivery, security, AI and operating-model questions. |
| [`docs/reference/enterprise-security-cyber-resilience-controls/`](docs/reference/enterprise-security-cyber-resilience-controls/README.md) | Enterprise security, cyber resilience, IAM, entitlements, data protection, DevSecOps, detection, incident response, AI security, audit evidence and control-library reference. |
| [`docs/reference/knowledge-base-master-index-roadmap-playbook/`](docs/reference/knowledge-base-master-index-roadmap-playbook/README.md) | Master index, roadmap, learning sequence, office sharing, repository governance, quality rubric and navigation standards for the knowledge base. |
| [`docs/reference/vendor-management-third-party-risk/`](docs/reference/vendor-management-third-party-risk/README.md) | Vendor management, outsourcing, third-party risk, procurement, due diligence, contracts, exit planning and bank-buyable software controls reference. |
| [`docs/reference/wealth-ai-product-strategy/`](docs/reference/wealth-ai-product-strategy/README.md) | Wealth AI strategy, product packaging, MVP prioritization, roadmap, operating model, governance, and business-case reference. |
| [`docs/reference/wealth-business-strategy-operating-model-transformation/`](docs/reference/wealth-business-strategy-operating-model-transformation/README.md) | Wealth business strategy, target operating model, transformation roadmap, governance, KPIs, value realization and executive communication reference. |
| [`docs/reference/wealth-platform-architecture/`](docs/reference/wealth-platform-architecture/README.md) | Wealth-platform architecture reference for domain boundaries, APIs, events, source ownership, analytics workflows, resilience, migration, and architecture governance. |
| [`docs/reference/wealth-product-framework/`](docs/reference/wealth-product-framework/README.md) | Cross-product canonical position, transaction, advisory, mandate, analytics, reporting, and knowledge-base quality framework. |
| [`docs/products/product-knowledge-coverage-matrix.md`](docs/products/product-knowledge-coverage-matrix.md) | Coverage and enrichment roadmap for the product library. |

## Adding New Material

1. Decide the category first: product, prompt, reference, or operations.
2. Create a folder with a stable lowercase slug, for example `docs/products/private-markets/`.
3. Add a local `README.md` that explains purpose, audience, reading order, and a non-personal curation note when applicable.
4. Add enrichment where useful: modelling distinctions, source ownership, API/UI implications, QA scenarios, controls, and stakeholder explanations.
5. Add or update the local worked-example file when the material introduces new lifecycle, cashflow, valuation, reporting, suitability, or implementation behavior.
6. Update the closest index file, the product worked-example index, the product coverage matrix, and this root README.
7. Keep draft, temporary, and personal scratch files out of versioned content.

## Naming Rules

Use lowercase hyphenated names for new folders and files:

```text
docs/products/structured-notes/
docs/prompts/research-workflows/
docs/reference/market-data-sources.md
```

Keep numbered files only when the reading order matters.

## Disclaimer

Some documents may discuss investment products, platform design, suitability, risk, regulation, tax, or operations. They are for education, product analysis, architecture, and documentation work only. They are not investment, legal, tax, regulatory, or client advice.
