# RAG Design: Chunking, Embeddings, Metadata, and Vector Stores

## Purpose

This file explains core design considerations for Retrieval-Augmented Generation.

RAG combines retrieval with generation. The retrieval layer must be designed with governance, quality, and security in mind.

---

## RAG Flow

```text
source documents
  -> classification and approval
  -> parsing
  -> chunking
  -> metadata enrichment
  -> embedding
  -> index/vector store
  -> query retrieval
  -> reranking/filtering
  -> prompt construction
  -> model response
  -> source citation and safety checks
```

---

## Chunking

Good chunks should:

- preserve meaning
- be small enough for retrieval
- include headings/context
- avoid splitting important tables incorrectly
- include source reference
- include classification
- include lifecycle status
- include owner/review metadata

Chunking strategies:

- heading-based
- paragraph-based
- semantic
- sliding window
- document-structure aware
- table-aware

---

## Metadata

Useful metadata:

```text
document_id
title
section
owner
classification
source_path
version
last_reviewed
status
system/domain
tags
effective_date
expiry_date
access_policy
```

Metadata enables filtering and governance.

---

## Embeddings

Embedding design should consider:

- model choice
- language support
- update frequency
- dimension size
- storage cost
- retrieval quality
- privacy/data policy
- embedding of restricted data
- reindexing strategy

---

## Vector Store

Vector store concerns:

- access control
- tenant separation
- metadata filtering
- encryption
- deletion
- reindexing
- backup/restore
- query latency
- cost
- monitoring

---

## Retrieval Quality

Improve retrieval with:

- metadata filters
- hybrid search
- reranking
- query rewriting
- document freshness boosting
- source priority
- feedback loop
- evaluation dataset

---

## RAG Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Chunk without metadata | No governance or filtering. |
| Vector store ignores access control | Data leakage. |
| Stale docs indexed as current | Wrong answers. |
| Huge chunks | Poor retrieval precision. |
| Tiny chunks without context | Incoherent answers. |
| No reindex strategy | Old data persists. |
| Retrieval quality not evaluated | Poor user trust. |

---

## Review Checklist

- Is chunking strategy defined?
- Is metadata complete?
- Is classification included?
- Is access policy enforced?
- Is source freshness tracked?
- Is vector store secured?
- Is deletion/reindexing defined?
- Is retrieval evaluated?
- Are citations preserved?
- Are stale/archived docs handled?

---

## Summary

RAG architecture is data-product engineering for knowledge.

Good answers require good source governance, metadata, retrieval, and evaluation.
