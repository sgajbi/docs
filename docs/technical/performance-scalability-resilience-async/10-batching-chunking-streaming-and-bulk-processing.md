# Batching, Chunking, Streaming, and Bulk Processing

## Purpose

This file explains how to process large volumes of work safely.

Batch and bulk workflows are common in reporting, ingestion, analytics, data products, document generation, and archive operations.

---

## Batch Design

A batch should define:

- batch id
- item id
- source selector
- item count
- schedule or trigger
- item state
- concurrency
- partial success
- retry
- replay
- evidence
- runbook

---

## Chunking

Chunking splits large work into smaller units.

Use chunking to:

- reduce memory usage
- bound transaction size
- improve retry behavior
- parallelize safely
- improve progress visibility

Chunk size should be measured and configurable.

---

## Streaming

Streaming is useful when:

- data is large
- consumers can process incrementally
- full materialization is expensive
- latency matters
- memory must be bounded

Streaming requires careful error handling and back-pressure.

---

## Partial Success

Batch workflows must define partial success.

Questions:

- Can some items succeed while others fail?
- How are failed items retried?
- What does batch status show?
- How is consumer notified?
- What evidence is retained?
- When is batch considered complete?

---

## Bulk Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| One giant transaction | Locking and rollback risk. |
| Load all records into memory | OOM failure. |
| No item-level status | Hard to recover partial failures. |
| No batch id | Traceability weak. |
| No retry/replay | Manual repair. |
| No progress metrics | Operators blind. |
| Batch runs during peak traffic | User impact. |

---

## Review Checklist

- Is batching needed?
- Is chunk size defined?
- Is item state tracked?
- Is partial success defined?
- Are retries bounded?
- Is replay supported?
- Are progress metrics available?
- Is memory bounded?
- Is schedule safe?
- Is evidence retained?

---

## Summary

Bulk work must be broken into observable, retryable, and recoverable units.

A batch is a workflow, not a loop.
