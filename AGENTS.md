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
9. Do not mix investment education, implementation design, and prompt engineering in one folder unless the document explicitly bridges those topics.
10. Keep disclaimers clear when material touches investments, regulation, tax, legal, or client advice.

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
