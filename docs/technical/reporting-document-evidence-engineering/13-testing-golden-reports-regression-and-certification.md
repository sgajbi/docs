# Testing, Golden Reports, Regression, and Certification

## Purpose

This file explains how to test reporting and document-generation systems.

Report tests must prove data correctness, layout stability, evidence integrity, archive metadata, and access control.

---

## Test Types

| Test Type | Purpose |
|---|---|
| Content model test | Validate structured report data. |
| Section test | Validate individual report sections. |
| Template test | Validate template selection and required sections. |
| Render smoke test | Prove document generation succeeds. |
| Visual regression | Detect layout changes where useful. |
| Golden report test | Compare deterministic expected report output. |
| Evidence test | Validate lineage, warnings, hash, archive metadata. |
| Access test | Validate download and report entitlement behavior. |
| Batch test | Validate scheduled/bulk report workflow. |

---

## Golden Reports

Golden reports should use:

- synthetic data
- deterministic inputs
- known template version
- known business date
- expected sections
- expected numbers
- expected warnings
- expected archive metadata
- expected hash rules where stable

---

## What To Assert

Assert:

- report job lifecycle
- content model values
- section ordering
- required disclaimers
- warnings/supportability
- generated document exists
- archive record exists
- document hash exists
- access control works
- evidence bundle complete

---

## Visual Testing

Visual tests can help detect:

- missing sections
- broken layout
- table overflow
- missing disclaimers
- chart rendering issue
- pagination issue

Do not rely only on visual tests for data correctness.

---

## Certification

Certification sweep may generate:

- sample portfolio review
- performance report
- risk report
- proposal document
- batch report
- archive retrieval proof
- evidence package

---

## Testing Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Only check PDF exists | Data correctness unproved. |
| Visual screenshot only | Numbers may be wrong. |
| Golden report uses real data | Privacy issue. |
| Template changes not reviewed | Unexpected client output. |
| Archive metadata untested | Retrieval/audit issue. |
| Warnings not tested | Degraded reports misleading. |
| Batch tested only manually | Release risk. |

---

## Review Checklist

- Are golden reports defined?
- Is data synthetic?
- Are content values asserted?
- Are templates versioned in tests?
- Are warnings tested?
- Is archive metadata tested?
- Is document hash tested?
- Is entitlement tested?
- Are batch reports tested?
- Is certification evidence generated?

---

## Summary

Report testing must prove more than rendering.

It must prove data, layout, evidence, archive, and access behavior.
