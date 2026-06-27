# Lending, Collateral And Buying Power Knowledge Map

This page connects loans, Lombard lending, margin lending, collateral, pledge structures, and buying-power concepts to reusable private-banking platform decisions.

Lending and collateral products are both client products and control layers. They affect cash availability, trading capacity, portfolio risk, advisory suitability, mandate compliance, margin calls, forced liquidation, and client reporting.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Loans, Lombard lending, margin, collateral and credit lines | [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | Cash loans, overdrafts, credit lines, Lombard lending, margin lending, collateral pledges, cross-pledging, LTV, haircuts, buying power, margin calls, forced liquidation, advisory, reporting, controls, and QA reference. |

## Platform Design Distinctions

| Concept | Practical Treatment |
|---|---|
| Facility | Approved credit arrangement with borrower, limit, currency, terms, status, tenor, and conditions. |
| Drawdown | Actual loan utilization under a facility; should not be confused with approved limit. |
| Exposure | Amount owed or economically at risk, including principal, accrued interest, fees, reserved exposure, and pending use. |
| Collateral | Asset pledged to secure exposure; legal pledge state and operational availability must be explicit. |
| Lending value | Market value adjusted by LTV, haircut, eligibility, concentration, FX, liquidity, rating, and policy rules. |
| Availability | Remaining borrowable amount after facility limit, collateral value, exposure, reservations, and blocks. |
| Buying power | Trading capacity after cash, credit, collateral, mandate, suitability, product eligibility, settlement, and order controls. |
| Margin call | Shortfall workflow requiring cure by cash, collateral, repayment, risk reduction, or liquidation. |
| Forced liquidation | Controlled sale/close-out workflow to reduce exposure when cure is not made or risk threshold is breached. |

## Reusable Modelling Pattern

Use separate layers:

```text
Facility Layer
  Borrower, facility limit, currency, tenor, status, covenants, purpose, approved products

Exposure Layer
  Drawn principal, accrued interest, fees, pending drawdowns, reserved exposure, margin requirement

Collateral Layer
  Eligible holdings, pledged quantity, market value, haircut, LTV, concentration, FX adjustment

Pledge Graph Layer
  Collateral provider, borrower, beneficiary, pledge direction, scope, effective dates, cross-pledge limits

Availability Layer
  Facility headroom, collateral headroom, exposure, reservations, blocked states, buying power

Workflow Layer
  Drawdown, repayment, interest accrual, pledge, release, substitution, margin call, cure, liquidation

Control Layer
  Reconciliation, stale price/FX checks, rule versioning, override approval, audit history
```

Do not calculate buying power from market value alone. A robust calculation must use eligibility, haircut/LTV, concentration, FX, liquidity, pledge direction, exposure, in-flight orders, mandate controls, and policy blocks.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Facility limit and status | Credit system, loan system, approval workflow, credit operations. |
| Drawn exposure | Loan system, core banking ledger, credit ledger, custodian financing statement. |
| Accrued interest and fees | Loan accrual engine, credit system, accounting ledger, product terms. |
| Collateral holding and value | Custodian, portfolio book, market data vendor, valuation policy. |
| Eligibility and haircuts | Credit policy, collateral policy, product master, risk engine, manual override register. |
| Pledge direction and scope | Legal pledge system, collateral management system, agreement documents, operations workflow. |
| In-flight reservations | Order management, trading workflow, buying-power engine, settlement queue. |
| Margin call status | Risk/credit workflow, operations case system, client communication record. |
| Liquidation actions | Order workflow, trade execution records, loan repayment ledger, operations approval. |

## API And UI Implications

APIs should expose:

1. approved limit, drawn exposure, accrued interest, fees, and utilization separately,
2. collateral market value and lending value separately,
3. haircut/LTV rule version and override reason,
4. facility headroom, collateral headroom, and final availability,
5. pledge graph direction and beneficiary,
6. reserved buying power for in-flight orders,
7. margin-call status, cure deadline, shortfall amount, and cure options,
8. stale price/FX/rating flags and blocked calculation states,
9. liquidation workflow state and audit references.

UIs should make these states visible:

1. facility is approved but not drawn,
2. collateral is held but not eligible,
3. collateral is pledged and cannot be freely sold,
4. buying power is blocked by stale prices or missing FX,
5. cross-pledge support is directional and not transitive unless explicitly allowed,
6. margin call is warning, active, overdue, cured, or liquidation-triggered,
7. sale of pledged collateral would reduce availability or trigger shortfall,
8. availability is limited by facility limit rather than collateral, or by collateral rather than facility.

## QA Scenarios

High-value scenarios:

1. simple Lombard availability where collateral value binds,
2. availability where facility limit binds,
3. in-flight order reserves and releases buying power,
4. price fall triggers margin call,
5. FX haircut reduces foreign-currency collateral value,
6. rating downgrade changes bond collateral haircut,
7. collateral sold but pledge release is missing,
8. cross-pledge supports only explicit beneficiary,
9. no transitive collateral support through a pledge chain,
10. forced liquidation sells collateral, books P&L, repays loan, and closes margin call.

## Useful Project Workflows

Use this pack when working on:

1. credit-line and loan position modelling,
2. collateral pool and pledge graph design,
3. LTV/haircut and eligibility engines,
4. buying-power and pre-trade checks,
5. margin-call and cure workflows,
6. forced-liquidation processing,
7. collateral substitution and release,
8. lending interest and fee accrual,
9. credit exposure and leverage reporting,
10. advisory suitability and mandate leverage controls.

## Related Packs

| Pack | Relationship |
|---|---|
| [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Cash availability, overdraft, funding, settlement, collateral cash, and buying-power inputs. |
| [`equities/`](equities/README.md) | Equity collateral eligibility, pledge state, short positions, corporate actions, and forced-sale implications. |
| [`bonds/`](bonds/README.md) | Bond collateral eligibility, rating, duration, issuer concentration, haircut, maturity, and default implications. |
| [`funds/`](funds/README.md) | Fund collateral eligibility, stale NAV, redemption liquidity, gates, and share-class restrictions. |
| [`derivatives/`](derivatives/README.md) | Margin, collateral, counterparty exposure, close-out, and hedge/overlay interactions. |
| [`private-markets/`](private-markets/README.md) | NAV lending, subscription lines, capital-call facilities, illiquid collateral, and valuation uncertainty. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, legal, tax, accounting, credit, regulatory, or client advice.
