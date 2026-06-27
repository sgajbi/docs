# Prompt Evaluation Checklist

Use this checklist before handing a prompt to another agent.

## Good Prompt Criteria

1. It names the target repository or repositories.
2. It names the exact RFC, feature, issue, endpoint, or workflow when applicable.
3. It tells the agent which Lotus context, skill, standard, or playbook to use.
4. It defines what is in scope and out of scope.
5. It preserves source ownership boundaries.
6. It asks for meaningful tests and evidence, not just code changes.
7. It includes documentation, wiki, context, supported-features, and skill-update expectations where truth can change.
8. It defines validation commands or validation depth.
9. It defines branch, PR, CI, and merge expectations.
10. It has a clear definition of done.

## Warning Signs

1. It says "make it better" without naming scope or evidence.
2. It mixes planning and implementation when approval is required first.
3. It asks for bank-buyable or production-ready claims without requiring proof.
4. It duplicates several large nudges instead of using one canonical prompt plus one short nudge.
5. It omits downstream consumers when API contracts may change.
6. It asks for screenshots before validation passes.
7. It asks an agent to update deployed local skills directly instead of platform-owned skill source and sync automation.
8. It asks for all apps, all RFCs, and all cleanup in one unbounded task without slice control.

## Reuse Guidance

Use:

1. `Phase 0 Ramp-Up` at the start of new repository work.
2. `Gold-Standard RFC Planning` when the RFC must be approved before coding.
3. `RFC Implementation Loop` after RFC approval.
4. `Backend Bank-Buyable Refactor` for backend cleanup or hardening.
5. `Workbench UI Refactor And Audit` for UI work.
6. `Endpoint Certification` for API-by-API validation.
7. `Demo And Live Runtime Proof` for evidence and screenshots.
8. `PR Loop And Branch Hygiene` before merge and closure.
