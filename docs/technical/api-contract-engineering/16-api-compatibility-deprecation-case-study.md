# 16 - API Compatibility and Deprecation Case Study

## 1. Purpose

This case study shows how to evolve an API contract when an existing field is ambiguous, overloaded or semantically wrong, while keeping current consumers safe.

Use this guide when:

- a field name no longer matches its actual meaning,
- consumers depend on legacy behavior,
- a new canonical field is needed,
- a removal or deprecation window must be governed,
- reports, UI panels, data products, AI retrieval or downstream integrations consume the API,
- the team needs evidence that the change is additive, observable and reversible.

The example is synthetic and product-neutral. It uses common wealth-platform concepts because date semantics, report cut-offs, valuation dates and booking dates are frequent sources of integration drift.

## 2. Scenario

An API exposes transaction data. The response has a field called `transaction_date`. Different consumers interpret it differently:

| Consumer | Current Interpretation |
|---|---|
| Trading UI | Trade or event date. |
| Cash projection | Settlement or value date when cash moves. |
| Operations report | Booking date when the transaction entered the book. |
| Performance engine | Economic effective date. |
| Data lake consumer | Generic transaction date with no further semantic check. |

The platform needs to clarify semantics without breaking existing consumers.

## 3. Compatibility Goal

The goal is not to rename the field immediately. The goal is to make the contract truthful and migratable.

| Goal | Treatment |
|---|---|
| Preserve old consumers | Keep `transaction_date` during a compatibility window. |
| Introduce precise semantics | Add explicit fields such as `trade_date`, `effective_date`, `value_date`, `settlement_date` and `booking_date` where supported. |
| Avoid silent behavior change | Do not change the meaning of `transaction_date` without versioning or deprecation evidence. |
| Support consumer migration | Provide examples, release notes, generated-client checks and usage telemetry. |
| Govern removal | Remove only after consumers have migrated and compatibility tests prove the window is complete. |

## 4. Contract Delta

### Current response

```json
{
  "transaction_id": "TXN-10001",
  "transaction_type": "BUY",
  "instrument_id": "BOND-ABC",
  "quantity": 100000,
  "transaction_date": "2026-06-25",
  "settlement_amount": -98500.00,
  "currency": "USD"
}
```

Problem: `transaction_date` is not precise enough for consumers to determine whether it means trade, effective, settlement, value or booking date.

### Additive enriched response

```json
{
  "transaction_id": "TXN-10001",
  "transaction_type": "BUY",
  "instrument_id": "BOND-ABC",
  "quantity": 100000,
  "transaction_date": "2026-06-25",
  "trade_date": "2026-06-25",
  "effective_date": "2026-06-25",
  "settlement_date": "2026-06-27",
  "value_date": "2026-06-27",
  "booking_date": "2026-06-26",
  "settlement_amount": -98500.00,
  "currency": "USD",
  "contract_metadata": {
    "schema_version": "1.1",
    "deprecated_fields": [
      {
        "field": "transaction_date",
        "replacement": "trade_date",
        "deprecation_reason": "transaction_date is legacy shorthand and should not be used for booking, settlement or value-date semantics",
        "earliest_removal_version": "2.0"
      }
    ]
  }
}
```

Compatibility treatment:

- `transaction_date` remains present and keeps its legacy meaning during the compatibility window.
- New fields are optional at first unless the endpoint can always populate them.
- OpenAPI examples must show date fields with different values so consumers do not assume equality.
- The deprecation note must be machine-readable where practical and human-readable in release notes.

## 5. Consumer Impact Assessment

| Consumer | Risk | Migration Action | Evidence Required |
|---|---|---|---|
| UI trade blotter | Medium | Read `trade_date` for trade/event display; use `settlement_date` for settlement column. | UI contract test and screenshot or visual regression. |
| Cash projection | High | Stop reading `transaction_date`; use `value_date` or `settlement_date` based on cash policy. | Cash projection golden case. |
| Performance engine | High | Use `effective_date` for economics and cashflow classification policy. | Performance regression case with late booking. |
| Operations report | Medium | Use `booking_date` for operational-entry reporting. | Report output test. |
| Data lake | Medium | Store all explicit dates and mark `transaction_date` deprecated. | Schema evolution test and downstream lineage note. |
| AI/RAG consumer | Medium | Update retrieval examples and field glossary to avoid ambiguous explanations. | Evaluation question covering date semantics. |

## 6. OpenAPI Requirements

OpenAPI should make the change obvious.

| Contract Area | Required Update |
|---|---|
| Field descriptions | Each date field must explain its business meaning and when it is populated. |
| Deprecated marker | Mark `transaction_date` as deprecated if the API standard supports it. |
| Examples | Include at least one example where trade, settlement and booking dates differ. |
| Nullability | State which fields are required, optional, nullable or unavailable by transaction type. |
| Error model | Do not change error semantics as part of the field migration unless versioned. |
| Changelog | Include compatibility statement, consumer action and earliest removal condition. |

Example field descriptions:

| Field | Description |
|---|---|
| `trade_date` | Date the trade, market execution or product event occurred. |
| `effective_date` | Date the economic effect applies for holdings, performance or policy treatment. |
| `settlement_date` | Date securities or cash settlement is expected or completed. |
| `value_date` | Date cash value is applied for availability, interest or FX treatment. |
| `booking_date` | Date the transaction was recorded in the book or platform. |
| `transaction_date` | Deprecated legacy shorthand retained for compatibility; use explicit date fields. |

## 7. Deprecation Plan

| Phase | Contract Behavior | Consumer Expectation | Exit Criteria |
|---|---|---|---|
| Discovery | Existing field remains; usage telemetry and consumer inventory are collected. | Consumers identified and classified. | Owner-approved inventory exists. |
| Additive release | New explicit fields are added; old field remains. | Consumers can adopt new fields without breaking old parsing. | OpenAPI, examples and contract tests pass. |
| Warning phase | `transaction_date` is marked deprecated in schema and docs. | Consumers receive release note and migration guidance. | Usage telemetry shows migration progress. |
| Compatibility freeze | Old field remains but no new consumers may depend on it. | New consumers must use explicit fields. | API review rejects new use of deprecated field. |
| Removal candidate | Removal proposed for next major version only. | Remaining consumers sign off or receive versioned alternative. | No production consumer uses old field. |
| Removal | Field removed only in versioned API or after governed compatibility window. | Consumers use explicit date fields. | Contract tests prove old and new versions behave as documented. |

## 8. Test Strategy

### Contract tests

| Test | Assertion |
|---|---|
| Old minimal response remains valid | Existing consumers can parse `transaction_date` during compatibility window. |
| New enriched response is valid | Explicit date fields conform to schema and examples. |
| Deprecated marker exists | Schema marks `transaction_date` deprecated or metadata declares deprecation. |
| Date divergence example | Example includes trade date, settlement date and booking date with different values. |
| Error semantics unchanged | Existing error responses remain compatible. |

### Consumer tests

| Consumer | Test Case |
|---|---|
| UI | Trade display uses `trade_date`; settlement display uses `settlement_date`. |
| Cash projection | Same trade with late settlement affects cash on value date, not trade date. |
| Performance | Late booking does not move economic performance date. |
| Operations report | Late booking appears in booking-date report. |
| Data lake | Schema stores explicit date fields and lineage labels. |

### Negative tests

| Negative Case | Expected Result |
|---|---|
| Consumer uses `transaction_date` for cash settlement | Test fails or emits migration warning. |
| Contract example omits new date fields | Documentation quality gate fails after additive release. |
| Strict client rejects unknown fields | Generated-client compatibility test catches the issue before rollout. |
| `booking_date` is populated with trade date by default | Semantic contract test fails. |

## 9. Observability and Governance

The migration should be observable.

| Signal | Purpose |
|---|---|
| Consumer inventory | Names owners, systems, contact path and migration status. |
| Deprecated field usage | Measures which consumers still request or deserialize legacy field. |
| Contract version adoption | Shows which consumers use schema version or generated client version. |
| Error and warning rates | Detects client breakage after additive release. |
| Support tickets | Tracks confusion caused by date semantics. |
| Release notes acknowledgement | Proves consumers were informed. |

For internal APIs, field-level usage may be difficult to measure directly. Use a combination of client inventory, generated-client versions, request headers, consumer tests and release sign-off.

## 10. Release Note Example

```text
Transaction API now includes explicit date fields: trade_date, effective_date,
settlement_date, value_date and booking_date.

The existing transaction_date field remains available for compatibility but is
deprecated as a semantic shorthand. New integrations should not use
transaction_date for booking, settlement, value-date or performance logic.

No fields are removed in this release. Existing clients should continue to
parse responses successfully. Consumers should migrate to the explicit date
field that matches their business use case before the next major API version.
```

## 11. PR Evidence Checklist

Before merging the API change, the PR should include:

1. OpenAPI diff showing additive fields and deprecation marker,
2. updated response examples,
3. consumer-impact note,
4. contract tests for old and new response shapes,
5. generated-client compatibility check,
6. at least one domain example where trade, settlement and booking dates differ,
7. release note or changelog entry,
8. owner for consumer migration tracking,
9. support/runbook note explaining the field semantics,
10. follow-up issue or roadmap item for the removal decision if removal is planned.

## 12. Anti-Patterns

| Anti-Pattern | Why It Is Dangerous |
|---|---|
| Renaming a field and removing the old one in the same minor release | Breaks consumers without a migration window. |
| Keeping old name but silently changing meaning | Produces worse failures because consumers do not know behavior changed. |
| Adding explicit fields without examples where dates differ | Consumers may still assume all dates are equivalent. |
| Deprecating without consumer inventory | The team cannot know when removal is safe. |
| Treating generated docs as enough communication | Critical consumers may need direct migration guidance and test evidence. |
| Using UI behavior to infer contract compatibility | Non-UI consumers may still break. |

## 13. Key Takeaway

API compatibility is not only schema shape. It is also semantic stability. A professional API evolution keeps old consumers safe, introduces clearer fields, makes deprecation visible, tests both old and new behavior, and removes legacy fields only through governed evidence.
