# Template, Layout, Rendering, and Document Generation

## Purpose

This file explains template and rendering design for enterprise report generation.

Templates and rendering engines should control presentation while preserving data provenance and content integrity.

---

## Template Responsibilities

A template owns:

- layout
- section ordering
- typography
- headers/footers
- tables
- chart placement
- page breaks
- disclaimers
- locale formatting
- style variations

A template should not own:

- domain calculations
- source data retrieval
- entitlement decisions
- performance methodology
- risk methodology

---

## Template Versioning

Capture:

- template id
- template version
- effective date
- supported report type
- supported locale/language
- owner
- approval state
- deprecation state

---

## Rendering Pipeline

```text
content model
  -> template selection
  -> render input validation
  -> rendering
  -> output validation
  -> document hash
  -> archive handoff
```

---

## Document Formats

Possible outputs:

- PDF
- HTML preview
- DOCX
- CSV/XLSX export
- JSON evidence package

Different formats may need different controls.

---

## Output Validation

Validate:

- document created
- expected sections present
- page count reasonable
- no rendering errors
- warnings included
- disclaimers included
- metadata embedded where required
- document hash generated

---

## Rendering Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Template contains calculation logic | Methodology drift. |
| Template not versioned | Cannot explain report later. |
| Renderer fetches live data | Not reproducible. |
| PDF generated without validation | Broken document archived. |
| Disclaimers manually pasted | Inconsistency. |
| Local fonts/assets missing in runtime | Production render failures. |
| No fallback for render failure | Job stuck. |

---

## Review Checklist

- Is template versioned?
- Is template approved?
- Is content model validated before rendering?
- Does renderer avoid domain logic?
- Are assets available in runtime?
- Are disclaimers controlled?
- Is output validated?
- Is document hash generated?
- Are render failures observable?
- Are golden render tests present?

---

## Summary

Rendering is presentation engineering, not domain engineering.

Templates should preserve evidence-backed content and produce controlled, versioned documents.
