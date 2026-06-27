# Practitioner Checklists and Review Questions

## Purpose

This file provides reusable checklists for reviewing data products, data mesh maturity, producer/consumer contracts, trust telemetry, access policy, certification, and operations.

Use these checklists during design reviews, PR reviews, governance reviews, KT sessions, and readiness assessments.

---

## Data Product Design Checklist

- [ ] Product name is clear.
- [ ] Version is defined.
- [ ] Producer is identified.
- [ ] Business purpose is clear.
- [ ] Canonical domain is identified.
- [ ] Schema is documented.
- [ ] Data dictionary exists.
- [ ] Semantic vocabulary is aligned.
- [ ] Source systems are listed.
- [ ] Lineage fields are defined.
- [ ] Freshness expectation is defined.
- [ ] Quality checks are defined.
- [ ] Access policy is linked.
- [ ] SLO policy is linked.
- [ ] Evidence policy is linked.
- [ ] Consumers are declared.
- [ ] Lifecycle state is honest.

---

## Producer Checklist

- [ ] Producer owns business meaning.
- [ ] Producer owns schema.
- [ ] Producer owns quality controls.
- [ ] Producer owns lineage and freshness.
- [ ] Producer owns supportability states.
- [ ] Producer publishes evidence.
- [ ] Producer defines compatibility policy.
- [ ] Producer defines deprecation policy.
- [ ] Producer maintains runbook.
- [ ] Producer monitors product health.

---

## Consumer Checklist

- [ ] Consumer declares product dependency.
- [ ] Consumer declares version used.
- [ ] Consumer declares fields used.
- [ ] Consumer declares purpose.
- [ ] Consumer declares freshness requirement.
- [ ] Consumer declares fallback behavior.
- [ ] Consumer declares impact if unavailable.
- [ ] Consumer declares access scope.
- [ ] Consumer tests dependency.
- [ ] Consumer handles degraded states.

---

## Lineage And Freshness Checklist

- [ ] Source owner is explicit.
- [ ] Source system is recorded.
- [ ] Business date is present where needed.
- [ ] Generated timestamp is separate from business date.
- [ ] Freshness bucket is present.
- [ ] Stale reason is present where applicable.
- [ ] Lineage reference is safe.
- [ ] Reconciliation state is available where needed.
- [ ] Evidence reference is available.
- [ ] Consumers can understand trust posture.

---

## Quality Checklist

- [ ] Schema validation exists.
- [ ] Semantic validation exists.
- [ ] Completeness checks exist.
- [ ] Freshness checks exist.
- [ ] Reconciliation checks exist where needed.
- [ ] Restricted data behavior is defined.
- [ ] Partial data behavior is defined.
- [ ] Stale data behavior is defined.
- [ ] Unknown quality state is not treated as success.
- [ ] Quality evidence is captured.

---

## Access And Security Checklist

- [ ] Data classification is defined.
- [ ] Consumer entitlement is defined.
- [ ] Field-level sensitivity is documented.
- [ ] Access policy is machine-checkable where possible.
- [ ] Restricted evidence is protected.
- [ ] Customer-facing evidence is filtered.
- [ ] Logs and metrics are safe.
- [ ] AI/RAG consumers are governed.
- [ ] No real client data appears in samples.
- [ ] Sensitive metadata is not exposed in catalog.

---

## Certification Checklist

- [ ] Producer contract validated.
- [ ] Consumer contract validated.
- [ ] Schema validated.
- [ ] Vocabulary validated.
- [ ] Access policy validated.
- [ ] SLO policy validated.
- [ ] Evidence policy validated.
- [ ] Runtime telemetry validated.
- [ ] Catalog publication validated.
- [ ] Gateway/UI consumption validated if applicable.
- [ ] Runbook/dashboard/alert references validated.
- [ ] Residual risks recorded.
- [ ] Certification timestamp and expiry defined.

---

## Operations Checklist

- [ ] Product has metrics.
- [ ] Product has dashboard.
- [ ] Product has alerts.
- [ ] Product has runbook.
- [ ] Telemetry recency monitored.
- [ ] Freshness monitored.
- [ ] Quality monitored.
- [ ] Certification expiry monitored.
- [ ] Consumer impact visible.
- [ ] Operating report generated.

---

## Review Questions

### Ownership

- Who owns this data product?
- Who supports it?
- Who approves breaking changes?
- Who owns data quality?

### Meaning

- What does each field mean?
- Are units, currency, date basis, and identifiers clear?
- Are aliases and deprecated values governed?

### Trust

- Where did the data come from?
- How fresh is it?
- What quality checks passed?
- What lineage exists?
- What evidence proves it?

### Consumption

- Who consumes it?
- What happens if it is stale?
- What happens if it is partial?
- What happens if it is unavailable?
- What happens if access is denied?

### Governance

- Is access policy defined?
- Is SLO policy defined?
- Is evidence policy defined?
- Is certification state honest?
- Is customer-facing evidence safe?

### Operations

- What dashboard shows posture?
- What alert fires if product is stale?
- What runbook should support follow?
- How is certification expiry handled?
- How is consumer impact assessed?

---

## Summary

Data-product maturity is visible in review quality.

A mature team can explain ownership, meaning, trust, access, evidence, consumers, and operations without relying on tribal knowledge.
