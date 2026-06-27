# Lotus App Work Prompt Pack

**Purpose:** Reusable prompts for high-quality Lotus application work across backend, frontend, RFC, CI, documentation, PR, demo, and thought-leadership workflows.

**Audience:** Sandeep and future Codex agents working on Lotus repositories.

**Source provenance:** Consolidated from `C:\Users\Sandeep\.codex\attachments\aa4e687b-b5af-4e25-ba97-7d367cf10595\pasted-text.txt` on 2026-06-27.

**Status:** Active reusable reference.

## Reading Order

| File | Purpose |
|---|---|
| [`placeholder-guide.md`](placeholder-guide.md) | Standard placeholders to replace before reusing prompts. |
| [`quick-use-menu.md`](quick-use-menu.md) | Fast map from task type to the right prompt. |
| [`copy-paste-workflows.md`](copy-paste-workflows.md) | Clean prompts designed for direct reuse after placeholder replacement. |
| [`examples.md`](examples.md) | Filled examples showing how to replace placeholders for real Lotus tasks. |
| [`core-operating-principles.md`](core-operating-principles.md) | Stable expectations that should be included or implied in most Lotus task prompts. |
| [`prompts.md`](prompts.md) | Canonical prompt library with additional reusable patterns. |
| [`nudge-snippets.md`](nudge-snippets.md) | Short reusable reminders for continuing work without restating the full prompt. |
| [`source-inventory.md`](source-inventory.md) | Deduplication map from pasted source headings to canonical prompt entries. |
| [`evaluation.md`](evaluation.md) | Checklist for deciding whether a generated prompt is strong enough to use. |

## How To Use This Pack

1. Open [`quick-use-menu.md`](quick-use-menu.md) and choose the workflow.
2. Open [`placeholder-guide.md`](placeholder-guide.md) and fill in app, RFC, path, branch, scope, and evidence placeholders.
3. Copy the matching prompt from [`copy-paste-workflows.md`](copy-paste-workflows.md).
4. Replace every placeholder before sending it to Codex.
5. Add a short nudge from [`nudge-snippets.md`](nudge-snippets.md) only after the main task is already established.
6. When a new recurring pattern appears, update this pack instead of keeping another loose prompt in a scratch file.

## Reuse Pattern

Most prompts are designed so you only change:

```text
<APP_NAME>
<APP_PATH>
<RFC_ID>
<RFC_PATH>
<FEATURE_BRANCH>
<SCOPE>
<TARGET_APPS>
<EVIDENCE_PATH>
```

That keeps the Lotus operating bar consistent while letting you reuse the same prompt across `lotus-risk`, `lotus-performance`, `lotus-manage`, `lotus-gateway`, `lotus-workbench`, `lotus-idea`, and other apps.

## Prompt Categories

| Category | Use |
|---|---|
| Phase 0 and ramp-up | Start a new Lotus repo task with the right context and branch posture. |
| RFC planning | Ask an agent to strengthen or draft an RFC before implementation. |
| RFC implementation | Execute implementation slice by slice with proof, docs, and CI control. |
| Backend hardening | Refactor or harden a backend app to bank-buyable standards. |
| Workbench and UI | Refactor or audit Workbench surfaces and front-office UX. |
| Endpoint certification | Certify APIs deeply, including downstream integration and Swagger quality. |
| Demo and live proof | Bring up canonical apps, validate real behavior, and capture evidence. |
| PR and branch hygiene | Run the full PR loop and avoid stranded durable truth. |
| Documentation and context sync | Keep README, wiki, context, skills, and AGENTS aligned. |
| LinkedIn closure | Draft truthful post-completion thought-leadership material. |

## Important Guardrail

These prompts are operating aids. They do not replace repository `AGENTS.md`, Lotus context, repo engineering context, skills, or current implementation truth.
