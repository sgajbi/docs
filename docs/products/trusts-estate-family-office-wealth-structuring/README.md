# Trusts, Estate Planning, Family Office and Wealth Structuring Reference Pack

**Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.

**Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

## 1. Why this pack exists

This pack covers the wealth-structuring layer that sits above financial products. Earlier product packs explain the assets themselves: notes, bonds, funds, equities, derivatives, structured products, private markets, cash/FX, loans/collateral, insurance and real assets. This pack explains **who owns those assets, who benefits from them, who controls them, how they are reported, and how wealth is transferred or governed across generations**.

In private banking and wealth platforms, this layer is critical because a portfolio may not belong directly to an individual. It may be owned through a trust, foundation, holding company, family office vehicle, VCC sub-fund, insurance wrapper, nominee, joint account or estate account. The same product holding can therefore have different legal owner, beneficial owner, settlor, controller, advisor, mandate, restrictions, tax profile, reporting group and suitability treatment.

## 2. Pack structure

| File | Focus |
|---|---|
| `01-wealth-structuring-fundamentals-and-taxonomy.md` | Core concepts, taxonomy, why structures exist, wealth platform implications |
| `02-trusts-foundations-and-fiduciary-structures.md` | Trusts, settlors, trustees, protectors, beneficiaries, fiduciary duties, trust lifecycle |
| `03-estate-planning-wills-probate-lpa-and-beneficiary-designations.md` | Wills, probate, intestacy, estate accounts, LPAs, beneficiary designations, succession events |
| `04-family-office-holding-vehicles-vcc-and-ownership-structures.md` | SFO, MFO, holding companies, investment companies, VCC, family governance, operating model |
| `05-lifecycle-transactions-ownership-position-and-reporting-modelling.md` | Onboarding, funding, ownership transfers, trust distributions, estate events, reporting hierarchy |
| `06-data-model-entity-beneficiary-mandate-and-access-control-design.md` | Canonical data model for structures, parties, relationships, mandates, entitlement and access |
| `07-advisory-mandate-suitability-governance-tax-and-client-experience.md` | Advisory questions, DPM mandate implications, governance controls, tax/estate awareness |
| `08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md` | Platform controls, reconciliation, QA scenarios, common breaks, glossary |
| `09-source-notes-and-further-reading.md` | Source notes and reference links |
| `10-worked-examples-and-implementation-patterns.md` | Worked examples for trust distributions, estate account restrictions, holding-company pledges, beneficiary reporting access, family-office consolidated reports, insurance policy roles, authority changes, support boundaries and regression tests |

## 3. Core mental model

Financial product packs answer:

```text
What asset does the client hold, how does it behave, and how is it valued?
```

This pack answers:

```text
Who legally owns the asset?
Who economically benefits?
Who controls decisions?
Who receives reporting?
Who is allowed to instruct?
What restrictions apply?
What happens on incapacity, death, succession, distribution or restructuring?
```

## 4. Key design principle

Do not treat a trust, family office or estate structure as a simple account label. Treat it as a **relationship graph** connecting legal entities, people, accounts, portfolios, mandates, powers, beneficiaries, restrictions and reporting groups.

Recommended conceptual model:

```text
Party / Entity
  ├── Relationship Role: settlor, trustee, beneficiary, protector, director, donor, donee, executor, advisor
  ├── Legal Structure: trust, estate, company, VCC, foundation, insurance wrapper, family office
  ├── Account / Portfolio: custody, cash, loan, DPM, advisory, execution-only
  ├── Mandate: discretionary, advisory, restricted, investment objective, risk profile
  ├── Entitlement: beneficial interest, income entitlement, capital entitlement, reporting access
  └── Controls: authority, limits, distribution rules, tax flags, KYC/AML, suitability, conflict checks
```

## 5. How this pack should be used

Use it to:

- study HNW/UHNW wealth structuring concepts;
- improve portfolio platform design for complex ownership structures;
- support advisory, mandate and reporting discussions;
- design canonical party/account/portfolio/beneficiary models;
- build reusable practitioner fluency in private banking, family office and UHNW platforms;
- guide QA scenarios for trust, estate, beneficiary and reporting edge cases.
