# Security, Entitlements, Sensitive Data, and Customer Evidence

## Purpose

This file explains security and privacy considerations for data products.

Data products often contain sensitive financial, client, account, portfolio, advisory, reporting, and operational information. Governance must protect data at every stage.

---

## Data Classification

Classify data by sensitivity.

Examples:

| Class | Examples |
|---|---|
| Public | Public product descriptions or generic metadata. |
| Internal | Internal architecture or non-sensitive operational metadata. |
| Restricted | Source paths, internal evidence, operational details. |
| Confidential | Client, account, portfolio, transaction, advisory, or report data. |
| Secret | Credentials, keys, tokens, passwords. |

---

## Entitlement-Aware Data Products

Access policy should consider:

- user role
- service identity
- consumer system
- tenant
- region
- booking center
- portfolio scope
- client scope
- field-level sensitivity
- purpose limitation

---

## Customer Evidence Versus Internal Evidence

Internal evidence may include:

- source paths
- logs
- detailed telemetry
- service names
- validation artifacts
- dependency details

Customer or buyer-facing evidence should filter:

- internal paths
- sensitive telemetry
- restricted identifiers
- entitlement details
- raw source data
- raw logs
- raw prompts/responses
- infrastructure details not approved for sharing

---

## AI/RAG Data Product Safety

AI consumers require additional controls:

- use approved data products
- enforce entitlement scope
- preserve source references
- avoid raw sensitive prompts
- separate generated text from authoritative data
- record model/evidence lineage
- block unsupported client-ready publication
- test prompt/context safety
- avoid training or external sharing unless approved

---

## Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Data catalog exposes restricted metadata | Information leakage. |
| Trust evidence includes internal file paths in public view | Security risk. |
| Consumer accesses product outside declared purpose | Governance breach. |
| AI index ignores entitlements | Data leakage. |
| Metrics labels include client or portfolio ids | Privacy and cardinality risk. |
| Static sample contains real data | Compliance issue. |
| Access policy documented but not enforced | False control. |

---

## Review Checklist

- Is data classified?
- Are entitlements defined?
- Is access policy enforceable?
- Are field restrictions defined?
- Is customer evidence filtered?
- Are internal evidence paths protected?
- Are AI consumers governed?
- Are logs and telemetry safe?
- Are metrics label-safe?
- Are tests present for restricted access?

---

## Summary

Data-product trust includes security trust.

A data product is not mature if it is discoverable and certified but leaks sensitive data through metadata, telemetry, evidence, logs, or AI context.
