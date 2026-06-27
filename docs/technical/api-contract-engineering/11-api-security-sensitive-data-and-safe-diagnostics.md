# API Security, Sensitive Data, and Safe Diagnostics

## Purpose

This file explains how API design must protect sensitive data and support safe diagnostics.

APIs often become the place where data, errors, logs, traces, screenshots, and generated evidence leak. Good design prevents this.

## Sensitive Data In API Context

Sensitive data may include:

- client names
- client identifiers
- account numbers
- portfolio identifiers
- transaction details
- holdings
- market values
- suitability rationale
- advisory notes
- report content
- archive document links
- prompts and model responses
- entitlement details
- internal source paths
- secrets and tokens

## Request Safety

APIs should validate and limit request payloads.

Controls:

- size limits
- schema validation
- allowed fields
- enum validation
- content-type validation
- file-type restrictions
- malware scanning where file upload exists
- authorization before expensive work
- sensitive field filtering

## Response Safety

Responses should expose only what the caller is allowed to see.

Controls:

- authorization filtering
- field-level restrictions where needed
- safe permission-blocked states
- no raw internal errors
- no storage paths
- no secrets
- no unrestricted diagnostic data

## Diagnostic APIs

Diagnostic APIs are useful but risky.

They should:

- require strong authorization
- expose bounded fields
- include reason codes
- include safe lifecycle state
- avoid raw payloads
- avoid sensitive source refs unless protected
- be audited
- have runbook guidance

## Logging API Requests

Avoid logging:

- full request body
- full response body
- authorization headers
- cookies
- secrets
- account identifiers
- prompts
- generated AI text
- raw entitlement errors

Log:

- operation
- status class
- reason code
- duration bucket
- correlation id
- dependency
- safe caller class where allowed

## Metrics Safety

Do not use dynamic sensitive identifiers as metric labels.

Unsafe labels:

- actor id
- account id
- portfolio id
- document id
- transaction id
- trace id
- raw error text

Safe labels:

- operation
- status class
- state
- reason code
- dependency
- freshness bucket

## API Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Raw exception in response | Internal information leak. |
| Raw payload in diagnostic route | Sensitive-data leak. |
| Metrics with account id | Privacy and cardinality issue. |
| Download route exposes storage path | Archive/security risk. |
| BFF bypasses authorization | Data leakage. |
| File upload without validation | Malware or storage risk. |
| Prompt content returned in logs | AI data leakage. |
| Error reveals hidden resource existence | Entitlement leakage. |

## Review Checklist

- Are requests schema-validated?
- Are response fields authorized?
- Are diagnostic routes protected?
- Are errors safe?
- Are logs safe?
- Are metrics label-safe?
- Are traces safe?
- Are file uploads controlled?
- Are document downloads mediated?
- Are AI inputs/outputs protected?
- Are generated evidence artifacts filtered?

## Summary

Safe APIs do not only protect successful responses.

They protect requests, errors, logs, metrics, traces, diagnostics, screenshots, generated evidence, and downstream consumers.
