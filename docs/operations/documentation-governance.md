# Documentation Governance

This repo should stay easy to extend as more product docs, prompt docs, and reference packs are added.

## Intake Workflow

1. Identify the source folder or file.
2. Decide whether the material is product, prompt, reference, or operations content.
3. Create or update the destination folder.
4. Preserve the source document content unless cleanup is required for readability or broken links.
5. Add a local README with purpose, audience, reading order, and provenance.
6. Update the nearest index file.
7. Run a quick link and tree review before committing.

## Classification

| Category | Destination | Examples |
|---|---|---|
| Product knowledge | `docs/products/<product-slug>/` | Structured notes, private markets, funds, mandates, reports. |
| Prompt material | `docs/prompts/<prompt-pack-slug>/` | Research prompts, coding prompts, documentation prompts. |
| Shared references | `docs/reference/` | External links, glossary, source lists, citation packs. |
| Repo workflow | `docs/operations/` | Naming rules, intake process, review checklist. |
| Templates | `templates/` | Product pack template, prompt pack template, decision note template. |

## Quality Rules

1. Every document set needs a `README.md`.
2. Every imported set needs provenance: source path, import date, and purpose.
3. Important reading order should be explicit.
4. Investment, legal, tax, or regulatory content must carry a clear disclaimer.
5. Do not keep generated shortcuts, temporary files, local editor files, or unreviewed scratch notes in git.
6. Keep filenames stable after import unless there is a strong reason to rename them.
