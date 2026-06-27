# 03 - Source Ownership, Golden Copy, Lineage and Metadata Management

## 1. Why source ownership matters

Many wealth-platform defects remain unresolved because no one agrees which system owns the truth. Source ownership solves this by assigning accountability for each domain, field and lifecycle event.

Without ownership:

- instrument master says a security is a bond, reporting says structured product;
- custodian position differs from internal calculated position;
- fund NAV arrives from vendor but operations manually overrides without lineage;
- advisory sees a product as eligible but OMS rejects it;
- performance uses one FX source while reporting uses another;
- a migration produces matching totals but wrong product classification.

## 2. Source-of-record versus source-of-use

| Concept | Meaning |
|---|---|
| Source of record | Authoritative system for a business object or field. |
| Source of origin | System where data first entered the ecosystem. |
| Golden copy | Controlled, validated, enriched version used by consuming systems. |
| Source of use | System/application that consumes data for a business process. |
| Derived source | Engine that calculates values from controlled inputs. |
| Fallback source | Secondary source used when primary source is unavailable or fails quality checks. |

Example:

| Field | Source of record | Golden copy | Source of use |
|---|---|---|---|
| ISIN | Instrument vendor/reference utility | Instrument master | Reporting, OMS, risk. |
| Equity closing price | Market data vendor/exchange | Price service | Valuation, performance. |
| Settled cash | Core banking/custodian | Cash balance service | Buying power, reports. |
| TWR | Performance engine | Analytics snapshot | Portfolio review, advisor UI. |
| Collateral eligibility | Credit policy/collateral engine | Buying power service | Pre-trade checks. |

## 3. Golden copy design

A golden copy is not simply a database table. It is a controlled product of ingestion, validation, enrichment, conflict resolution, lineage and certification.

Golden copy should include:

- source identifier;
- source priority;
- validation status;
- effective date;
- version;
- lineage ID;
- steward approval status;
- override flag;
- quality score;
- downstream publication status.

## 4. Source hierarchy

A source hierarchy defines which source wins when values differ.

Example pricing hierarchy:

| Priority | Source | Condition |
|---:|---|---|
| 1 | Exchange close | Listed liquid instruments, valid trading day. |
| 2 | Vendor evaluated price | Bonds/OTC instruments with valid quality score. |
| 3 | Issuer quote | Structured products where issuer provides bid/indicative quote. |
| 4 | Model price | Approved valuation model with required inputs. |
| 5 | Manual override | Approved and time-limited only. |
| 6 | Previous price | Allowed only with stale-price flag and materiality rules. |

The hierarchy must be product-specific. A listed equity may use exchange close; a private-market fund may use last capital-account statement; a structured note may use issuer bid or vendor price.

## 5. Metadata management

Metadata is data about data. It makes governance executable.

| Metadata type | Examples |
|---|---|
| Business metadata | Definition, owner, domain, CDE flag, usage. |
| Technical metadata | Data type, length, precision, nullable, schema version, API path. |
| Operational metadata | Run ID, load time, source file, event offset, status. |
| Quality metadata | Rule result, score, exception, override, certification. |
| Lineage metadata | Source, transformation, derived fields, consumer report/API. |
| Security metadata | Classification, sensitivity, access entitlement, retention. |

## 6. Data lineage levels

Lineage should be represented at multiple levels.

| Level | Purpose | Example |
|---|---|---|
| System lineage | Shows system-to-system flow. | Market vendor -> price service -> valuation engine -> report. |
| Dataset lineage | Shows table/file/API dependencies. | `prices_eod` feeds `holding_valuation_snapshot`. |
| Field lineage | Shows how a field is computed. | Base market value = local value x FX rate. |
| Calculation lineage | Shows inputs and formula version. | TWR uses BOD MV, EOD MV, external cashflows and formula v3. |
| Report lineage | Shows which data produced a published report. | Report R123 uses snapshot S456 and calculation run C789. |

## 7. Lineage example: market value on a client report

```text
Client report market value
  = Position quantity
    x Local price
    x Price multiplier
    x FX rate to report currency
    + Accrued income where report basis is dirty/accrued-inclusive
```

Lineage should identify:

| Input | Required lineage |
|---|---|
| Quantity | Custodian/accounting source, transaction replay version, position date. |
| Price | Vendor/source, price date, price type, stale flag, override flag. |
| Multiplier | Instrument master, effective date. |
| FX | FX source, rate type, valuation date, quote convention. |
| Accrued income | Accrual engine, day count, coupon schedule, calculation version. |
| Report number | Snapshot ID, generated timestamp, user, template version. |

## 8. Data contracts

A data contract formalises expectations between producer and consumer.

A good data contract includes:

| Section | Content |
|---|---|
| Purpose | Business process supported. |
| Owner | Producer and consumer owners. |
| Schema | Fields, data types, enums, precision, mandatory status. |
| Semantics | Business definitions and examples. |
| Delivery SLA | Frequency, cut-off, retry, publication calendar. |
| Quality rules | Validations and thresholds. |
| Change management | Notice period, compatibility, versioning. |
| Error handling | Rejection, quarantine, replay, correction. |
| Security | Data classification and access. |
| Lineage | Run ID, source file, event ID, correlation ID. |

## 9. Ownership matrix by domain

| Domain | Source owner | Consumer examples | Common control |
|---|---|---|---|
| Client/account | CRM/core banking | Advisory, reporting, access control | Account-to-client reconciliation. |
| Product master | Reference data | OMS, suitability, risk, reporting | CDE completeness and classification checks. |
| Prices | Market data / valuation control | Valuation, P&L, risk | Stale/missing price controls. |
| FX | Treasury/market data | Valuation, performance | Quote convention and date checks. |
| Transactions | OMS/custodian/core banking | Accounting, positions, performance | Duplicate and control-total checks. |
| Positions | Custodian/accounting book | Reporting, risk, collateral | Position reconciliation. |
| Corporate actions | Custodian/CA operations | Positions, cost basis, income | CA event status and entitlement reconciliation. |
| Mandates | DPM/mandate platform | Advisory, pre-trade, reporting | Rule version and applicability controls. |
| Analytics | Calculation engines | Advisor UI, reports | Input completeness and reproducibility controls. |

## 10. Versioning and effective dating

Data changes over time. A platform should store history, not overwrite silently.

| Data type | Versioning need |
|---|---|
| Instrument classification | Changes due to correction or product reclassification. |
| Credit rating | Effective-dated rating events. |
| Fund risk rating | Changes affect suitability and historical advice evidence. |
| Client risk profile | Historical recommendations require historical profile. |
| Mandate rules | Proposal must be tested against rule version active at proposal time. |
| Benchmark composition | Performance attribution must reproduce historical composition. |
| Price and FX | Immutable by valuation date/source, with correction versions. |
| Report snapshot | Locked once published, with restatement mechanism. |

## 11. Handling conflicting sources

When sources disagree:

1. Determine whether the difference is due to timing, definition, currency, precision or true error.
2. Apply source hierarchy and product-specific rules.
3. Record exception and consumer impact.
4. Allow approved temporary override if needed.
5. Correct the authoritative source where possible.
6. Retain lineage and audit trail of decision.

Example:

| Break | Possible cause | Resolution |
|---|---|---|
| Custodian position 100, internal position 120 | Pending corporate action, unsettled trade, duplicate transaction | Reconcile by basis: trade date vs settlement date; inspect CA event. |
| Price vendor 95, issuer quote 88 | Bid/ask basis difference, indicative vs executable, stale vendor price | Select source based on valuation policy and mark quality. |
| Fund NAV date differs | Time zone, late fund publication, share class mismatch | Confirm share class and NAV calendar. |

## 12. Practitioner takeaway

Source ownership and lineage are not documentation luxuries. They are production controls. Every critical reported number should answer: who owns it, where it came from, how it was transformed, whether it passed quality controls, and whether a user can rely on it for the current purpose.
