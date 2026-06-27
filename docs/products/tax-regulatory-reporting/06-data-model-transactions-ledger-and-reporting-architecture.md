# 06 - Data Model, Transactions, Ledger and Reporting Architecture

## 1. Design goal

The goal is to build a product-neutral reporting architecture that can support tax-aware reporting across all investment products without hardcoding product-specific tax rules inside transaction processing.

Core principle:

```text
Book the economics accurately first.
Classify tax and reporting treatment through enrichment layers.
```

## 2. Canonical architecture

```text
Source systems
  -> transaction ingestion
  -> product master enrichment
  -> client/account/entity enrichment
  -> accounting ledger
  -> position engine
  -> tax-lot engine
  -> income/withholding engine
  -> reportability engine
  -> reporting marts
  -> statements, tax packs, regulatory files
```

## 3. Core entities

| Entity | Purpose |
|---|---|
| Client/person | Natural person data, tax residence, TIN, suitability |
| Entity | Company, trust, foundation, family office vehicle |
| Account | Booked financial account |
| Portfolio | Analytical grouping of accounts/positions |
| Mandate | DPM/advisory model constraints/objective |
| Instrument | Security/product master |
| Product contract | Detailed terms for notes, loans, insurance, private funds etc. |
| Transaction | Economic event booked to account |
| Transaction leg | Cash/security/tax/fee/security movement legs |
| Position | Current holdings/balances |
| Tax lot | Acquisition lots and cost basis |
| Income event | Dividend/coupon/interest/distribution entitlement/payment |
| Withholding record | Tax withheld and rate basis |
| Tax document | W-8/W-9/CRS self-cert and local forms |
| Reportability record | CRS/FATCA status and reporting-year snapshot |
| Statement record | Issued report line with versioning |

## 4. Transaction model

Use transaction groups and legs to represent complex events.

Example dividend:

| Leg | Type | Amount |
|---|---|---:|
| 1 | Gross dividend income | +1,000 |
| 2 | Withholding tax | -300 |
| 3 | Net cash receipt | +700 |

Example structured note physical settlement:

| Leg | Type | Instrument | Quantity/amount |
|---|---|---|---:|
| 1 | Note redemption out | Note | -100,000 nominal |
| 2 | Equity delivery in | Equity | +500 shares |
| 3 | Cash-in-lieu | Cash | +25 |
| 4 | Tax/fee if applicable | Cash | -5 |

## 5. Recommended transaction types

| Category | Transaction types |
|---|---|
| Income | DIVIDEND, INTEREST, COUPON, FUND_DISTRIBUTION, REIT_DISTRIBUTION, ANNUITY_PAYMENT |
| Tax | WITHHOLDING_TAX, TAX_RECLAIM_RECEIPT, TAX_RECLAIM_WRITEOFF, TAX_ADJUSTMENT |
| Trading | BUY, SELL, SUBSCRIPTION, REDEMPTION, SWITCH, TRANSFER_IN, TRANSFER_OUT |
| Capital events | MATURITY, CALL_REDEMPTION, TENDER, MERGER_CASH, LIQUIDATION |
| Corporate action | SPLIT, BONUS, RIGHTS, SPINOFF, MERGER_STOCK, RETURN_OF_CAPITAL |
| Derivatives | OPTION_PREMIUM, EXERCISE, ASSIGNMENT, FUTURE_VARIATION_MARGIN, SWAP_SETTLEMENT |
| Private markets | COMMITMENT, CAPITAL_CALL, DISTRIBUTION, RECALLABLE_DISTRIBUTION, COMMITMENT_REDUCTION |
| Loans/margin | LOAN_DRAWDOWN, LOAN_REPAYMENT, INTEREST_CHARGE, COLLATERAL_PLEDGE, MARGIN_CALL |
| Insurance | PREMIUM, SURRENDER, CLAIM, POLICY_LOAN, RIDER_CHARGE |
| Corrections | REVERSAL, CANCEL_CORRECT, RESTATEMENT_ADJUSTMENT |

## 6. Tax classification dimensions

A transaction should be enriched with multiple independent dimensions.

| Dimension | Examples |
|---|---|
| tax_event_category | Income, disposal, return of capital, expense, non-taxable, unknown |
| income_subtype | Dividend, interest, coupon, REIT distribution, annuity |
| capital_event_subtype | Sale, redemption, maturity, merger, liquidation |
| source_country | US, SG, HK, JP, etc. |
| client_tax_jurisdiction | Client/reporting tax residence |
| withholding_regime | Domestic WHT, treaty WHT, FATCA, statutory |
| tax_form_basis | W-8BEN, W-8BEN-E, W-9, CRS self-cert |
| reportability | CRS reportable, FATCA reportable, local reportable |
| tax_lot_method | FIFO, average, specific ID |
| certainty_level | Final, estimated, provisional, corrected |

## 7. Data model for withholding

| Field | Description |
|---|---|
| withholding_id | Unique withholding record |
| income_transaction_id | Linked income event |
| account_id | Account |
| beneficial_owner_id | Tax beneficial owner |
| source_country | Income source |
| tax_authority | Relevant authority |
| statutory_rate | Default rate |
| applied_rate | Actual rate applied |
| treaty_rate | Treaty rate if used |
| gross_amount | Before tax |
| withheld_amount | Deducted tax |
| net_amount | After tax |
| reclaimable_amount | Potential reclaim |
| documentation_id | Supporting form |
| calculation_version | Rule/calculation version |

## 8. Data model for CRS/FATCA reporting

| Field | Description |
|---|---|
| reporting_year | Calendar/reporting year |
| account_id | Reported account |
| report_type | CRS/FATCA |
| account_holder_id | Legal holder |
| controlling_person_id | If applicable |
| tax_residence_country | Reported jurisdiction |
| tin | Tax identification number |
| account_balance | Year-end balance/value |
| interest_amount | Gross interest |
| dividend_amount | Gross dividends |
| other_income_amount | Other income |
| gross_proceeds | Sale/redemption proceeds |
| closed_account_flag | Account closed during year |
| validation_status | Passed/failed |
| submission_status | Draft/submitted/accepted/rejected/corrected |

## 9. Effective dating

Tax-aware reporting requires effective dating for:

- tax residency;
- address;
- beneficial ownership;
- client classification;
- account ownership;
- product classification;
- tax documentation;
- treaty eligibility;
- reporting regime rules;
- withholding rates;
- corporate action terms;
- product domicile/status.

Never assume current client data applies to historical events. Historical reports must use the data effective on the event/reporting date.

## 10. Reporting mart design

Do not run client statements directly from raw transactions. Build curated reporting marts:

| Mart | Purpose |
|---|---|
| holdings_mart | Positions, quantities, cost, market values |
| income_mart | Gross/net income and withholding |
| realised_pnl_mart | Disposal and lot-level P&L |
| tax_lot_mart | Open/closed lot views |
| reportability_mart | CRS/FATCA/local tax reporting |
| statement_mart | Issued client statement lines |
| mandate_mart | DPM objectives/restrictions/compliance |
| performance_mart | TWR/MWR/contribution/attribution |

## 11. Audit and lineage

Every reported number should be traceable:

```text
Report line
  -> reporting mart record
  -> enriched transaction/income/tax lot
  -> source event/file/API
  -> product/client/master data version
  -> calculation rule version
```

Fields to store:

- source system;
- source file/message ID;
- ingestion timestamp;
- correlation ID;
- calculation version;
- report version;
- user/system override;
- approval workflow;
- exception status.

## 12. Configuration, not hardcoding

Tax rules should be implemented as configurable policy tables/rules:

| Rule type | Examples |
|---|---|
| income classification | Product/income code to tax category |
| withholding rate | Source country + beneficial-owner type + document status |
| treaty reduction | Residency + treaty + valid form |
| reportability | CRS/FATCA/local report flags |
| lot relief | FIFO/average/specific ID by jurisdiction/account |
| statement display | Gross/net policy and language |
| correction threshold | Materiality and notification rules |

## 13. Idempotency and corrections

Tax/reporting systems must handle repeated files and corrections.

Recommended controls:

- idempotency key on source event;
- versioned transaction correction;
- reversal plus replacement rather than mutation-only;
- statement versioning;
- year-end report corrections;
- audit trail for manual overrides;
- automated diff between original and corrected outputs.

## 14. Performance and analytics integration

Performance systems need tax-aware inputs but should not become tax engines.

Provide to performance engine:

- gross income amount;
- net income amount;
- withholding tax amount;
- external vs internal cashflow classification;
- fee/tax classification;
- valuation and cash timing;
- correction/restatement markers.

Performance policy then decides whether returns are gross-of-tax, net-of-tax or both.

