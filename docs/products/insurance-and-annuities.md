# Insurance And Annuities Knowledge Map

This page connects insurance, annuity, protection-linked, and investment-linked policy products to reusable wealth-platform design decisions.

Insurance and annuity products should be handled as contractual policy products with financial values, coverage benefits, actuarial assumptions, policy roles, cashflows, guarantees, and lifecycle events. They should not be collapsed into ordinary securities or fund positions.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Insurance, annuities, protection-linked and investment-linked policies | [`insurance-annuities/`](insurance-annuities/README.md) | Policy taxonomy, protection products, ILPs, annuities, retirement income, policy lifecycle, valuation, surrender, claims, beneficiaries, advisory, reporting, implementation, controls, and QA reference. |

## Platform Design Distinctions

| Question | Normal Security Or Fund | Insurance And Annuities |
|---|---|---|
| Primary position view | Units, shares, nominal, or contracts. | Policy contract with owner, insured/life assured, annuitant, beneficiary, payer, coverages, riders, values, and status. |
| Value basis | Market price, NAV, model value, or clean/dirty price. | Account value, cash value, surrender value, guaranteed value, projected value, death benefit, income base, or benefit base. |
| Cashflow focus | Trade cash, income, fees, redemptions, maturities. | Premiums, top-ups, withdrawals, charges, policy loans, loan interest, claims, surrenders, maturity benefits, annuity payouts. |
| Risk focus | Market, credit, rate, FX, liquidity, volatility, issuer. | Mortality/longevity, lapse, insurer credit, guarantee basis, surrender risk, liquidity, charges, premium sufficiency, investment risk for linked products. |
| Legal roles | Account owner and beneficial owner. | Policy owner, insured/life assured, annuitant, beneficiary, payer, assignee, trustee, advisor, insurer. |
| Reporting challenge | Show holdings, market value, return, income, risk. | Separate investable value from protection benefit, surrender access, projected benefit, estate role, premium obligation, and income stream. |

## Reusable Modelling Pattern

Use separate layers:

```text
Policy Contract Layer
  Policy number, insurer, product type, status, issue date, maturity, currency, jurisdiction

Role Layer
  Owner, insured/life assured, annuitant, beneficiary, payer, advisor, assignee, trustee

Coverage And Benefit Layer
  Death benefit, maturity benefit, rider benefits, income guarantees, waiver benefits

Investment And Value Layer
  Account value, cash value, surrender value, guaranteed value, projected value, ILP sub-funds

Cashflow Layer
  Premiums, top-ups, charges, withdrawals, policy loans, claims, maturity proceeds, annuity payouts

Lifecycle Layer
  Application, underwriting, issue, lapse, reinstatement, beneficiary change, surrender, claim, maturity

Control Layer
  Source date, value basis, guarantee basis, disclosure state, reconciliation, privacy, audit history
```

Do not report death benefit, income base, account value, and surrender value as interchangeable wealth values. Each has a different meaning and use.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Policy identity and status | Insurer, policy administration system, custodian statement, product master, operations workflow. |
| Policy roles and beneficiaries | Insurer policy record, signed forms, trustee/estate records, client onboarding, restricted operations record. |
| Coverage and rider terms | Policy schedule, insurer illustration, product master, policy document, underwriting decision. |
| Premium schedule and payment status | Insurer statement, billing system, cash ledger, standing instruction, operations workflow. |
| Account value and sub-fund holdings | Insurer, ILP platform, fund administrator, fund NAV source, custodian statement. |
| Cash surrender value | Insurer quote, policy administration system, surrender quotation, operations workflow. |
| Policy loans and assignments | Insurer, lending system, collateral system, legal pledge/assignment document, credit workflow. |
| Claims, maturity and annuity payouts | Insurer notice, claim workflow, cash ledger, benefit payment statement. |
| Projections and guarantees | Insurer illustration, product terms, actuarial model, reporting policy, manually reviewed assumptions. |

Sensitive health, underwriting, beneficiary, and estate-planning data should have explicit access controls and should not be treated as ordinary portfolio metadata.

## API And UI Implications

APIs should expose:

1. policy contract, coverage, rider, role, value, and cashflow records separately,
2. value basis, source date, guarantee status, and projection assumptions,
3. account value, cash value, surrender value, death benefit, income base, and guaranteed value as separate fields,
4. ILP sub-fund holdings and look-through exposure only when source-backed,
5. premium schedule, missed-premium state, lapse risk, and future obligation,
6. policy loan balance, loan interest, assignment, collateral and benefit reduction states,
7. claim, surrender, maturity, annuitization, and reinstatement lifecycle states,
8. privacy flags for beneficiary, health, claim, and estate-sensitive information.

UIs should make these states visible:

1. policy value is estimated, projected, guaranteed, non-guaranteed, or insurer-quoted,
2. surrender value differs from account or cash value,
3. death benefit is not current liquid wealth,
4. premium payments are due, missed, suspended, or insufficient,
5. policy loan may reduce benefits or create lapse risk,
6. annuity payout is an income stream, not always a liquid asset,
7. ILP exposure is investment-linked and may require fund look-through,
8. beneficiary or claim details are restricted.

## QA Scenarios

High-value scenarios:

1. term policy created with death benefit but no investable value,
2. whole-life policy reports cash value, surrender value, guaranteed value, and death benefit separately,
3. ILP premium buys sub-fund units and charges are posted without overstating investment return,
4. surrender quote reduces accessible value after charges and policy loan,
5. missed premium moves policy toward grace-period or lapse state,
6. policy loan accrues interest and reduces net policy value,
7. beneficiary change updates role history without changing financial value,
8. claim closes protection coverage and books cash only when benefit payment is confirmed,
9. immediate annuity converts capital into payout schedule and restricts liquidity,
10. projected values are labelled and never treated as guaranteed unless the source explicitly guarantees them.

## Useful Project Workflows

Use this pack when working on:

1. policy-contract and coverage data modelling,
2. insurance and annuity client reporting,
3. ILP sub-fund exposure and performance treatment,
4. retirement-income and annuity cashflow projections,
5. protection, estate, beneficiary, and claim workflows,
6. policy loan and collateral assignment modelling,
7. suitability, liquidity, concentration, and complexity checks,
8. insurer statement ingestion and reconciliation,
9. privacy and access controls for sensitive policy data,
10. migration from policy statements, insurer portals, spreadsheets, or advisor records.

## Related Packs

| Pack | Relationship |
|---|---|
| [`funds/`](funds/README.md) | Useful for ILP sub-funds, NAV, fees, look-through exposure, and fund performance treatment. |
| [`pooled-investment-products.md`](pooled-investment-products.md) | Useful for fund-wrapper comparisons and look-through decisions. |
| [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | Useful for policy loans, premium financing, collateral assignment, lending value, and pledge states. |
| [`structured-products/`](structured-products/README.md) | Useful for guarantees, participation, caps/floors, indexed annuities, and payoff explanation patterns. |
| [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Useful for premiums, payouts, policy cashflows, liquidity reserves, and settlement currency treatment. |
| [`private-markets/`](private-markets/README.md) | Useful for illiquidity, long-horizon planning, document-driven values, and suitability framing. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, insurance, legal, tax, actuarial, regulatory, estate-planning, or client advice.
