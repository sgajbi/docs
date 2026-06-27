# Reporting Capability Model and Architecture

## Purpose

This file explains the end-to-end architecture of enterprise reporting capabilities.

Reporting is a cross-domain capability. It consumes portfolio, performance, risk, advisory, market, and client data, then produces controlled documents and evidence.

---

## Reporting Capability Model

```text
report request
  -> authorization and scope validation
  -> report job creation
  -> data snapshot selection
  -> section data collection
  -> report content model assembly
  -> template and layout selection
  -> rendering
  -> validation
  -> archive storage
  -> retrieval/download
  -> audit and evidence
```

---

## Core Components

| Component | Responsibility |
|---|---|
| Report API | Accepts report requests, exposes status/result, handles idempotency. |
| Report orchestration | Coordinates sections, snapshots, rendering, archive, and lifecycle. |
| Domain services | Own source data and analytics outputs. |
| Report content model | Canonical structured representation of report sections. |
| Template engine | Defines layout, styling, sections, and rendering rules. |
| Renderer | Produces PDF, HTML, DOCX, or other output. |
| Archive service | Stores document and metadata with retention and access controls. |
| Evidence store | Stores lineage, hashes, warnings, and generation metadata. |
| BFF/UI | Provides product workflow: preview, status, download, retry. |

---

## Service Boundary Rules

Reporting service may own:

- report job lifecycle
- report request contract
- report content assembly
- report-specific snapshot references
- report evidence
- rendering handoff
- archive handoff
- report status and supportability

Reporting service should not own:

- source portfolio data
- official performance methodology
- official risk methodology
- client master data
- instrument master data
- document archive truth unless archive is part of same capability
- entitlement policy truth

---

## Reporting Architecture Patterns

### Pattern 1: Orchestrated Report Generation

```text
report service
  -> portfolio service
  -> performance service
  -> risk service
  -> advisory service
  -> renderer
  -> archive
```

Use when report requires multiple domains.

### Pattern 2: Snapshot-Based Report

```text
snapshot selection
  -> content model generation
  -> render from immutable report package
```

Use when reproducibility matters.

### Pattern 3: Async Report Job

```text
request
  -> 202 accepted
  -> worker generates report
  -> status polling
  -> archive/download
```

Use for expensive or long-running generation.

---

## Reporting Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| UI generates official report locally | Weak evidence and control. |
| Renderer owns domain calculations | Methodology drift. |
| Report pulls raw databases directly | Source ownership bypass. |
| Report output not archived | Evidence gap. |
| Report generated from live changing data | Not reproducible. |
| Report job state only in memory | Lost progress. |
| No report ownership boundary | Support confusion. |

---

## Review Checklist

- What report type is being generated?
- Which service owns report lifecycle?
- Which domain services own the data?
- Is generation synchronous or async?
- Is snapshot needed?
- Is archive required?
- Is evidence captured?
- Is report access controlled?
- Are unsupported states explicit?
- Are runbooks available?

---

## Summary

Reporting architecture is evidence architecture.

A report should be built from authoritative inputs, generated through controlled workflow, and retained with explainable evidence.
