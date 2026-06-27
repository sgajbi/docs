# Knowledge-Base Taxonomy and Information Architecture

## Purpose

This file explains how to organize a professional technical knowledge base.

Good information architecture makes knowledge easy to find, maintain, and reuse.

---

## Knowledge Categories

A mature knowledge base usually includes:

| Category | Purpose |
|---|---|
| Product knowledge | User journeys, capabilities, feature status, workflows. |
| Domain knowledge | Business concepts, calculations, rules, data meaning. |
| Technical architecture | Services, boundaries, integrations, runtime, APIs. |
| Engineering standards | Coding, testing, CI/CD, security, observability, delivery rules. |
| Operations | Runbooks, dashboards, alerts, incidents, support procedures. |
| Governance | ADRs, RFCs, decisions, evidence, policies, controls. |
| Learning and KT | Onboarding, study plans, role-based learning paths. |
| Reference packs | Reusable professional explanations and concept guides. |

---

## Folder Design Principles

Good folder design:

- stable names
- lowercase slugs
- one purpose per folder
- local README in each pack
- root index
- cross-links where useful
- minimal duplication
- clear ownership
- lifecycle status

---

## Suggested Structure

```text
docs/
  products/
  domains/
  architecture/
  api/
  data-products/
  operations/
  security/
  quality/
  delivery/
  reference/
  prompts/
  templates/
  decisions/
  learning/
  evidence/
```

Adjust to repository purpose.

---

## Indexing

Each major folder should have:

- README
- purpose
- audience
- reading order
- document list
- owner
- status
- maintenance notes

Indexes are essential for discoverability.

---

## Naming

Good names:

```text
portfolio-performance-methodology.md
api-contract-governance.md
report-job-runbook.md
adr-0012-async-report-generation.md
```

Poor names:

```text
newdoc.md
final_v3.md
architecture_latest.md
notes.md
```

---

## Taxonomy Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Everything in one folder | Hard to find. |
| Same topic in many places | Conflicting truth. |
| No local index | New joiners lost. |
| File names vague | Search weak. |
| Packs without reading order | Learning difficult. |
| Old docs never archived | Stale information reused. |

---

## Review Checklist

- Is folder structure clear?
- Does each folder have a README?
- Are names stable?
- Is reading order defined?
- Is duplication controlled?
- Are docs discoverable from root?
- Are owners defined?
- Is lifecycle status visible?
- Are obsolete docs archived?

---

## Summary

A knowledge base is a product.

Its users need navigation, structure, ownership, and trusted content.
