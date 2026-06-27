# Backend Service Design Checklists

## Purpose

This file provides concise checklists for backend service design, review, and readiness.

Use these checklists during design reviews, PR reviews, refactoring, onboarding, and production-readiness checks.

## Service Mission Checklist

- [ ] Service mission is clear.
- [ ] Service type is identified.
- [ ] Owned capability is stated.
- [ ] Authoritative data is listed.
- [ ] Authoritative rules are listed.
- [ ] Public contracts are listed.
- [ ] Upstream dependencies are listed.
- [ ] Downstream consumers are listed.
- [ ] Explicit non-ownership is documented.
- [ ] Current support status is honest.

## Boundary Checklist

- [ ] UI owns only presentation and interaction.
- [ ] BFF owns only product composition and response shaping.
- [ ] Domain service owns domain rules and state.
- [ ] Shared capability service does not absorb consuming-domain logic.
- [ ] Platform governance does not duplicate app-local truth.
- [ ] Worker processes are tied to owned durable state.
- [ ] Cross-service dependencies are explicit.
- [ ] Unsupported states are documented.

## Clean Architecture Checklist

- [ ] Controllers are thin.
- [ ] Use cases are explicit.
- [ ] Domain is framework-free.
- [ ] Ports exist for external dependencies.
- [ ] Infrastructure implements ports.
- [ ] DTOs are not domain models.
- [ ] ORM/persistence models are not API responses.
- [ ] Configuration is centralized and typed.
- [ ] Domain behavior is testable without infrastructure.

## Domain Checklist

- [ ] Entities have identity and lifecycle.
- [ ] Value objects validate important concepts.
- [ ] Policies are named and testable.
- [ ] Calculations are deterministic.
- [ ] Lifecycle transitions are explicit.
- [ ] Domain errors are meaningful.
- [ ] Reason codes are bounded.
- [ ] Domain events are meaningful.
- [ ] Golden examples exist for critical logic.

## Application Layer Checklist

- [ ] Commands and queries are explicit.
- [ ] Use cases are named around business operations.
- [ ] Use cases call ports rather than infrastructure.
- [ ] Idempotency is handled for write workflows.
- [ ] Transaction boundaries are clear.
- [ ] External calls are not hidden inside long DB transactions.
- [ ] Results are structured.
- [ ] Application errors are mapped safely.

## API Checklist

- [ ] Request and response DTOs are explicit.
- [ ] OpenAPI summary and description are current.
- [ ] Error responses are documented.
- [ ] Caller context is typed.
- [ ] Idempotency header is required where needed.
- [ ] Pagination is defined for lists.
- [ ] Aliases are governed.
- [ ] Unsupported states are documented.
- [ ] API contract tests exist.

## Persistence Checklist

- [ ] State ownership is clear.
- [ ] Schema is versioned.
- [ ] Migrations are tested.
- [ ] Transaction boundaries are explicit.
- [ ] Idempotency is enforced with constraints where needed.
- [ ] Concurrency behavior is safe.
- [ ] Repository methods are domain-oriented.
- [ ] Retention rules are defined.
- [ ] Recovery posture is documented.

## Eventing And Async Checklist

- [ ] Async execution is justified.
- [ ] Execution state is durable.
- [ ] Status transitions are explicit.
- [ ] Event schemas are versioned.
- [ ] Outbox is used where state/event consistency matters.
- [ ] Retries are bounded.
- [ ] Dead-letter behavior is defined.
- [ ] Replay is authorized and audited.
- [ ] Metrics and logs exist.
- [ ] Retention cleanup exists.

## Error And Supportability Checklist

- [ ] Error categories are defined.
- [ ] Supportability states are explicit.
- [ ] Reason codes are bounded.
- [ ] Warnings are structured.
- [ ] Problem details are safe.
- [ ] Permission-blocked is first-class.
- [ ] Stale and partial states are visible.
- [ ] Logs do not leak raw payloads.
- [ ] Degraded states are tested.

## Security Checklist

- [ ] Secrets are not in source/docs/tests/artifacts.
- [ ] Configuration is redacted in diagnostics.
- [ ] Auth assumptions are documented.
- [ ] Authorization is enforced server-side.
- [ ] Caller context is propagated safely.
- [ ] Write actions are audited.
- [ ] Permission-blocked responses are safe.
- [ ] Logs, metrics, traces, and events are source-safe.
- [ ] Dependencies and containers are scanned.
- [ ] AI boundaries are governed where relevant.

## Observability Checklist

- [ ] Operation vocabulary is stable.
- [ ] Logs are structured.
- [ ] Metrics have bounded labels.
- [ ] Trace propagation is handled.
- [ ] Health endpoint exists.
- [ ] Readiness endpoint is meaningful.
- [ ] Supportability is visible.
- [ ] Dashboards map to implemented metrics.
- [ ] Alerts have runbooks.
- [ ] Diagnostics are safe.

## Testing Checklist

- [ ] Domain tests cover rules and calculations.
- [ ] Use-case tests use fake ports.
- [ ] API tests cover validation and errors.
- [ ] Contract tests protect schemas and vocabulary.
- [ ] Integration tests cover real adapters.
- [ ] Worker tests cover retry/replay.
- [ ] Security tests cover permission and sensitive-data boundaries.
- [ ] Observability tests protect metric/log safety.
- [ ] Runtime smoke tests prove container startup.
- [ ] Test data is synthetic.

## PR Readiness Checklist

- [ ] Scope is clear.
- [ ] Commits are meaningful.
- [ ] Tests are relevant.
- [ ] CI gates are healthy or failures explained.
- [ ] Docs updated.
- [ ] Runbooks updated if operational behavior changed.
- [ ] API/event/data-product contracts updated if changed.
- [ ] Security impact described.
- [ ] Observability impact described.
- [ ] Rollback or mitigation plan described.
- [ ] Follow-up backlog listed.

## Production Readiness Checklist

- [ ] Runtime profile documented.
- [ ] Docker image builds.
- [ ] Service starts in supported profile.
- [ ] Health and readiness meaningful.
- [ ] Dependencies documented.
- [ ] Configuration and secrets safe.
- [ ] Metrics/logs/traces available.
- [ ] Dashboards and alerts exist.
- [ ] Runbooks exist.
- [ ] Migration plan tested.
- [ ] Rollback plan exists.
- [ ] Support owner clear.
- [ ] Release evidence available.
- [ ] Known limits documented.

## Summary

Checklists do not replace engineering judgment. They make engineering judgment repeatable.

Use them to make reviews more objective and to improve backend maturity one slice at a time.
