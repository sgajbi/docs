# API Review Checklists and Governance Playbook

## Purpose

This file provides practical checklists for API design reviews, PR reviews, and governance.

Use it before implementing a new API, changing an existing API, or promoting an API as supported.

## API Design Checklist

- [ ] Capability is clear.
- [ ] API owner is identified.
- [ ] Consumers are identified.
- [ ] Route name is meaningful.
- [ ] HTTP method is correct.
- [ ] Request model is explicit.
- [ ] Response model is explicit.
- [ ] Error model is documented.
- [ ] Caller context is defined.
- [ ] Authorization behavior is defined.
- [ ] Idempotency is defined where needed.
- [ ] Pagination is defined where needed.
- [ ] Supportability states are defined.
- [ ] Versioning is defined.
- [ ] OpenAPI examples are synthetic and current.
- [ ] Tests are planned.

## BFF / Experience API Checklist

- [ ] Product need is clear.
- [ ] Upstream dependencies are listed.
- [ ] Upstream authority is preserved.
- [ ] BFF does not recompute domain truth.
- [ ] Caller context is forwarded.
- [ ] Partial/degraded states are mapped.
- [ ] Permission-blocked states are safe.
- [ ] Raw upstream errors are hidden.
- [ ] Response is stable for UI.
- [ ] Contract tests are present.

## Domain API Checklist

- [ ] Domain ownership is explicit.
- [ ] Route exposes domain capability.
- [ ] Methodology or business rule is documented.
- [ ] Source data assumptions are clear.
- [ ] Lineage/evidence returned where needed.
- [ ] Supportability included where needed.
- [ ] API does not include UI-only concerns.
- [ ] Tests prove domain correctness.
- [ ] OpenAPI is current.

## Request/Response Checklist

- [ ] DTOs are separate from domain models.
- [ ] Persistence models are not exposed.
- [ ] Field names are canonical.
- [ ] Enums are governed.
- [ ] Required fields are justified.
- [ ] Optional fields have clear meaning.
- [ ] Unsupported combinations are rejected.
- [ ] Metadata is structured.
- [ ] Warnings are structured.
- [ ] Sensitive fields are excluded.

## Error Checklist

- [ ] Problem-details model used.
- [ ] Status codes are appropriate.
- [ ] Reason codes are bounded.
- [ ] Permission-blocked is safe.
- [ ] Stale/partial/degraded states are visible.
- [ ] Raw internal errors are hidden.
- [ ] Errors are documented in OpenAPI.
- [ ] Error tests exist.

## Pagination Checklist

- [ ] List is bounded.
- [ ] Default page size is set.
- [ ] Max page size is enforced.
- [ ] Pagination model is chosen.
- [ ] Sort order is stable.
- [ ] Cursor/keyset used for large datasets.
- [ ] Filters are validated.
- [ ] Authorization applies to filtered results.
- [ ] Index implications considered.
- [ ] Pagination tests exist.

## Idempotency Checklist

- [ ] Operation can cause side effects.
- [ ] Idempotency key required where needed.
- [ ] Key scope defined.
- [ ] Request hash stored.
- [ ] Duplicate same request behavior defined.
- [ ] Duplicate different request returns conflict.
- [ ] Retention defined.
- [ ] Tests cover duplicate/conflict behavior.

## Security Checklist

- [ ] Auth assumptions documented.
- [ ] Caller context validated.
- [ ] Authorization enforced server-side.
- [ ] Permission-blocked responses safe.
- [ ] Logs exclude sensitive data.
- [ ] Metrics labels are bounded.
- [ ] Diagnostics are protected.
- [ ] File/document routes are mediated.
- [ ] AI prompts/responses protected where relevant.
- [ ] Security tests exist.

## Observability Checklist

- [ ] Operation name defined.
- [ ] Logs structured.
- [ ] Metrics defined.
- [ ] Trace propagation defined.
- [ ] Supportability measured.
- [ ] Dependency failures measured.
- [ ] SLO relevant for critical APIs.
- [ ] Dashboard reflects implemented metrics.
- [ ] Alerts have runbooks.

## Versioning Checklist

- [ ] Change compatibility assessed.
- [ ] Consumers identified.
- [ ] Version bump needed?
- [ ] Aliases documented?
- [ ] Deprecation path defined?
- [ ] Migration window defined?
- [ ] Contract tests updated?
- [ ] OpenAPI updated?
- [ ] Release notes prepared?

## API PR Evidence Template

```markdown
## API Summary

## Owner and consumers

## Route / method

## Request and response impact

## Error and supportability impact

## Idempotency / pagination / versioning

## Security and caller-context impact

## Observability impact

## Tests and validation

## Documentation updated

## Compatibility and migration notes

## Residual risks
```

## Governance Playbook

### Before Build

1. Define capability and owner.
2. Identify consumers.
3. Choose route/method.
4. Draft request/response/error model.
5. Define auth, idempotency, pagination, and supportability.
6. Review compatibility and versioning.
7. Define tests.

### During Build

1. Keep controller thin.
2. Map DTOs explicitly.
3. Add OpenAPI metadata.
4. Add contract tests.
5. Add error tests.
6. Add observability.
7. Update docs.

### Before Merge

1. Run local checks.
2. Confirm OpenAPI gate.
3. Confirm contract tests.
4. Confirm security posture.
5. Confirm docs and examples.
6. Confirm CI evidence.
7. Record gaps.

### After Merge

1. Confirm main/release gate.
2. Publish docs/wiki if needed.
3. Monitor early metrics.
4. Track consumer migration.
5. Clean up aliases when safe.

## Summary

API governance should make good design easier and bad drift harder.

A mature API review asks not only "does it work?" but "is it owned, documented, compatible, secure, observable, testable, and supportable?"
