# Documentation Repository Agent Guide

This repository is the source of truth for Sandeep's reusable documentation library.

## Operating Rules

1. Keep root clean. Put authored documentation under `docs/` and reusable skeletons under `templates/`.
2. Place product knowledge under `docs/products/<product-slug>/`.
3. Place prompt libraries, prompt patterns, and AI operating notes under `docs/prompts/`.
4. Place external-source lists, glossaries, and citation material under `docs/reference/`.
5. Place repo maintenance and documentation workflow material under `docs/operations/`.
6. Add or update an index whenever a new document set is added.
7. Prefer stable, lowercase, hyphenated file and folder names.
8. Preserve source provenance for imported packs, including source folder, import date, and original purpose.
9. Treat imported packs as the starting point, not the final product. Add professional enrichment where it improves project usefulness, including platform modelling notes, API/UI implications, QA scenarios, source-ownership questions, stakeholder explanations, and cross-product comparisons.
10. Do not mix investment education, implementation design, and prompt engineering in one folder unless the document explicitly bridges those topics.
11. Keep disclaimers clear when material touches investments, regulation, tax, legal, or client advice.

## Product Knowledge Enrichment Rule

When adding or updating product material:

1. preserve the imported source content unless cleanup is required for professional tone, readability, broken links, or repository standards,
2. add separate enrichment maps or companion sections when broader project guidance is useful,
3. improve the material toward practical private-banking, portfolio-platform, advisory, reporting, API, UI, QA, and operating use,
4. remove narrow preparation-only framing and use practitioner, stakeholder, project, or knowledge-base language,
5. update the product coverage matrix when the product library gains a new area or an important gap is identified.

## Standard Product Pack Shape

Use this structure when adding a new product family:

```text
docs/products/<product-slug>/
  README.md
  01-overview-and-taxonomy.md
  02-lifecycle-and-transactions.md
  03-data-model-and-risk.md
  04-platform-implementation.md
```

Adjust filenames only when the source material has a better natural sequence.
