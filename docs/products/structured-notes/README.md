# Structured Notes Reference Pack

**Purpose:** A professional study and project reference for understanding, explaining, modelling, valuing, and processing notes in a wealth-management / private-banking platform.

**Audience:** Product owners, business analysts, architects, engineers, QA, operations, advisors, and anyone who needs to understand notes beyond surface-level product descriptions.

**Source provenance:** Imported from `C:\Users\Sandeep\Downloads\notes_reference_pack` into `docs/products/structured-notes/` on 2026-06-27.

**Last updated:** 2026-06-27

---

## Reading Order

| File | Purpose |
|---|---|
| `01-notes-fundamentals-and-product-taxonomy.md` | Explains what notes are, how they differ from bonds/deposits, why clients buy them, key risks, and major product variants. |
| `02-note-lifecycle-transactions-and-position-modelling.md` | Explains lifecycle events, transaction types, position modelling, physical settlement, maturity, autocall, credit events, and performance treatment. |
| `03-note-data-model-valuation-and-risk.md` | Gives a common data model for notes, nullable/extension attributes, valuation hierarchy, pricing inputs, clean/dirty price handling, and look-through risk. |
| `04-platform-implementation-advisory-and-interview-reference.md` | Provides implementation checklists, advisory/suitability views, reporting requirements, operational controls, test cases, glossary, and interview-ready explanations. |

---

## Core Mental Model

A note is a **debt instrument** issued by a bank, company, or financial institution. The investor lends money to the issuer and receives a promised payoff according to the note terms.

In private banking, the word **note** often means a **structured note**.

A structured note usually combines:

| Component | Meaning |
|---|---|
| Debt instrument | The issuer owes money to the investor. |
| Embedded derivative | Determines coupon, upside, downside, barrier, autocall, conversion, credit event, FX outcome, etc. |
| Issuer credit exposure | Investor depends on the issuer's ability to pay. |
| Lifecycle rules | Observation dates, coupon dates, barrier checks, call events, maturity rules, settlement terms. |

The client owns **one note position**. The embedded option, CDS-like exposure, FX option, barrier, or autocall feature should generally be modelled as **contract terms**, not as separate client positions. A new client position is created only when something is legally delivered, such as shares delivered after physical settlement.

---

## Scope Covered

This pack covers:

- Plain vanilla notes / medium-term notes
- Principal-protected notes
- Equity-linked notes
- Fixed coupon notes / reverse convertibles
- Worst-of notes
- Autocallable / Phoenix notes
- Dual currency notes / dual currency investments
- Credit-linked notes
- Interest-rate-linked notes / floating-rate notes / range accrual notes
- Exchange-traded notes
- Common product model
- Lifecycle and transaction model
- Position model
- Valuation approach
- Listing and liquidity treatment
- Performance treatment
- Advisory and suitability considerations
- Platform implementation controls
- QA and regression test scenarios

---

## Design Principle for Wealth Platforms

Do **not** model every note subtype as a completely separate security and accounting model.

Use:

```text
Instrument
  └── NoteContract
        ├── Underlyings
        ├── Coupon schedules
        ├── Observation schedules
        ├── Barrier terms
        ├── Settlement terms
        ├── Optional subtype extensions
        ├── Lifecycle events
        └── Valuation records
```

Accounting transactions should remain compact and economic:

```text
Subscription / Buy
Coupon payment
Sale
Autocall redemption
Maturity redemption
Physical delivery
Credit event write-down
Recovery payment
Default write-off
Fees / taxes / corrections
```

Observation checks, barrier touches, coupon determinations, and autocall triggers are **lifecycle events first**. They become transactions only when there is an actual economic posting.

---

## External Reference Sources

These sources were used as external validation and investor-education references:

- FINRA — Understanding Structured Notes With Principal Protection: https://www.finra.org/investors/insights/structured-notes-principal-protection
- Investor.gov — Investor Bulletin: Structured Notes: https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins-76
- Investor.gov — Structured Notes With Principal Protection: https://www.investor.gov/introduction-investing/investing-basics/investment-products/structured-notes-principal-protection
- MoneySense Singapore — Understanding Structured Notes: https://www.moneysense.gov.sg/understanding-structured-notes/
- MoneySense Singapore — Understanding Specified Investment Products: https://www.moneysense.gov.sg/understanding-specified-investment-products/
- DBS Treasures — Structured Investments: https://www.dbs.com.sg/treasures/investments/product-suite/structured-investments
- Standard Chartered Singapore — Fixed Coupon Notes: https://www.sc.com/sg/wealth/investment/fixed-coupon-notes/

---

## Important Disclaimer

This pack is for education, architecture, business analysis, and platform design. It is not investment advice, legal advice, tax advice, or regulatory advice. Real product treatment depends on jurisdiction, offering documents, custodian accounting, issuer confirmation, tax rules, suitability policy, and the bank's internal product governance.
