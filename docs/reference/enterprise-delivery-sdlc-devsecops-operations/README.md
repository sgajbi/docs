# Enterprise Delivery, SDLC, DevSecOps, Release Management and Production Operations Reference Pack

## Purpose

This reference pack explains how to build, test, release, operate and continuously improve **bank-grade wealth management software**.

It connects the product/domain knowledge base to actual execution:

- product discovery and requirements
- agile delivery and architecture governance
- secure software development lifecycle
- CI/CD and release engineering
- test strategy and quality engineering
- production readiness and operational support
- incident, problem and change management
- observability and SRE practices
- audit, compliance and evidence
- migration, cutover and country rollout
- AI-assisted engineering and controlled automation
- operating metrics and continuous improvement

The pack is designed for future study, office knowledge sharing, enterprise architecture discussions, platform-delivery planning and reusable project work.

## Curation Note

Curated from provided source material on 2026-06-27 and normalized for reusable practitioner, product, platform and implementation reference.

## Suggested repository location

```text
docs/reference/enterprise-delivery-sdlc-devsecops-operations/
```

## Reading order

| File | Focus |
|---|---|
| `01-enterprise-delivery-operating-model-and-taxonomy.md` | Delivery operating model, roles, governance layers and end-to-end taxonomy. |
| `02-sdlc-agile-product-engineering-and-requirements-governance.md` | Product discovery, requirements, Agile, ADRs, acceptance criteria and backlog governance. |
| `03-devsecops-ci-cd-secure-supply-chain-and-release-engineering.md` | DevSecOps, CI/CD, release engineering, secure SDLC, SBOM, provenance and supply chain. |
| `04-quality-engineering-test-strategy-non-functional-and-automation.md` | Test strategy, automation, non-functional testing, performance, resilience and data-quality testing. |
| `05-production-readiness-sre-observability-incident-and-problem-management.md` | Production readiness, SRE, observability, incident/problem management and support model. |
| `06-change-release-deployment-and-environment-management.md` | Change governance, release calendars, deployment patterns, rollback, environments and configuration. |
| `07-ai-assisted-engineering-copilots-agents-and-governed-automation.md` | AI-assisted engineering, AI coding assistants and agentic engineering tools, guardrails, prompt patterns, reviews and evidence. |
| `08-wealth-platform-controls-compliance-audit-and-risk-operations.md` | Bank-grade controls, audit evidence, model risk touchpoints, data protection and operational risk. |
| `09-data-migration-cutover-regression-and-country-rollout-delivery.md` | Migration, parallel run, cutover, reconciliation, country rollout and release sign-off. |
| `10-metrics-dora-operating-rhythm-roadmap-and-continuous-improvement.md` | Metrics, DORA, health reviews, operating rhythm, roadmap and improvement loops. |
| `11-glossary-practitioner-reference-and-source-notes.md` | Glossary, practitioner questions and answers, practitioner checklist and sources. |
| `12-worked-examples-and-implementation-patterns.md` | Practical delivery examples for traceability, CI/CD, release evidence, production readiness, incidents, migration, AI-assisted engineering and operating metrics. |

## How this pack fits the wider knowledge base

This pack is not a product pack like bonds, funds or equities. It is the **execution layer** for all packs.

```text
Product knowledge
  -> platform architecture
  -> data and accounting model
  -> advisory/reporting workflows
  -> delivery, SDLC, release and operations
```

For a wealth platform, good delivery cannot be separated from domain correctness. A technically successful release can still fail if:

- positions are booked incorrectly
- performance is restated without lineage
- reports are generated from stale prices
- suitability controls are bypassed
- cross-border access rules are wrong
- corporate actions are missed
- production support cannot explain a break
- AI-generated changes are not reviewed or tested

## Core principle

> Bank-grade software delivery is not just about shipping code. It is about shipping controlled, explainable, secure, observable and supportable business capability.


> **Disclaimer**
>
> This material is for education, architecture, product management, engineering, delivery and operating-model design. It is not legal, regulatory, audit, tax, security-certification, investment, client or compliance advice. Adapt the patterns to the bank, jurisdiction, risk appetite, control framework and technology stack in use.
