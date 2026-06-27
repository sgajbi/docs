# Insurance, Annuities and Protection-Linked Investment Products Reference Pack

## Purpose

This pack is part of a high-quality wealth management product knowledge base. It is designed for future study, office knowledge sharing, advisory conversations, DPM / mandate design, analytics, reporting and platform implementation.

Insurance and annuity products are different from normal securities. They combine legal contract rights, protection benefits, actuarial risk pooling, investment-linked assets, premium schedules, surrender rights, charges, riders, guarantees, claims and sometimes estate-planning objectives. A platform must therefore treat them as **policy contracts with financial values**, not merely as fund positions or bonds.

## Pack structure

| File | Focus |
|---|---|
| `01-insurance-annuities-fundamentals-and-taxonomy.md` | Product families, roles, core concepts and taxonomy |
| `02-life-insurance-ilp-and-protection-product-deep-dive.md` | Term life, whole life, endowment, universal life, ILP, riders and product combinations |
| `03-annuities-retirement-income-and-longevity-products.md` | Immediate/deferred annuities, fixed/variable/indexed annuities, payout options and income modelling |
| `04-lifecycle-transactions-position-policy-and-cashflow-modelling.md` | Policy lifecycle, transaction types, events, positions, premiums, claims, surrender and maturity |
| `05-data-model-policy-contract-coverage-beneficiary-funds-and-ledger-design.md` | Common data model for policies, coverages, riders, funds, charges, valuations and roles |
| `06-valuation-performance-risk-liquidity-and-capital-treatment.md` | Cash value, surrender value, account value, policy loans, guarantees, risk and analytics treatment |
| `07-advisory-mandate-suitability-estate-and-reporting-deep-dive.md` | Advisory, DPM, suitability, estate planning, client reporting and portfolio context |
| `08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md` | Implementation guidance, controls, reconciliation, QA scenarios and glossary |
| `09-practitioner-reference-review-questions-and-product-combinations.md` | Practitioner explanations, product combinations, review questions and reusable mental models |
| `10-source-notes-and-further-reading.md` | Source notes and useful references |
| `11-worked-examples-and-implementation-patterns.md` | Practical examples for ILP premiums and charges, policy loans, surrender, lapse, claims, annuity payouts, reporting, controls and QA |

## How to use this pack

1. Read files 01 to 03 for product understanding.
2. Read files 04 to 06 for platform, lifecycle, valuation, transaction and analytics modelling.
3. Read files 07 to 09 for advisory, mandate, reporting, implementation and practitioner review readiness.
4. Use file 10 as a source trail and starting point for deeper reading.

## Core modelling principle

A policy should be modelled as a **contractual asset or liability with one or more coverage and value components**.

```text
Policy Contract
  |-- Policy roles: owner, insured/life assured, annuitant, beneficiary, payer, advisor
  |-- Coverage components: death benefit, maturity benefit, rider benefits, income guarantees
  |-- Investment components: cash value, account value, ILP sub-funds, bonus/dividend account
  |-- Cashflow schedules: premiums, charges, top-ups, withdrawals, annuity payouts
  |-- Lifecycle events: issue, underwriting, lapse, reinstatement, claim, surrender, maturity
  `-- Valuation records: surrender value, account value, guaranteed value, projected value
```

Do not model every policy as a normal traded security. Some policies behave like protection contracts, some like long-term savings plans, some like fund wrappers, some like retirement-income contracts and some like estate-planning structures.

## Key platform warning

For reporting and analytics, avoid mixing these values without clear labels:

| Value | Meaning |
|---|---|
| Premium paid | Historical money paid by client |
| Account value | Gross investment-linked value before surrender effects |
| Cash value | Accumulated value inside policy, depending on product type |
| Cash surrender value | Amount accessible on surrender after surrender charges, loans and adjustments |
| Sum assured / death benefit | Benefit payable on insured event, not necessarily current wealth value |
| Guaranteed value | Contractually guaranteed amount under stated conditions |
| Non-guaranteed value | Projected or discretionary value such as bonuses/dividends |

These are not interchangeable.

## Source provenance

Imported from C:\Users\Sandeep\Downloads\insurance_annuities_reference_pack.zip\insurance_annuities_reference_pack on 2026-06-27. The imported material was normalized for repository naming and reusable practitioner framing.
