# Current-State, Target-State, and Transition Architecture

## Purpose

This file explains how to document architecture for migration and replatforming.

Migration requires at least three architecture views: current, target, and transition.

---

## Current-State Architecture

Current-state documentation should show:

- existing applications
- source systems
- data stores
- interfaces
- users
- reports
- batch jobs
- runtime platform
- security model
- operational processes
- pain points and constraints

---

## Target-State Architecture

Target-state documentation should show:

- future services
- ownership boundaries
- data products
- APIs/events/files
- runtime platform
- security model
- observability model
- deployment model
- support model
- decommissioned components

---

## Transition Architecture

Transition architecture shows coexistence.

It should include:

- temporary bridges
- dual writes
- data sync
- adapters
- compatibility layers
- routing rules
- migration flags
- old/new ID mapping
- fallback paths
- cutover switch points
- temporary reports or reconciliations

---

## Architecture Views

Use separate views for:

- business capability
- application/service
- data flow
- integration
- runtime
- security
- reporting/archive
- operations
- cutover sequence

---

## Transition Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Only target architecture documented | Teams miss coexistence complexity. |
| Temporary bridge becomes permanent | Architecture debt. |
| Decommissioning not planned | Cost and support burden. |
| Routing rules unclear | Users hit wrong system. |
| Data sync not monitored | Divergence. |
| Security model differs silently | Access risk. |

---

## Review Checklist

- Is current state documented?
- Is target state documented?
- Is transition state documented?
- Are temporary components labelled?
- Are decommission targets clear?
- Are data flows shown?
- Are security boundaries shown?
- Are integration paths clear?
- Are fallback paths documented?
- Are operational impacts shown?

---

## Summary

Migration architecture is not only where we end up.

It must also explain how we get there safely.
