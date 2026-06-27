# AI-Assisted Documentation, RAG Readiness, and Knowledge Retrieval

## Purpose

This file explains how to write documentation that is useful for humans and safe for AI-assisted retrieval.

AI can help summarize, search, and draft documentation, but it needs well-structured, current, governed source material.

---

## RAG-Friendly Documentation

Good RAG-friendly docs are:

- clearly titled
- scoped
- chunkable
- structured with headings
- status-labelled
- owner-labelled
- current
- evidence-linked
- free of contradictions
- free of sensitive data
- written in precise language

---

## Metadata

Useful metadata:

```text
title:
owner:
status:
last reviewed:
classification:
related systems:
source of truth:
evidence links:
```

---

## Grounding

AI answers should be grounded in source docs.

Docs should distinguish:

- implemented
- planned
- proposed
- deprecated
- unsupported
- unknown

This helps AI avoid overclaiming.

---

## AI-Assisted Documentation Uses

Safe uses:

- summarize existing docs
- draft first version from source material
- generate checklists
- identify duplicates
- suggest structure
- prepare KT notes
- compare docs for drift

Risky uses:

- inventing undocumented capability
- generating client-ready claims
- summarizing restricted data for broad audience
- producing architecture decision without review
- updating docs without owner validation

---

## AI Documentation Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| AI writes docs without source grounding | Hallucinated truth. |
| Docs lack status labels | AI overclaims. |
| Contradictory docs | Wrong retrieval answers. |
| Sensitive docs indexed broadly | Data leakage. |
| AI-generated docs not reviewed | Quality risk. |
| Old docs not archived | Stale answers. |

---

## Review Checklist

- Are docs well structured?
- Is status clear?
- Is owner clear?
- Is classification clear?
- Are evidence links present?
- Are contradictions removed?
- Are stale docs archived?
- Is AI output reviewed?
- Is retrieval access controlled?
- Are sensitive details excluded?

---

## Summary

AI can make documentation more useful only if the source documentation is structured, current, governed, and safe.
