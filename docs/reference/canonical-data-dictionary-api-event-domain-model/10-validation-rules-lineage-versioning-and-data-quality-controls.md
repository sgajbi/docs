# 10 - Validation Rules, Lineage, Versioning and Data Quality Controls

## 1. Validation rule types

| Rule type | Example |
|---|---|
| required field | portfolio must have base currency |
| domain value | product risk rating must be allowed value |
| referential integrity | transaction instrument exists |
| date logic | settlement date >= trade date unless allowed |
| numeric logic | quantity cannot be negative unless short allowed |
| currency consistency | amount currency must be valid ISO currency |
| status transition | booked -> settled allowed, settled -> draft not allowed |
| entitlement | user can access portfolio |
| product rule | complex product requires eligibility |
| data freshness | price must be within stale threshold |
| reconciliation | position equals transaction-derived position |

## 2. Data lineage fields

Every calculated or reported value should trace:

| Lineage element | Example |
|---|---|
| source_system | custodian, market data vendor |
| source_record_id | source transaction ID |
| ingestion_batch_id | batch/run |
| transformation_version | ETL version |
| calculation_version | performance-v1.2 |
| input_snapshot_id | source data snapshot |
| output_snapshot_id | report snapshot |
| generated_by | service/user |
| generated_at | timestamp |
| approval_id | if reviewed/approved |

## 3. Versioning

Version:

- APIs
- event schemas
- calculation methods
- rulesets
- report templates
- product taxonomy
- data mappings
- AI prompts
- AI evaluations
- model versions
- migration mappings

## 4. Data-quality dimensions

| Dimension | Meaning |
|---|---|
| completeness | required data present |
| accuracy | data reflects truth/source |
| timeliness | data is current enough |
| consistency | same meaning across systems |
| validity | conforms to allowed formats/domains |
| uniqueness | no duplicates |
| integrity | relationships valid |
| lineage | traceable to source |
| explainability | understandable and auditable |

## 5. Example validation rules

### Transaction

- transaction_id required
- portfolio_id required
- transaction_type required
- trade_date required
- amount currency required for cash legs
- settlement_date required for settled trades
- reversal transaction must link to original

### Position

- position quantity equals sum of transaction quantity deltas
- closed position quantity must be zero
- cost basis must not be negative unless policy allows
- market value requires price or valuation method

### Price

- price date required
- source required
- price value > 0 except special cases
- stale flag if older than product threshold
- price basis must match instrument type

### Report

- report snapshot must list data sources
- report date required
- template version required
- degraded data must create exception
- archived report must be immutable

### AI

- answer must link to source retrieval records
- client data requires entitlement check
- advice-related output requires human review
- prohibited actions cannot be executed by AI

## 6. Reconciliation patterns

| Reconciliation | Purpose |
|---|---|
| transaction count | source vs target |
| cash balance | ledger vs custodian |
| position quantity | internal vs custodian |
| market value | internal vs statement/vendor |
| performance | old vs new engine |
| report totals | report vs source snapshot |
| entitlement | access policy vs actual access |
| AI source | answer citations vs approved sources |

## 7. Degraded data status

Use explicit statuses:

| Status | Meaning |
|---|---|
| complete | all required data available |
| partial | some non-critical data missing |
| degraded | output generated with limitations |
| blocked | output cannot be generated |
| estimated | estimate used |
| stale | stale input used |
| unsupported | product/analytics not supported |

## 8. Key takeaway

Validation, lineage and data-quality controls are what make wealth outputs trustworthy.
