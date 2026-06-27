# Product Source Ownership Deep Dive

This guide expands the cross-product source-ownership matrix into product-family operating detail.

Use it when a product feature needs more than generic holding, transaction, price, and report support. The goal is to make source ownership, calculation dependencies, reporting labels, QA scenarios, degraded states, and future candidate capabilities explicit before implementation.

## Source Ownership Pattern

Every product-specific capability should separate these layers:

| Layer | Question | Example |
|---|---|---|
| Source truth | Who is authoritative for the fact? | Custodian owns settled holding; issuer notice owns observation result; fund administrator owns NAV. |
| Platform model | How is the fact represented? | Position, lifecycle event, cashflow, valuation, restriction, pledge, exposure, report section. |
| Calculation | What deterministic calculation uses the fact? | Yield, accrual, entitlement, lending value, attribution, IRR, barrier distance. |
| Report label | How should users understand it? | Confirmed, projected, stale, estimated, partial, restricted, unsupported. |
| QA proof | What catches real defects? | Missing source, stale source, source conflict, restatement, partial support, unsupported subtype. |
| Support boundary | What should not be inferred? | No fill or settlement from proposal, no direct equity ownership before delivery, no available cash from requested redemption. |

## Bonds: Issuer And Credit Event Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Issuer identity and hierarchy | Security master, issuer data vendor, internal issuer hierarchy. | Issuer concentration, credit exposure, connected-issuer reporting. | Unknown issuer, duplicate issuer, stale hierarchy. | Two bonds with different tickers map to the same issuer group and concentration report aggregates both. |
| Coupon schedule and day count | Security master, issuer terms, pricing vendor, operations override. | Accrued interest, income projection, dirty value, yield. | Missing schedule, unsupported convention, disputed terms. | Coupon accrual blocks or labels partial when day count is missing. |
| Call, put, sinking fund, amortisation | Security master, issuer terms, event vendor. | Yield-to-worst, maturity ladder, projected cashflows. | Missing optionality schedule, unconfirmed event, unsupported amortisation. | Yield-to-worst is unavailable when callable schedule is absent. |
| Rating and outlook | Rating agency feed, market-data vendor, internal credit team. | Credit bucket, mandate eligibility, collateral haircut, risk reporting. | Stale rating, split rating, watchlist only, missing effective date. | Downgrade effective date recalculates credit bucket and collateral availability. |
| Default, restructuring, recovery | Issuer notice, custodian, trustee, operations workflow, legal notice. | Valuation impairment, income stop/accrual change, recovery cashflow, risk reporting. | Pending event, uncertain recovery, disputed notice. | Default event stops normal coupon projection and preserves recovery assumption label. |

Professional reports should show maturity date, next coupon, accrued interest, yield basis, callability, issuer group, rating date, credit bucket, stale source flags, and default/recovery notes where relevant.

## Equities: Corporate Action Ownership

| Event | Primary Source Truth | Platform Treatment | Calculation Or Report Impact | Boundary |
|---|---|---|---|---|
| Cash dividend | Issuer/corporate-action vendor, custodian confirmation. | Entitlement event and cashflow on pay date. | Gross income, withholding tax, net cash, income report. | Do not treat withholding tax as investment loss. |
| Stock split | Corporate-action vendor, custodian booking. | Quantity adjustment and cost-per-share adjustment. | Quantity, price comparability, cost basis continuity. | Total cost basis should not change unless cash-in-lieu applies. |
| Rights issue | Issuer terms, custodian election workflow. | Entitlement, election, funding reservation, new share allocation. | Subscription cash, resulting quantity, cost-basis policy. | Do not book new shares before election/allocation is confirmed. |
| Spin-off | Issuer/custodian event, tax-lot policy. | New security position and cost-basis allocation. | Market value, realized/unrealized P&L, tax-lot reporting. | Do not classify spin-off as a normal buy unless booking policy says so. |
| Merger or takeover | Issuer/custodian event, election result. | Cash, stock, mixed consideration, fractional handling. | Realized P&L, new holding, cash-in-lieu, tax reporting. | Do not close old holding until legal event is processed. |
| Trading halt or delisting | Exchange, market-data vendor, custodian. | Price stale/unsupported state and liquidity exception. | Valuation, liquidity, risk, restriction reporting. | Do not assume zero value unless source policy supports write-down. |

QA should include entitlement date logic, fractional quantities, election deadline, cash reservation, source conflict, reversal/correction, and cost-basis impact.

## Funds: Dealing, NAV And Liquidity Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Share-class identity | Fund platform, transfer agent, fund data vendor. | Eligibility, fees, currency, distribution type, suitability. | Unknown share class, wrong currency, stale fee data. | Accumulating and distributing share classes do not share distribution treatment. |
| Dealing calendar and cut-off | Fund platform, transfer agent, fund administrator. | Order acceptance, expected dealing date, settlement projection. | Missing calendar, missed cut-off, holiday mismatch. | Order after cut-off moves to next eligible dealing date. |
| NAV and NAV date | Fund administrator, pricing vendor, fund platform. | Market value, performance, contribution, allocation, drift. | Stale NAV, estimated NAV, suspended NAV. | Stale NAV marks allocation and performance partial. |
| Gate, suspension, side pocket | Fund administrator, manager notice, platform operations. | Liquidity, redemption support, reporting exception. | Pending notice, partial redemption, side-pocket value unknown. | Requested redemption does not become available cash until confirmed and settled. |
| Look-through holdings | Manager file, data vendor, fund administrator. | Exposure, risk, attribution, mandate checks. | Partial coverage, stale file, unmapped holdings. | Look-through report shows coverage percentage and unknown bucket. |

Reports should separate requested order, accepted order, confirmed units, dealing NAV, settlement date, stale NAV, liquidity state, and look-through coverage.

## Derivatives: Margin, Collateral And Exposure Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Contract terms | Trade capture, broker, counterparty confirmation, clearing house. | Notional, payoff, expiry, exercise, settlement. | Missing confirmation, term mismatch, unsupported subtype. | Contract cannot publish exposure when multiplier or underlying is missing. |
| Valuation and sensitivities | Valuation vendor, internal model, broker statement, market data. | MTM, Greeks, DV01, CS01, stress loss, hedge effectiveness. | Stale valuation, missing Greeks, model unsupported. | Market value may be small while notional and delta exposure remain material. |
| Initial and variation margin | Clearing house, broker, collateral system, cash ledger. | Cash movements, liquidity, risk, collateral reporting. | Disputed margin, missing call, stale statement. | Variation margin changes cash but does not close the contract. |
| Fixings and resets | Market-data vendor, calculation agent, counterparty notice. | Swap cashflows, option payoff, NDF settlement, floating reset. | Missing fixing, disputed fixing, holiday mismatch. | NDF settlement blocks until fixing is confirmed. |
| Collateral and CSA terms | Collateral system, legal agreement, counterparty records. | Counterparty exposure, eligible collateral, interest on collateral. | Missing CSA, disputed collateral value, unsupported rehypothecation. | Collateral movement is not investment P&L unless methodology explicitly supports it. |

Reports should show market value, notional, sensitivity exposure, margin cash, collateral value, counterparty, expiry, purpose, hedge link, and supportability state.

## Structured Products And Notes: Payoff And Observation Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Term sheet fields | Issuer, distributor, product master, operations enrichment. | Payoff schema, coupon rule, barrier, observation, settlement. | Missing term, conflicting terms, unsupported payoff. | Scenario calculator blocks when payoff term is missing. |
| Underlying mapping | Product master, market-data vendor, issuer terms. | Underlying exposure, barrier distance, scenario payoff. | Missing mapping, stale underlying price, corporate action on underlying. | Barrier distance unavailable when underlying mapping is incomplete. |
| Observation result | Issuer notice, custodian, calculation agent. | Coupon confirmation, autocall, barrier event, redemption path. | Pending observation, disputed result, missing source notice. | Indicative market level does not book confirmed coupon or redemption. |
| Issuer quote and liquidity | Issuer quote, valuation vendor, secondary-market desk. | Market value, exit value, liquidity label, performance. | Stale quote, indicative quote, no bid. | Report distinguishes indicative valuation from executable liquidity. |
| Physical delivery or cash settlement | Issuer/custodian event, operations workflow. | Close note, create delivered asset, residual cash, cost basis. | Delivery pending, missing share quantity, cost-basis unknown. | Direct equity position is created only after legal delivery. |

Reports should show issuer, underlyings, coupon condition, next observation, barrier distance, scenario labels, issuer quote date, liquidity limitation, and delivered-asset status.

## Private Markets: Administrator, GP And Valuation Committee Ownership

| Data Area | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Commitment and legal terms | Subscription documents, GP, administrator, internal operations. | Commitment, unfunded, eligibility, transfer restriction. | Missing agreement, amended commitment, side letter unknown. | Unfunded commitment does not go negative without explicit source adjustment. |
| Capital call notice | GP, administrator, investor portal, operations intake. | Cash reservation, paid-in, unfunded, funding calendar. | Missing due date, duplicate notice, disputed amount. | Capital call reserves cash before it becomes paid-in. |
| Distribution notice | GP, administrator, custodian, operations classification. | Income, return of capital, realized gain, recallable capital. | Unclassified distribution, estimated split, late correction. | Distribution is not all income unless source classification says so. |
| NAV and valuation date | Administrator, GP, valuation committee, capital account statement. | Market value, performance, allocation, multiples. | Stale NAV, restated NAV, valuation uncertainty. | NAV restatement preserves prior value and recalculation lineage. |
| Cashflow history | Custodian, administrator, internal ledger, migrated history. | IRR, TVPI, DPI, RVPI, MWR. | Partial history, missing terminal value, migrated-only summary. | IRR is unavailable or partial when dated history is incomplete. |

Reports should show commitment, paid-in, unfunded, recallable, NAV date, received date, distribution classification, restatement state, liquidity terms, and partial-history labels.

## Insurance And Annuities: Policy Value Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Policy parties and roles | Insurer, policy admin, onboarding documents. | Owner, insured, beneficiary, payer, advisor, access control. | Restricted beneficiary, missing role, role conflict. | Beneficiary data is hidden when permission is insufficient. |
| Premium schedule and status | Insurer, cash ledger, policy admin. | Premium due, grace period, lapse risk, cash planning. | Missed premium, grace state, disputed payment. | Missed premium creates visible grace/lapse workflow. |
| Account/cash/surrender value | Insurer quote, policy statement, policy admin. | Market/reporting value, surrender decision, liquidity. | Stale quote, missing charge, loan not included. | Surrender value deducts charges and loans; death benefit is not accessible cash. |
| Guarantee and projection basis | Insurer illustration, product terms. | Client explanation, retirement projection, guarantee label. | Projection treated as guarantee, stale illustration. | Projected non-guaranteed value is labelled separately from guaranteed value. |
| Claim, maturity, annuity payout | Insurer, claims workflow, policy admin. | Cashflow schedule, income reporting, lifecycle status. | Pending claim, unsupported payout schedule. | Annuity income is reported as income stream, not liquid market value. |

Reports should separate account value, surrender value, death benefit, guarantee basis, policy loan, premium status, beneficiary restrictions, and payout schedule.

## Lending, Lombard, Margin And Collateral: Pledge Graph Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Facility terms | Credit system, loan system, legal agreement. | Facility limit, maturity, rate, covenants, utilization. | Expired facility, missing limit, unsupported covenant. | Availability fails closed when facility status is stale or suspended. |
| Pledge relationship | Collateral system, legal pledge records, custodian. | Eligible collateral, pledge direction, cross-pledge, release. | Missing pledge, disputed quantity, unsupported cross-pledge. | Collateral from one entity is not reused for another without explicit legal linkage. |
| Haircut and eligibility | Credit policy, collateral system, market data, rating feed. | Lending value, availability, margin call, stress. | Stale price, missing FX, stale rating, unsupported asset. | Rating downgrade changes haircut and can trigger shortfall. |
| Reservations and in-flight orders | Order system, credit system, cash ledger. | Buying power, execution readiness, pre-trade checks. | Duplicate reservation, missing release, stale pending order. | Proposed buy reserves capacity and blocks double-use of the same headroom. |
| Margin call and cure | Credit workflow, advisor workflow, operations. | Shortfall, cure deadline, escalation, liquidation risk. | Missing cure option, disputed call, expired deadline. | Uncured shortfall escalates and reports permitted cures. |

Reports should show facility limit, drawn exposure, collateral market value, eligible value, haircut, lending value, availability, pledge scope, reservations, shortfall, and cure deadline.

## Wealth Structuring And Family Reporting: Authority Ownership

| Source Truth | Owner Candidates | Used For | Degraded State | QA Scenario |
|---|---|---|---|---|
| Legal owner and beneficial owner | KYC, trust documents, corporate registry, onboarding. | Reporting perimeter, concentration, access control. | Unknown owner, stale registry, restricted document. | Consolidated report excludes or masks restricted entity data. |
| Authority to act | Trustee mandate, power of attorney, board resolution, executor authority. | Advisory consent, DPM authority, trading permission. | Expired authority, missing signatory, disputed role. | Trade/rebalance blocks when approving party lacks authority. |
| Entity hierarchy | Family-office records, trust/company documents, onboarding. | Consolidated reporting, exposure rollup, tax/regulatory reporting. | Missing entity link, circular ownership, stale hierarchy. | Family exposure aggregates only authorized entities. |
| Restricted documents and sensitive fields | Document vault, compliance records, insurer/trustee. | Report visibility, export controls, advisor view. | Permission-limited, legal hold, redacted field. | Export removes restricted beneficiary or trustee details. |
| Pledge or encumbrance by entity | Collateral system, legal pledge, custodian. | Available liquidity, lending, reporting caveat. | Missing pledge direction, unsupported transitive pledge. | Holding-company pledge does not imply personal-account pledge. |

Reports should show consolidation basis, included entities, excluded entities, role authority, restricted sections, beneficial-owner caveats, pledge state, and audience-specific access labels.

## Promotion Checklist For Product-Specific Support

Before promoting any product-specific feature from future candidate to supported:

1. Name the source owner and backup source.
2. Define the source timestamp and freshness policy.
3. Define the source conflict and correction process.
4. Define the calculation method, convention, rounding, and unsupported input behavior.
5. Define report labels for confirmed, projected, stale, estimated, partial, restricted, and unsupported states.
6. Define API/UI behavior for missing or stale inputs.
7. Add happy-path, stale-source, missing-source, conflicting-source, restatement, permission, and unsupported-subtype QA scenarios.
8. Preserve audit lineage for source values, overrides, recalculations, and report output.
9. Document downstream consumers: advisory, DPM, reporting, risk, performance, operations, or migration.
10. Keep future candidate language until source ownership and QA evidence exist.

## Disclaimer

This guide is for education, product analysis, documentation, platform design, reporting design, QA, operations, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, fiduciary, or client advice.
