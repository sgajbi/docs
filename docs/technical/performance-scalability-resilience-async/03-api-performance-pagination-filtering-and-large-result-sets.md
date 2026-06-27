# API Performance, Pagination, Filtering, and Large Result Sets

## Purpose

This file explains how to design APIs that perform well with large collections such as transactions, holdings, reports, events, audit records, and work items.

Unbounded APIs are one of the most common causes of slow UIs and backend incidents.

---

## Core Rule

Never return unbounded lists for data that can grow.

Every collection API should define:

- default page size
- maximum page size
- stable sort order
- pagination model
- filter model
- response metadata
- authorization behavior
- index strategy
- known limits

---

## Pagination Patterns

| Pattern | Use | Trade-Off |
|---|---|---|
| Offset/limit | Small stable datasets | Slow and unstable for large offsets. |
| Cursor | Large or changing datasets | More complex but stable. |
| Keyset | Ordered large datasets | Efficient but requires deterministic sort keys. |
| Time-window | Financial events/transactions | Natural but date semantics must be clear. |

---

## Transaction API Design

For transactions:

- require portfolio/account scope
- require or strongly encourage date range
- use keyset or cursor for large datasets
- use stable ordering such as business date plus id
- define maximum page size
- return summary separately if needed
- provide detail endpoint for heavy records
- index filter and sort columns
- test realistic volume

---

## Response Size

Large responses cause:

- slow network transfer
- memory pressure
- serialization cost
- browser slowdown
- gateway timeout
- logging risk

Use:

- pagination
- projection/field selection where governed
- summary/detail split
- compression where appropriate
- streaming for exports where appropriate

---

## Filtering

Filters should be:

- validated
- indexed
- authorized
- bounded
- documented
- compatible with sort and pagination

Avoid arbitrary filter dictionaries without contract.

---

## API Performance Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| `GET /transactions` returns all records | Slow UI and backend overload. |
| Offset pagination on huge table | Poor performance. |
| Sort by non-unique field only | Duplicate/missing records across pages. |
| Filter not indexed | Full scan. |
| API returns heavy detail for list view | Wasteful. |
| UI downloads all pages to filter locally | Backend and browser load. |
| Response includes unused fields | Serialization and network cost. |

---

## Review Checklist

- Is list bounded?
- Is pagination defined?
- Is max page size enforced?
- Is stable sort defined?
- Are filters indexed?
- Is date range required where needed?
- Is authorization applied server-side?
- Is response size controlled?
- Are summary/detail routes separated?
- Is large-volume testing present?

---

## Summary

API performance starts with contract design.

Pagination, filtering, sorting, and response shape must be designed before data volume grows.
