# 17 - Implementation Evidence Reading Map

This guide helps readers connect neutral technical concepts to implementation evidence without copying product-specific names into the learning material. Use it when learning a new repository, preparing KT, validating whether a concept is grounded in code or improving future technical guides.

The goal is to build source-backed judgement while keeping reusable documentation neutral.

## Evidence Categories

| Neutral concept | Evidence to inspect |
|---|---|
| Platform governance repository | Platform README, standards, central engineering context, validation automation, CI lane docs and scaffolding. |
| Domain-authoritative service | README, repository engineering context, architecture docs, API docs, contracts, owned capability list and tests. |
| Experience API | Gateway routes, upstream contract maps, composition services, caller-context handling, partial-state mapping and integration tests. |
| Product UI | Product architecture docs, API proxy code, capability-gated navigation, browser tests, accessibility checks and UI evidence. |
| Shared capability service | Service contracts, runbooks, evidence policy, retention rules, supportability docs and operational tests. |
| Clean backend structure | API layer, application layer, domain layer, ports, adapters, infrastructure, observability, security, migrations and contracts. |
| Multi-lane CI | Repository-native commands, workflow files, quality scorecards, merge gates, report-only checks and release evidence. |
| Data products and mesh | Producer declarations, consumer declarations, source manifests, data-product catalog, trust telemetry, access policy and certification gates. |
| Observability and SRE | Health endpoints, readiness endpoints, metrics, structured logs, trace propagation, diagnostics APIs, dashboards, alerts and runbooks. |
| Security controls | Configuration validation, secret handling, caller context, authorization tests, sensitive-evidence guards, dependency audits and workflow permissions. |
| Testing strategy | Unit, integration, contract, browser, migration, runtime-smoke, OpenAPI and certification tests. |
| Runtime and infrastructure | Dockerfiles, compose files, orchestration manifests, ingress docs, environment contracts, health checks and deployment smoke evidence. |
| Git and release hygiene | PR templates, branch hygiene notes, review evidence, release notes, decision records and documentation updates. |
| Async and durable workflows | Execution records, polling endpoints, worker code, result storage, retry policy, dead-letter handling, replay controls and retention jobs. |

## Repository Type Map

| Repository type | Neutral role |
|---|---|
| Platform repository | Owns standards, validation, scaffolding, context, CI policy, runtime conventions and cross-repository evidence. |
| Domain data service | Owns source business data, read plane, control plane, data products, lineage and source contracts. |
| Analytics service | Owns calculations, methodology, async execution, lineage and supportability of analytics output. |
| Experience API | Owns product-facing composition, caller-context forwarding and partial/degraded-state mapping. |
| Product UI | Owns user experience, rendering, browser validation and gateway-backed product proof. |
| Opportunity or intelligence service | Owns candidate lifecycle, evidence, review queues, scoring, feedback and conversion intent. |
| Reporting service | Owns report jobs, snapshots, review payloads, batch materialization, rendering handoff and report evidence. |
| Render or archive service | Owns document rendering, archive metadata, controlled retrieval, retention and evidence. |
| AI capability service | Owns workflow execution, model-risk evidence, prompt and response boundary controls, supportability and feedback. |

## Files Worth Inspecting

When reading a repository, inspect the smallest useful set first:

```text
README.md
AGENTS.md
REPOSITORY-ENGINEERING-CONTEXT.md
Makefile
pyproject.toml
package.json
Dockerfile
docker-compose.yml
.github/workflows/*
docs/architecture/*
docs/standards/*
docs/operations/*
docs/runbooks/*
docs/guides/*
quality/*
contracts/*
wiki/*
tests/*
```

Not every repository will have every path. Missing expected files can be a useful signal, but it should be interpreted against the repository's role.

## Pattern Evidence Summary

### Platform Governance

Look for shared automation, context system, standards, validators, scaffolds, platform contracts, CI lane mapping, developer onboarding, ingress/runtime support and cross-application validation.

Neutral lesson:

```text
Platform governance should be executable. Standards without validators, scaffolds or runbooks drift quickly.
```

### Domain Authority

Look for explicit owned capabilities, explicit not-owned capabilities, read plane, control plane, source-data contracts, lineage, validation gates and supportability metadata.

Neutral lesson:

```text
A domain service must clearly state what truth it owns and what truth remains upstream or downstream.
```

### Experience API

Look for gateway route families, upstream service maps, caller-context forwarding, partial-readiness handling, product-safe errors, no recomputation of domain truth and UI-facing contract tests.

Neutral lesson:

```text
An experience API should make products easier to build without stealing domain ownership.
```

### Product UI

Look for gateway-backed routes, app shell, API proxy, capability-gated navigation, browser validation, seeded product proof and disabled, partial, degraded or permission-blocked states.

Neutral lesson:

```text
A product UI should render truthfully. It should not create business truth to make a screen look complete.
```

### Backend Structure

Look for API layer, application layer, domain layer, ports, infrastructure, observability, security, migrations, contracts, docs and wiki source.

Neutral lesson:

```text
Strong backend structure keeps business rules testable and infrastructure replaceable.
```

### CI/CD And Quality

Look for feature lanes, merge gates, main releasability gates, Docker parity, OpenAPI gates, security audits, migration gates, observability gates, repository hygiene and quality scorecards.

Neutral lesson:

```text
CI should produce useful evidence at the right level of cost and confidence.
```

### Observability

Look for health endpoints, readiness endpoints, metrics, structured logs, trace or correlation handling, supportability states, diagnostics APIs, runbooks, dashboard and alert references and sensitive-data rejection tests.

Neutral lesson:

```text
Observability must be useful and safe. Unsafe observability is a security problem.
```

### Data Mesh

Look for producer declarations, consumer declarations, source manifests, data-product catalog, trust telemetry, SLO policies, access policies, evidence packs and certification gates.

Neutral lesson:

```text
Data mesh is an ownership and evidence model, not just a catalog.
```

## Maintenance Guidance

When future repositories introduce useful implementation patterns:

1. decide whether the pattern is reusable,
2. translate it into neutral terminology,
3. add or update the relevant technical guide,
4. update this evidence map with the source area,
5. avoid copying product-specific marketing language,
6. distinguish implemented, planned, proposed and unsupported states,
7. keep examples synthetic and source-safe.

## Operating Principle

The learning documents should remain neutral, but the concepts should stay grounded in real implementation patterns.
