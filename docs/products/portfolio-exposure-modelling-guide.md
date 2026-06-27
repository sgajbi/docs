# Portfolio Exposure Modelling Guide

This guide defines a reusable standard for modelling portfolio exposure across product families, wrappers, legal positions, look-through holdings, derivatives, liabilities, commitments, collateral, mandates, and reporting views.

Use it when designing portfolio views, risk dashboards, DPM monitoring, advisory checks, concentration reports, source intake, data contracts, QA scenarios, or client-reporting labels.

## Core Principle

Portfolio exposure is not one number.

A robust platform should preserve several exposure lenses at the same time:

1. legal holding,
2. accounting position,
3. market-value allocation,
4. economic exposure,
5. look-through exposure,
6. notional exposure,
7. sensitivity-adjusted exposure,
8. issuer or counterparty exposure,
9. currency exposure,
10. liquidity exposure,
11. collateral exposure,
12. commitment or future-funding exposure,
13. mandate exposure,
14. reporting exposure.

The correct exposure lens depends on the decision being made. A client statement, risk dashboard, margin-call engine, DPM limit check, suitability workflow, and tax report may all need different denominators and labels.

## Exposure Lens Definitions

| Exposure Lens | Meaning | Use For | Do Not Use For |
|---|---|---|---|
| Legal holding | What the client legally owns or owes. | Positions, custody, accounting, reconciliation. | Full economic risk when wrappers or derivatives embed other exposures. |
| Market-value allocation | Current value by product, asset class, currency, account, or mandate. | Portfolio allocation, statements, contribution weights. | Leverage, derivative notional, liquidity, or future obligations. |
| Economic exposure | Underlying risk sensitivity after wrapper, leverage, hedge, or look-through treatment. | Risk, stress, concentration, mandate monitoring. | Legal ownership or client-reporting market value without labels. |
| Look-through exposure | Exposure from underlying holdings or references inside a fund, note, policy, or vehicle. | Asset allocation, sector, country, currency, manager and concentration views. | Final output when coverage, source date, or mapping quality is missing. |
| Notional exposure | Contract reference amount. | Derivatives, structured products, swaps, forwards, futures, overlays. | Market value, cash balance, or maximum loss without payoff context. |
| Sensitivity-adjusted exposure | Exposure scaled by delta, DV01, CS01, beta, duration, or other risk sensitivity. | Risk analytics, hedge monitoring, stress testing. | Legal position or simple client statement allocation. |
| Issuer exposure | Exposure to issuer credit or product provider. | Bonds, notes, deposits, structured products, certificates, ETNs. | Underlying market exposure unless issuer is also the underlying. |
| Counterparty exposure | Exposure to a contractual counterparty. | OTC derivatives, FX, lending, collateral, structured OTC products. | Exchange-traded market allocation without counterparty risk. |
| Manager exposure | Exposure to manager, GP, sponsor, advisor, or fund platform. | Funds, private markets, hedge funds, family-office vehicles. | Direct security issuer concentration. |
| Currency exposure | Local-currency and reporting-currency exposure after cash, assets, liabilities, hedges, and settlements. | FX risk, funding, reporting-currency return, mandate currency limits. | Product family allocation alone. |
| Liquidity exposure | Amount accessible by sale, redemption, maturity, surrender, settlement, or transfer after constraints. | Liquidity planning, advisory, DPM, client reporting. | Market value without settlement, gate, pledge, lock-up, or surrender terms. |
| Collateral exposure | Pledged value, eligible value, haircut-adjusted lending value, and shortfall. | Lending, margin, buying power, collateral monitoring. | Free portfolio value or investment allocation. |
| Commitment exposure | Future funding, callable capital, premium obligation, facility exposure, or unfunded commitment. | Private markets, insurance, loans, capital planning. | Current NAV or market value. |
| Mandate exposure | Exposure measured against portfolio rules, risk limits, restrictions, and permitted products. | DPM controls, advisory suitability, exception monitoring. | General portfolio analytics without the mandate's definitions. |

## Product Family Exposure Matrix

| Product Area | Legal Position | Main Exposure Lenses | Key Modelling Caution |
|---|---|---|---|
| Cash | Cash balance by account and currency. | Available cash, restricted cash, projected cash, currency exposure, bank exposure. | Ledger cash, settled cash, available cash, and projected cash are different. |
| Deposits | Deposit claim against bank. | Bank credit, maturity bucket, currency, rate reset, liquidity/breakage exposure. | Deposit principal is not free cash before maturity unless breakage is supported. |
| Money market funds | Fund units. | NAV exposure, liquidity exposure, fund issuer/manager, underlying short-term credit exposure. | Treat as fund exposure, not cash, unless reporting policy explicitly maps it to liquidity bucket. |
| FX spot and forwards | Currency trade or forward contract. | Currency exposure, settlement exposure, counterparty exposure, hedge ratio, forward MTM. | Forward notional is not cash and should not inflate available liquidity. |
| Bonds | Nominal debt claim. | Issuer, credit rating, duration, spread, maturity, currency, sector, country, liquidity. | Market value allocation and duration/spread exposure answer different questions. |
| Equities | Shares or listed units. | Issuer, sector, country, currency, beta/factor, dividend, liquidity, restriction. | Corporate actions can change quantity, cost basis, and entitlement without changing economic exposure proportionally. |
| Funds | Fund units or shares. | Top-level fund, manager, strategy, share class, NAV, liquidity, look-through holdings. | Look-through must show coverage percentage, date, source, and residual unknown bucket. |
| Insurance and annuities | Policy contract and benefit rights. | Insurer, policy value basis, ILP sub-fund exposure, surrender liquidity, guarantee, mortality/longevity. | Death benefit, account value, surrender value, and investable exposure are not interchangeable. |
| Loans and collateral | Liability and pledge relationship. | Drawn exposure, available headroom, collateral market value, lending value, LTV, shortfall, rate exposure. | Liability, collateral, and buying power must remain gross and linked. |
| Private markets | Fund interest, commitment, paid-in, unfunded obligation. | Manager, strategy, vintage, NAV, unfunded, liquidity, sector/geography look-through, valuation lag. | Commitment, paid-in, unfunded, NAV, and distributions require separate exposure views. |
| Real estate and infrastructure | Listed units, fund units, partnership interest, or direct asset. | Property/infrastructure sector, geography, leverage, income, liquidity, appraisal/NAV date. | Listed REIT equity exposure and real-asset economic exposure should both be visible. |
| Commodities and precious metals | Physical holding, account claim, ETP/fund unit, derivative, or note. | Commodity, unit, wrapper, notional, roll, storage/provider, counterparty, collateral value. | Physical, account, ETF, future, note, and mining equity exposures are not equivalent. |
| Derivatives | Contract with rights and obligations. | Notional, delta, DV01, CS01, vega, counterparty, margin, collateral, hedge link, expiry. | Market value can be small while economic exposure or stress loss is large. |
| Structured products | Wrapper-specific claim or contract. | Issuer, underlyings, payoff, barrier, coupon condition, scenario loss, liquidity, currency. | Issuer exposure and underlying exposure must both be represented. |
| Structured notes | Debt note with embedded derivative. | Issuer credit, note subtype, underlyings, barrier/autocall/coupon state, maturity ladder. | Do not report direct underlying ownership until delivery actually occurs. |
| Tax and regulatory reporting | Reportable account, event, owner, beneficial owner, classification. | Reportability, withholding, tax-lot, beneficial-owner, jurisdiction, documentation exposure. | Regulatory scope is not always the same as portfolio or account scope. |
| Trusts and wealth structures | Legal owner, beneficial owner, controller, authorized party. | Entity, beneficiary, mandate, reporting, access, tax residence, authority, relationship exposure. | Consolidated family exposure must not leak restricted beneficiary or entity data. |

## Denominator Rules

| View | Recommended Denominator | Why |
|---|---|---|
| Client statement allocation | Supported market value or clearly labelled value basis. | Clients need a reconciled value view. |
| Risk allocation | Economic exposure or sensitivity-adjusted exposure where source-backed. | Risk may differ from market value for derivatives, structured products, hedges, and leverage. |
| Liquidity ladder | Accessible value by settlement/redemption/maturity/surrender date after restrictions. | Liquidity planning requires timing and constraints. |
| Credit concentration | Issuer, counterparty, bank, insurer, borrower, sponsor, or manager exposure. | Credit risk follows obligor/provider dependency, not only asset class. |
| Currency exposure | Local value plus cash, liabilities, hedges, pending settlements, and reporting FX. | Portfolio currency risk includes assets, liabilities, and contracts. |
| Collateral view | Pledged value, eligible value, haircut-adjusted value, and shortfall. | Lending decisions need collateral eligibility, not total wealth. |
| Commitment view | Commitment, unfunded, callable amount, premium due, facility limit, or future obligation. | Future funding and liquidity stress are not captured by current market value. |
| Mandate compliance | Mandate-defined exposure rule. | DPM and advisory rules may use market value, risk exposure, issuer, liquidity, rating, or product family. |
| Tax/regulatory report | Regime-defined reportable balance, income, transaction, owner, or beneficial owner. | Legal/reportable scope may differ from portfolio analytics scope. |

## Look-Through Rules

Look-through exposure should never appear as fully reliable unless these fields are known:

1. source owner,
2. source date,
3. coverage percentage,
4. top-level instrument or wrapper,
5. underlying identifier,
6. underlying classification,
7. weighting method,
8. currency method,
9. stale-data policy,
10. residual unknown bucket,
11. double-count prevention rule,
12. permission and privacy scope.

If look-through coverage is partial, report both the covered exposure and the residual unclassified exposure. Do not silently allocate missing exposure to cash, alternatives, other, or the top-level wrapper.

## Netting Rules

Do not net exposures unless the view explicitly requires it.

| Gross View To Preserve | Netting Risk |
|---|---|
| Assets and liabilities | Net worth hides leverage and margin-call risk. |
| Long and short derivatives | Net exposure hides gross notional and counterparty exposure. |
| Collateral and borrowing | Net value hides pledged assets and liquidity constraint. |
| Issuer and underlying | Structured products hide issuer credit if only underlying is shown. |
| Fund wrapper and look-through | Look-through-only view can lose manager, liquidity, and fee exposure. |
| Family entity and beneficiary | Consolidated view can leak or misstate legal ownership. |
| Taxable and non-taxable accounts | Combined view may misstate reportability or withholding treatment. |

## Mandate And Advisory Use

Exposure models should support:

1. product eligibility checks,
2. issuer and counterparty limits,
3. asset-class and look-through allocation limits,
4. currency limits,
5. liquidity bucket limits,
6. concentration limits,
7. derivative and leverage permissions,
8. collateral and borrowing constraints,
9. private-market commitment limits,
10. insurance and policy liquidity constraints,
11. tax and regulatory documentation checks,
12. role and authority checks for trusts and family structures.

When a source cannot prove the exposure lens needed by a mandate rule, the rule should fail closed, degrade explicitly, or route to exception review. It should not pass because another exposure lens is available.

## Reporting Labels

Use specific labels:

| Label | Use When |
|---|---|
| Market value allocation | The view uses current supported market value or value basis. |
| Economic exposure | The view uses underlying or risk-adjusted exposure. |
| Look-through allocation | The view expands fund, note, policy, vehicle, or structure holdings. |
| Notional exposure | The view uses contractual notional. |
| Delta-adjusted exposure | The view uses derivative delta or equivalent sensitivity. |
| Duration exposure | The view uses interest-rate sensitivity. |
| Credit exposure | The view follows issuer, borrower, counterparty, insurer, bank, or manager dependency. |
| Liquidity exposure | The view uses accessible value after timing and restrictions. |
| Collateral value | The view uses pledged market value. |
| Lending value | The view uses haircut-adjusted eligible collateral value. |
| Commitment exposure | The view uses unfunded or callable obligation. |
| Reportable exposure | The view follows tax, regulatory, legal, or disclosure scope. |

## Implementation Checklist

1. Store exposure lens separately from product family and asset class.
2. Store source, timestamp, value basis, currency, and supportability state for every exposure result.
3. Preserve legal position, market value, notional, sensitivity, look-through, collateral, and commitment views separately.
4. Define denominator and netting rules per report, dashboard, API, and mandate rule.
5. Store look-through coverage, source date, and residual unknown bucket.
6. Preserve issuer, counterparty, manager, bank, insurer, borrower, and sponsor identifiers.
7. Link collateral exposures to pledged holdings and loan/facility records.
8. Link derivative exposures to contract terms, underlyings, multipliers, sensitivities, margin, and collateral.
9. Link structured-product exposure to issuer, wrapper, payoff, underlyings, observation state, and scenario assumptions.
10. Link trust and family-office views to legal owner, beneficial owner, reporting recipient, and access rights.
11. Block or degrade unsupported exposure views instead of substituting a nearby metric.
12. Add QA assertions for each exposure lens used in reports or controls.

## QA Scenarios

1. Fund has 80 percent look-through coverage and 20 percent residual unknown bucket.
2. Equity autocall shows issuer credit exposure and underlying equity exposure separately.
3. Option position shows market value, notional, delta-adjusted exposure, and premium cashflow separately.
4. Futures margin movement changes cash but not notional exposure.
5. Private-market fund shows NAV, paid-in, unfunded, commitment, and liquidity lock-up separately.
6. Lombard account shows pledged market value, haircut-adjusted lending value, outstanding loan, and availability separately.
7. REIT appears in listed equity allocation and real-estate economic exposure where policy requires both.
8. Insurance policy exposes account value, surrender liquidity, death benefit, insurer exposure, and ILP sub-fund exposure separately.
9. FX forward reduces currency exposure in hedge view but does not become cash before settlement.
10. Trust structure shows family-level consolidation without exposing restricted beneficiary data.
11. Missing derivative delta blocks delta-adjusted exposure but still allows labelled market value.
12. Stale fund holdings file marks look-through and attribution partial.

## Related Guides

Use this guide together with:

1. [Product Taxonomy And Vocabulary Guide](product-taxonomy-and-vocabulary-guide.md)
2. [Product Performance, Attribution And Risk Guide](product-performance-attribution-risk-guide.md)
3. [Product Payoff, Scenario And Stress Guide](product-payoff-scenario-and-stress-guide.md)
4. [Product Capability Boundary Matrix](product-capability-boundary-matrix.md)
5. [Source Ownership, Calculation And Reporting Matrix](source-ownership-calculation-reporting-matrix.md)
6. [Client Reporting And Portfolio Review Guide](client-reporting-and-portfolio-review-guide.md)
7. [Tax, Regulatory And Reporting](tax-regulatory-and-reporting.md)
8. [Wealth Structuring, Trusts And Family Office](wealth-structuring-trusts-and-family-office.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, reporting, documentation, and QA work. It is not investment, legal, tax, regulatory, accounting, fiduciary, credit, trading, or client advice.
