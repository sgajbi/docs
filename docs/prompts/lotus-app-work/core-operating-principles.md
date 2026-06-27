# Core Operating Principles

Use these principles as the common base for Lotus implementation prompts.

## Engineering Posture

1. Build clean, readable, modular, maintainable code.
2. Prefer simple, correct designs over local hacks.
3. Preserve behavior unless the change is intentional, tested, and documented.
4. Remove dead code, duplicated logic, stale compatibility paths, and misleading documentation when encountered.
5. Use private-banking and enterprise software vocabulary accurately.
6. Keep domain ownership clear across Lotus repositories.
7. Do not implement decorative UI or documentation claims that are not backed by real backend support.

## Quality Bar

1. Add meaningful tests that prove business behavior, integration boundaries, error handling, and regression risk.
2. Avoid superficial tests that only increase coverage count.
3. Keep OpenAPI, Swagger, examples, request/response models, errors, and vocabulary implementation-backed.
4. Strengthen security, dependency hygiene, observability, correlation, tracing, auditability, idempotency, and operational diagnostics where relevant.
5. Treat performance, latency, scalability, and workload isolation as product quality concerns.

## Lotus Delivery Rules

1. Load Lotus context in the target repo `AGENTS.md` order before substantial work.
2. Use the relevant Lotus skill when the task matches a skill boundary.
3. Work on a feature branch off current `main` unless explicitly instructed otherwise.
4. Use small, meaningful commits with normal linear history when the task requires commits.
5. Use GitHub checks asynchronously when local full validation is expensive.
6. Monitor CI and fix forward promptly.
7. Treat merged to `main`, green releasability, published wiki where needed, and clean branch hygiene as done.

## Documentation And Evidence

1. Treat README, docs, wiki, supported-features, RFCs, and context as part of the implementation.
2. Keep documentation polished, audience-aware, and implementation-backed.
3. Do not claim demo readiness, bank-buyable status, data mesh certification, or production support without evidence.
4. Capture machine-readable evidence where possible.
5. Use live canonical runtime proof when the task changes user-visible product behavior.
6. Publish wiki after merge when repo-local wiki source changed.

## Downstream And Upstream Responsibility

1. Validate upstream and downstream integration, not just local behavior.
2. Do not trust Gateway blindly when backend source ownership matters.
3. If another app must change, either implement it when in scope or create a clear issue with evidence.
4. Do not clone source-owned methodology or data truth into the wrong repository.
5. Update every downstream consumer when intentional contract changes are made.
