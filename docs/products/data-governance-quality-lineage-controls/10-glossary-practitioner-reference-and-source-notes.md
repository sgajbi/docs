# 10. Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| CDE | Critical Data Element; a field whose incorrect value can materially affect decisions, reporting, risk, accounting or controls. |
| Data governance | Operating model for defining, owning, controlling and using data responsibly. |
| Data quality | Degree to which data is fit for intended business use. |
| Data steward | Person/team responsible for operational quality and definitions within a data domain. |
| Source of record | Authoritative source for a field or business object. |
| Golden copy | Controlled, validated, enriched and publishable version of source data. |
| Lineage | Trace from source through transformations to consumption. |
| Provenance | Origin and history of a data item. |
| Data contract | Agreement between producer and consumer covering schema, semantics, SLA, quality and change rules. |
| Reconciliation | Comparison of independently produced values to detect differences. |
| Break | Reconciliation or data-quality exception requiring classification and action. |
| Certification | Controlled sign-off that data is fit for a defined purpose. |
| Degraded state | Data is usable only with limitations or disclosures. |
| Override | Controlled manual replacement or adjustment of source data. |
| Snapshot | Immutable point-in-time view of data for report/calculation/audit. |
| Idempotency | Ability to process same input multiple times without duplicate effect. |
| Restatement | Reissue or correction of previously published output. |
| SLA | Service level agreement, usually between producer and consumer. |
| OLA | Operational level agreement, usually internal to support SLA. |

## 2. Practitioner Explanations

### What is data governance?

Data governance is the operating model that ensures data is clearly defined, owned, controlled, traceable and fit for business use. In a wealth platform it covers product master, prices, FX, transactions, positions, client/account data, mandates, performance, risk and reporting. Good governance means every critical field has an owner, source, definition, quality rule, lineage and remediation path.

### What is the difference between data quality and reconciliation?

Data quality checks whether a data item is valid, complete, timely, consistent and fit for use. Reconciliation compares two or more independently produced views of the same business reality. For example, checking that every position has a price is a data-quality rule; comparing custodian position quantity against accounting position quantity is reconciliation.

### What is golden copy?

Golden copy is the controlled, validated and publishable version of data produced from one or more sources. It is not just a copy of a source feed. It includes source hierarchy, validation, enrichment, effective dating, lineage, override status and certification.

### Why is lineage important?

Lineage allows a platform to trace a reported number back to its source inputs and transformations. In portfolio reporting, if a client asks why market value changed, lineage should identify the position quantity, price, FX rate, accrual, calculation version and report snapshot used.

### How should a platform handle missing data?

The platform should not silently assume or default critical values. It should classify the missing data by severity and consumer impact. Low-impact missing data may produce a warning; critical missing price, mandate or suitability data may block report generation or proposal approval. If a fallback or estimate is used, it should be disclosed and traceable.

### How do you design data controls for advisory and DPM?

Start with decisions: recommendation, mandate check, rebalance, trade and client report. Identify the data needed for each decision, mark CDEs, assign source owners, define quality rules and create degraded-state behaviours. Mandate engines should return pass, fail, warning or unable-to-evaluate, not silently pass when data is missing.

### Why do migrations fail from a data perspective?

Migrations fail when teams compare only counts or totals and ignore taxonomy, transaction classification, cost basis, performance continuity, corporate actions, price basis, FX conventions and report labels. A good migration control framework compares positions, cash, market values, P&L, performance, allocation, risk, reports and source lineage, while documenting expected differences.

## 3. Practitioner Operating Checklist

| Question | Best answer |
|---|---|
| Who owns this field? | The data owner/steward in the source ownership matrix. |
| Is the value reliable? | Check quality status, source, freshness, reconciliation and override flag. |
| Why did report number change? | Use report snapshot and lineage to trace positions, prices, FX, transactions and formulas. |
| Can we publish with missing data? | Follow severity, materiality and reporting policy; block or disclose when needed. |
| Why did performance change after month-end? | Check backdated transactions, price/FX corrections, corporate actions and recalculation lineage. |
| Why does custodian differ from platform? | Check trade-date/settlement-date basis, corporate actions, pending trades, mapping and timing. |
| What should be tested? | Every production incident and migration defect should become a regression scenario. |
| What evidence is needed? | Rule result, reconciliation result, break closure, approval, snapshot, lineage and certification. |

## 4. Source notes and further reading

The following external references informed the control principles in this pack. They are used as education and governance context; apply institution-specific policy and jurisdictional rules as required.

### Banking and risk data aggregation

- Basel Committee on Banking Supervision, **Principles for effective risk data aggregation and risk reporting**, BCBS 239, January 2013.
  https://www.bis.org/publ/bcbs239.pdf

Key relevance: governance, data architecture, accuracy, completeness, timeliness, adaptability, risk reporting quality, distribution, review and remediation.

### Technology and operational resilience

- Monetary Authority of Singapore, **Technology Risk Management Guidelines**, January 2021.
  https://www.mas.gov.sg/regulation/guidelines/technology-risk-management-guidelines

Key relevance: technology risk governance, system resilience, data confidentiality/integrity/availability, controls and operational readiness.

### Data quality standards

- ISO, **ISO 8000-1:2022 Data quality - Part 1: Overview**.
  https://www.iso.org/standard/81745.html

Key relevance: data quality, data governance, data quality management and data quality assessment concepts.

### Data management body of knowledge

- DAMA International, **DAMA-DMBOK: Data Management Body of Knowledge**.
  https://www.dama.org/

Key relevance: data governance, data architecture, metadata, master/reference data, data quality, data warehousing and data management disciplines.

### Financial market infrastructures and operational controls

- CPMI-IOSCO, **Principles for financial market infrastructures**.
  https://www.bis.org/cpmi/publ/d101a.pdf

Key relevance: governance, risk management, settlement, operational risk and market infrastructure control principles.

### Benchmarks and reporting integrity

- IOSCO, **Principles for Financial Benchmarks**, July 2013.
  https://www.iosco.org/library/pubdocs/pdf/IOSCOPD415.pdf

Key relevance: benchmark governance, methodology, transparency, accountability and conflict controls.

## 5. Knowledge-base integration guidance

This pack should be cross-linked with:

- Market Data, Reference Data, Pricing and Instrument Master Design;
- Investment Accounting, Ledger, Fees, P&L, Accruals and Book-of-Record Design;
- Trade Lifecycle, Order Management, Settlement, Custody and Investment Operations;
- Portfolio Reporting, Statement Design, Portfolio Review and Client Experience;
- Portfolio Performance, Attribution, Risk, Benchmarks and Mandate Analytics;
- Product Governance, APU and Target Market Framework;
- all product-family reference packs.

## 6. Final takeaway

Data governance is not a documentation exercise. It is the control system that lets a wealth platform prove that client reports, advisory decisions, portfolio analytics, mandate checks, valuations, migrations and operational processes are based on reliable, explainable and traceable data.
