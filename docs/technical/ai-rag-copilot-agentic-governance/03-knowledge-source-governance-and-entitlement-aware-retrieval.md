# Knowledge Source Governance and Entitlement-Aware Retrieval

## Purpose

This file explains how to govern knowledge sources used by AI and RAG systems.

RAG quality and safety depend on source quality, access control, freshness, and retrieval boundaries.

---

## Approved Sources

Approved sources should be:

- owned
- classified
- current
- reviewed
- indexed intentionally
- access-controlled
- versioned where needed
- safe for intended use
- linked to source of truth

Examples:

- source-controlled architecture docs
- approved runbooks
- API contracts
- data-product contracts
- policy documents
- knowledge-base packs
- product capability docs
- ticket/incident summaries where approved

---

## Source Classification

| Classification | AI Use |
|---|---|
| Public | Broad use allowed. |
| Internal | Internal assistant use. |
| Restricted | Controlled retrieval only. |
| Confidential | Strict entitlement and purpose controls. |
| Secret | Never indexed or retrieved. |

---

## Entitlement-Aware Retrieval

Retrieval should enforce:

- user identity
- role
- tenant/region
- document classification
- data-product access
- client/account/portfolio scope
- purpose limitation
- tool context
- session scope

AI should not retrieve what the user cannot access directly.

---

## Source Freshness

RAG should consider:

- last reviewed date
- document lifecycle status
- source version
- deprecation state
- stale/archived flag
- replacement document
- evidence recency

Outdated sources should be downranked, marked, or excluded.

---

## Source Governance Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Index everything by default | Data leakage and poor quality. |
| No document classification | Access risk. |
| Archived docs retrieved as current | Wrong answers. |
| AI accesses docs user cannot see | Entitlement breach. |
| Source ownership missing | Trust weak. |
| Secrets accidentally indexed | Critical incident. |
| No source freshness | Stale guidance. |

---

## Review Checklist

- Are sources approved?
- Are sources classified?
- Are owners defined?
- Is access enforced?
- Is freshness tracked?
- Are archived docs excluded or marked?
- Are secrets excluded?
- Is retrieval scoped by caller?
- Is source lineage captured?
- Are retrieval tests present?

---

## Summary

RAG is only as safe as its source governance.

Approved, entitled, current sources are the foundation of trustworthy AI answers.
