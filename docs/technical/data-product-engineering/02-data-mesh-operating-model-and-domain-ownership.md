# Data Mesh Operating Model and Domain Ownership

## Purpose

This file explains the operating model behind data mesh.

Data mesh is not a tooling choice. It is a way of assigning data ownership, governance, quality, access, and support responsibility to the domains that understand the data.

---

## Core Data Mesh Idea

Data mesh applies product thinking to data.

The key principles are:

1. domain ownership
2. data as a product
3. self-serve platform enablement
4. federated computational governance

---

## Domain Ownership

The domain that understands and produces the data should own the data product.

Producer responsibility includes:

- business meaning
- schema
- source lineage
- quality checks
- freshness expectations
- access rules
- SLO
- evidence
- consumer impact
- lifecycle and versioning
- operational support

Platform teams enable discovery, validation, and governance, but they do not become the business owner.

---

## Producer, Platform, Consumer Roles

| Role | Responsibility |
|---|---|
| Producer domain | Owns data meaning, schema, quality, lineage, evidence, and support. |
| Platform governance | Provides standards, validators, catalog, policy contracts, telemetry collection, and certification gates. |
| Consumer domain | Declares usage, dependency, fallback behavior, and impact of unavailability. |
| Gateway / API layer | Publishes governed access and discovery without becoming data authority. |
| Product UI | Displays data-product posture and trust state without inventing certification. |
| Operations / SRE | Monitors runtime posture, SLOs, trust telemetry, and incidents. |
| Governance / risk | Reviews policy, access, evidence, privacy, and control posture. |

---

## Federated Governance

Federated governance means shared standards with distributed ownership.

Good governance is:

- consistent across domains
- machine-checkable where possible
- lightweight enough for adoption
- strict where risk matters
- versioned
- evidence-based

Examples:

- common producer contract schema
- common consumer contract schema
- canonical vocabulary
- trust metadata registry
- access policy schema
- SLO policy schema
- evidence-pack schema
- certification gate

---

## Self-Serve Platform Enablement

A platform should help producers create good data products.

Useful platform capabilities:

- scaffolding templates
- contract validators
- schema registry
- data-product catalog
- lineage/evidence registry
- trust telemetry collection
- certification gate
- maturity scorecard
- access-policy validation
- SLO policy validation
- documentation templates
- onboarding playbooks

---

## Ownership Anti-Patterns

| Anti-Pattern | Why It Fails |
|---|---|
| Central data team owns all semantics | Domain knowledge becomes separated from data production. |
| Platform catalog becomes product authority | Catalog visibility confused with truth. |
| Consumer copies and mutates data | Downstream versions diverge. |
| Reporting team becomes source of truth | Presentation layer becomes business authority. |
| AI index becomes trusted source | Retrieval cache replaces governed data. |
| Producer declares product but does not support it | Consumers depend on unsupported data. |

---

## Domain Ownership Checklist

- Is producer domain identified?
- Does producer own business meaning?
- Does producer own schema?
- Does producer own quality rules?
- Does producer own lineage?
- Does producer own freshness posture?
- Does producer own supportability?
- Does producer publish evidence?
- Does platform only govern and enable?
- Are consumers declared?
- Is access policy clear?
- Is lifecycle state honest?

---

## Data Mesh Maturity Levels

| Level | Description |
|---|---|
| L0 | Data exists but ownership and contracts are unclear. |
| L1 | Producer ownership and schema documented. |
| L2 | Producer/consumer contracts and validation exist. |
| L3 | Lineage, freshness, quality, and supportability are included. |
| L4 | Runtime telemetry and certification evidence are produced. |
| L5 | Data product is operated with SLOs, access controls, dashboards, runbooks, and maturity review. |

---

## Review Questions

- Is this data mesh or just a catalog?
- Does the producer domain own the product?
- Are consumers declared?
- Is governance executable?
- Does the platform avoid owning domain truth?
- Can certification be proven by evidence?
- Is the product observable at runtime?
- Is access policy explicit?
- Is maturity improving over time?

---

## Summary

Data mesh works only when ownership is real.

The platform can standardize and automate, but the producing domain must remain accountable for data meaning, quality, lineage, and support.
