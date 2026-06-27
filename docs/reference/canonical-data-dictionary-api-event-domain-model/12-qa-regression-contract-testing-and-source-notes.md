# 12 - QA, Regression, Contract Testing and Source Notes

## 1. Purpose

This file defines how to test canonical data, API and event contracts.

## 2. Contract testing types

| Test type | Purpose |
|---|---|
| Schema validation | payload conforms to schema |
| Consumer-driven contract | producer does not break consumers |
| Backward compatibility | older consumers still work |
| Example validation | examples are valid |
| Error contract | errors follow standard shape |
| Pagination test | large queries behave correctly |
| Idempotency test | retry does not duplicate |
| Entitlement test | unauthorized access blocked |
| Event replay test | replay is safe |
| Lineage test | output traces to source |
| DQ propagation test | degraded input appears in output status |

## 3. API contract regression

Every API release should test:

- required fields present
- optional fields allowed
- unknown fields handling policy
- enum compatibility
- pagination behavior
- filters
- sorting
- error responses
- authentication
- authorization
- rate limits
- idempotency
- correlation ID propagation

## 4. Event contract regression

Every event release should test:

- event envelope valid
- event type correct
- schema version correct
- data payload valid
- partition key present
- correlation and causation IDs present
- duplicate event handling
- out-of-order handling if possible
- dead-letter behavior
- replay behavior
- consumer compatibility

## 5. Data dictionary QA

Check:

- field names are consistent
- types are correct
- money fields have currency
- dates have clear meaning
- nullable fields have reason
- enums are controlled
- effective-dated fields handled
- source owner specified
- lineage required
- reporting impact documented

## 6. Golden contract scenarios

Minimum golden scenarios:

| Scenario | Expected |
|---|---|
| portfolio positions with bond | nominal amount and clean price |
| equity with corporate action | adjusted quantity and cost |
| fund subscription | units from NAV and fees |
| note autocall | lifecycle event then redemption |
| private capital call | commitment and funded position |
| FX conversion | two currency legs |
| stale price | degraded valuation and report flag |
| suitability failure | 422 business validation |
| mandate breach | event and workflow task |
| AI grounded response | source references and approval flag |

## 7. Breaking-change checklist

Before changing schema:

- identify consumers
- classify change
- test compatibility
- add new version if breaking
- document migration
- notify consumers
- support dual versions if needed
- update examples
- update QA scenarios
- update generated docs

## 8. Source notes and further reading

Key standards and references:

1. OpenAPI Specification.
   - Defines a standard, programming language-agnostic interface description for HTTP APIs.
   - https://spec.openapis.org/oas/latest.html

2. AsyncAPI Specification.
   - Used to describe asynchronous and event-driven APIs.
   - https://www.asyncapi.com/docs/reference/specification/latest

3. CloudEvents.
   - Specification for describing event data in a common way.
   - https://cloudevents.io/

4. RFC 9457 - Problem Details for HTTP APIs.
   - Defines a problem-details format for HTTP API error responses.
   - https://www.rfc-editor.org/rfc/rfc9457.html

5. JSON Schema Specification.
   - Defines schema vocabulary for validating JSON data.
   - https://json-schema.org/specification

6. ISO 20022.
   - Universal financial industry message scheme and repository with data dictionary and business process catalogue.
   - https://www.iso20022.org/

7. ISO 4217.
   - Currency codes.
   - https://www.iso.org/iso-4217-currency-codes.html

## 9. Interview-ready explanation

A canonical data dictionary and contract catalog is the implementation bridge between business domain knowledge and platform engineering. It defines the meaning of entities such as clients, portfolios, instruments, transactions, positions, valuations, performance and reports, and then expresses them through APIs, events and schemas. For wealth platforms, this is critical because calculation, reporting, suitability, mandates and AI explanations depend on consistent business meaning, source lineage, versioned contracts, validation and auditability.

## 10. Key takeaway

The best domain model is not only documented. It is enforced through APIs, event schemas, validation, tests and operational controls.
