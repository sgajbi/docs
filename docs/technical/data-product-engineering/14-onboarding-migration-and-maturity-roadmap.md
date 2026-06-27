# Onboarding, Migration, and Maturity Roadmap

## Purpose

This file explains how to onboard new data products and mature existing tables, feeds, files, APIs, and reports into governed data products.

Data mesh maturity is incremental. The goal is to move from implicit data sharing to explicit, validated, operated products.

---

## Onboarding Steps

### Step 1: Identify Candidate Product

Ask:

- What data is reused?
- Who produces it?
- Who consumes it?
- What business capability does it support?
- What problems exist today?

### Step 2: Define Ownership

Document:

- producer domain
- support owner
- data steward
- technical owner
- consumers
- platform dependencies

### Step 3: Define Contract

Create:

- producer declaration
- schema
- data dictionary
- semantic vocabulary
- source systems
- lineage and freshness fields
- quality states
- access/SLO/evidence policies

### Step 4: Implement Serving Surface

Choose:

- API
- event
- batch
- snapshot
- catalog metadata
- combination of surfaces

### Step 5: Add Validation

Add:

- schema tests
- quality tests
- contract tests
- access tests
- lineage tests
- telemetry tests

### Step 6: Publish To Catalog

Publish product metadata with honest lifecycle state.

### Step 7: Certify

Run certification gate and capture evidence.

### Step 8: Operate

Add:

- metrics
- dashboards
- alerts
- runbook
- operating report
- certification history

---

## Migration From Legacy Feed

Legacy feed migration path:

```text
legacy feed
  -> field profiling
  -> source ownership mapping
  -> data dictionary
  -> producer contract
  -> consumer declaration
  -> quality checks
  -> governed serving surface
  -> catalog publication
  -> certification
  -> deprecate raw feed
```

---

## Maturity Roadmap

| Stage | Description |
|---|---|
| Stage 0 | Data exists but ownership unclear. |
| Stage 1 | Producer owner and schema documented. |
| Stage 2 | Data dictionary, vocabulary, and quality rules added. |
| Stage 3 | Producer/consumer contracts validated. |
| Stage 4 | Lineage, freshness, access, SLO, and evidence policies added. |
| Stage 5 | Runtime telemetry and certification gate implemented. |
| Stage 6 | Gateway/catalog/UI discovery and consumer proof added. |
| Stage 7 | Operating reports, dashboards, alerts, and maturity review active. |

---

## Onboarding Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Start with catalog entry only | Visibility without trust. |
| Ignore current consumers | Breaking changes. |
| Skip data dictionary | Semantic confusion. |
| Treat legacy feed name as product name | Technical artifact becomes business product. |
| Certify before runtime evidence | False maturity. |
| No deprecation path | Old and new contracts coexist forever. |
| No runbook | Production support weak. |

---

## Review Checklist

- Is product candidate valuable?
- Is producer owner clear?
- Are consumers known?
- Is schema documented?
- Is vocabulary canonical?
- Are lineage/freshness fields defined?
- Are quality checks implemented?
- Are access/SLO/evidence policies attached?
- Is catalog state honest?
- Is certification evidence generated?
- Is operating model defined?
- Is deprecation path planned?

---

## Summary

Data-product onboarding is a maturity journey.

Start with ownership and meaning. Add contracts, quality, access, evidence, telemetry, certification, and operations over time.
