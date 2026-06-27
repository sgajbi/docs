# 09 - Data Quality, Reconciliation, Migration and Exception Case Studies

## 1. Stale price exception

### Scenario

Equity price is three business days old.

### Expected handling

- stale price flag
- valuation still possible using last available price if policy permits
- report disclosure
- downstream risk/performance warning
- exception assigned to data owner

### QA assertions

- stale threshold is product/market aware.
- report does not silently treat stale price as current.
- AI explanation mentions stale data if used.

## 2. Missing FX rate

### Scenario

Portfolio holds EUR asset, base currency USD. EUR/USD missing.

### Expected handling

- attempt approved fallback source
- if unavailable, valuation degraded
- reporting exception shown
- performance calculation may exclude or fail by policy

### QA assertions

- no unapproved FX source used.
- missing FX error is traceable.
- report total market value either blocked or flagged.

## 3. Position reconciliation break

### Scenario

Internal position shows 1,000 shares. Custodian shows 900 shares.

### Expected workflow

```text
Break detected
-> classify break
-> identify source difference
-> assign owner
-> investigate transactions/corporate actions
-> correct or explain
-> certify resolution
```

### QA assertions

- break is visible in dashboard.
- report can be blocked or flagged based on materiality.
- correction is auditable.

## 4. Corporate action migration issue

### Scenario

Stock split occurred before migration. Legacy system already adjusted quantity, but migration file also applies split.

### Bad result

Quantity doubles twice.

### Expected control

- corporate action history reconciliation
- effective-date check
- migrated quantity versus expected quantity
- no duplicate application

### QA assertions

- idempotency key prevents duplicate CA application.
- migration validation catches abnormal quantity jump.

## 5. Performance history migration

### Scenario

New platform starts in 2026 but client requires since-inception performance from 2020.

### Expected options

- migrate historical transactions and valuations
- migrate validated performance time series
- use legacy performance as historical baseline
- disclose methodology if blended

### QA assertions

- continuity date is visible.
- performance methodology version is captured.
- old/new system reconciliation performed.

## 6. Report restatement

### Scenario

A bond price was wrong in prior monthly statement.

### Expected workflow

- identify impacted reports
- calculate corrected values
- decide restatement policy
- regenerate corrected statement if required
- archive original and corrected versions
- explain reason
- maintain audit trail

### QA assertions

- reports are versioned.
- correction is traceable.
- client/advisor can see latest valid version.

## 7. AI using outdated document

### Scenario

AI answers product eligibility using old policy document.

### Expected control

- document effective-date filtering
- approved-source retrieval only
- outdated-source warning
- answer blocked if current policy unavailable

### QA assertions

- AI citations are current.
- superseded documents are not used as authoritative.
- retrieval logs show source version.

## 8. Key takeaway

Data-quality and migration examples must test degraded states, not only happy paths.
