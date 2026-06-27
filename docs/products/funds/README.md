# Funds Reference Pack

## Purpose

This documentation pack explains **funds** from a wealth-management, advisory, product-control, portfolio-analytics and platform-implementation perspective.

It is designed for:

- Professional learning and practitioner refresh
- Office knowledge sharing
- Business analysis and architecture discussions
- Portfolio and wealth platform design
- Product master, transaction, position, valuation and performance modelling
- Suitability, advisory and reporting conversations

The pack focuses on practical financial-product understanding, not legal advice, tax advice or investment recommendation.

**Source provenance:** Imported from `C:\Users\Sandeep\Downloads\funds_reference_pack.zip\funds_reference_pack` into `docs/products/funds/` on 2026-06-27.

---

## Files

| File | Purpose |
|---|---|
| `01-funds-fundamentals-and-product-taxonomy.md` | Explains what funds are, key fund structures, product types, share classes, open-ended vs closed-ended funds, mutual funds, unit trusts, ETFs, money market funds, hedge funds and private market funds. |
| `02-fund-lifecycle-transactions-and-position-modelling.md` | Explains fund lifecycle, order flow, subscriptions, redemptions, switches, transfers, distributions, reinvestment, corporate actions, transaction types and position modelling. |
| `03-fund-data-model-nav-valuation-fees-and-risk.md` | Explains common data model, NAV, bid/offer price, clean modelling of fees, valuation sources, income handling, look-through exposure, risk, performance and analytics. |
| `04-platform-implementation-advisory-and-practitioner-reference.md` | Explains platform implementation patterns, controls, edge cases, suitability/advisory considerations, QA scenarios, practitioner explanations and glossary. |

---

## Recommended Learning and Project Sequence

1. Start with `01-funds-fundamentals-and-product-taxonomy.md`.
2. Then study `02-fund-lifecycle-transactions-and-position-modelling.md` to understand how fund events become system transactions and positions.
3. Then study `03-fund-data-model-nav-valuation-fees-and-risk.md` for platform modelling, valuation, NAV and performance.
4. Finally use `04-platform-implementation-advisory-and-practitioner-reference.md` for project application, advisory framing, stakeholder explanation and implementation review.

---

## High-Level Mental Model

A fund is a pooled investment vehicle.

Investors do not directly own every underlying security held by the fund. Instead, they own **units or shares of the fund**, and the fund itself owns a portfolio of assets.

```text
Investor cash
    ↓
Fund subscription
    ↓
Investor receives fund units/shares
    ↓
Fund manager invests pooled money into portfolio assets
    ↓
Investor return comes from NAV movement and distributions
```

For platform modelling:

```text
Accounting position = fund units/shares held by the client
Look-through exposure = underlying assets held by the fund, if available
Valuation = units × NAV/price
Income = distributions, dividends, interest, capital gains, return of capital
Lifecycle = subscription, redemption, switch, transfer, distribution, reinvestment, split, merger, liquidation
```

---

## Key Design Principle

Do not model every underlying holding of a fund as a client accounting position unless the client legally owns those securities directly.

The client position should usually be:

```text
Client owns Fund ABC units
```

The platform may separately calculate:

```text
Client has look-through exposure to equities, bonds, sectors, countries and currencies through Fund ABC
```

Accounting and reporting must keep these separate.

---

## Core References

This pack uses the following investor-education and regulatory references as grounding sources:

- FINRA: Mutual Funds — NAV, share classes, fund expenses and mutual-fund basics.
- Investor.gov / SEC: Mutual funds, ETFs, closed-end funds, expense ratios and investor bulletins.
- MoneySense Singapore: Unit trusts, pricing and fees, total expense ratio, and Singapore investor education.
- MAS: Offers of Collective Investment Schemes and Singapore CIS framework.

The pack is written as a professional learning and system-design reference, so it deliberately translates these concepts into transaction, position, valuation and platform architecture language.
