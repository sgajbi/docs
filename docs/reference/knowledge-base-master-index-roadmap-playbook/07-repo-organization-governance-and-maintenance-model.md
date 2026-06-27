# 07 - Repo Organization, Governance and Maintenance Model

## 1. Why repository governance matters

A knowledge base grows quickly. Without rules, it becomes hard to navigate.

Governance ensures:

- stable structure
- consistent naming
- no duplication
- clear ownership
- accurate references
- reusable templates
- quality improvements
- discoverability
- long-term maintainability

## 2. Recommended repo structure

```text
docs/
  products/
    structured-notes/
    bonds/
    funds/
    equities/
    derivatives/
    ...
    knowledge-base-master-index-roadmap-playbook/
  reference/
    private-banking-platform-knowledge-map.md
    wealth-product-framework/
  prompts/
    ...
  operations/
    documentation-governance.md
templates/
```

## 3. Folder naming rules

Use:

- lowercase
- hyphen-separated
- stable names
- no dates in folder names unless necessary
- no spaces
- no temporary labels

Good:

```text
docs/reference/portfolio-reporting-statement-client-experience/
```

Avoid:

```text
docs/reference/New Pack Final v3/
```

## 4. File naming rules

Use numbered files when reading order matters:

```text
01-fundamentals.md
02-lifecycle.md
03-data-model.md
```

Use non-numbered files for standalone references:

```text
glossary.md
source-notes.md
qa-matrix.md
```

## 5. README requirements

Every pack README should include:

- purpose
- audience
- reading order
- core principle
- repo path
- disclaimer
- source/provenance note

## 6. Metadata block

Consider adding at top of each file:

```yaml
---
title: "Structured Notes Lifecycle"
domain: "wealth-products"
status: "reference"
owner: "Sandeep"
last_reviewed: "2026-06-27"
tags: ["structured-notes", "lifecycle", "transactions", "valuation"]
---
```

## 7. Maintenance cadence

| Frequency | Activity |
|---|---|
| Weekly | add notes/examples from work and study |
| Monthly | review one pack for quality and duplicates |
| Quarterly | update source references and regulatory changes |
| Semi-annually | review taxonomy and roadmap |
| Annually | archive outdated material and refresh master index |

## 8. Change rules

When adding material:

1. Check whether an existing file already covers it.
2. Add to the most specific file.
3. Avoid duplicating full sections.
4. Link to related files.
5. Add examples if possible.
6. Add QA/control implications.
7. Update README if reading order changes.
8. Update coverage matrix.

## 9. Versioning

Use Git history for versioning. Avoid filenames like:

- final
- final2
- latest
- old
- new
- copy

If major rewrite is needed, update the same file and rely on Git.

## 10. Quality ownership

For each pack, define:

| Role | Responsibility |
|---|---|
| Content owner | maintains domain accuracy |
| Technical reviewer | checks platform design |
| QA reviewer | checks testability |
| Source reviewer | checks references |
| User | applies content and gives feedback |

## 11. Key takeaway

A professional knowledge repo needs the same discipline as a software repo: structure, ownership, review and continuous improvement.
