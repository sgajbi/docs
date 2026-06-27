# Canonical Domain Model, Data Dictionary, and Vocabulary

## Purpose

This file explains how canonical domain models, data dictionaries, and vocabulary governance support trusted data products.

Data products fail when fields are technically valid but semantically unclear.

---

## Canonical Model

A canonical model defines stable business concepts.

Examples in wealth platforms:

- client
- account
- portfolio
- holding
- transaction
- instrument
- price
- valuation
- cashflow
- performance return
- risk metric
- benchmark
- proposal
- report job
- document
- evidence pack
- entitlement scope

---

## Why Canonical Vocabulary Matters

Without canonical vocabulary:

- one service says `ITD`, another says `SI`
- one API uses `portfolioId`, another uses `portfolio_id`
- one field means trade date, another means booking date
- one "return" is gross, another is net
- one "exposure" is notional, another is market value
- consumers misread values

Canonical vocabulary prevents semantic drift.

---

## Data Dictionary Entry

A data dictionary field should define:

```text
field name
business meaning
type
required/optional
allowed values
unit
currency
date/time basis
source owner
lineage requirement
quality rule
privacy classification
example
consumer notes
```

---

## Example Field Definition

```text
Field: asOfDate
Meaning: Business date for which the data product is valid.
Type: date
Required: yes
Format: YYYY-MM-DD
Source owner: producer domain
Quality rule: must be present for all snapshot products
Consumer note: not the same as generatedAt timestamp
```

---

## Common Semantic Distinctions

| Distinction | Why It Matters |
|---|---|
| Trade date vs settlement date | Affects transaction reporting and cash availability. |
| Book value vs market value | Affects performance and reporting. |
| Legal holding vs analytical exposure | Affects risk, attribution, and look-through. |
| Gross return vs net return | Affects performance interpretation. |
| Business date vs generated timestamp | Affects freshness and audit. |
| Source id vs product id | Affects lineage and consumer stability. |
| Proposed vs certified | Affects consumer reliance. |

---

## Vocabulary Governance

Governance should define:

- canonical terms
- allowed enum values
- aliases
- deprecated values
- meaning
- owner
- version
- migration posture
- validation rules

---

## Aliases

Aliases may support compatibility.

Example:

```text
Legacy values: ONE_YEAR, THREE_YEAR, ITD
Canonical values: 1Y, 3Y, SI
```

Rules:

- accept legacy aliases only when documented
- normalize internally
- prefer canonical values in examples
- define removal plan
- test alias behavior

---

## Data Dictionary Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Field name only, no meaning | Misinterpretation. |
| Same concept has many names | Contract drift. |
| Enum values undocumented | Consumer confusion. |
| Date basis unclear | Incorrect reports. |
| Currency basis unclear | Incorrect aggregation. |
| Deprecated values remain forever | Complexity. |
| AI/RAG uses ungoverned fields | Unsafe generated explanations. |

---

## Review Checklist

- Is field meaning clear?
- Is type defined?
- Is unit/currency/date basis defined?
- Is source owner listed?
- Are enum values governed?
- Are aliases documented?
- Are privacy classifications defined?
- Are quality rules attached?
- Are examples synthetic?
- Does vocabulary align across APIs/events/data products?

---

## Summary

Data products need language discipline.

A schema tells consumers what fields exist. A data dictionary tells them what fields mean.
