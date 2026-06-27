# Docs

Personal documentation library for product knowledge, platform design notes, prompt material, and reference packs.

This repo is organized so new document sets can be added without turning the root into a loose file dump.

## Repository Map

| Area | Purpose |
|---|---|
| [`docs/products/`](docs/products/README.md) | Product and domain knowledge packs. |
| [`docs/prompts/`](docs/prompts/README.md) | Prompt libraries, AI workflows, reusable instructions, and evaluation notes. |
| [`docs/reference/`](docs/reference/README.md) | External references, glossaries, citations, and source lists. |
| [`docs/operations/`](docs/operations/documentation-governance.md) | How to add, classify, and maintain docs in this repo. |
| [`templates/`](templates/README.md) | Reusable templates for future document packs. |

## Current Document Sets

| Document Set | Location | Status |
|---|---|---|
| Bonds reference pack | [`docs/products/bonds/`](docs/products/bonds/README.md) | Imported from `C:\Users\Sandeep\Downloads\bonds_reference_pack.zip\bonds_reference_pack` on 2026-06-27. |
| Structured notes reference pack | [`docs/products/structured-notes/`](docs/products/structured-notes/README.md) | Imported from `C:\Users\Sandeep\Downloads\notes_reference_pack` on 2026-06-27. |

## Reusable Knowledge Maps

| Map | Purpose |
|---|---|
| [`docs/products/fixed-income-and-structured-products.md`](docs/products/fixed-income-and-structured-products.md) | Product and platform-design comparison for bonds and structured notes. |
| [`docs/reference/private-banking-platform-knowledge-map.md`](docs/reference/private-banking-platform-knowledge-map.md) | Reusable checklist for applying product knowledge to APIs, UI, docs, QA, and source-ownership decisions. |

## Adding New Material

1. Decide the category first: product, prompt, reference, or operations.
2. Create a folder with a stable lowercase slug, for example `docs/products/private-markets/`.
3. Add a local `README.md` that explains purpose, audience, reading order, and source provenance.
4. Update the closest index file and this root README.
5. Keep draft, temporary, and personal scratch files out of versioned content.

## Naming Rules

Use lowercase hyphenated names for new folders and files:

```text
docs/products/structured-notes/
docs/prompts/research-workflows/
docs/reference/market-data-sources.md
```

Keep numbered files only when the reading order matters.

## Disclaimer

Some documents may discuss investment products, platform design, suitability, risk, regulation, tax, or operations. They are for education, product analysis, architecture, and documentation work only. They are not investment, legal, tax, regulatory, or client advice.
