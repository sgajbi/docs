# 12 - Source Notes and Further Reading

## 1. Source note

This pack is primarily a platform architecture synthesis for wealth-management systems. It combines general architecture principles with wealth-specific patterns around products, positions, transactions, accounting, performance, risk, reporting, advisory, mandates and controls.

It is not a regulatory rulebook and should not be used as legal, tax, regulatory or investment advice.

## 2. Key standards and references

| Area | Source | Why useful |
|---|---|---|
| HTTP API contracts | OpenAPI Specification | Standard language-agnostic interface description for HTTP APIs. |
| Event/API contracts | AsyncAPI Specification | Documentation and governance of event-driven/asynchronous APIs. |
| Event envelope | CloudEvents Specification | Common metadata envelope for event interoperability. |
| Observability | OpenTelemetry | Standardized telemetry: traces, metrics and logs. |
| Cloud architecture | AWS Well-Architected Framework | Operational excellence, security, reliability, performance, cost and sustainability pillars. |
| Cloud native | CNCF | Cloud-native ecosystem and platform operating model concepts. |
| Security | OWASP ASVS | Security control verification for applications. |
| Digital identity | NIST SP 800-63 | Identity and authentication assurance guidance. |
| Data governance | BCBS 239 | Risk data aggregation and reporting principles. |
| Data quality | ISO 8000 | Data quality standards and concepts. |
| Fair value | IFRS 13 | Fair-value measurement concepts. |

## 3. Practical reading list

- OpenAPI Specification, latest published version.
- AsyncAPI Specification, latest published version.
- CloudEvents specification.
- OpenTelemetry documentation.
- AWS Well-Architected Framework.
- CNCF cloud-native materials.
- OWASP Application Security Verification Standard.
- NIST Digital Identity Guidelines.
- BCBS 239 principles for effective risk data aggregation and risk reporting.
- ISO 8000 data quality standard family.
- Enterprise Integration Patterns by Hohpe and Woolf.
- Domain-Driven Design by Eric Evans.
- Implementing Domain-Driven Design by Vaughn Vernon.
- Designing Data-Intensive Applications by Martin Kleppmann.

## 4. Source interpretation guidance

Use architecture standards as principles, not as copy-paste templates. Wealth platforms require additional domain-specific decisions:

- trade-date vs settlement-date basis
- official vs provisional valuation
- client/account/portfolio hierarchy
- product taxonomy and look-through exposure
- report snapshot reproducibility
- suitability and mandate control lineage
- event replay and recalculation impact
- country/booking-centre configuration

## 5. Applying this pack in a repository

Recommended location:

```text
docs/reference/wealth-platform-architecture/
```

Recommended cross-links:

- product packs
- market/reference data pack
- investment accounting pack
- trade lifecycle operations pack
- portfolio analytics pack
- portfolio reporting pack
- data governance and controls pack
- client/account master data pack
- entitlements/advisor workbench pack

## 6. Maintenance checklist

Review this pack when:

- a new product family is added
- a new country/booking centre is onboarded
- a major source system changes
- API/event standards change
- reporting requirements change
- performance/risk methodology changes
- a new advisory or DPM workflow is introduced
- platform moves to a new infrastructure/cloud model
- incident or audit finding reveals a governance gap
