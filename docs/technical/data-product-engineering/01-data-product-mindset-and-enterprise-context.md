# Data Product Mindset and Enterprise Context

## Purpose

This file explains what a data product is and why it matters in enterprise banking and wealth-management platforms.

The goal is to shift thinking from "data as output" to "data as a governed, reusable, trusted product".

---

## What Is A Data Product?

A data product is a source-owned, reusable, governed data capability that consumers can discover, understand, trust, access, and operate against.

It includes:

- business meaning
- schema
- ownership
- source lineage
- freshness
- quality posture
- access rules
- SLO
- evidence
- support model
- lifecycle status

A table, API, file, or event can be a delivery mechanism for a data product, but it is not the whole product.

---

## Data Product Versus Data Asset

| Data Asset | Data Product |
|---|---|
| May be a table, file, feed, or report | Governed reusable capability |
| Often technical-owner focused | Business/domain producer-owned |
| May lack consumer contract | Has explicit consumer contract |
| May lack lineage | Includes lineage and source posture |
| May lack SLO | Defines service expectations |
| May be hard to discover | Catalogued and discoverable |
| May not expose trust | Includes freshness, quality, and evidence |
| May not define access rules | Has access policy |

---

## Why Data Products Matter In Wealth Platforms

Wealth platforms reuse data across:

- portfolio views
- holdings
- transactions
- valuations
- performance
- risk
- advisory workflows
- discretionary portfolio management
- reporting
- document generation
- archive retrieval
- compliance review
- AI/RAG grounding
- operations dashboards
- migration and reconciliation

If these consumers use inconsistent or ungoverned data, the platform becomes difficult to explain and support.

---

## Common Data-Product Examples

| Data Product | Typical Producer | Typical Consumers |
|---|---|---|
| Portfolio Holdings Snapshot | Portfolio/account domain | Performance, risk, reporting, UI |
| Transaction History | Transaction/source domain | UI, reporting, performance, tax |
| Performance Returns Series | Performance analytics domain | UI, reporting, advisory, DPM |
| Risk Metrics Snapshot | Risk analytics domain | UI, advisory, reporting |
| Advisory Proposal Evidence | Advisory workflow domain | Reporting, archive, AI support |
| Report Evidence Pack | Reporting domain | Render, archive, audit, operations |
| Data-Product Trust Snapshot | Platform/data governance | Gateway, UI, operators |
| Archive Document Metadata | Archive domain | UI, reporting, operations |

---

## Data Product Consumer Questions

A consumer should be able to answer:

- What is this data product?
- Who owns it?
- What version is this?
- What fields are available?
- What do those fields mean?
- How fresh is the data?
- What quality checks passed?
- Can I access it?
- What happens if it is stale or partial?
- What SLO applies?
- What evidence proves the product is supported?
- How do I handle breaking changes?
- Who supports incidents?

---

## Data Product Is Not A Data Dump

Poor pattern:

```text
Expose table as API.
Let consumers figure out meaning.
No owner, no SLO, no lineage, no quality state.
```

Better pattern:

```text
Define product contract.
Declare producer and consumers.
Expose API/event/batch surfaces.
Include schema, semantics, freshness, lineage, access, SLO, evidence, and supportability.
Certify through tests and telemetry.
```

---

## Data Product Mindset

A data product should be:

- owned like a product
- documented like a contract
- tested like an API
- observed like a service
- secured like client data
- versioned like a public interface
- supported like a production capability
- retired through a lifecycle, not abandoned

---

## Common Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Table exposed as product without semantics | Consumers misinterpret data. |
| Dashboard treated as data product | Display becomes confused with reusable contract. |
| Gateway becomes data owner | Composition layer steals producer authority. |
| UI reads raw files directly | Governance, entitlement, and lineage bypassed. |
| Static evidence presented as live trust | False certification posture. |
| No consumer declaration | Impact of change unknown. |
| No freshness metadata | Consumers cannot judge usability. |
| No supportability states | Partial/stale data appears reliable. |

---

## Review Questions

- What business capability does this data product represent?
- Who is the producer?
- Who are the consumers?
- What does the schema mean?
- What source systems feed it?
- What lineage is available?
- What quality controls exist?
- What access policy applies?
- What SLO applies?
- What evidence proves current support?
- What lifecycle state is it in?

---

## Summary

A data product is not data made available. It is data made trustworthy, discoverable, governable, and supportable.
