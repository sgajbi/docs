# 04 - Data Mesh And Data Product Engineering

A data mesh is an operating model for trustworthy domain-owned data products. It is not just a catalog, dashboard, data lake or replication pattern.

For deeper data-product and data mesh engineering guidance, use [`data-product-engineering/`](data-product-engineering/README.md).

## Core Principle

```text
Domain owns product truth.
Platform governs discovery, policy, evidence and certification.
Consumers read through approved contracts and trust posture.
```

## Data Product Definition

A data product is a named, versioned, owned data contract with:

1. stable product id,
2. domain owner,
3. schema,
4. source ownership,
5. lineage,
6. freshness policy,
7. completeness policy,
8. quality policy,
9. access policy,
10. evidence policy,
11. runtime telemetry,
12. consumer contract.

Example:

```text
Product: PortfolioHoldingsAsOf:v1
Owner: portfolio-service
Consumer: gateway-service, risk-service, reporting-service
Trust posture: current, complete, reconciled
Access: advisor and operations roles scoped to entitled portfolios
```

## Certification Is More Than Catalog Inclusion

Catalog-visible means "known to the platform." Certified means "proved by source declarations, runtime telemetry, policies, tests and evidence."

| State | Meaning |
|---|---|
| Planned | Product is future work. Do not present as available. |
| Declared | Source-owned declaration exists, but runtime proof is incomplete. |
| Catalog-visible | Product appears in generated discovery artifacts. |
| Certified | Product has source declaration, runtime trust telemetry, SLO/access/evidence policy and passing validation. |
| Degraded | Product exists but current telemetry shows freshness, quality, lineage or source-readiness issues. |

## Trust Telemetry

Trust telemetry answers whether a product is usable now.

Minimum signals:

1. freshness,
2. completeness,
3. quality,
4. reconciliation status,
5. lineage coverage,
6. source availability,
7. last successful run,
8. open exceptions,
9. supportability state.

## Access Policy

Every data product needs an access policy:

1. allowed roles,
2. allowed use cases,
3. tenant or booking-centre scope,
4. portfolio/account/client scope,
5. denial behavior,
6. audit event,
7. evidence filtering rules.

Avoid exposing data-product metadata that reveals restricted source paths, raw identifiers, client names or operational internals.

## Evidence Policy

Evidence packs are useful for audits, demos, QA and operations, but they must be filtered.

Customer-authorized evidence should not include:

1. raw source payloads,
2. local machine paths,
3. internal secret names,
4. unrestricted telemetry dumps,
5. client identifiers outside the permitted scope,
6. trace ids or correlation ids as public labels,
7. internal vulnerability or incident details.

## Producer Workflow

For a new data product:

1. define product boundary and owner,
2. create source-owned declaration,
3. define schema and version,
4. define source lineage and source-of-record,
5. add runtime telemetry,
6. define access and evidence policies,
7. publish through read-only discovery,
8. add API and contract tests,
9. add certification gate,
10. update docs and runbooks.

## Consumer Workflow

Consumers should:

1. consume through approved APIs or generated contracts,
2. preserve source lineage,
3. preserve supportability state,
4. fail closed when source posture is unavailable for high-risk workflows,
5. avoid local reconstruction of source-owned methodology,
6. record consumer-specific use of the product.

## Data Product Anti-Patterns

Avoid:

1. treating a CSV or table as a certified product,
2. hand-editing generated catalog files,
3. letting gateway or UI become product authority,
4. hiding stale or partial data,
5. inventing local lifecycle states,
6. certifying products without telemetry,
7. exposing raw source evidence to broad audiences.

## Engineering Review Questions

1. Is the product id stable and versioned?
2. Is ownership clear?
3. Is runtime trust measurable?
4. Is access policy explicit?
5. Can consumers see degraded state?
6. Are generated artifacts derived from source truth?
7. Does certification run in CI or release validation?
