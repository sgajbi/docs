# Enterprise Platform Reference Map

This map connects the enterprise reference packs used for regulated wealth-platform design, delivery, operations, security, resilience and AI-enabled workflows.

Use it when a project question cuts across architecture, runtime platform, SDLC, cyber controls, AI governance and product-domain behavior.

## Reference Layers

| Layer | Primary reference | Use |
|---|---|---|
| Domain and platform architecture | [`wealth-platform-architecture/`](wealth-platform-architecture/README.md) | Define domain boundaries, APIs, events, source ownership, analytics workflows, migration and architecture governance. |
| Product and analytical model | [`wealth-product-framework/`](wealth-product-framework/README.md) | Align position, transaction, advisory, mandate, analytics and reporting vocabulary across product families. |
| Runtime and cloud platform | [`enterprise-cloud-kubernetes-platform-engineering/`](enterprise-cloud-kubernetes-platform-engineering/README.md) | Design Kubernetes, networking, storage, identity, observability, resilience, cost and regulated workload patterns. |
| Delivery and operations | [`enterprise-delivery-sdlc-devsecops-operations/`](enterprise-delivery-sdlc-devsecops-operations/README.md) | Govern requirements, SDLC, CI/CD, quality engineering, releases, production readiness, incidents and operating metrics. |
| Security and cyber resilience | [`enterprise-security-cyber-resilience-controls/`](enterprise-security-cyber-resilience-controls/README.md) | Apply IAM, entitlements, data protection, API security, DevSecOps, detection, incident response, AI security and audit controls. |
| AI and decision intelligence | [`ai-decision-intelligence-wealth-platform/`](ai-decision-intelligence-wealth-platform/README.md) | Design advisor copilots, RAG, agent workflows, model governance, evaluations, human oversight and AI operations. |
| AI product strategy | [`wealth-ai-product-strategy/`](wealth-ai-product-strategy/README.md) | Shape AI product packaging, roadmap, MVP selection, business case, governance and operating model. |
| Operating model | [`private-banking-operating-model/`](private-banking-operating-model/README.md) | Ground performance, advisory, DPM, suitability, execution, custody, lending and controls in reusable operating-model guidance. |

## Project Question Routing

| If the project question is... | Start with | Then cross-check |
|---|---|---|
| What owns this capability and which API or event boundary should exist? | [`wealth-platform-architecture/`](wealth-platform-architecture/README.md) | [`wealth-product-framework/`](wealth-product-framework/README.md), product pack, delivery reference. |
| How should this workload run, scale, recover and be observed? | [`enterprise-cloud-kubernetes-platform-engineering/`](enterprise-cloud-kubernetes-platform-engineering/README.md) | Security reference, delivery reference, architecture reference. |
| What evidence is needed before release? | [`enterprise-delivery-sdlc-devsecops-operations/`](enterprise-delivery-sdlc-devsecops-operations/README.md) | Security reference, product QA matrix, architecture governance. |
| How should client, account, report or AI-context access be controlled? | [`enterprise-security-cyber-resilience-controls/`](enterprise-security-cyber-resilience-controls/README.md) | Entitlements product pack, architecture reference, AI decision-intelligence reference. |
| How should a RAG, copilot or agent workflow stay grounded and controlled? | [`ai-decision-intelligence-wealth-platform/`](ai-decision-intelligence-wealth-platform/README.md) | Security reference, AI product strategy, product evidence guide. |
| How should a new product capability be documented from implementation evidence? | [`../products/product-implementation-evidence-review-guide.md`](../products/product-implementation-evidence-review-guide.md) | Product coverage matrix, source ownership matrix, architecture reference. |
| How should a production incident, data break or cyber event be handled? | [`enterprise-security-cyber-resilience-controls/`](enterprise-security-cyber-resilience-controls/README.md) | Delivery operations reference, data governance pack, cloud operations reference. |

## Security Reference Positioning

The enterprise security and cyber resilience reference is the control layer across the platform. It should be used whenever work touches:

1. identity, authentication, authorization or entitlements,
2. privileged access or break-glass operations,
3. client, account, portfolio, report, document or interaction data,
4. API, UI, workflow, batch, event, integration or data-export boundaries,
5. secrets, keys, certificates, tokens or sensitive configuration,
6. cloud, Kubernetes, container, registry or network controls,
7. CI/CD, dependencies, provenance or supply-chain security,
8. detection, logging, SIEM/SOAR, fraud or anomaly monitoring,
9. incident response, BCP, DR, ransomware or crisis handling,
10. AI prompts, retrieval context, model/tool calls, generated output or human approval.

## Cross-Reference Patterns

| Pattern | References to combine | Expected output |
|---|---|---|
| Secure portfolio API | Architecture, security, product framework, product pack. | API contract with entitlement model, input validation, source ownership, audit event and degraded-state behavior. |
| Client report generation | Product reporting pack, security, cloud, delivery. | Report job design with snapshot lineage, access control, rendering, delivery, archive, test evidence and incident path. |
| Advisor copilot | AI decision intelligence, security, AI product strategy, product evidence guide. | RAG and tool-use design with entitlement-aware retrieval, prompt controls, output review, forbidden actions and evaluation evidence. |
| Market-data ingestion | Cloud, architecture, data governance, market-data pack. | Batch/event pipeline design with source ownership, lineage, stale-price handling, reconciliation and operational alerts. |
| Rebalance workflow | Product framework, model portfolios pack, delivery, security. | Proposal-to-action flow with suitability checks, mandate restrictions, approval evidence, execution boundary and audit trail. |
| Production release | Delivery, cloud, security, architecture. | Release plan with CI evidence, threat model, deployment strategy, rollback, observability, runbook and support ownership. |
| Cyber incident | Security, delivery operations, cloud, data governance. | Containment, evidence preservation, affected-entity analysis, remediation, regression tests and communication inputs. |

## Minimum Evidence For Enterprise Work

For any durable enterprise design or delivery decision, capture:

1. business capability and owner,
2. source of record and source owner,
3. API, event, batch, UI or report boundary,
4. actor, resource, action and policy decision where access is involved,
5. data classification and retention posture,
6. resilience, observability and operational owner,
7. release gate and rollback path,
8. audit event, evidence package and exception handling,
9. known limitations and future candidates,
10. links to the product or reference pack that supports the decision.

## Maintenance Rule

Update this map when a new enterprise reference pack is added, when a reference pack changes its scope, or when a repeated project workflow needs a clearer route through architecture, delivery, cloud, security, AI and product-domain material.
