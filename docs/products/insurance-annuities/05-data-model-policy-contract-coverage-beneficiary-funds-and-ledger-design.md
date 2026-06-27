# 05 - Data Model: Policy Contract, Coverage, Beneficiary, Funds and Ledger Design

## 1. Why a dedicated model is needed

Insurance products have more parties, lifecycle states and value concepts than normal instruments. A platform that models them only as securities will miss beneficiaries, insured lives, riders, surrender charges, policy loans, guaranteed/non-guaranteed components and claims.

## 2. Canonical object model

```text
InsuranceProduct
  `-- PolicyContract
        |-- PolicyRole[]
        |-- CoverageComponent[]
        |-- Rider[]
        |-- BeneficiaryDesignation[]
        |-- PremiumSchedule
        |-- PolicyValue[]
        |-- PolicyCharge[]
        |-- PolicyLoan[]
        |-- InvestmentAccount?      # ILP / variable policy only
        |     `-- FundHolding[]
        |-- AnnuityPayoutTerms?     # annuity only
        |-- Assignment[]            # collateral/trust/lender assignment
        |-- LifecycleEvent[]
        `-- Transaction[]
```

## 3. Product master table

| Field | Description |
|---|---|
| product_id | Product identifier |
| insurer_id | Insurance company |
| product_family | LIFE / ANNUITY / ILP / RIDER / HEALTH / HYBRID |
| product_type | Term, whole life, endowment, UL, ILP, variable annuity, etc. |
| jurisdiction | Issuing jurisdiction |
| currency | Policy currency |
| premium_mode_allowed | Single, regular, flexible |
| has_cash_value | True/false |
| has_investment_account | True/false |
| has_guarantees | True/false |
| has_bonus_or_dividend | True/false |
| supports_policy_loan | True/false |
| supports_partial_withdrawal | True/false |
| supports_fund_switch | True/false |
| surrender_charge_schedule_id | Optional |
| complexity_classification | Simple / complex / investment-linked / SIP-like |
| disclosure_document_refs | PHS, product summary, policy contract, illustration |

## 4. Policy contract table

| Field | Description |
|---|---|
| policy_id | Unique policy ID |
| product_id | Product reference |
| policy_number | Insurer policy number |
| account_id | Client portfolio/account |
| owner_party_id | Owner |
| insurer_id | Insurer |
| issue_date | Policy start |
| effective_date | Coverage effective date |
| maturity_date | Maturity date if applicable |
| expiry_date | Coverage end date if applicable |
| policy_status | Active/lapsed/surrendered/matured/etc. |
| policy_currency | Contract currency |
| premium_amount | Standard premium |
| premium_frequency | Annual/monthly/single/etc. |
| next_premium_due_date | Next due date |
| payment_method | Cash/account/debit/standing order |
| free_look_end_date | Cooling-off end if available |
| reporting_value_basis | Account value/surrender value/benefit value |
| source_system | Insurer, custodian, manual, aggregator |
| last_statement_date | Last insurer statement |

## 5. Policy role table

| Field | Description |
|---|---|
| policy_id | Parent policy |
| party_id | Person/entity |
| role_type | OWNER / LIFE_ASSURED / INSURED / ANNUITANT / BENEFICIARY / PAYER / TRUSTEE / ASSIGNEE |
| role_start_date | Effective from |
| role_end_date | Effective to |
| share_pct | For beneficiaries or ownership shares |
| irrevocable_flag | For beneficiary/assignment where relevant |
| relationship | Spouse, child, trust, company, etc. |

## 6. Coverage component table

| Field | Description |
|---|---|
| coverage_id | Coverage identifier |
| policy_id | Parent policy |
| coverage_type | Death, critical illness, disability, waiver, accident, income, maturity |
| covered_life_id | Insured life |
| sum_assured | Coverage amount |
| benefit_formula | Formula if not fixed |
| coverage_start_date | Start |
| coverage_end_date | End |
| premium_component | Premium attributable to coverage if available |
| claim_status | None/pending/paid/declined |
| exclusions_summary | Optional text/code |

## 7. Beneficiary designation table

| Field | Description |
|---|---|
| policy_id | Parent policy |
| beneficiary_party_id | Beneficiary |
| beneficiary_type | Primary/contingent |
| share_pct | Share of proceeds |
| effective_date | Effective date |
| revocable_flag | Whether owner can change without consent |
| trust_nomination_flag | Jurisdiction-specific |
| reporting_visibility | Whether shown in client/advisor report |

## 8. Premium schedule table

| Field | Description |
|---|---|
| policy_id | Parent policy |
| due_date | Premium due date |
| premium_amount | Gross premium |
| premium_currency | Currency |
| payment_status | Due/paid/overdue/waived/skipped |
| grace_period_end_date | If overdue |
| waiver_reason | If waived |
| payment_transaction_id | Linked transaction |

## 9. Policy value table

Store values with explicit type. Never overwrite or mix them.

| Field | Description |
|---|---|
| policy_id | Parent policy |
| valuation_date | Date |
| value_type | ACCOUNT_VALUE / CASH_VALUE / SURRENDER_VALUE / GUARANTEED_VALUE / DEATH_BENEFIT / BENEFIT_BASE / POLICY_LOAN_BALANCE |
| gross_amount | Gross value |
| adjustment_amount | Charges/loans/discounts if applicable |
| net_amount | Net value |
| currency | Currency |
| source | Insurer statement / estimated / model / advisor input |
| confidence_level | Confirmed/indicative/projected |
| guaranteed_flag | True/false |

## 10. ILP / variable investment account tables

### Investment account

| Field | Description |
|---|---|
| policy_id | Parent policy |
| investment_account_id | Internal account |
| valuation_currency | Base currency |
| risk_profile | Policy/investment risk profile |
| allocation_model | Static / advisor-selected / client-selected / mandate-linked |

### Fund holding

| Field | Description |
|---|---|
| investment_account_id | Parent account |
| fund_id | ILP sub-fund |
| units | Units held |
| nav_per_unit | NAV/unit price |
| nav_date | NAV date |
| market_value | Units x NAV |
| allocation_pct | Allocation weight |
| cost_basis | Optional, depending statement availability |
| fund_currency | Fund currency |

## 11. Charge table

| Charge type | Examples |
|---|---|
| Premium allocation charge | Deducted before investment allocation |
| Cost of insurance | Mortality/protection charge |
| Policy administration charge | Fixed/percentage charge |
| Rider charge | Rider cost |
| Fund management charge | Usually embedded in NAV for funds |
| Mortality and expense charge | Common in variable annuity style products |
| Surrender charge | Early withdrawal/termination |
| Switching charge | Fund switch fee |
| Loan interest | Policy loan interest |

## 12. Assignment and collateral table

Insurance can be assigned to a lender or trust.

| Field | Description |
|---|---|
| assignment_id | Assignment record |
| policy_id | Parent policy |
| assignee_party_id | Lender/trust/beneficiary |
| assignment_type | Collateral / absolute / trust / legal |
| effective_date | Start |
| release_date | End |
| secured_amount | Loan exposure secured, if known |
| priority_rank | Priority of assignment |
| documentation_status | Pending/active/released |

## 13. Ledger design

For high-quality implementation, maintain both:

1. **Policy event ledger** - lifecycle state changes.
2. **Financial transaction ledger** - cash/value/unit movements.
3. **Valuation ledger** - periodic policy value snapshots.
4. **Exposure ledger** - investment-linked look-through and risk exposures.

This separation prevents lifecycle events such as claim submission or grace-period start from being confused with actual cash postings.
