# 08 — Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Platform capabilities required

A wealth platform supporting real estate, REITs, infrastructure and real-asset products should support:

- instrument master for REITs, trusts, funds, private funds, infrastructure and direct property;
- listed security trade processing;
- fund NAV and subscription/redemption processing;
- private fund commitment/capital-call/distribution model;
- direct property valuation, rent, expense and linked debt model;
- distribution component classification;
- corporate actions including rights, mergers, splits and DRP;
- look-through exposure by property type, geography, tenant, manager, sponsor and currency;
- valuation source/date/confidence flags;
- liquidity bucket and mandate classification;
- performance contribution by price/appraisal, income, FX and fees;
- risk analytics for leverage, concentration, interest rate, valuation and liquidity;
- reporting controls and reconciliation.

## 2. Architecture pattern

```text
Market/reference data
  - security master, exchange, sector, issuer, sponsor
  - REIT/property/infrastructure attributes
  - prices, NAVs, FX rates, distributions

Lifecycle/event engine
  - corporate actions
  - capital calls
  - distributions
  - appraisal/NAV updates
  - rights/tenders/mergers

Transaction ledger
  - trades
  - cashflows
  - income/return of capital/tax/fees
  - contributions/distributions

Position engine
  - listed units
  - fund units
  - commitments and NAV
  - direct property values

Analytics engine
  - valuation
  - performance
  - income
  - risk
  - exposure/look-through
  - mandate compliance

Reporting/advisory APIs
  - portfolio review
  - suitability
  - client reporting
  - DPM monitoring
```

## 3. Data quality controls

| Control | Why it matters |
|---|---|
| Price/NAV freshness | Prevent stale valuation being shown as current |
| Distribution component validation | Avoid misclassifying return of capital as income |
| Corporate-action completeness | Rights/splits/mergers affect quantity and cost basis |
| Commitment reconciliation | Private funds need call/unfunded accuracy |
| Appraisal date control | Direct property/private NAVs can be stale |
| FX consistency | Cross-border assets and distributions require correct FX |
| Sector classification | Real estate/infrastructure allocation reporting depends on it |
| Leverage data validation | Gearing and LTV are key risk metrics |
| Liquidity classification | Mandate/advisory restrictions depend on it |
| Duplicate transaction check | Prevent doubled distributions or NAV updates |
| Reversal/correction support | Corporate action and fund notices may be corrected |

## 4. Reconciliation controls

| Reconciliation | Match between |
|---|---|
| Position reconciliation | Platform units vs custodian statement |
| Cash reconciliation | Distribution/call/expense cash vs bank ledger |
| NAV reconciliation | Manager NAV vs posted valuation |
| Commitment reconciliation | Fund admin commitment/called/unfunded vs platform |
| Corporate-action reconciliation | CA event terms vs custodian outcomes |
| Distribution reconciliation | Gross/tax/net cash and component classification |
| Direct property reconciliation | Appraisal/manual source vs reporting value |
| Loan/property reconciliation | Linked debt vs collateral value |
| FX reconciliation | Transaction FX and valuation FX sources |
| Mandate reconciliation | Exposure classifications vs mandate rules |

## 5. Test scenarios

### Listed REIT scenarios

| Scenario | Expected result |
|---|---|
| Buy listed REIT | Units increase, cash decreases, cost basis set |
| Sell partial REIT position | Units decrease, realised P&L calculated |
| Receive distribution | Cash increases, income recorded |
| Distribution includes return of capital | Income/ROC split correct; cost basis adjusted if policy requires |
| Rights issue announced and exercised | Rights created, cash paid, new units booked |
| Rights lapse | Rights removed with correct realised result if any |
| Unit consolidation | Quantity decreases, cost per unit adjusts, market value unchanged apart from rounding |
| REIT merger for cash and stock | Old units removed, cash/new units booked, P&L calculated |
| Trading suspended | Price status flagged, valuation policy applied |

### Private real estate fund scenarios

| Scenario | Expected result |
|---|---|
| Commitment booked | Commitment increases, no cash movement until call |
| Capital call paid | Cash decreases, called capital increases, unfunded decreases |
| NAV update received late | NAV applied with valuation date and lag flag |
| Income distribution | Cash increases, income classified |
| Return of capital distribution | Cash increases, capital returned tracked |
| Recallable distribution | Cash received but unfunded/recallable adjusted |
| Secondary sale | NAV/cost/commitment/unfunded transfer handled |
| Fund final liquidation | Position closed, final P&L/IRR available |

### Direct property scenarios

| Scenario | Expected result |
|---|---|
| Property added manually | Non-custody asset created with source and confidence |
| Appraisal update | Valuation changes, unrealised gain/loss recorded |
| Rent received | Cash income booked |
| Expense paid | Expense booked |
| Mortgage linked | Liability included, net equity calculated |
| Property sold | Asset removed, cash received, realised gain/loss calculated |
| Ownership share changes | Client share value recalculated |

### Infrastructure scenarios

| Scenario | Expected result |
|---|---|
| Capital call for infrastructure project | Commitment/call/unfunded updated |
| Construction delay disclosed | Lifecycle event/risk flag updated |
| Availability payment received | Income distribution booked |
| NAV impairment | Valuation loss recognised |
| Refinancing distribution | Distribution component classified correctly |

## 6. Migration checks

| Check | Description |
|---|---|
| Product classification | REIT vs equity vs fund vs private fund vs direct property |
| Quantity/cost | Units and cost basis from source systems |
| Income history | Distribution history for performance and reporting |
| Corporate actions | Historical splits/rights/mergers reflected |
| NAV history | Private fund NAV history imported with dates |
| Commitment history | Commitment/called/unfunded/distribution values reconciled |
| FX history | Historical valuation and cashflow FX available |
| Direct asset source | Manual/valuation source captured |
| Mandate mapping | Existing exposures mapped to new classification |

## 7. Production triage questions

When a REIT/real asset holding looks wrong, ask:

1. Is the wrapper classification correct?
2. Is the latest price/NAV/appraisal available?
3. Is the valuation date stale?
4. Did a distribution, rights issue, split or merger occur?
5. Were cash, tax and return-of-capital components booked correctly?
6. Is FX conversion using correct date/rate?
7. Is there a manager/custodian correction notice?
8. Is the position from trade ledger, custodian feed or manual holding?
9. Are look-through exposures updated?
10. Are mandate and liquidity classifications correct?

## 8. Common edge cases

| Edge case | Risk |
|---|---|
| REIT distribution booked twice | Income overstated |
| Return of capital treated as yield | Income sustainability overstated |
| Rights issue ignored | Position/dilution/cost basis wrong |
| NAV lag hidden | Client sees stale private asset value as current |
| Direct property entered without ownership % | Wealth overstated |
| Linked mortgage excluded | Net worth overstated |
| FX on distribution missing | Income/return wrong |
| Business trust classified as REIT | Mandate/regulatory/reporting misclassification |
| Private fund treated like daily fund | Liquidity and valuation wrong |
| Infrastructure revenue treated as guaranteed | Risk understated |

## 9. Glossary

| Term | Meaning |
|---|---|
| REIT | Real Estate Investment Trust; vehicle owning/financing real estate |
| S-REIT | Singapore-listed REIT |
| NAV | Net asset value |
| Cap rate | Net operating income divided by property value |
| NOI | Net operating income |
| Occupancy | Percentage of space leased/occupied |
| WALE | Weighted average lease expiry |
| Gearing | Debt relative to assets/equity |
| Interest coverage ratio | Earnings/cashflow relative to interest expense |
| Distribution yield | Annual distributions divided by price/NAV |
| Return of capital | Distribution representing capital return rather than income |
| Core real estate | Stabilised lower-risk property strategy |
| Value-add real estate | Strategy improving/repositioning assets |
| Opportunistic real estate | Higher-risk development/distressed strategy |
| Concession | Right to operate infrastructure asset for period |
| Availability payment | Payment for keeping infrastructure asset available |
| Merchant exposure | Exposure to market prices/volumes rather than fixed contract |
| Appraisal | Valuation estimate by qualified valuer |
| NAV lag | Delay between valuation date and reporting date |
| Stapled security | Multiple securities legally stapled and traded together |

## 10. Interview-ready explanation

> Real estate and infrastructure products must be modelled by both wrapper and economic exposure. A listed REIT trades like an equity but needs property-specific analytics such as distribution yield, NAV, gearing, occupancy and lease expiry. A private real estate or infrastructure fund behaves like private markets, with commitments, capital calls, NAV lag and limited liquidity. Direct property needs appraisal, rental income, expenses and linked debt modelling. For advisory and DPM mandates, the platform must capture liquidity, valuation source, leverage, geography, property type, income classification, concentration and collateral eligibility. Reporting should distinguish market value, NAV/appraisal date, recurring income, return of capital, FX effects and valuation confidence.
