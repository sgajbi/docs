# Product Taxonomy And Vocabulary Guide

This guide defines reusable vocabulary for product documentation, platform design, data intake, APIs, UI labels, analytics, reporting, and QA.

Use it when adding a new product pack, normalizing source data, designing product screens, reviewing calculations, or deciding whether a feature is genuinely supported by available data.

## Core Principle

Do not use product family, legal wrapper, economic exposure, transaction type, lifecycle event, valuation basis, liquidity status, and supportability state interchangeably.

A strong product model can answer all of these separately:

1. What is the broad product family?
2. What is the product subtype?
3. What is the legal or operational wrapper?
4. What is the client legally holding or owing?
5. What economic exposure does it create?
6. What cashflows, events, and obligations can occur?
7. How is it valued?
8. How liquid is it?
9. What data is source-backed, derived, estimated, stale, or unsupported?
10. What should be shown in client reporting, advisory, risk, and operations views?

## Taxonomy Layers

| Layer | Meaning | Example | Common Mistake |
|---|---|---|---|
| Product family | Broad library grouping used for navigation and governance. | Bonds, funds, derivatives, insurance, real estate. | Treating family as the complete product definition. |
| Product subtype | Business subtype within a family. | Floating-rate note, equity autocall, open-ended mutual fund, Lombard facility. | Using vague labels such as "investment" or "alternative" when a precise subtype exists. |
| Wrapper or legal form | Legal or operational container. | Note, deposit, fund unit, policy, swap contract, physical holding, loan facility. | Treating wrapper behavior as the same as underlying exposure. |
| Legal holding or obligation | What the client owns, owes, or has contracted. | 1,000 fund units, USD 1 million bond nominal, policy contract, loan drawdown. | Reporting notional, commitment, or exposure as if it were the legal position. |
| Economic exposure | Market, credit, rate, FX, commodity, property, or other sensitivity. | Equity index exposure through a note; gold exposure through an ETF; property exposure through a REIT. | Ignoring look-through or embedded exposure because the wrapper is not direct. |
| Transaction type | Economic posting that changes cash, position, cost, income, fee, tax, or obligation. | Buy, sell, coupon, dividend, premium, drawdown, capital call, redemption. | Calling every product event a transaction before it posts economically. |
| Lifecycle event | Product state event that may or may not create a transaction. | Barrier observation, autocall test, corporate action election, margin call, policy lapse notice. | Missing important events because no cash posted yet. |
| Valuation basis | How market value or account value is measured. | Exchange price, NAV, clean price plus accrued, model value, surrender value, appraisal. | Mixing incompatible value bases in the same report total without labels. |
| Liquidity basis | How fast and under what constraints value can be realized. | T+2 listed sale, monthly dealing, gated fund, locked-up private fund, pledged collateral. | Treating market value as immediately available cash. |
| Risk classification | Primary and secondary risks for suitability, concentration, stress, and controls. | Issuer credit, counterparty, rates, FX, leverage, liquidity, concentration. | Using asset class alone as the risk classification. |
| Reporting classification | How the item should appear in client, management, and regulatory-style reports. | Income asset, liability, alternative investment, derivative overlay, pledged collateral. | Using system category labels that do not match business meaning. |
| Supportability state | Whether data and calculation claims are source-backed. | Supported, source-backed extension, partial, estimated, stale, blocked, unsupported. | Showing unsupported analytics as successful results. |

## Product Family Classification Matrix

| Product Family | Typical Subtypes | Legal Holding Or Obligation | Primary Exposure Lens | Key Reporting Distinction |
|---|---|---|---|---|
| Bonds | Government, corporate, financial, high yield, convertible, callable, floating-rate, inflation-linked. | Nominal debt claim against issuer. | Rates, credit spread, issuer, FX, duration, convexity, call risk. | Clean price, accrued interest, dirty value, income, maturity ladder, yield, duration, credit quality. |
| Cash, deposits, money market and FX | Ledger cash, time deposit, certificate of deposit, treasury bill, repo, money market fund, FX spot, forward, swap, NDF. | Cash balance, deposit claim, security, fund unit, repo claim, FX contract. | Currency, bank credit, issuer, liquidity, settlement, forward FX, counterparty. | Ledger versus settled versus available cash, restricted cash, value date, maturity, projected liquidity. |
| Commodities, precious metals and real assets | Physical metal, allocated or unallocated metal account, commodity ETF/ETP, futures, options, swaps, commodity-linked notes, real-asset proxies. | Physical quantity, account claim, security, fund unit, derivative contract, note claim. | Commodity price, unit, storage/provider, roll, issuer, counterparty, collateral. | Unit of measure, wrapper, notional exposure, delivery or custody status, collateral value, price source. |
| Derivatives | Listed option, OTC option, future, forward, swap, CDS, structured OTC payoff. | Contract with rights and obligations. | Delta, gamma, vega, rates, FX, credit, commodity, notional, counterparty, margin. | Market value versus notional versus exposure, margin, collateral, lifecycle events, settlement. |
| Equities | Common shares, preference shares, ADR/GDR, rights, warrants, listed trusts, ETFs when treated as listed securities. | Shares or listed units. | Issuer, market, sector, country, currency, beta, corporate actions. | Trade date versus settlement date, lots, cost basis, dividends, corporate actions, listed price. |
| Funds | Mutual funds, UCITS, ETFs, hedge funds, fund of funds, private funds, share classes. | Fund units or shares. | Look-through holdings, manager, strategy, liquidity, NAV, currency, concentration. | NAV date, share class, subscription/redemption, distribution, fees, gates, suspensions, look-through coverage. |
| Insurance and annuities | Term life, whole life, universal life, investment-linked policy, annuity, rider, policy loan. | Policy contract, premium obligation, benefit right, loan obligation. | Insurer credit, mortality/longevity, guarantee, sub-fund market exposure, liquidity, lapse. | Cash value, surrender value, death benefit, account value, premium status, beneficiaries, guarantee basis. |
| Loans, Lombard, margin and collateral | Credit facility, overdraft, drawdown, Lombard loan, margin loan, pledge, guarantee. | Client liability and collateral pledge. | Interest rate, FX, collateral volatility, haircut, LTV, liquidity, margin-call risk. | Outstanding balance, limit, availability, collateral value, pledged assets, covenants, margin-call status. |
| Private markets | Private equity, private credit, venture, infrastructure funds, private real estate funds, secondaries, co-investments. | Commitment, fund interest, unfunded obligation, capital-call obligation. | Manager, vintage, strategy, NAV lag, liquidity, unfunded, leverage, concentration. | Commitment, paid-in, unfunded, distributions, NAV date, TVPI, DPI, RVPI, IRR, valuation lag. |
| Real estate, REITs and infrastructure | Listed REIT, business trust, property security, infrastructure security, private fund, direct property. | Listed units, fund units, partnership interest, direct asset title or beneficial interest. | Property type, geography, leverage, lease income, cap rates, infrastructure usage/regulation. | Listed versus private versus direct valuation, income quality, appraisal date, leverage, occupancy, redemption limits. |
| Structured products | Notes, certificates, deposits, warrants, structured funds, accumulators, decumulators, OTC structured contracts. | Wrapper-specific claim or contract. | Underlying, issuer, payoff, barrier, coupon condition, autocall, volatility, credit, liquidity. | Wrapper, payoff formula, observation schedule, scenario outcomes, issuer quote lineage, secondary liquidity. |
| Structured notes | Autocall, reverse convertible, capital-protected note, participation note, credit-linked note, range-accrual note. | Debt note claim against issuer with embedded payoff. | Issuer credit plus embedded derivative exposure to underlying assets. | Legal note position, underlyings, coupon condition, barrier, call/maturity outcome, physical settlement terms. |

## Wrapper Versus Exposure Examples

| Scenario | Wrapper | Economic Exposure | Correct Treatment |
|---|---|---|---|
| Gold bar in custody | Physical holding or custody account. | Gold spot price, storage/custody, insurance, liquidity. | Show quantity and unit, custody status, valuation source, and collateral eligibility separately. |
| Gold ETF | Fund or listed security. | Gold price plus ETF structure, tracking, issuer/provider, liquidity. | Do not report as physical gold; keep fund/security position and commodity exposure distinct. |
| Gold future | Derivative contract. | Gold price, leverage, margin, roll, expiry. | Market value, notional exposure, margin, and roll behavior must be separate. |
| Gold-linked structured note | Note or structured product wrapper. | Issuer credit plus commodity-linked payoff. | Report note issuer, payoff terms, observation events, and commodity exposure separately. |
| Mining company equity | Equity security. | Company equity, sector, commodity sensitivity, issuer-specific operating risk. | Do not treat as direct gold holding. |
| REIT | Listed equity or trust unit. | Property income, leverage, sector, geography, equity market liquidity. | Report listed security position while preserving real estate exposure tags. |
| Private real estate fund | Fund interest or partnership interest. | Property portfolio, manager, leverage, valuation lag, liquidity limits. | Track commitment/NAV/cashflows separately from property look-through. |
| Investment-linked policy | Insurance policy wrapper. | Policy value plus sub-fund market exposure, insurer, charges, surrender terms. | Do not report sub-fund NAV as the full policy benefit without policy value basis. |
| Fund of funds | Fund unit. | Indirect manager and asset exposure through underlying funds. | Distinguish legal unit, top-level NAV, look-through coverage, and stale underlying data. |
| Structured deposit | Deposit wrapper. | Bank credit plus embedded market payoff. | Separate deposit claim, payoff condition, maturity, and deposit-breakage or protection terms. |

## Naming Rules

Use these naming rules in documents, schemas, APIs, UI labels, tests, and reports:

1. Use `product family` for broad knowledge-base grouping.
2. Use `product subtype` for the specific business product type.
3. Use `wrapper` or `legal form` for the legal or operational container.
4. Use `legal holding` for what the client owns.
5. Use `obligation` for what the client owes or may need to fund.
6. Use `economic exposure` for risk and analytics sensitivity.
7. Use `position` only for a current legal/accounting holding or liability balance.
8. Use `transaction` only when an economic posting occurs.
9. Use `lifecycle event` for observations, notices, elections, calls, lapses, and state changes.
10. Use `valuation basis` when different value definitions can exist.
11. Use `liquidity basis` when money cannot be treated as immediately available.
12. Use `supportability state` for implementation and data-quality coverage.
13. Use `source owner` for the system, provider, function, or document that owns truth.
14. Use `lineage` for how a field was sourced, transformed, calculated, or overridden.
15. Use `reporting label` for client-facing or management-facing presentation text.

## Recommended Supportability States

| State | Meaning | Reporting Behavior |
|---|---|---|
| Supported | Required source data, calculations, labels, tests, and reconciliation are complete. | May be shown as normal product output. |
| Source-backed extension | Product-specific behavior is supported because source evidence is available. | Show normally with lineage and clear field definitions. |
| Partial | Some components are available but important product behavior is missing. | Show available values with limitation labels and exclude unsupported conclusions. |
| Estimated | Value is based on model, proxy, manual estimate, stale quote, or incomplete source input. | Label clearly; avoid precision that implies source truth. |
| Stale | Source value is older than the accepted freshness window. | Show date, source, and stale state; block dependent decisions when required. |
| Pending | Transaction, order, lifecycle event, or data update is not complete. | Keep separate from confirmed position, cash, income, and performance. |
| Blocked | A required source, validation, or control condition failed. | Do not present downstream analytics as successful. |
| Unsupported | The product behavior is outside current implementation or data coverage. | Do not fabricate calculations, lifecycle outcomes, or client-reporting claims. |

## Valuation Basis Rules

| Product Area | Common Value Bases | Required Clarification |
|---|---|---|
| Bonds | Clean price, accrued interest, dirty value, amortized cost, model value. | State whether market value includes accrued interest. |
| Cash and deposits | Ledger balance, settled balance, available balance, projected value, breakage value. | State restrictions, reservations, value date, maturity, and penalty assumptions. |
| Equities | Exchange price, last traded price, close price, corporate-action-adjusted price. | State price date, market, currency, and settlement/corporate-action status. |
| Funds | NAV, estimated NAV, stale NAV, redemption value. | State NAV date, dealing date, share class, gate/suspension state, and redemption constraints. |
| Insurance | Account value, cash value, surrender value, death benefit, guaranteed value. | Never mix benefit basis without labels. |
| Loans | Outstanding principal, accrued interest, payoff amount, limit, available credit. | Separate liability value from collateral and availability. |
| Private markets | Reported NAV, adjusted NAV, estimated NAV, commitment, paid-in, unfunded. | State valuation date, received date, restatement, and cashflow completeness. |
| Real estate | Listed price, fund NAV, appraisal value, manager value, estimated value. | State direct/listed/private wrapper and valuation date. |
| Derivatives | Mark-to-market, model value, settlement amount, notional, exposure. | Do not report notional as market value. |
| Structured products | Issuer quote, model value, redemption value, scenario value, protection amount. | State quote source, date, liquidity, payoff assumptions, and issuer risk. |

## Liquidity And Availability Rules

Liquidity is not the same as market value.

| Term | Use When | Do Not Use For |
|---|---|---|
| Market liquidity | The holding can be traded in a market, subject to bid/ask and volume. | Guaranteed cash availability. |
| Dealing liquidity | A fund or product has subscription/redemption windows and cut-offs. | Intraday tradeability unless the product actually supports it. |
| Contractual maturity | Cash is expected at a maturity date or payoff event. | Early exit value unless breakage terms are known. |
| Available cash | Cash that can be used after settlement, restrictions, reservations, and facility rules. | Ledger cash, unsettled cash, pledged cash, blocked cash, or projected proceeds. |
| Collateral liquidity | Value eligible under haircut and pledge rules. | Market value without collateral eligibility. |
| Private liquidity | Transfer, secondary sale, redemption, or distribution path in private assets. | Reported NAV as immediately realizable cash. |
| Surrender liquidity | Insurance value available after surrender terms, charges, tax, and restrictions. | Death benefit, account value, or guaranteed maturity value. |

## Common Anti-Patterns

Avoid these mistakes in documents, designs, source mappings, and tests:

1. Calling a money market fund "cash" without showing fund, NAV, liquidity, gate, and credit/liquidity risk.
2. Treating every structured note as a plain bond because the legal wrapper is debt.
3. Treating REITs only as equities and losing property-sector, leverage, and income exposure.
4. Treating death benefit, surrender value, and account value as one insurance value.
5. Treating derivative margin as product exposure.
6. Treating derivative notional as market value.
7. Treating NAV as liquidity.
8. Treating commitment or unfunded commitment as current market value.
9. Treating a lifecycle event as a transaction before an economic posting occurs.
10. Reporting pledged collateral as freely available.
11. Netting assets and liabilities without preserving gross exposure and collateral linkage.
12. Hiding stale prices, estimated values, or manual overrides.
13. Using "alternative" as a product definition without subtype, wrapper, liquidity, and valuation basis.
14. Showing look-through exposure without coverage percentage, source date, and data quality.
15. Presenting unsupported calculations as normal output because another product family supports similar behavior.

## Product Intake Classification Checklist

Use this checklist when importing or reviewing a new product source:

1. Identify product family, subtype, wrapper, and legal holding.
2. Identify economic exposure and whether look-through is required.
3. Identify source owner for product master, transactions, positions, valuation, lifecycle events, risk, performance, and reporting labels.
4. Identify required identifiers: ISIN, CUSIP, SEDOL, ticker, exchange, contract id, policy number, facility id, fund/share-class id, property id, or internal product id.
5. Identify quantity type: units, shares, nominal, contract size, notional, policy value, commitment, principal, physical quantity, or facility balance.
6. Identify price or value basis and required date.
7. Identify currency, FX conversion rule, and reporting currency treatment.
8. Identify income, fee, tax, premium, interest, coupon, distribution, and charge classification.
9. Identify pending, unsettled, restricted, pledged, stale, estimated, blocked, and unsupported states.
10. Identify lifecycle events that are not ordinary transactions.
11. Identify advisory, suitability, concentration, mandate, and DPM constraints.
12. Identify client-reporting labels and disclosures needed for meaningful interpretation.
13. Identify reconciliation controls and failure behavior.
14. Identify QA cases across normal, edge, stale, partial, blocked, corrected, and migrated data.

## API And UI Vocabulary Checklist

Use these checks before accepting a data model, endpoint, screen, table, or report:

1. Does every field label distinguish legal holding from exposure?
2. Does every value field carry value basis, source, currency, and date where needed?
3. Are transaction, lifecycle event, order, and position states separate?
4. Are pending and confirmed amounts displayed separately?
5. Are cash, available cash, projected cash, and restricted cash separate?
6. Are liabilities, collateral, and pledged assets linked without hiding gross values?
7. Are look-through values labelled with coverage, source, date, and quality?
8. Are unsupported calculations withheld rather than inferred?
9. Are stale, estimated, manual, and blocked states visible?
10. Can QA prove each client-facing label from source data or documented calculation?

## Review Prompts For Product Docs

When reviewing a product document, ask:

1. Is the product family precise enough?
2. Is the subtype clear?
3. Is the wrapper named separately from exposure?
4. Is the legal holding or obligation explicit?
5. Are valuation bases named and not mixed?
6. Are lifecycle events separate from transactions?
7. Are cashflows classified by business meaning?
8. Are risk, suitability, concentration, and mandate implications visible?
9. Are reporting labels consistent with source-backed meaning?
10. Are unsupported states and future candidates separated from current support?

## Related Guides

Use this guide together with:

1. [Cross-Product Transaction And Position Data Model](cross-product-transaction-position-data-model.md)
2. [Product Capability Boundary Matrix](product-capability-boundary-matrix.md)
3. [Product Source Intake And Migration Guide](product-source-intake-and-migration-guide.md)
4. [Source Ownership, Calculation And Reporting Matrix](source-ownership-calculation-reporting-matrix.md)
5. [Product Calculation Example Catalog](product-calculation-example-catalog.md)
6. [Product Performance, Attribution And Risk Guide](product-performance-attribution-risk-guide.md)
7. [Client Reporting And Portfolio Review Guide](client-reporting-and-portfolio-review-guide.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, documentation, and QA work. It is not investment, legal, tax, regulatory, or client advice.
