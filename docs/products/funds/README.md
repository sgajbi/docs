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

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

---

## Files

| File | Purpose |
|---|---|
| `01-funds-fundamentals-and-product-taxonomy.md` | Explains what funds are, key fund structures, product types, share classes, open-ended vs closed-ended funds, mutual funds, unit trusts, ETFs, money market funds, hedge funds and private market funds. |
| `02-fund-lifecycle-transactions-and-position-modelling.md` | Explains fund lifecycle, order flow, subscriptions, redemptions, switches, transfers, distributions, reinvestment, corporate actions, transaction types and position modelling. |
| `03-fund-data-model-nav-valuation-fees-and-risk.md` | Explains common data model, NAV, bid/offer price, clean modelling of fees, valuation sources, income handling, look-through exposure, risk, performance and analytics. |
| `04-platform-implementation-advisory-and-practitioner-reference.md` | Explains platform implementation patterns, controls, edge cases, suitability/advisory considerations, QA scenarios, practitioner explanations and glossary. |
| `05-worked-examples-and-implementation-patterns.md` | Provides practical examples for subscriptions, cut-offs, NAV confirmation, redemptions, gates, distributions, share classes, ETFs, look-through, private funds, NAV restatements, equalization, conversions, mergers, re-registration, suspensions, eligibility changes, tax-status changes, rebates, levy disputes, side-pocket exits, closure distributions, master-feeder restructures, liquidity stress, recallable distributions, ETF market-maker stress, cross-border share-class restrictions, administrator restatements, tax-lot migration, liquidity facilities, redemption-in-kind stress, stale private-fund statements, manager replacement, side-pocket write-offs, ETF index rebalances, distribution waterfalls, series accounting, retirement-plan eligibility, expense-cap reimbursements, ETF heartbeat trades, liquidation tax reporting, share-class closure migration, clean-share conversion, private-fund clawback escrow, ETF custom basket rejection, currency-hedged class breaks, fund board suspension votes, private-fund continuation vehicles, ETF authorized-participant concentration stress, fund tax-character restatements, UCITS concentration breaches, private-credit PIK income, fund distribution clawback notices, semi-liquid queue switching, ETF collateral basket failures, valuation committee overrides, class hedging cost allocation, side-letter notice conflicts, fee rebate clawbacks, fund liquidity notice blackouts, subscription prefunding breaks, ETF share split processing, feeder tax blocker changes, NAV strike operational incidents, investor-level MFN election controls, redemption-fee waivers, private-fund key-person events, ETF index methodology changes, fund audit qualifications, side-pocket secondary transfers, subscription equalization true-ups, expense-ratio cap expiries, suspended dealing side-letter overrides, private-fund GP-led tender elections, ETF securities-lending revenue allocation, feeder-master NAV mismatch repairs, distribution currency-election breaks, performance-fee equalization reopeners, ETF in-kind redemption cash-substitute fees, private-fund capital-call default remedies, fund administrator transition breaks, liquidation gate waterfalls, ESG exclusion reclassification events, swing-pricing threshold disputes, fund-of-funds stale underlying NAVs, ETF authorized-participant failed settlements, private-fund side-letter fee cap expiries, semi-liquid redemption queue proration, fund benchmark reclassification events, fund proxy-voting restrictions, ETF trading halt fair-value overrides, private-fund NAV side-letter reporting tiers, fund expense accrual cut-off disputes, UCITS eligible-asset breach cures, feeder liquidity mismatch stress, fund proxy-voting split ballots, ETF stale intraday indicative value publication, private-fund fee letter amendment disputes, expense reimbursement clawbacks, UCITS passive breach from market movement, feeder emergency liquidity facility drawdowns, fund vote instruction deadline misses, ETF fair-value basket proxy gaps, private-fund most-favoured-nation fee elections, expense-cap board renewal delays, UCITS breach cure sequencing, feeder facility repayment waterfalls, fund vote exception acceptance disputes, ETF basket proxy expiry renewals, MFN election retroactive fee adjustments, expense-cap retroactive approval restatements, UCITS cure trade capacity shortfalls, feeder facility interest allocation disputes, fund vote exception acceptance reversal disputes, ETF proxy renewal expiry breaches, MFN retroactive adjustment clawbacks, expense-cap restatement investor notices, UCITS residual breach extension approvals, feeder facility interest reallocation reversals and QA. |

---

## Recommended Learning and Project Sequence

1. Start with `01-funds-fundamentals-and-product-taxonomy.md`.
2. Then study `02-fund-lifecycle-transactions-and-position-modelling.md` to understand how fund events become system transactions and positions.
3. Then study `03-fund-data-model-nav-valuation-fees-and-risk.md` for platform modelling, valuation, NAV and performance.
4. Use `04-platform-implementation-advisory-and-practitioner-reference.md` for project application, advisory framing, stakeholder explanation and implementation review.
5. Use `05-worked-examples-and-implementation-patterns.md` for practical calculations, lifecycle scenarios, reporting behavior and QA assertions.

---

## High-Level Mental Model

A fund is a pooled investment vehicle.

Investors do not directly own every underlying security held by the fund. Instead, they own **units or shares of the fund**, and the fund itself owns a portfolio of assets.

```text
Investor cash
    ->
Fund subscription
    ->
Investor receives fund units/shares
    ->
Fund manager invests pooled money into portfolio assets
    ->
Investor return comes from NAV movement and distributions
```

For platform modelling:

```text
Accounting position = fund units/shares held by the client
Look-through exposure = underlying assets held by the fund, if available
Valuation = units x NAV/price
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

- FINRA: Mutual Funds - NAV, share classes, fund expenses and mutual-fund basics.
- Investor.gov / SEC: Mutual funds, ETFs, closed-end funds, expense ratios and investor bulletins.
- MoneySense Singapore: Unit trusts, pricing and fees, total expense ratio, and Singapore investor education.
- MAS: Offers of Collective Investment Schemes and Singapore CIS framework.

The pack is written as a professional learning and system-design reference, so it deliberately translates these concepts into transaction, position, valuation and platform architecture language.
