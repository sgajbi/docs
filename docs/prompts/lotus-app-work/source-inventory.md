# Source Inventory And Deduplication Map

This file records how the pasted prompt dump was consolidated.

Source: provided prompt collection consolidated during the 2026-06-27 import.

Import date: 2026-06-27

## Canonicalization Summary

| Source heading or section | Canonical destination | Treatment |
|---|---|---|
| `ci enforcement-governance` | `core-operating-principles.md`, `prompts.md` section 10 | Kept as CI governance principle; not a standalone prompt unless changing CI gates. |
| `demo api` | `prompts.md` section 7 | Merged into live runtime proof and demo certification prompt. |
| `github issue` | `prompts.md` section 11 | Kept as reusable integration-gap issue prompt. |
| `Bank-Buyable` | `prompts.md` sections 4 and 15 | Merged into backend refactor and assessment prompts. |
| `goal based refactoring` | `prompts.md` section 4 | Generalized from `lotus-performance` to any backend repo. |
| `regular nudge`, `regualar backend nudge`, `nudge to continue` | `nudge-snippets.md` | Deduplicated into short reusable nudges. |
| `Second last nudge before RFC closure` | `nudge-snippets.md` | Kept as second-last hardening nudge. |
| `Last nudge for RFC closure`, `RFC completion` | `nudge-snippets.md`, `prompts.md` sections 3 and 8 | Merged into closure and PR loop prompts. |
| `UI nudge`, `Audit UI`, `Refactor UI`, `big Refactor` UI section | `prompts.md` section 5, `nudge-snippets.md` | Deduplicated into one Workbench UI refactor/audit prompt. |
| `RFC`, `Make gold standard RFC` | `prompts.md` section 2 | Merged into one RFC planning prompt. |
| `Code standard nudge` | `nudge-snippets.md` | Kept as short reusable code quality reminder. |
| `No squash - linear history` | `nudge-snippets.md` | Kept as linear-history reminder. |
| `Bring stack up and grab screenshots` | `prompts.md` section 7 | Kept with canonical validation-before-screenshots guardrail. |
| `RFc completion statu` | `prompts.md` section 15 | Merged into bank-buyable/product assessment prompt. |
| `brnach hiegine` | `prompts.md` section 9 | Corrected spelling and retained as durable-truth reconciliation prompt. |
| `local and remote skill sync`, `agent context and skill update` | `prompts.md` section 10 | Merged into documentation/context/wiki/skill sync prompt. |
| `agent ramp up` | `prompts.md` section 1 | Kept as Phase 0 ramp-up prompt. |
| `branches` | `prompts.md` sections 8 and 9 | Treated as branch hygiene pattern; specific `lotus-core` cleanup not kept as canonical prompt. |
| `continuation` | `prompts.md` section 13 | Kept as executive clean handoff prompt. |
| `Retrospective : - after close of RFC` | `prompts.md` section 12 | Kept as post-completion retrospective prompt. |
| `Two chat pattern`, `second window prompt` | `prompts.md` sections 12 and 13 | Preserved as reviewer/handoff concepts, not separate canonical prompts. |
| `api test and integration` | `prompts.md` section 6 | Kept as endpoint certification prompt. |
| `core nudge` | `nudge-snippets.md` | Covered by backend hardening nudge. |
| `Reminder for agent` | `nudge-snippets.md`, `prompts.md` sections 2 and 3 | Merged into RFC slice expectations. |
| `PR loop` | `prompts.md` section 8 | Kept as full PR loop prompt. |
| `collaboration between two agent` | `prompts.md` section 13 | Covered by handoff and coordination guidance; not made primary because it depends on temporary local files. |
| `continue tomorrow` | `prompts.md` section 13 | Covered by clean handoff prompt. |
| report/render/archive live proof prompt | `prompts.md` section 7 | Merged into live runtime proof prompt. |
| `refactor backend`, `big Refactor` backend section | `prompts.md` section 4 | Deduplicated into backend bank-buyable refactor prompt. |
| RFC-0086/RFC-0087 `lotus-advise` prompt | `prompts.md` sections 2, 3, and 6 | Treated as an example of RFC/domain-product implementation; not made canonical because it is repo/RFC-specific. |
| `LinkedIn` and LinkedIn workflow prompts | `prompts.md` section 14 | Kept as post-completion thought-leadership prompt. |
| `feedback on workbench` and `lotus-manage` assessment text | `prompts.md` section 15 | Converted into assessment prompt; source answer text not kept as reusable instruction. |
| Gateway + Workbench downstream realization prompt | `prompts.md` sections 1, 3, 5, 6, and 8 | Pattern preserved; specific campaign retire/supersede task is not canonical but can be recreated with the reusable sections. |
| `Goal: Implement lotus-idea RFCs...` | `prompts.md` sections 1, 2, 3, 4, 8, and 10 | Treated as a specific execution prompt composed from reusable parts. |
| `lotus-performance enterprise refactoring goal` | `prompts.md` section 16 and `copy-paste-workflows.md` Enterprise Backend Refactoring Program | Upgraded from a one-off refactor instruction into a reusable program prompt with baseline scorecard, phase gates, modularity guardrails, non-squash commit strategy, validation expectations, and documentation/context maintenance. |

## Duplicate Themes Removed

1. Repeated "bank-buyable, enterprise-grade, production-ready" language was consolidated into core principles and backend/UI prompts.
2. Repeated "meaningful tests, not superficial tests" language was kept once in core principles and relevant prompts.
3. Repeated documentation/wiki/context expectations were consolidated into the sync prompt.
4. Repeated branch/PR/CI guidance was consolidated into PR loop and durable-truth reconciliation prompts.
5. Repeated RFC second-last/final closure requirements were consolidated into nudge snippets and RFC prompts.
6. App-specific examples were kept as patterns, not copied verbatim into every canonical prompt.
