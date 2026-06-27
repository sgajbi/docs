# 10 - Worked Examples and Implementation Patterns

Use these examples when modelling listed REITs, business trusts, private real estate funds, direct property, infrastructure funds, distributions, appraisals, leverage, liquidity, advisory, DPM, reporting, QA, and implementation boundaries.

## 1. Wrapper Versus Economic Exposure

| Wrapper | Operational model | Economic exposure | Main source owner | Platform warning |
|---|---|---|---|---|
| Listed REIT | equity-like trade and settlement | property sectors, leverage, rental income | exchange, issuer, corporate-action source | Do not model as direct property. |
| REIT ETF/fund | fund/security position | basket of REITs/property securities | fund admin or exchange | Look-through may lag NAV/price. |
| Private real estate fund | commitment/NAV/private fund model | properties, leverage, appraisal marks | fund manager/admin | NAV and valuation date often lag. |
| Direct property | asset record/document-heavy model | specific property | appraisal/custody/legal records | Needs ownership, valuation and document controls. |
| Infrastructure fund | fund/private fund/security model | assets, concessions, utilities, renewables | manager/admin/exchange | Contract/regulatory risk matters. |
| Structured property exposure | structured product model | index/property/REIT reference | issuer/calculation agent | Underlying is exposure, not held property. |

Design rule:

```text
Accounting wrapper drives booking.
Economic exposure drives allocation, risk, concentration and explanation.
```

## 2. Listed REIT Distribution

Scenario:

- Client holds 10,000 REIT units.
- Distribution per unit: SGD 0.035.
- Withholding/tax adjustment: SGD 50.

Calculation:

```text
gross distribution = 10,000 x 0.035 = 350
net cash = 350 - 50 = 300
```

Platform treatment:

- entitlement date must use exchange/corporate-action source;
- distribution should be classified according to available source detail;
- withholding/tax should not be treated as market loss;
- distribution yield should use a clear denominator and period basis;
- REIT income should remain separate from property valuation movement.

## 3. REIT Rights Issue

Scenario:

- Client holds 5,000 REIT units.
- Rights ratio: 1 new unit for every 5 existing units.
- Subscription price: SGD 1.20.

Entitlement:

```text
rights entitlement = 5,000 / 5 = 1,000 rights
subscription cash required = 1,000 x 1.20 = 1,200
```

Controls:

- capture election options and deadline;
- model rights as entitlement until exercised/sold/lapsed;
- if exercised, book new units and cash debit;
- if sold, book sale proceeds;
- if lapsed, record lapse with audit trail;
- ensure DPM/advisory authority for election is clear.

## 4. Private Real Estate Capital Call

Scenario:

- Commitment: USD 750,000.
- Capital call: 15%.
- Due date: 10 business days.

Calculation:

```text
cash required = 750,000 x 15% = 112,500
remaining unfunded commitment = 750,000 - 112,500 = 637,500
```

Implementation:

- store capital call notice;
- update paid-in and unfunded commitment;
- create cash-planning alert;
- do not update NAV until manager/admin valuation is sourced;
- report unfunded commitment as future obligation.

## 5. Appraisal Restatement

Scenario:

- Property fund reports NAV of USD 1,200,000 for Q2.
- Later appraisal restates NAV to USD 1,150,000.
- Published client report used the original NAV.

Required workflow:

```text
receive restatement -> preserve original NAV -> store restated NAV -> assess report impact -> recalculate analytics -> disclose or correct according to policy
```

Controls:

- valuation date and received date must be separate;
- restatement reason should be captured;
- performance and allocation should be recalculated for impacted periods;
- stale or restated values should be visible in client and advisor review.

## 6. Infrastructure Fund Distribution

Scenario:

- Infrastructure fund distribution: USD 40,000.
- Source classification:
  - operating income: USD 25,000;
  - return of capital: USD 10,000;
  - realized gain: USD 5,000.

Treatment:

| Component | Reporting treatment |
|---|---|
| operating income | income section where policy supports it |
| return of capital | capital return / basis reduction where applicable |
| realized gain | realized gain section |
| cash receipt | cash balance increase |

Do not show the full distribution as recurring income. Infrastructure distributions may include return of capital, refinancing proceeds, asset-sale gains or one-off items.

## 7. Redemption Queue

Scenario:

- Client requests redemption of USD 300,000 from a semi-liquid real estate fund.
- Fund accepts USD 90,000 in current window.
- USD 210,000 remains queued.

Platform states:

- requested redemption;
- accepted amount;
- queued/deferred amount;
- expected next window;
- settlement date;
- remaining NAV exposure;
- gate/queue source notice.

Reporting:

- do not show queued amount as settled cash;
- show remaining exposure until units are redeemed;
- flag liquidity constraint for advisor and DPM monitoring.

## 8. Direct Property Valuation

Scenario:

- Client has direct property exposure through a reportable entity.
- Latest appraisal: SGD 5,000,000.
- Appraisal date: 2026-03-31.
- Report date: 2026-06-30.

Treatment:

- show appraisal value with appraisal date;
- do not imply daily valuation;
- store property identifier, ownership share, currency, valuation source and document reference;
- if ownership is indirect through a company/trust, report ownership route and access restrictions;
- apply FX translation using report-date FX while preserving appraisal currency/date.

## 9. Leverage And Interest-Rate Sensitivity

Scenario:

- REIT total assets: SGD 10 billion.
- Debt: SGD 4 billion.
- Gearing: 40%.
- Interest cost rises by 1%.

Simplified annual interest impact:

```text
incremental interest cost = 4,000,000,000 x 1% = 40,000,000
```

Analytics implication:

- listed REIT price risk includes equity-market behavior and property fundamentals;
- distribution sustainability depends on rental income, financing cost, occupancy, debt maturity and refinancing risk;
- reporting should not describe REIT distribution yield as bond coupon;
- mandate risk should include sector/geography/gearing and interest-rate sensitivity where available.

## 10. Property-Level Debt Maturity Ladder

Scenario:

- A property vehicle owns three assets.
- Property-level debt is financed through separate loans.
- Refinancing risk matters because interest rates have risen.

| Property | Debt balance | Maturity | Interest rate |
|---|---:|---|---:|
| Office Tower A | 20,000,000 | 2027 | 3.20% |
| Retail Centre B | 15,000,000 | 2029 | 4.10% |
| Logistics Park C | 25,000,000 | 2031 | 4.50% |

Debt maturity view:

| Bucket | Debt balance | Portfolio share |
|---|---:|---:|
| 0 to 2 years | 20,000,000 | 33.3% |
| 3 to 5 years | 15,000,000 | 25.0% |
| 6+ years | 25,000,000 | 41.7% |

Reporting treatment:

- show debt maturity as property-level operating risk, not client loan exposure unless the client is directly liable;
- include floating/fixed-rate split and hedging where source-backed;
- label missing loan maturity or covenant data as incomplete;
- connect refinancing risk to income sustainability and valuation sensitivity.

## 11. Tenant Concentration

Scenario:

A property fund reports rental income concentration by tenant.

| Tenant | Annual rent |
|---|---:|
| Tenant A | 4,000,000 |
| Tenant B | 2,500,000 |
| Tenant C | 1,500,000 |
| Other tenants | 12,000,000 |

Calculation:

```text
total annual rent = 20,000,000
top tenant concentration = 4,000,000 / 20,000,000 = 20.0%
top three tenant concentration = (4,000,000 + 2,500,000 + 1,500,000) / 20,000,000 = 40.0%
```

Platform treatment:

- source tenant concentration from manager/property reports;
- classify concentration by tenant, sector, lease expiry and geography where available;
- use concentration in risk and mandate monitoring, not as accounting position data;
- hide or aggregate tenant names where confidentiality restrictions apply.

## 12. Occupancy Change And Income Impact

Scenario:

Occupancy falls from 95% to 88% in a property portfolio with potential gross rent of 30,000,000.

Calculation:

```text
prior occupied rent base = 30,000,000 x 95% = 28,500,000
current occupied rent base = 30,000,000 x 88% = 26,400,000
estimated annual rent reduction = 2,100,000
```

Reporting and analytics:

- occupancy is an operating metric, not a cash transaction by itself;
- income projections, valuation assumptions and distribution sustainability may change;
- valuation impact should be source-backed by appraisal, manager estimate or analyst model;
- report occupancy date and source quality.

## 13. Lease Expiry And Weighted Average Lease Expiry

Scenario:

A REIT reports lease expiries for three properties.

| Lease group | Annual rent | Years to expiry |
|---|---:|---:|
| Group A | 6,000,000 | 1 |
| Group B | 9,000,000 | 3 |
| Group C | 5,000,000 | 6 |

Calculation:

```text
WALE = sum(annual rent x years to expiry) / total annual rent
WALE = (6,000,000 x 1 + 9,000,000 x 3 + 5,000,000 x 6) / 20,000,000
WALE = 3.15 years
```

Correct treatment:

- disclose whether WALE is based on rent, area or another basis;
- connect near-term lease expiries to vacancy, renewal, rent reversion and valuation risk;
- use lease expiry buckets in DPM concentration and income-risk review where source-backed;
- label stale lease data before using it in client-ready reporting.

## 14. Infrastructure Concession Renewal

Scenario:

An infrastructure fund owns a toll-road concession expiring in eight years. Renewal is possible but not guaranteed.

| Attribute | Value |
|---|---|
| Remaining concession term | 8 years |
| Revenue model | Regulated toll revenue |
| Inflation linkage | Partial |
| Renewal status | Not yet confirmed |

Platform treatment:

- store concession expiry as an operating-risk field, not as a security maturity unless the instrument legally matures;
- distinguish contractual cashflow period from valuation assumption;
- report renewal uncertainty and regulatory dependency;
- avoid treating projected post-renewal cashflows as guaranteed income.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Renewal is unconfirmed | Reporting labels post-expiry assumptions as projected/uncertain. |
| Regulator changes tariff rules | Valuation and income assumptions carry source and effective date. |
| Concession expires | Exposure, valuation and income projections update from source event. |

## 15. Renewable Power-Price Exposure

Scenario:

An infrastructure vehicle owns renewable assets with part of generation under fixed power-purchase agreements and part exposed to merchant power prices.

| Revenue type | Share of expected revenue |
|---|---:|
| Fixed PPA | 70% |
| Merchant power price | 30% |

Risk treatment:

| Area | Correct treatment |
|---|---|
| Income stability | Fixed PPA portion is more contracted, subject to counterparty and contract terms. |
| Market exposure | Merchant portion is sensitive to power prices, curtailment and production volume. |
| Inflation | Indexation applies only if contract terms support it. |
| Reporting | Do not describe all renewable revenue as stable contracted income. |

QA assertions:

| Scenario | Expected behavior |
|---|---|
| PPA counterparty rating changes | Counterparty risk and income-quality labels update. |
| Merchant price source is stale | Revenue sensitivity is stale or blocked. |
| Production volume is missing | Power-price exposure is labelled partial. |

## 16. Direct-Property Ownership Transfer

Scenario:

A family holding company transfers a 40% ownership interest in a direct property to a trust.

| Attribute | Before | After |
|---|---:|---:|
| Holding company ownership | 100% | 60% |
| Trust ownership | 0% | 40% |
| Appraised property value | 5,000,000 | 5,000,000 |

Calculation:

```text
trust reportable value = 5,000,000 x 40% = 2,000,000
holding company reportable value = 5,000,000 x 60% = 3,000,000
```

Implementation treatment:

- transfer requires legal document, effective date, ownership percentage and valuation basis;
- do not duplicate 100% property value in both owner hierarchies;
- preserve historical reporting lineage before and after transfer;
- access control, beneficial ownership, tax reporting and collateral eligibility may change.

## 17. Development Project Cost-To-Complete

Scenario:

A private real estate vehicle owns a development project that is not yet income producing.

| Attribute | Value |
|---|---:|
| Budgeted development cost | 80,000,000 |
| Cost incurred to date | 45,000,000 |
| Committed but unpaid contracts | 20,000,000 |
| Contingency reserve | 5,000,000 |

Calculation:

```text
remaining budget before commitments = 80,000,000 - 45,000,000 = 35,000,000
cost-to-complete including commitments and contingency = 20,000,000 + 5,000,000 = 25,000,000
uncommitted budget headroom = 35,000,000 - 25,000,000 = 10,000,000
```

Treatment:

- development exposure should be labelled separately from stabilized property exposure;
- costs incurred, committed costs and contingency should not be confused with current appraisal value;
- delays, permits, funding gaps and cost overruns affect risk before they become distributions or NAV changes;
- DPM and advisory reviews should treat construction and leasing risk as distinct risk factors.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Cost-to-complete exceeds remaining budget | Project risk and funding shortfall are flagged. |
| Appraisal is based on completion value | Report labels valuation assumption and does not imply current sale price. |
| Permit is pending | Development milestone status remains source-limited. |

## 18. Construction Drawdown And Commitment Funding

Scenario:

A property development fund issues a construction drawdown notice against investor commitments.

| Attribute | Value |
|---|---:|
| Investor commitment | 500,000 |
| Prior paid-in capital | 200,000 |
| Drawdown percentage | 12% |
| Due date | 10 business days |

Calculation:

```text
cash required = 500,000 x 12% = 60,000
paid-in after drawdown = 200,000 + 60,000 = 260,000
unfunded after drawdown = 500,000 - 260,000 = 240,000
```

Implementation treatment:

- drawdown notice creates a cash obligation and liquidity-planning event;
- paid-in and unfunded commitment update only when the drawdown is accepted or settled according to policy;
- drawdown purpose, project, due date and notice source should be retained;
- late funding, partial funding or waived funding should produce explicit exception states.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Drawdown notice is missing due date | Cash planning is incomplete and escalation is required. |
| Cash is insufficient | Funding shortfall workflow opens before due date. |
| Drawdown is project-specific | Exposure and paid-in tracking preserve project reference. |

## 19. Rent-Free Period And Lease Incentive

Scenario:

A new office lease has headline rent but includes a rent-free period.

| Attribute | Value |
|---|---:|
| Lease term | 5 years |
| Annual headline rent | 1,200,000 |
| Rent-free period | 6 months |
| Total lease months | 60 |

Calculation:

```text
gross headline rent over term = 1,200,000 x 5 = 6,000,000
rent-free value = 1,200,000 x 6 / 12 = 600,000
effective rent over term = 6,000,000 - 600,000 = 5,400,000
effective annual rent = 5,400,000 / 5 = 1,080,000
```

Treatment:

- headline rent, effective rent and cash rent should be labelled separately;
- occupancy may improve before cash rent starts;
- valuation and income projections should use the source-backed lease convention;
- client reporting should avoid presenting headline rent as immediate recurring cash income.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Rent-free period exists | Effective rent and cash timing are visible. |
| Lease incentive is missing | Income projection is labelled partial. |
| Rent starts after report date | No cash income is booked before rent commencement. |

## 20. Capex Reserve For Property Refurbishment

Scenario:

A real estate fund reserves capital for refurbishment before reletting space.

| Attribute | Value |
|---|---:|
| Property NAV before reserve | 30,000,000 |
| Approved capex budget | 2,500,000 |
| Reserve funded this period | 1,000,000 |
| Expected remaining reserve | 1,500,000 |

Reporting treatment:

```text
available distributable cash impact = -1,000,000 funded reserve
remaining committed reserve = 1,500,000
```

Treatment:

- capex reserve is not an investment loss by itself;
- reserve funding may reduce distributable cash while preserving or improving asset value;
- project budget, approval source, draw schedule and remaining commitment should be visible;
- cost overruns should be tracked separately from approved reserve usage.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Reserve is funded | Cash and distributable amount decrease with reserve lineage. |
| Capex invoice arrives | Invoice is matched to approved budget and reserve. |
| Budget is exceeded | Overrun exception is raised and valuation assumptions are reviewed. |

## 21. Direct Property Sale Completion

Scenario:

A direct property sale completes after signing and closing conditions are satisfied.

| Attribute | Value |
|---|---:|
| Sale price | 6,200,000 |
| Selling costs | 120,000 |
| Outstanding property loan payoff | 2,000,000 |
| Prior appraisal value | 6,000,000 |

Calculation:

```text
net sale proceeds before loan payoff = 6,200,000 - 120,000 = 6,080,000
net cash after loan payoff = 6,080,000 - 2,000,000 = 4,080,000
sale premium versus appraisal = 6,200,000 - 6,000,000 = 200,000
```

Implementation treatment:

- signed sale, conditional sale and completed sale are different states;
- remove or reduce property exposure only after completion evidence;
- payoff of property-level debt is separate from selling costs and net proceeds;
- tax, ownership hierarchy, collateral pledge and reporting entitlement may change after completion.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Sale contract signed but not completed | Property remains held with pending sale state. |
| Loan payoff differs from estimate | Net proceeds are adjusted with settlement lineage. |
| Pledged property is sold | Collateral release or substitution workflow is required. |

## 22. Valuation Committee Override

Scenario:

A valuation committee approves an override to a stale appraisal because a material tenant default occurred after the appraisal date.

| Attribute | Value |
|---|---:|
| Latest appraisal value | 40,000,000 |
| Committee override haircut | 8% |
| Override expiry | 60 days |

Calculation:

```text
override value = 40,000,000 x (1 - 8%) = 36,800,000
valuation adjustment = -3,200,000
```

Controls:

- preserve original appraisal and committee-adjusted value separately;
- record approver, reason, evidence, effective date and expiry;
- label the value as committee-adjusted rather than appraiser-confirmed;
- expire the override unless refreshed by new appraisal or renewed committee decision.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Override lacks approver | Client-ready reporting is blocked or escalated. |
| Override expires | Valuation degrades until new source evidence is available. |
| New appraisal arrives | Override is closed or superseded with lineage. |

## 23. Green-Building Certification Impact

Scenario:

A property receives a green-building certification that affects financing spread and tenant demand analytics.

| Attribute | Before | After |
|---|---:|---:|
| Loan balance | 20,000,000 | 20,000,000 |
| Financing spread | 2.20% | 2.05% |
| Annual energy-cost saving estimate | - | 150,000 |

Calculation:

```text
annual financing cost reduction = 20,000,000 x (2.20% - 2.05%) = 30,000
combined annual operating benefit estimate = 30,000 + 150,000 = 180,000
```

Treatment:

- certification status should be source-backed with effective date and expiry/renewal status;
- financing benefit should be separated from operating-cost estimate;
- do not treat certification as valuation uplift unless appraisal or model source supports it;
- sustainability labels should preserve source and standard used.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Certification expires | Sustainability and financing labels degrade or update. |
| Energy saving is estimated | Report labels assumption separately from realized cashflow. |
| Financing spread changes | Interest-cost analytics update from loan source. |

## 24. Infrastructure Revenue Clawback

Scenario:

A regulated infrastructure asset exceeds allowed revenue and must return part of the excess through a future tariff adjustment.

| Attribute | Value |
|---|---:|
| Allowed annual revenue | 50,000,000 |
| Actual annual revenue | 54,000,000 |
| Clawback percentage on excess | 75% |

Calculation:

```text
excess revenue = 54,000,000 - 50,000,000 = 4,000,000
clawback obligation = 4,000,000 x 75% = 3,000,000
retained excess before tax/fees = 1,000,000
```

Treatment:

- clawback is a regulatory or contractual adjustment, not ordinary recurring income;
- future revenue projections should reflect tariff reduction or payable obligation;
- source should identify regulator/contract term, calculation period, effective date and dispute state;
- distributions funded by excess revenue may not be repeatable.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Clawback notice arrives | Obligation and future revenue impact are visible. |
| Clawback is disputed | Report labels obligation as disputed with source state. |
| Distribution uses excess revenue | Income-quality explanation distinguishes one-off excess from sustainable revenue. |

## 25. Zoning Change And Development Rights Revaluation

Scenario:

A direct property receives zoning approval that increases permitted gross floor area, but the project still requires construction financing and municipal permits.

| Attribute | Value |
|---|---:|
| Prior appraised value | 30,000,000 |
| Appraiser uplift from zoning approval | 12% |
| Estimated incremental development cost | 2,500,000 |

Calculation:

```text
zoning_adjusted_value = 30,000,000 x (1 + 12%) = 33,600,000
net_development_option_value = 3,600,000 - 2,500,000 = 1,100,000
```

Treatment:

- do not treat zoning approval as completed development income;
- separate appraised uplift, required capex, permits and execution risk;
- preserve approval date, expiry, restrictions and source document;
- review loan covenants, insurance, liquidity planning and mandate concentration after material uplift.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Zoning is proposed but not approved | No valuation uplift is finalized. |
| Appraiser has not reflected zoning | Analytics can show pending event, but client-ready valuation remains source-limited. |
| Development cost is missing | Net value impact is labelled incomplete. |

## 26. Compulsory Acquisition Or Eminent Domain

Scenario:

A government authority compulsorily acquires 20% of a property lot for transport infrastructure.

| Attribute | Value |
|---|---:|
| Pre-acquisition appraised value | 10,000,000 |
| Acquired portion | 20% |
| Compensation offered | 2,300,000 |
| Estimated legal and advisory costs | 100,000 |

Calculation:

```text
book_value_of_acquired_portion = 10,000,000 x 20% = 2,000,000
net_compensation = 2,300,000 - 100,000 = 2,200,000
gain_before_tax = 2,200,000 - 2,000,000 = 200,000
remaining_property_basis = 10,000,000 - 2,000,000 = 8,000,000
```

Treatment:

- record legal notice, effective date, appeal/dispute status and compensation source;
- reduce property exposure only for the acquired portion after legal effect;
- separate compensation receivable, costs, realized gain/loss and remaining property value;
- review collateral, insurance, lease terms and reporting perimeter after acquisition.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Compensation is offered but disputed | Receivable and gain remain disputed/provisional. |
| Only part of property is acquired | Remaining property stays active with adjusted basis. |
| Loan collateral relied on full property | Collateral review workflow opens. |

## 27. Property Insurance Claim And Repair Reserve

Scenario:

A property suffers storm damage. Insurance covers part of repair cost, but cash is needed before final insurer settlement.

| Attribute | Value |
|---|---:|
| Estimated repair cost | 900,000 |
| Deductible | 100,000 |
| Insurer approved coverage | 650,000 |
| Initial repair reserve | 300,000 |

Calculation:

```text
expected_insurance_recovery = 650,000
net_owner_cost_before_overruns = 900,000 - 650,000 = 250,000
reserve_surplus = 300,000 - 250,000 = 50,000
```

Treatment:

- separate repair expense, insurance receivable, deductible and reserve movements;
- do not treat insurance recovery as received cash before settlement;
- preserve claim status, adjuster report, coverage decision and repair progress;
- review valuation, occupancy, rent interruption and covenant impact.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Insurer approval is pending | Recovery is estimated or blocked by policy. |
| Repair cost exceeds estimate | Reserve shortfall and valuation review are triggered. |
| Insurance proceeds are received | Receivable closes and cash movement links to claim id. |

## 28. Environmental Remediation Obligation

Scenario:

Environmental assessment identifies contamination requiring remediation before sale or redevelopment.

| Attribute | Value |
|---|---:|
| Current appraisal before remediation | 18,000,000 |
| Estimated remediation cost | 1,400,000 |
| Contingency | 15% |

Calculation:

```text
remediation_reserve = 1,400,000 x (1 + 15%) = 1,610,000
environmentally_adjusted_value = 18,000,000 - 1,610,000 = 16,390,000
```

Treatment:

- track environmental report, regulator order, remediation plan, cost estimate and completion evidence;
- separate valuation adjustment from actual cash spend until invoices are paid;
- flag sale restrictions, lending constraints and suitability implications;
- review whether the obligation belongs to property owner, tenant, fund or prior owner.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Remediation estimate lacks source | Adjustment is blocked or labelled provisional. |
| Regulator order changes scope | Reserve and valuation basis are effective-dated. |
| Sale report is generated | Environmental obligation and liquidity restriction are disclosed where required. |

## 29. Property Loan Covenant Breach

Scenario:

A property loan has a maximum loan-to-value covenant of 60%. Property value falls after appraisal.

| Attribute | Before | After |
|---|---:|---:|
| Loan balance | 12,000,000 | 12,000,000 |
| Appraised value | 22,000,000 | 18,500,000 |
| Covenant maximum LTV | 60% | 60% |

Calculation:

```text
current_ltv = 12,000,000 / 18,500,000 = 64.86%
required_value_at_60_ltv = 12,000,000 / 60% = 20,000,000
value_shortfall = 20,000,000 - 18,500,000 = 1,500,000
```

Treatment:

- distinguish internal risk threshold from formal lender covenant;
- open breach, waiver, cure, equity injection or asset-sale workflow as applicable;
- update collateral value, liquidity planning and client/advisor reporting;
- preserve covenant source, test date, cure period and lender response.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Covenant breach is inside cure period | Status is breach-pending-cure, not automatically defaulted. |
| Waiver is received | Waiver terms and expiry are stored. |
| Appraisal is stale | Covenant calculation is blocked or labelled stale according to policy. |

## 30. Tenant Default Workout

Scenario:

A major tenant representing 18% of rent defaults and enters restructuring discussions.

| Attribute | Value |
|---|---:|
| Annual rent from tenant | 1,800,000 |
| Total annual rent | 10,000,000 |
| Expected recoverable rent during workout | 40% |

Calculation:

```text
tenant_concentration = 1,800,000 / 10,000,000 = 18%
annual_rent_at_risk = 1,800,000 x (1 - 40%) = 1,080,000
estimated_total_rent_after_workout = 10,000,000 - 1,080,000 = 8,920,000
```

Treatment:

- separate tenant default status, rent arrears, negotiated concession and lease termination;
- update concentration, occupancy, WALE, valuation assumptions and distribution quality;
- do not book expected recoveries until source-backed;
- preserve confidentiality controls for tenant identity where required.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Tenant default is material | Valuation and income-quality review opens. |
| Tenant identity is restricted | Report masks tenant name but preserves concentration metric. |
| Workout terms are agreed | Rent schedule and impairment assumptions update with lineage. |

## 31. Infrastructure Availability Deduction

Scenario:

An availability-based infrastructure asset misses performance standards and suffers a deduction from contracted payment.

| Attribute | Value |
|---|---:|
| Contracted quarterly availability payment | 5,000,000 |
| Availability deduction | 6% |
| Remediation cost estimate | 250,000 |

Calculation:

```text
payment_deduction = 5,000,000 x 6% = 300,000
net_quarter_impact_before_tax = 300,000 + 250,000 = 550,000
```

Treatment:

- separate payment deduction from remediation cost;
- preserve contract standard, measurement period, operator report and dispute state;
- update income projection, distribution quality and operator performance analytics;
- avoid treating deduction as recurring unless performance issue persists.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Deduction is disputed | Income impact is reported with disputed state. |
| Remediation cost is estimated | Cost remains estimate until invoice or budget approval. |
| Performance failure repeats | Analytics identify recurring operator or asset-quality issue. |

## 32. Regulated Asset Base Reset

Scenario:

A utility infrastructure asset receives a regulated asset base (RAB) reset that changes allowed return.

| Attribute | Before | After |
|---|---:|---:|
| Regulated asset base | 100,000,000 | 108,000,000 |
| Allowed return | 5.0% | 4.6% |

Calculation:

```text
allowed_return_before = 100,000,000 x 5.0% = 5,000,000
allowed_return_after = 108,000,000 x 4.6% = 4,968,000
allowed_return_delta = -32,000
```

Treatment:

- separate RAB growth from allowed-return percentage change;
- preserve regulator determination, effective date, appeal period and inflation indexation basis;
- update valuation, income projection and distribution quality assumptions;
- report whether the reset is final, appealed or provisional.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Reset is provisional | Valuation and income projection are labelled provisional. |
| RAB increases but allowed return falls | Analytics show net allowed-return impact, not only asset-base growth. |
| Appeal succeeds | Historical projections are superseded with lineage. |

## 33. Ground-Lease Reset

Scenario:

A property is held on a long-term ground lease. The rent resets every ten years based on appraised land value. The reset materially changes property-level distributable income.

| Attribute | Before reset | After reset |
|---|---:|---:|
| Annual ground rent | 600,000 | 950,000 |
| Net operating income before ground rent | 4,800,000 | 4,800,000 |
| Cap rate | 5.50% | 5.50% |

Calculation:

```text
noi_after_old_ground_rent = 4,800,000 - 600,000 = 4,200,000
noi_after_new_ground_rent = 4,800,000 - 950,000 = 3,850,000
value_impact = (3,850,000 - 4,200,000) / 5.50% = -6,363,636
```

Treatment:

- separate ground rent from operating expenses and financing costs;
- preserve reset notice, appraisal basis, effective date, dispute state and lease terms;
- update income projection, valuation, debt covenant metrics and distribution quality;
- do not change legal ownership or property quantity because only lease economics changed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Reset notice is provisional | Valuation impact is labelled provisional. |
| Reset is disputed | Old and proposed ground-rent bases remain visible. |
| Report is generated | Income yield explains ground-rent reset impact separately from market movement. |

## 34. Lease Indexation Dispute

Scenario:

A tenant lease includes annual CPI indexation. The landlord and tenant disagree on whether a capped or uncapped CPI rate applies.

| Attribute | Landlord view | Tenant view |
|---|---:|---:|
| Current annual rent | 1,200,000 | 1,200,000 |
| CPI increase | 7.00% | 7.00% |
| Contractual cap asserted | None | 4.00% |

Calculation:

```text
landlord_indexed_rent = 1,200,000 x (1 + 7.00%) = 1,284,000
tenant_indexed_rent = 1,200,000 x (1 + 4.00%) = 1,248,000
disputed_annual_rent = 36,000
```

Treatment:

- preserve lease clause, CPI source, cap/floor interpretation, dispute owner and effective date;
- use accepted rent for cash projection and disputed rent for scenario analysis until resolved;
- update valuation assumptions only when dispute state and policy allow;
- avoid booking disputed incremental rent as receivable before entitlement is confirmed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Cap interpretation is unresolved | Rent forecast is disputed/source-limited. |
| Tenant pays capped rent | Cash ledger uses paid amount; disputed receivable requires policy support. |
| Dispute resolves | Lease rent schedule is versioned with lineage. |

## 35. Data-Centre Power Constraint

Scenario:

A data-centre asset has strong leasing demand, but utility connection limits cap rentable capacity until a grid upgrade completes.

| Attribute | Value |
|---|---:|
| Built IT load capacity | 40 MW |
| Utility power currently available | 28 MW |
| Stabilized rent per MW | 1,100,000 |
| Expected grid upgrade delay | 18 months |

Calculation:

```text
power_constrained_capacity = min(40, 28) = 28 MW
deferred_capacity = 40 - 28 = 12 MW
deferred_annual_rent_potential = 12 x 1,100,000 = 13,200,000
```

Treatment:

- distinguish physical completion from power-available leasing capacity;
- track grid connection agreement, upgrade milestone, capex, tenant commitments and delay risk;
- update valuation, income ramp, development status and concentration analytics;
- do not report full stabilized income until power capacity is source-confirmed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Building is complete but power is constrained | Stabilized income is capped by available MW. |
| Grid upgrade slips | Income ramp and valuation assumptions update with milestone lineage. |
| Tenant pre-leases deferred capacity | Lease commitment is separate from current deliverable capacity. |

## 36. Renewable Curtailment

Scenario:

A renewable infrastructure asset is forced to curtail generation because of grid congestion. The offtake contract provides partial compensation.

| Attribute | Value |
|---|---:|
| Expected generation | 120,000 MWh |
| Curtailed generation | 9,000 MWh |
| Contract price | 72 per MWh |
| Curtailment compensation | 45 per MWh |

Calculation:

```text
lost_contract_revenue = 9,000 x 72 = 648,000
curtailment_compensation = 9,000 x 45 = 405,000
net_revenue_shortfall = 243,000
```

Treatment:

- separate generated revenue, curtailed volume, compensation and disputed claims;
- preserve grid notice, offtake contract, metering file and compensation source;
- update income quality, debt service coverage and distribution sustainability;
- distinguish recurring curtailment risk from one-off grid event.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Curtailment is uncompensated | Full lost revenue remains at risk. |
| Compensation is disputed | Receivable is provisional or blocked by policy. |
| Curtailment repeats | Asset analytics flag recurring grid constraint. |

## 37. Concession Handback Obligation

Scenario:

An infrastructure concession must hand back the asset in a specified condition at expiry. Independent inspection estimates required lifecycle maintenance before handback.

| Attribute | Value |
|---|---:|
| Years to concession expiry | 4 |
| Estimated handback works | 6,500,000 |
| Existing lifecycle reserve | 3,200,000 |
| Annual additional reserve needed | 825,000 |

Calculation:

```text
reserve_shortfall = 6,500,000 - 3,200,000 = 3,300,000
annual_additional_reserve = 3,300,000 / 4 = 825,000
```

Treatment:

- track concession agreement, handback standard, inspection report, reserve policy and expiry date;
- reduce distributable cash for required reserve build-up where policy requires;
- update valuation and terminal value assumptions separately from current operating income;
- preserve whether obligation is fixed, disputed, indexed or contingent.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Inspection report is missing | Handback reserve is estimate-limited. |
| Reserve shortfall exists | Distributable cash and valuation assumptions reflect required reserve. |
| Concession is extended | Handback timing and reserve schedule are recalculated with lineage. |

## 38. Brownfield Redevelopment Overrun

Scenario:

A brownfield redevelopment project exceeds budget because of demolition and remediation surprises.

| Attribute | Original | Revised |
|---|---:|---:|
| Total budget | 25,000,000 | 31,000,000 |
| Incurred cost | 10,000,000 | 14,000,000 |
| Committed unpaid cost | 9,000,000 | 12,000,000 |
| Contingency reserve | 3,000,000 | 2,000,000 |

Calculation:

```text
original_headroom = 25,000,000 - 10,000,000 - 9,000,000 - 3,000,000 = 3,000,000
revised_headroom = 31,000,000 - 14,000,000 - 12,000,000 - 2,000,000 = 3,000,000
budget_increase = 6,000,000
```

Treatment:

- distinguish increased approved budget from remaining headroom;
- track change orders, remediation reports, contractor claims, funding approvals and completion date;
- update development exposure, liquidity planning, valuation and project risk state;
- do not treat budget increase as completed value until appraisal or completion evidence supports it.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Revised budget is not approved | Overrun is provisional and escalated. |
| Funding source is missing | Liquidity/capital-call forecast is incomplete. |
| Project valuation is reported | Development value labels cost, budget and appraisal basis separately. |

## 39. Direct-Property Co-Owner Dispute

Scenario:

A client owns 60% of a direct property with another family entity. The co-owner disputes a proposed sale and refuses to approve required documents.

| Attribute | Value |
|---|---:|
| Appraised property value | 12,000,000 |
| Client ownership | 60% |
| Reportable share before dispute discount | 7,200,000 |
| Liquidity discount under dispute scenario | 12% |

Calculation:

```text
discounted_reportable_share = 7,200,000 x (1 - 12%) = 6,336,000
liquidity_discount = 864,000
```

Treatment:

- preserve legal ownership, co-owner rights, sale authority, dispute state and document status;
- distinguish valuation of ownership share from liquidity-adjusted exit estimate;
- block sale-completion workflow until authority and documents are complete;
- disclose governance/liquidity restriction where material to client reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Co-owner approval is required but missing | Sale cannot complete. |
| Appraisal remains valid | Value can be reported while liquidity is restricted. |
| Dispute resolves | Liquidity discount and sale workflow update from effective date. |

## 40. Property Tax Reassessment

Scenario:

A direct property is reassessed by the local tax authority. The annual property tax expense increases, reducing net operating income and valuation assumptions.

| Attribute | Previous | Reassessed |
|---|---:|---:|
| Assessed value | 8,000,000 | 9,500,000 |
| Tax rate | 1.20% | 1.20% |
| Annual property tax | 96,000 | 114,000 |

Calculation:

```text
tax_increase = 114,000 - 96,000 = 18,000
monthly_noi_impact = 18,000 / 12 = 1,500
```

Treatment:

- preserve assessment notice, appeal window, effective date and tax jurisdiction;
- update net operating income, valuation assumptions and distribution forecast only from effective date;
- distinguish recurring property tax from one-off penalties or arrears;
- show appeal or disputed state separately from final accepted tax expense.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Assessment is final | Property tax expense and NOI forecast update. |
| Assessment is under appeal | Expense is provisional or scenario-labelled. |
| Report spans effective date | Old and new tax assumptions are applied by date. |

## 41. REIT Merger Consideration

Scenario:

A listed REIT is acquired by another REIT through mixed cash and unit consideration. The client receives cash and new units, with fractional units paid in cash.

| Attribute | Value |
|---|---:|
| Old REIT units | 10,000 |
| Cash consideration per old unit | 0.42 |
| New unit exchange ratio | 0.55 |
| Fractional cash adjustment | 18 |

Calculation:

```text
cash_component = 10,000 x 0.42 = 4,200
new_units = floor(10,000 x 0.55) = 5,500
total_cash = 4,200 + 18 = 4,218
```

Treatment:

- close or reduce old REIT units only after corporate-action completion evidence;
- create new REIT units from confirmed exchange ratio and identifier;
- keep cash consideration, fractional cash and tax classification separate;
- refresh sector, geography, leverage, distribution and concentration analytics for the new REIT.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Merger completion is confirmed | Old units close and new units/cash post with event lineage. |
| Exchange ratio is preliminary | New units remain estimated or pending. |
| Fractional unit exists | Fractional cash posts separately from main cash consideration. |

## 42. Property Manager Replacement

Scenario:

A private real estate fund replaces the property manager for a logistics portfolio after service issues. The assets remain the same, but operating assumptions and remediation costs change.

| Attribute | Value |
|---|---:|
| Annual gross rent | 5,200,000 |
| Old manager fee | 3.00% |
| New manager fee | 3.40% |
| One-off transition cost | 120,000 |

Calculation:

```text
old_fee = 5,200,000 x 3.00% = 156,000
new_fee = 5,200,000 x 3.40% = 176,800
annual_fee_increase = 20,800
first_year_total_impact = 20,800 + 120,000 = 140,800
```

Treatment:

- preserve manager termination notice, new appointment, effective date and transition budget;
- update operating-expense forecast, NOI, valuation and governance watchlist where material;
- do not treat manager replacement as a sale, fund restructure or property ownership change;
- track whether service-quality issues affect tenant retention, lease renewals or capex plan.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Manager changes | Ownership and asset positions remain unchanged. |
| Transition cost is approved | First-year NOI and reporting commentary reflect one-off cost. |
| New fee is effective mid-year | Fee calculation splits by effective date. |

## 43. Infrastructure Refinancing

Scenario:

An infrastructure asset refinances project debt. The new debt reduces interest cost but introduces an upfront break cost and extended maturity.

| Attribute | Old debt | New debt |
|---|---:|---:|
| Debt outstanding | 40,000,000 | 40,000,000 |
| Interest rate | 6.20% | 5.10% |
| Annual interest | 2,480,000 | 2,040,000 |
| Break/refinancing cost | - | 650,000 |

Calculation:

```text
annual_interest_saving = 2,480,000 - 2,040,000 = 440,000
payback_years = 650,000 / 440,000 = 1.48
```

Treatment:

- track old facility, new facility, maturity, covenants, rate basis and security package;
- separate recurring interest savings from one-off refinancing cost;
- update debt maturity ladder, distribution capacity, DSCR and valuation assumptions;
- preserve lender approval, legal completion date and cash settlement evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Refinancing completes | Debt terms and maturity ladder update from completion date. |
| Break cost is unpaid | Payable remains open and distributable cash is constrained. |
| New covenants differ | Covenant monitoring updates to new facility terms. |

## 44. Data-Room Due Diligence Gap

Scenario:

A direct property acquisition is under review. The data room lacks a current environmental report and final rent roll, making valuation and suitability approval incomplete.

| Diligence item | Status | Impact |
|---|---|---|
| Title report | Complete | Ownership confirmed |
| Rent roll | Draft only | Income estimate limited |
| Environmental report | Missing | Remediation risk unknown |
| Debt term sheet | Complete | Financing scenario available |

Treatment:

- track each diligence item, owner, status, materiality and decision impact;
- block final recommendation or acquisition approval when critical evidence is missing;
- allow preliminary valuation only with explicit diligence-gap labels;
- preserve data-room version, source timestamp and approval conditions.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Critical diligence item is missing | Final approval is blocked or escalated. |
| Preliminary valuation is produced | Output is labelled due-diligence-limited. |
| Data-room document is updated | Diligence status and valuation assumptions refresh with version lineage. |

## 45. Real-Asset ESG Remediation Plan

Scenario:

A real-estate asset fails an energy-efficiency threshold. The manager proposes a remediation plan that requires capex but may protect rental income and financing terms.

| Attribute | Value |
|---|---:|
| Required remediation capex | 1,800,000 |
| Expected annual energy saving | 210,000 |
| Potential green-financing margin benefit | 0.25% |
| Debt outstanding | 20,000,000 |

Calculation:

```text
annual_financing_saving = 20,000,000 x 0.25% = 50,000
annual_total_benefit = 210,000 + 50,000 = 260,000
simple_payback_years = 1,800,000 / 260,000 = 6.92
```

Treatment:

- preserve ESG assessment, remediation plan, capex approval, timeline and certification target;
- separate mandatory compliance capex from optional value-enhancement capex;
- update valuation, financing, tenant retention and mandate/ESG reporting only when source-backed;
- track risk of missed certification, cost overrun and green-financing covenant impact.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Remediation is approved | Capex reserve and project state update. |
| Certification is not yet achieved | ESG status remains remediation-in-progress, not certified. |
| Green-financing margin benefit is conditional | Benefit remains scenario-labelled until lender confirms. |

## 46. REIT Debt Covenant Waiver Package

Scenario:

A listed REIT breaches an interest-coverage covenant after rate increases. Lenders grant a temporary waiver with a fee, additional reporting and a reduced distribution policy.

| Attribute | Before breach | Waiver package |
|---|---:|---:|
| EBITDA | 90,000,000 | 90,000,000 |
| Interest expense | 38,000,000 | 38,000,000 |
| Covenant threshold | 2.50x | 2.20x temporary waiver |
| Waiver fee | - | 1,200,000 |

Calculation:

```text
interest_coverage = 90,000,000 / 38,000,000 = 2.37x
covenant_gap_to_original = 2.50x - 2.37x = 0.13x
```

Treatment:

- preserve lender waiver letter, covenant period, fee, revised threshold, reporting obligations and distribution restrictions;
- keep the REIT holding and valuation separate from loan covenant state;
- update leverage, distribution quality, watchlist and mandate risk from the effective waiver terms;
- distinguish waiver from permanent amendment, default, refinancing or asset sale.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Waiver is approved | Covenant state changes to waived with expiry and conditions. |
| Waiver fee is paid | Fee is tracked separately from recurring interest expense. |
| Waiver expires without cure | Breach workflow reopens or escalates. |

## 47. Infrastructure Concession Extension Bid

Scenario:

An infrastructure operator bids to extend a concession. The extension could add terminal value, but the bid is competitive and not yet awarded.

| Attribute | Value |
|---|---:|
| Current concession years remaining | 6 |
| Proposed extension | 8 years |
| Bid cost incurred | 2,400,000 |
| Estimated annual EBITDA if awarded | 7,500,000 |
| Probability-weighted success case | 40% |

Scenario value:

```text
undiscounted_extension_ebitda = 8 x 7,500,000 = 60,000,000
probability_weighted_ebitda = 60,000,000 x 40% = 24,000,000
```

Treatment:

- preserve bid submission, authority, bid cost, award criteria, decision date and probability basis;
- label valuation uplift as scenario or bid-contingent until concession award is legally confirmed;
- expense or capitalize bid cost only according to accounting policy and source evidence;
- update cashflow, terminal value and risk only when the concession state changes.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Bid is submitted | Bid state and cost are tracked without extending concession term. |
| Award is not confirmed | Valuation uplift remains scenario-labelled. |
| Extension is awarded | Concession term, cashflow and terminal assumptions update from effective date. |

## 48. Direct-Property Tenant-Improvement Allowance

Scenario:

A landlord grants a tenant-improvement allowance to secure a long lease. Cash outflow occurs upfront, while lease economics improve over the committed lease term.

| Attribute | Value |
|---|---:|
| Tenant-improvement allowance | 750,000 |
| Lease term | 10 years |
| Annual rent | 1,200,000 |
| Rent-free period | 3 months |

Lease economics:

```text
annualized_allowance_cost = 750,000 / 10 = 75,000
rent_free_cash_impact = 1,200,000 x 3 / 12 = 300,000
first_year_cash_impact = 750,000 + 300,000 = 1,050,000
```

Treatment:

- preserve lease, allowance approval, tenant works scope, drawdown schedule and rent-free terms;
- separate upfront cash outflow from recurring NOI and straight-line/effective rent analytics;
- update lease expiry, WALE, tenant concentration and valuation only from executed lease evidence;
- monitor unspent allowance, reimbursement claims and completion evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Allowance is approved | Capex or lease incentive reserve is created. |
| Tenant draws allowance in stages | Cashflow follows approved drawdown evidence. |
| Lease is not executed | Allowance and valuation uplift remain blocked or provisional. |

## 49. Renewable Offtake Replacement

Scenario:

A renewable project replaces an expiring power-purchase agreement with a lower-priced offtake contract. Merchant exposure decreases, but contracted revenue also falls.

| Attribute | Old offtake | New offtake |
|---|---:|---:|
| Contracted annual MWh | 180,000 | 170,000 |
| Contract price per MWh | 72 | 64 |
| Term | 5 years | 7 years |

Revenue impact:

```text
old_annual_revenue = 180,000 x 72 = 12,960,000
new_annual_revenue = 170,000 x 64 = 10,880,000
annual_revenue_reduction = 2,080,000
```

Treatment:

- preserve old contract expiry, new offtake agreement, contracted volume, price, term and credit support;
- separate price reduction from merchant-risk reduction and tenor extension;
- update valuation, DSCR, distribution capacity and revenue quality from effective contract date;
- track uncontracted volume and counterparty credit exposure.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| New offtake is signed | Revenue forecast updates from effective date. |
| Old contract expires before replacement | Merchant exposure is visible for the gap period. |
| Contracted volume changes | Revenue quality and uncontracted volume analytics update. |

## 50. Valuation Cap-Rate Shock Committee

Scenario:

A valuation committee applies a cap-rate shock to an office property after market evidence shows weaker leasing demand. The shock affects valuation but does not create a cash transaction.

| Attribute | Base case | Shock case |
|---|---:|---:|
| Stabilized NOI | 3,600,000 | 3,600,000 |
| Cap rate | 5.25% | 6.00% |
| Implied value | 68,571,429 | 60,000,000 |

Valuation impact:

```text
value_delta = 60,000,000 - 68,571,429 = -8,571,429
value_decline_percentage = 8,571,429 / 68,571,429 = 12.50%
```

Treatment:

- preserve market evidence, committee minutes, approved cap rate, effective date, rationale and expiry/review date;
- label valuation change as committee-approved or scenario-adjusted according to policy;
- do not book a realized loss unless a sale or impairment event is source-confirmed;
- update LTV, mandate concentration, performance attribution and client reporting commentary.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Committee shock is approved | Valuation uses approved cap rate with source label. |
| Shock is scenario-only | Report keeps base valuation and scenario valuation separate. |
| Sale later occurs | Realized result is calculated from sale proceeds, not shock alone. |

## 51. Property Escrow Release Dispute

Scenario:

A property sale closes with part of proceeds held in escrow for warranty claims. The buyer disputes release after identifying defects.

| Attribute | Value |
|---|---:|
| Gross sale proceeds | 12,000,000 |
| Escrow holdback | 600,000 |
| Cash released at closing | 11,400,000 |
| Disputed repair claim | 180,000 |

Escrow exposure:

```text
undisputed_escrow = 600,000 - 180,000 = 420,000
cash_received_percentage = 11,400,000 / 12,000,000 = 95.00%
```

Treatment:

- close property ownership only from legal completion evidence;
- separate received cash, escrow receivable, disputed claim and final release;
- preserve sale agreement, escrow terms, defect notice, dispute state and release approval;
- do not treat disputed escrow as available cash or final realized proceeds until released.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Sale completes with escrow | Property closes and escrow receivable is tracked separately. |
| Buyer disputes release | Disputed amount is not available cash. |
| Escrow is released | Cash posts from release date with dispute lineage. |

## 52. Direct-Property Easement Dispute

Scenario:

A direct-property holding is subject to a disputed access easement. The property remains owned and income-producing, but sale liquidity and development assumptions are impaired until the dispute is resolved.

| Attribute | Value |
|---|---:|
| Current appraisal value | 18,000,000 |
| Liquidity discount while disputed | 8.00% |
| Estimated legal reserve | 250,000 |
| Discounted reporting scenario | 16,310,000 |

Scenario value:

```text
liquidity_discount = 18,000,000 x 8.00% = 1,440,000
scenario_value_after_reserve = 18,000,000 - 1,440,000 - 250,000 = 16,310,000
```

Treatment:

- preserve title documents, easement claim, legal opinion, dispute owner, reserve and expected resolution path;
- keep legal ownership and appraisal value separate from liquidity-adjusted scenario value;
- block unsupported development or sale assumptions when access rights are unresolved;
- report dispute state without overstating impairment as a completed sale loss.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Easement dispute is open | Property remains held but liquidity/development assumptions are flagged. |
| Legal reserve is approved | Reserve is tracked separately from appraisal value. |
| Dispute resolves | Scenario discount is removed or updated only from source evidence. |

## 53. REIT Rights Issue Underwriting Failure

Scenario:

A REIT rights issue is underwritten, but an underwriter fails to take up its residual commitment. The REIT still issues subscribed units, while the shortfall affects expected proceeds, leverage reduction and market confidence.

| Attribute | Value |
|---|---:|
| Target gross proceeds | 500,000,000 |
| Valid subscriptions | 430,000,000 |
| Underwriter residual commitment | 70,000,000 |
| Failed underwriter take-up | 40,000,000 |
| Actual proceeds | 460,000,000 |

Shortfall:

```text
expected_proceeds = 500,000,000
actual_proceeds = 430,000,000 + 30,000,000 = 460,000,000
proceeds_shortfall = 40,000,000
```

Treatment:

- process client rights, subscriptions and lapsed rights from confirmed registrar data;
- separate REIT-level funding shortfall from client entitlement processing;
- update leverage, refinancing plan and distribution assumptions only from final proceeds;
- preserve underwriting agreement, failure notice, remedial plan and exchange announcement.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Client subscribes valid rights | Client units increase from accepted subscription only. |
| Underwriter fails residual take-up | REIT proceeds analytics update without changing client rights result. |
| Final proceeds are unknown | Leverage reduction and use-of-proceeds analytics remain provisional. |

## 54. Infrastructure Inflation-Index Lag

Scenario:

An infrastructure concession has tariff indexation linked to CPI with a six-month lag. Current CPI has increased sharply, but allowed revenue uses the lagged index until the next reset.

| Attribute | Current CPI | Lagged CPI |
|---|---:|---:|
| Index level | 128.0 | 123.5 |
| Base index | 120.0 | 120.0 |
| Base annual tariff revenue | 25,000,000 | 25,000,000 |

Allowed revenue:

```text
current_indexed_revenue = 25,000,000 x 128.0 / 120.0 = 26,666,667
lagged_allowed_revenue = 25,000,000 x 123.5 / 120.0 = 25,729,167
index_lag_difference = 937,500
```

Treatment:

- preserve tariff formula, base index, lag convention, reset date and regulator/funder source;
- separate current inflation exposure from legally allowed tariff revenue;
- update projections only when the lagged reset becomes effective;
- explain timing lag in performance attribution and income forecasts.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Current CPI moves before reset | Scenario analytics may show exposure, but allowed revenue uses lagged formula. |
| Reset date arrives | Allowed revenue updates from source-backed reset. |
| CPI source is restated | Revenue calculation versions prior and restated inputs. |

## 55. Property Tax Appeal Settlement

Scenario:

A property tax reassessment increases annual tax expense. The owner appeals and later settles at a lower assessed value. NOI and valuation assumptions need effective-dated correction.

| Attribute | Reassessment | Settlement |
|---|---:|---:|
| Assessed value | 42,000,000 | 38,500,000 |
| Tax rate | 1.20% | 1.20% |
| Annual tax expense | 504,000 | 462,000 |

Settlement impact:

```text
annual_tax_reduction = 504,000 - 462,000 = 42,000
value_impact_at_6_cap = 42,000 / 6.00% = 700,000
```

Treatment:

- preserve original assessment, appeal filing, settlement notice, effective date and refund/payable state;
- update recurring tax expense, NOI and valuation assumptions from the settlement effective date;
- separate tax expense correction from rent income and market cap-rate movement;
- track any refund receivable or additional tax payable as a distinct cash event.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Appeal is pending | Expense and valuation are labelled disputed or provisional by policy. |
| Settlement is confirmed | NOI and valuation assumptions update from effective date. |
| Refund is due | Receivable is tracked separately from valuation uplift. |

## 56. Renewable Repowering Capex

Scenario:

A wind farm repowers turbines to increase output and extend asset life. Capex is incurred before the uplift is fully operational, so valuation and distribution forecasts must separate approved capex, committed spend and operational uplift.

| Attribute | Value |
|---|---:|
| Approved repowering capex | 8,000,000 |
| Incurred capex | 3,200,000 |
| Expected annual EBITDA uplift | 1,450,000 |
| Remaining capex | 4,800,000 |

Payback estimate:

```text
remaining_capex = 8,000,000 - 3,200,000 = 4,800,000
simple_payback_years = 8,000,000 / 1,450,000 = 5.52
```

Treatment:

- preserve board approval, EPC contract, grid study, capex budget, incurred cost and commissioning milestones;
- separate construction-stage cash outflow from stabilized EBITDA uplift;
- do not include full uplift in recurring income until commissioning evidence exists;
- update renewable generation, curtailment, offtake and valuation assumptions from source milestones.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Capex is approved but not commissioned | Capex commitment is visible while income uplift remains scenario or provisional. |
| Costs overrun | Budget headroom and distribution capacity update. |
| Commissioning completes | Forecast EBITDA and valuation update from effective date. |

## 57. Data-Centre Tenant Power Default

Scenario:

A data-centre tenant defaults on contracted power capacity payments. The tenant occupies space, but power reservation revenue and credit exposure are impaired until cure, replacement or termination.

| Attribute | Value |
|---|---:|
| Contracted power capacity | 12 MW |
| Annual rent per MW | 950,000 |
| Expected recovery percentage | 35.00% |
| Annual rent at risk | 7,410,000 |

Rent at risk:

```text
annual_contract_rent = 12 x 950,000 = 11,400,000
annual_rent_at_risk = 11,400,000 x (1 - 35.00%) = 7,410,000
```

Treatment:

- preserve lease, power reservation agreement, default notice, cure period, security deposit and replacement-tenant plan;
- separate physical occupancy, contracted power capacity, rent receivable and credit recovery estimate;
- update tenant concentration, WALE, NOI, valuation and debt covenant analytics from default state;
- avoid booking estimated recovery as cash before receipt or settlement agreement.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Tenant defaults on power capacity | Rent-at-risk and tenant concentration update with confidentiality controls. |
| Cure period is active | Lease termination and replacement assumptions remain provisional. |
| Replacement tenant signs | Revenue forecast updates from executed contract and power availability. |

## 58. Property Title Defect Cure

Scenario:

A direct-property holding has a title defect discovered during refinancing diligence. The appraised value remains supported, but collateral eligibility and sale liquidity are restricted until the defect is cured.

| Attribute | Value |
|---|---:|
| Appraised value | 18,000,000 |
| Legal reserve estimate | 150,000 |
| Liquidity discount while uncured | 7.50% |
| Expected cure cost | 85,000 |

Scenario value:

```text
liquidity_discount = 18,000,000 x 7.50% = 1,350,000
scenario_value = 18,000,000 - 1,350,000 - 150,000 = 16,500,000
```

Treatment:

- preserve title report, defect type, legal opinion, cure plan, reserve estimate, collateral restriction and effective date;
- separate legal ownership value from liquidity-adjusted scenario value and lending collateral eligibility;
- release title restriction only after source-backed cure evidence, registry update or counsel sign-off;
- explain in reporting that appraisal value, sale liquidity and collateral value are different lenses.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Title defect is open | Property remains held, but collateral and sale-readiness labels are restricted. |
| Cure evidence is received | Restriction, reserve and liquidity discount are effective-dated and removed by policy. |
| Refinance report is generated | Appraisal, scenario value and collateral eligibility are shown separately. |

## 59. REIT Scrip Dividend Election

Scenario:

A listed REIT offers a scrip dividend election. The client elects to receive units instead of cash for part of the distribution.

| Attribute | Value |
|---|---:|
| Existing units | 12,000 |
| Cash dividend per unit | 0.045 |
| Scrip election percentage | 60.00% |
| Scrip issue price | 2.40 |

Scrip units:

```text
gross_distribution = 12,000 x 0.045 = 540
scrip_value = 540 x 60.00% = 324
scrip_units = floor(324 / 2.40) = 135
residual_cash = 324 - 135 x 2.40 = 0
cash_dividend = 540 x 40.00% = 216
```

Treatment:

- preserve issuer terms, election deadline, accepted election, issue price, tax treatment and fractional-cash policy;
- create new REIT units only after final corporate-action confirmation;
- separate cash dividend, scrip units, fractional cash, withholding and cost-basis policy;
- prevent double income where custodian sends both election and final distribution files.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Valid scrip election is accepted | Units and cash are created according to final terms. |
| Election is late or rejected | Default cash or default scrip treatment applies from issuer terms. |
| Tax reporting is generated | Cash, scrip value, withholding and cost basis reconcile to source files. |

## 60. Infrastructure Availability-Payment Benchmarking

Scenario:

A social-infrastructure asset receives availability payments. Actual deduction is compared with a benchmark peer range to identify operating underperformance.

| Attribute | Asset | Benchmark |
|---|---:|---:|
| Contracted quarterly payment | 6,000,000 | 6,000,000 |
| Deduction percentage | 3.20% | 1.25% |
| Remediation cost | 90,000 | 40,000 |

Benchmark variance:

```text
asset_deduction = 6,000,000 x 3.20% = 192,000
benchmark_deduction = 6,000,000 x 1.25% = 75,000
excess_deduction = 192,000 - 75,000 = 117,000
excess_operating_impact = 117,000 + (90,000 - 40,000) = 167,000
```

Treatment:

- preserve contract availability formula, operator report, deduction reason, peer benchmark source and remediation plan;
- separate contractual deduction, remediation cost, benchmark variance and disputed recovery;
- avoid treating benchmark underperformance as a receivable unless the concession terms create a recoverable claim;
- use benchmarking for operating review, valuation assumptions and manager challenge notes.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Deduction exceeds benchmark | Excess impact is flagged for operating review. |
| Deduction is disputed | Receivable remains provisional until source-backed settlement. |
| Benchmark source is stale | Benchmark comparison is labelled partial or blocked. |

## 61. Lease Break-Option Exercise

Scenario:

A tenant exercises a lease break option two years before contractual expiry. The property must separate contracted rent loss, reletting assumptions and break-fee cash.

| Attribute | Value |
|---|---:|
| Annual rent | 1,200,000 |
| Remaining term before break | 2 years |
| Contractual break fee | 300,000 |
| Expected downtime before reletting | 6 months |

Net rent-at-risk:

```text
gross_rent_lost = 1,200,000 x 2 = 2,400,000
expected_downtime_loss = 1,200,000 x 6 / 12 = 600,000
net_near_term_income_impact = expected_downtime_loss - 300,000 = 300,000
```

Treatment:

- preserve lease clause, notice date, notice validity, break date, break-fee terms, dilapidation claims and reletting plan;
- update WALE, occupancy, rent forecast and tenant concentration from the break effective date;
- separate break fee, dilapidation receivable, downtime loss and new lease assumptions;
- keep exercise provisional when notice validity or condition compliance is disputed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Valid break notice is received | Lease expiry and WALE update from break date. |
| Break conditions are disputed | Lease status remains provisional with dispute lineage. |
| Break fee is paid | Cash receipt is separate from recurring rent and vacancy assumptions. |

## 62. Renewable Warranty Claim

Scenario:

A solar asset suffers inverter failures covered by equipment warranty. The asset incurs repair cost before warranty recovery is approved.

| Attribute | Value |
|---|---:|
| Repair cost incurred | 420,000 |
| Deductible | 35,000 |
| Expected warranty recovery | 300,000 |
| Lost generation estimate | 180,000 |

Net exposure:

```text
recoverable_repair_cost = min(420,000 - 35,000, 300,000) = 300,000
unrecovered_repair_cost = 420,000 - 300,000 = 120,000
total_near_term_impact = 120,000 + 180,000 = 300,000
```

Treatment:

- preserve failure report, warranty terms, claim filing, deductible, repair invoice, recovery approval and generation-loss estimate;
- separate repair expense, warranty receivable, denied amount and lost generation;
- do not book expected warranty recovery as received cash until settlement confirmation;
- update availability, offtake delivery, EBITDA and valuation assumptions from outage duration and claim status.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Warranty claim is filed | Receivable is provisional until approval evidence exists. |
| Claim is partially denied | Denied portion remains expense or reserve according to policy. |
| Generation loss is reported | Lost revenue is separate from repair recovery. |

## 63. Data-Centre Grid-Connection Queue Delay

Scenario:

A data-centre development is complete, but full grid connection is delayed. The shell is built, yet contracted capacity cannot fully convert into income.

| Attribute | Value |
|---|---:|
| Built capacity | 30 MW |
| Temporary available power | 12 MW |
| Pre-leased rent per MW per year | 1,050,000 |
| Expected queue delay | 9 months |

Deferred revenue:

```text
deferred_capacity_mw = 30 - 12 = 18
annual_deferred_revenue = 18 x 1,050,000 = 18,900,000
delay_period_deferred_revenue = 18,900,000 x 9 / 12 = 14,175,000
```

Treatment:

- preserve grid-connection agreement, queue position, temporary-power approval, utility milestone and tenant contract conditions;
- separate completed shell value, powered capacity, deferred capacity and pre-lease income assumptions;
- avoid treating unpowered capacity as recurring rent until grid evidence supports service availability;
- update liquidity, covenant, valuation and development-risk reporting from queue-delay status.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Grid connection is delayed | Powered capacity and deferred revenue are reported separately. |
| Temporary power is approved | Only temporary available capacity supports income recognition. |
| Utility milestone changes | Forecasts version prior and revised connection dates. |

## 64. Real-Estate Loan Maturity-Extension Negotiation

Scenario:

A private real estate fund negotiates an extension on a property-level loan that would otherwise mature before leasing stabilization is complete.

| Attribute | Current terms | Extension proposal |
|---|---:|---:|
| Outstanding loan | 18,000,000 | 18,000,000 |
| Current maturity | 2026-12-31 | 2028-12-31 |
| Extension fee | n/a | 180,000 |
| Current interest margin | 2.25% | 2.75% |
| Stabilized NOI expected | 1,900,000 | 2,350,000 |

Extension impact:

```text
extension_fee_rate = 180,000 / 18,000,000 = 1.00%
annual_incremental_interest = 18,000,000 x (2.75% - 2.25%) = 90,000
noi_uplift_expected = 2,350,000 - 1,900,000 = 450,000
```

Treatment:

- preserve lender term sheet, maturity date, extension conditions, fee, margin change and covenant package;
- separate one-off extension fee, recurring interest cost, loan maturity ladder and valuation assumptions;
- do not treat extension as completed refinancing until lender execution evidence exists;
- route liquidity, covenant, valuation and distribution policy review when extension conditions are unresolved.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Extension term sheet is non-binding | Loan remains current-maturity with pending extension note. |
| Extension is executed | Maturity ladder, financing cost and covenant package update from effective date. |
| Conditions are missed | Default/refinancing risk workflow opens. |

## 65. Anchor-Tenant Insolvency Administration

Scenario:

An anchor tenant enters insolvency administration. Rent is unpaid, occupancy remains physically unchanged, and recovery depends on administrator decision.

| Attribute | Value |
|---|---:|
| Tenant share of rental income | 38.00% |
| Annual contracted rent | 2,400,000 |
| Rent unpaid | 400,000 |
| Expected recovery | 35.00% |

Rent-at-risk:

```text
expected_recovery_cash = 400,000 x 35.00% = 140,000
expected_write_off = 400,000 - 140,000 = 260,000
annual_income_at_risk = 2,400,000
```

Treatment:

- preserve insolvency notice, lease terms, unpaid rent, administrator correspondence and recovery estimate;
- separate physical occupancy, legal lease status, receivable, expected recovery and rent-at-risk;
- update tenant concentration, valuation assumptions and covenant analytics from insolvency status;
- apply confidentiality controls where tenant details are restricted.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Tenant enters administration | Rent receivable and rent-at-risk states are opened. |
| Administrator confirms continued occupation | Occupancy and cash collection assumptions remain separate. |
| Recovery estimate changes | Expected recovery and write-off are versioned. |

## 66. REIT Asset-Sale Special Distribution

Scenario:

A listed REIT sells a mature asset and declares a special distribution. The distribution includes operating income and capital-return components.

| Component | Amount per unit |
|---|---:|
| Operating distribution | 0.025 |
| Capital-return special distribution | 0.075 |
| Units held | 20,000 |
| Withholding rate on income component | 10.00% |

Distribution split:

```text
operating_distribution = 20,000 x 0.025 = 500
capital_return_distribution = 20,000 x 0.075 = 1,500
withholding = 500 x 10.00% = 50
net_cash = 500 + 1,500 - 50 = 1,950
```

Treatment:

- preserve asset-sale announcement, distribution circular, component split, ex-date, pay date and withholding terms;
- separate ordinary distribution, capital return, withholding, cost-basis adjustment and realized performance impact;
- do not classify the entire cash amount as recurring income;
- update REIT leverage and portfolio composition analytics after asset disposal.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Special distribution is confirmed | Cash is split by source-backed income/capital components. |
| Component split is missing | Tax/reporting remains source-limited. |
| REIT asset disposal changes leverage | Exposure analytics update separately from cash distribution. |

## 67. Infrastructure Step-In-Right Event

Scenario:

A lender or grantor exercises step-in rights after repeated project company service failures. The equity investor retains ownership but loses operational control during remediation.

| Attribute | Value |
|---|---:|
| Quarterly availability payment | 4,500,000 |
| Deduction during step-in | 12.00% |
| Step-in remediation cost | 350,000 |
| Expected duration | 2 quarters |

Step-in impact:

```text
quarterly_deduction = 4,500,000 x 12.00% = 540,000
two_quarter_deductions = 540,000 x 2 = 1,080,000
total_near_term_impact = 1,080,000 + 350,000 = 1,430,000
```

Treatment:

- preserve concession agreement, step-in notice, triggering default, control rights, remediation plan and expiry conditions;
- separate ownership, operational control, availability deductions, remediation cost and valuation assumptions;
- do not treat step-in as disposal unless legal ownership changes;
- update risk, governance and income-quality reporting until control is restored.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Step-in notice is issued | Operational-control state changes without closing ownership. |
| Deductions continue during step-in | Availability revenue and remediation cost remain separate. |
| Control is restored | Step-in state closes with evidence and residual actions. |

## 68. Direct-Property Foreign-Ownership Restriction

Scenario:

A direct-property purchase is subject to foreign-ownership restrictions. The client has signed a purchase agreement, but regulator approval is pending.

| Attribute | Value |
|---|---:|
| Purchase price | 6,800,000 |
| Deposit paid | 680,000 |
| Regulatory approval status | Pending |
| Potential penalty if rejected | 120,000 |

Restricted completion exposure:

```text
cash_at_risk = deposit_paid + potential_penalty
cash_at_risk = 680,000 + 120,000 = 800,000
```

Treatment:

- preserve purchase agreement, buyer nationality/residency status, regulatory filing, approval deadline and deposit terms;
- keep signed, conditional and completed ownership states separate;
- do not create full property holding or collateral value until legal completion and registration evidence exists;
- report deposit, penalty exposure and conditional ownership status separately.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Regulator approval is pending | Property holding remains conditional or source-limited. |
| Approval is rejected | Deposit/penalty workflow opens without creating final ownership. |
| Approval is granted | Ownership and valuation activate from completion evidence. |

## 69. Brownfield Contamination Indemnity Claim

Scenario:

A brownfield property acquisition includes seller indemnity for known contamination. Remediation cost exceeds the initial reserve and the buyer files an indemnity claim.

| Attribute | Value |
|---|---:|
| Initial remediation reserve | 1,200,000 |
| Updated remediation estimate | 1,850,000 |
| Indemnity cap | 500,000 |
| Seller accepted claim | 350,000 |

Indemnity exposure:

```text
reserve_shortfall = 1,850,000 - 1,200,000 = 650,000
maximum_recoverable = min(650,000, 500,000) = 500,000
unrecovered_shortfall_after_acceptance = 650,000 - 350,000 = 300,000
```

Treatment:

- preserve environmental report, acquisition agreement, indemnity cap, claim notice, seller response and remediation estimate;
- separate remediation reserve, indemnity receivable, accepted recovery, disputed recovery and valuation impact;
- avoid treating claimed indemnity as cash until settlement;
- update lending, sale and valuation restrictions while contamination remains unresolved.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Updated estimate exceeds reserve | Reserve shortfall and recoverable claim are calculated. |
| Seller partially accepts claim | Accepted receivable and disputed amount remain separate. |
| Remediation is unresolved | Property report carries environmental restriction and valuation impact. |

## 70. Property Lease Renewal Inducement

Scenario:

A direct-property tenant renews for five years. The headline rent is higher than the expiring lease, but the renewal includes a rent-free period and a tenant-improvement allowance.

| Attribute | Value |
|---|---:|
| Renewal annual headline rent | 1,260,000 |
| Lease term | 5 years |
| Rent-free period | 3 months |
| Tenant-improvement allowance | 150,000 |

Effective annual rent:

```text
rent_free_value = 1,260,000 x 3 / 12 = 315,000
effective_annual_rent = (1,260,000 x 5 - 315,000 - 150,000) / 5
effective_annual_rent = 1,167,000
```

Treatment:

- preserve executed lease renewal, inducement approval, tenant-improvement budget, rent-free schedule and lease accounting basis;
- separate headline rent, effective rent, cash-rent timing, allowance cashflow and valuation assumptions;
- update WALE, rent forecast, capex reserve and tenant concentration from the renewal effective date;
- avoid overstating recurring cash income during the rent-free period.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Rent-free terms exist | Effective rent is lower than headline rent and cash timing is explicit. |
| Allowance is approved | Allowance is tracked separately from recurring rent. |
| Lease renewal is executed | WALE and rent forecast update from the renewal effective date. |

## 71. Real-Estate Fund Secondary Sale

Scenario:

A client sells a private real-estate fund interest in a secondary transaction at a discount to latest NAV. The buyer also assumes future unfunded commitments.

| Attribute | Value |
|---|---:|
| Reported NAV sold | 2,000,000 |
| Secondary price percentage | 92% |
| Transfer fee | 20,000 |
| Unfunded commitment assumed by buyer | 300,000 |

Net cash and discount:

```text
gross_secondary_price = 2,000,000 x 92% = 1,840,000
net_cash = 1,840,000 - 20,000 = 1,820,000
gross_discount_to_nav = 2,000,000 - 1,840,000 = 160,000
net_discount_after_fee = 2,000,000 - 1,820,000 = 180,000
```

Treatment:

- preserve transfer agreement, manager consent, price letter, fee schedule, NAV statement and unfunded commitment transfer evidence;
- process as a secondary transfer rather than a fund redemption;
- separate NAV, sale price, transfer fee, realized discount, remaining commitment and consent status;
- close exposure only when transfer completion and register update are sourced.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Transfer is pending consent | Holding remains open and sale cash is not final. |
| Buyer assumes commitments | Unfunded commitment is reduced only after transfer completion. |
| Sale price is below NAV | Realized discount is calculated separately from NAV restatement. |

## 72. REIT Privatization Offer

Scenario:

A listed REIT receives a privatization offer. Client units are exchanged for cash after scheme approval and delisting.

| Attribute | Value |
|---|---:|
| Units held | 100,000 |
| Offer price per unit | 2.35 |
| Last pre-offer market price | 2.20 |
| Withholding or transaction tax | 0 |

Offer cash and premium:

```text
gross_offer_cash = 100,000 x 2.35 = 235,000
market_value_before_offer = 100,000 x 2.20 = 220,000
offer_premium = 235,000 - 220,000 = 15,000
```

Treatment:

- preserve scheme document, shareholder approval, court or regulator approval where applicable, effective date and delisting notice;
- separate market price, offer consideration, election status, completion status, accrued distribution and tax handling;
- stop listed-market pricing only after delisting or suspension source evidence;
- avoid closing the holding before effective-date corporate-action confirmation.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Scheme approval is pending | Position remains listed or suspended according to market source. |
| Scheme becomes effective | Units close and cash consideration posts from official terms. |
| Distribution record date differs | Distribution entitlement remains separate from privatization cash. |

## 73. Infrastructure Tariff Reset Appeal

Scenario:

A regulated infrastructure asset receives a lower allowed-return determination. The manager appeals and provides a valuation sensitivity for partial recovery.

| Attribute | Value |
|---|---:|
| Regulated asset base | 500,000,000 |
| Approved allowed return | 5.70% |
| Appealed allowed return case | 6.00% |
| Client economic share | 1.00% |

Client annual sensitivity:

```text
appeal_return_delta = 6.00% - 5.70% = 0.30%
asset_level_revenue_sensitivity = 500,000,000 x 0.30% = 1,500,000
client_share_sensitivity = 1,500,000 x 1.00% = 15,000
```

Treatment:

- preserve regulator determination, tariff model, appeal filing, manager sensitivity and final decision date;
- separate approved tariff, appealed scenario, probability weighting and final allowed revenue;
- report appeal upside as contingent valuation sensitivity, not booked income;
- update cashflow forecast only when regulator decision or settlement evidence is final.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Appeal is open | Revenue uplift is scenario-labelled and not booked as cash. |
| Appeal succeeds partially | Allowed revenue updates from final decision terms. |
| Appeal fails | Prior scenario sensitivity is removed without reversing realized cash. |

## 74. Renewable Power Purchase Agreement Curtailment Cap

Scenario:

A renewable project has a power purchase agreement that compensates curtailment up to a contractual cap. Actual curtailment exceeds the cap.

| Attribute | Value |
|---|---:|
| Annual generation basis | 100,000 MWh |
| Contractual curtailment compensation cap | 5% |
| Actual curtailed generation | 8% |
| PPA price | 60 per MWh |

Uncompensated curtailment above cap:

```text
compensated_curtailment_mwh = 100,000 x 5% = 5,000
actual_curtailment_mwh = 100,000 x 8% = 8,000
uncompensated_curtailment_mwh = 8,000 - 5,000 = 3,000
revenue_at_risk = 3,000 x 60 = 180,000
```

Treatment:

- preserve grid notice, PPA clause, generation data, curtailment statement and compensation calculation;
- separate generated output, compensated curtailment, uncompensated curtailment, disputed receivable and recurring grid-risk flag;
- avoid treating above-cap curtailment as receivable unless contract recovery evidence exists;
- feed persistent curtailment into valuation, operating-risk and covenant sensitivity.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Curtailment is within cap | Compensation can be accrued if source-backed. |
| Curtailment exceeds cap | Above-cap revenue remains at-risk or disputed, not booked cash. |
| Grid issue recurs | Recurring risk flag updates operating and valuation analytics. |

## 75. Data-Centre Liquid-Cooling Retrofit Capex

Scenario:

A data-centre asset approves a liquid-cooling retrofit to support high-density tenants. The retrofit requires upfront capex and is expected to increase annual NOI after commissioning.

| Attribute | Value |
|---|---:|
| Approved retrofit capex | 4,000,000 |
| Expected annual NOI uplift | 550,000 |
| Commissioning holdback | 400,000 |
| Client economic share | 5% |

Payback and client cash planning:

```text
simple_payback_years = 4,000,000 / 550,000 = 7.27
client_gross_capex_share = 4,000,000 x 5% = 200,000
client_holdback_share = 400,000 x 5% = 20,000
```

Treatment:

- preserve capex approval, engineering scope, tenant power-density requirement, commissioning milestone, holdback terms and lease amendment;
- separate approved capex, incurred capex, remaining commitment, holdback, commissioning evidence and income uplift;
- treat expected NOI uplift as scenario until tenant acceptance and commissioning evidence are sourced;
- update liquidity forecast, valuation assumptions, debt covenant model and operational-risk notes.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Capex is approved but not commissioned | Spend and commitment update; income uplift remains scenario-labelled. |
| Commissioning completes | NOI uplift may activate from acceptance evidence and effective date. |
| Holdback remains unresolved | Holdback cash and vendor obligation stay separate from recurring NOI. |

## 76. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Objective | income, inflation hedge, diversification, growth, real asset exposure, or tactical trade? |
| Wrapper | listed REIT, fund, private fund, direct property, infrastructure, structured product? |
| Liquidity | exchange liquidity, fund dealing, lock-up, redemption queue, secondary market, property sale horizon? |
| Valuation | exchange price, NAV, appraisal, model, manager estimate, stale or restated value? |
| Leverage | property-level debt, fund leverage, REIT gearing, refinancing and rate exposure? |
| Concentration | property sector, geography, tenant, manager, vehicle, currency, infrastructure subsector? |
| DPM mandate | allowed wrapper, illiquid allocation, income target, leverage limit, concentration cap? |
| Reporting | legal holding, economic exposure, income classification, valuation date and liquidity label? |

## 77. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed REIT | security position, trades, price, distributions, corporate actions, gearing where sourced | property-sector look-through and detailed operating-metric analytics |
| Private real estate fund | commitment, calls, NAV, distributions, valuation date, queues, holdbacks, development drawdowns, capex reserves and tenant/default indicators where sourced | advanced liquidity queue simulation, project-level cashflow and manager-level stress modelling |
| Direct property | ownership record, appraisal value, source date, documents, ownership percentage, sale state, committee override, zoning, insurance, environmental and covenant status where sourced | document-backed ownership graph and property-level cashflow model |
| Infrastructure exposure | fund/security/private fund position, sector tags, concession, revenue model, clawback terms, availability deductions, regulated asset base reset terms, tariff appeals, PPA curtailment caps and data-centre retrofit capex where sourced | advanced concession, inflation-linkage, offtake, clawback and regulatory-risk analytics |
| Reporting | wrapper, exposure, value, income, source date, liquidity label, operating metrics, development state and governance overrides where sourced | consolidated real-asset income and stress dashboard |

## 78. Regression Test Pack

Minimum release-gate scenarios:

1. Listed REIT distribution books gross, tax and net cash correctly.
2. REIT rights issue creates entitlement, election and new units when exercised.
3. Private real estate capital call updates paid-in and unfunded commitment.
4. Appraisal restatement preserves original and restated NAV.
5. Infrastructure distribution separates income, capital return and gain.
6. Redemption queue shows accepted and deferred amounts.
7. Direct property valuation preserves appraisal date and report-date FX.
8. Stale appraisal or NAV labels report output degraded.
9. REIT leverage/rate sensitivity appears as analytics, not as bond duration.
10. Missing look-through source prevents unsupported property-sector allocation claims.
11. Property-level debt maturity ladder labels missing loan and covenant data.
12. Tenant concentration calculates top-tenant and top-three exposure with confidentiality controls.
13. Occupancy change updates analytics without booking a cash transaction.
14. Lease expiry WALE states basis and source date.
15. Infrastructure concession renewal separates legal term, valuation assumption and projected cashflow.
16. Renewable power-price exposure separates contracted and merchant revenue.
17. Direct-property ownership transfer updates ownership hierarchy without duplicating value.
18. Development project cost-to-complete separates incurred cost, committed cost, contingency and current valuation.
19. Construction drawdown updates paid-in, unfunded commitment and liquidity planning with project lineage.
20. Rent-free period separates headline rent, effective rent and cash rent timing.
21. Capex reserve reduces distributable cash without misclassifying approved refurbishment as investment loss.
22. Direct property sale completion separates signed, conditional and completed sale states and loan payoff.
23. Valuation committee override preserves original appraisal, adjusted value, approver, reason and expiry.
24. Green-building certification impacts financing and operating analytics only when source-backed.
25. Infrastructure revenue clawback separates excess revenue, clawback obligation and sustainable income quality.
26. Zoning change preserves approved/proposed state and separates valuation uplift from required development cost.
27. Compulsory acquisition reduces only the legally acquired portion and tracks disputed compensation separately.
28. Property insurance claim separates repair cost, deductible, recovery receivable, reserve and received cash.
29. Environmental remediation records obligation, reserve, valuation impact and sale/lending restrictions.
30. Property loan covenant breach preserves covenant source, cure period, waiver state and collateral impact.
31. Tenant default workout updates rent-at-risk, concentration, valuation assumptions and confidentiality controls.
32. Infrastructure availability deduction separates payment deduction, remediation cost and dispute state.
33. Regulated asset base reset separates RAB growth, allowed-return percentage change and regulatory finality.
34. Ground-lease reset separates lease economics from operating performance, valuation and ownership state.
35. Lease indexation dispute preserves accepted, disputed and resolved rent schedules with source lineage.
36. Data-centre power constraint caps deliverable capacity and income before grid upgrade evidence.
37. Renewable curtailment separates generated revenue, compensation, disputed receivable and recurring grid risk.
38. Concession handback obligation tracks reserve shortfall, inspection evidence and terminal-value impact.
39. Brownfield redevelopment overrun separates approved budget, incurred cost, committed cost and valuation basis.
40. Direct-property co-owner dispute separates ownership value from liquidity-adjusted exit estimate and sale authority.
41. Property tax reassessment updates recurring tax expense, NOI and valuation assumptions from effective date.
42. REIT merger consideration separates old units, new units, cash consideration and fractional cash.
43. Property manager replacement updates operating assumptions without changing ownership state.
44. Infrastructure refinancing separates recurring interest saving, one-off cost, maturity ladder and covenant changes.
45. Data-room due diligence gap blocks final approval or labels preliminary valuation as source-limited.
46. Real-asset ESG remediation separates approved capex, certification state, financing benefit and reporting status.
47. REIT debt covenant waiver package tracks waiver state, fee, expiry and distribution restrictions.
48. Infrastructure concession extension bid keeps bid-contingent value separate from awarded concession terms.
49. Direct-property tenant-improvement allowance separates upfront cash, rent-free period and recurring lease economics.
50. Renewable offtake replacement updates contracted revenue, merchant exposure and counterparty credit from source terms.
51. Valuation cap-rate shock separates committee-approved valuation from cash sale or realized loss events.
52. Property escrow release dispute separates received cash, escrow receivable, disputed claim and final release.
53. Direct-property easement dispute separates legal ownership, appraisal value, legal reserve and liquidity-adjusted scenario value.
54. REIT rights issue underwriting failure separates client rights processing from REIT-level proceeds shortfall.
55. Infrastructure inflation-index lag applies lagged tariff formula before current CPI flows through allowed revenue.
56. Property tax appeal settlement effective-dates NOI, valuation and refund/payable treatment.
57. Renewable repowering capex separates approved capex, incurred cost, remaining spend and commissioned income uplift.
58. Data-centre tenant power default updates rent-at-risk, concentration, recovery and replacement-tenant assumptions.
59. Property title defect cure separates appraisal value, legal reserve, liquidity scenario value and collateral eligibility.
60. REIT scrip dividend election creates source-backed units, cash and tax treatment without double-counted income.
61. Infrastructure availability-payment benchmarking separates contractual deduction, remediation cost, peer variance and disputed recovery.
62. Lease break-option exercise updates WALE, rent forecast, break-fee cash and dispute state from valid notice evidence.
63. Renewable warranty claim separates repair expense, warranty receivable, denied amount and lost generation.
64. Data-centre grid-connection queue delay separates built capacity, powered capacity, deferred capacity and forecast revenue.
65. Real-estate loan maturity extension separates one-off fee, recurring interest cost, maturity ladder and unresolved conditions.
66. Anchor-tenant insolvency administration separates physical occupancy, legal lease status, receivable, recovery and rent-at-risk.
67. REIT asset-sale special distribution separates ordinary income, capital return, withholding and cost-basis treatment.
68. Infrastructure step-in-right event changes operational-control state without closing ownership unless legal title changes.
69. Direct-property foreign-ownership restriction keeps signed, conditional and completed ownership states separate.
70. Brownfield contamination indemnity claim separates reserve shortfall, accepted recovery, disputed recovery and valuation impact.
71. Lease renewal inducement separates headline rent, effective rent, rent-free cash timing and tenant-improvement allowance.
72. Real-estate fund secondary sale separates NAV, sale price, transfer fee, realized discount, consent state and transferred commitments.
73. REIT privatization offer separates market value, offer cash, distribution entitlement, delisting state and completion evidence.
74. Infrastructure tariff reset appeal keeps approved revenue and appeal scenario separate until final regulator decision.
75. Renewable PPA curtailment cap separates compensated curtailment, above-cap revenue at risk and disputed receivable.
76. Data-centre liquid-cooling retrofit separates approved capex, holdback, commissioning status and scenario-labelled NOI uplift.

## 79. Property Valuation Appeal Reversal

Scenario:

A direct property valuation was increased after a successful tax and valuation appeal. A later independent appraisal reverses part of the uplift because comparable sales evidence was not accepted.

| Attribute | Value |
|---|---:|
| Appealed valuation | 12,500,000 |
| Reversed uplift | 750,000 |
| Revised valuation | 11,750,000 |
| Client ownership share | 40% |

Client valuation impact:

```text
client_valuation_reversal = reversed_uplift x client_ownership_share
client_valuation_reversal = 750,000 x 40% = 300,000
```

Treatment:

- preserve original appraisal, appeal evidence, revised appraisal, comparable-sales challenge, valuation committee decision and effective date;
- separate valuation appeal, appraisal reversal, tax assessment impact and realized sale economics;
- update portfolio value, performance contribution, collateral value and client reporting from the approved effective date;
- keep prior valuation versions auditable rather than overwriting history;
- route collateral or loan covenant impacts through lending controls where property value supports borrowing.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Appeal uplift is reversed | Valuation decreases with appraisal lineage. |
| Prior client report was delivered | Corrected report preserves prior and revised valuation. |
| Property is pledged | Collateral and LTV analytics recalculate from effective date. |

## 80. REIT Index Inclusion Event

Scenario:

A listed REIT is added to a major property index. Index-tracking funds are expected to buy the REIT, but client portfolios should not treat expected flow as guaranteed performance.

| Attribute | Value |
|---|---:|
| Units held | 75,000 |
| Pre-inclusion price | 1.84 |
| Post-announcement price | 1.93 |
| Units held market-value uplift | 6,750 |

Market-value movement:

```text
market_value_uplift = units_held x (post_announcement_price - pre_inclusion_price)
market_value_uplift = 75,000 x (1.93 - 1.84) = 6,750
```

Treatment:

- preserve index provider notice, effective date, index weight, market price source, trading-volume evidence and rebalancing assumptions;
- separate realized price movement from expected index demand;
- update benchmark-relative analytics, liquidity assumptions and tracking-error context from effective date;
- avoid labelling expected passive buying as certain income or guaranteed return;
- keep tax, distribution and corporate-action treatment unchanged unless separate source events exist.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Index inclusion is announced | Benchmark and liquidity metadata update from effective date. |
| Price moves before effective date | Market-value uplift is performance movement, not confirmed index cashflow. |
| Index inclusion is delayed | Pending state remains separate from active membership. |

## 81. Infrastructure Concession Refinancing Waiver

Scenario:

An infrastructure concession has debt covenants restricting refinancing. Lenders grant a waiver to refinance early, with a one-off waiver fee and lower recurring margin.

| Attribute | Value |
|---|---:|
| Outstanding debt | 220,000,000 |
| Old margin | 2.10% |
| New margin | 1.70% |
| Waiver fee | 1,100,000 |

Annual interest saving:

```text
annual_interest_saving = outstanding_debt x (old_margin - new_margin)
annual_interest_saving = 220,000,000 x (2.10% - 1.70%) = 880,000

simple_fee_payback_years = waiver_fee / annual_interest_saving
simple_fee_payback_years = 1,100,000 / 880,000 = 1.25
```

Treatment:

- preserve concession agreement, lender waiver, refinancing term sheet, fee invoice, covenant package and execution date;
- separate one-off waiver fee from recurring interest saving;
- update debt maturity ladder, covenant state, valuation assumptions and distribution capacity after execution evidence;
- keep waiver-pending economics scenario-labelled until refinancing closes;
- disclose material leverage and covenant changes in portfolio reviews where policy requires it.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Waiver approved but refinancing not closed | Economics remain pending or scenario-labelled. |
| Refinancing closes | Interest cost, maturity ladder and fee cashflow update with source evidence. |
| Waiver expires unused | Scenario benefit is removed without reversing realized cash. |

## 82. Renewable Merchant-Price Floor Reset

Scenario:

A renewable project has merchant-price downside protection that resets annually. The new floor is lower than the prior floor, increasing revenue-at-risk under merchant exposure.

| Attribute | Prior | New |
|---|---:|---:|
| Merchant volume | 40,000 MWh | 40,000 MWh |
| Price floor | 48 | 42 |
| Forward merchant price | 39 | 39 |
| Incremental exposed price gap | n/a | 6 |

Incremental revenue at risk:

```text
prior_floor_gap = max(prior_floor - forward_price, 0)
prior_floor_gap = max(48 - 39, 0) = 9
new_floor_gap = max(new_floor - forward_price, 0)
new_floor_gap = max(42 - 39, 0) = 3
protection_reduction = (prior_floor_gap - new_floor_gap) x merchant_volume
protection_reduction = (9 - 3) x 40,000 = 240,000
```

Treatment:

- preserve PPA/hedge floor terms, reset notice, market price source, merchant volume forecast and effective date;
- separate contracted revenue, merchant revenue, floor protection and exposed revenue-at-risk;
- update valuation, income stability and downside stress assumptions from the reset date;
- avoid treating reduced protection as realized loss until market prices and production are known;
- label forward-looking revenue sensitivity as scenario output.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Floor resets lower | Revenue-at-risk and downside stress update from effective date. |
| Forward price remains below floor | Protection calculation uses new floor only after reset date. |
| Production volume changes | Revenue-at-risk recalculates from sourced generation forecast. |

## 83. Data-Centre Power-Density Covenant Breach

Scenario:

A data-centre tenant lease requires a minimum deliverable power density. Grid constraints prevent delivery, creating a rent abatement and covenant breach.

| Attribute | Value |
|---|---:|
| Contracted power density | 18 kW/rack |
| Delivered power density | 14 kW/rack |
| Affected racks | 300 |
| Monthly abatement per affected rack | 250 |

Monthly abatement:

```text
power_density_shortfall = contracted_power_density - delivered_power_density
power_density_shortfall = 18 - 14 = 4

monthly_abatement = affected_racks x abatement_per_rack
monthly_abatement = 300 x 250 = 75,000
```

Treatment:

- preserve lease covenant, power-delivery report, grid constraint evidence, tenant notice, abatement formula and cure plan;
- separate physical occupancy from deliverable power capacity and rent collection;
- update rent-at-risk, covenant breach state, valuation assumptions and remediation capex forecast;
- avoid treating built space as fully income-producing when power density is below covenant;
- disclose recurring breach risk where it affects income quality or tenant concentration.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Delivered power breaches covenant | Rent abatement and breach workflow open. |
| Tenant still occupies space | Occupancy and income-producing capacity remain separate. |
| Grid upgrade completes | Breach clears only after source-backed power evidence. |

## 84. Real-Estate Fund Recallable Distribution Funding

Scenario:

A real-estate fund distributes proceeds from an asset sale but designates part of the distribution as recallable for future follow-on investment. Liquidity planning must not treat the recallable amount as permanently returned capital.

| Attribute | Value |
|---|---:|
| Distribution received | 600,000 |
| Recallable percentage | 35% |
| Recallable amount | 210,000 |
| Non-recallable cash | 390,000 |

Recallable distribution:

```text
recallable_amount = distribution_received x recallable_percentage
recallable_amount = 600,000 x 35% = 210,000

non_recallable_cash = distribution_received - recallable_amount
non_recallable_cash = 600,000 - 210,000 = 390,000
```

Treatment:

- preserve distribution notice, recallable designation, partnership terms, remaining commitment, future call conditions and cash receipt;
- separate cash received, recallable capital, non-recallable return and remaining commitment;
- update liquidity planning and future capital-call exposure for recallable amount;
- avoid treating recallable distribution as final realized profit without commitment context;
- explain recallability in client reporting and DPM funding assumptions.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Distribution is recallable | Cash receipt posts while future call exposure remains. |
| Future call uses recalled capital | Commitment and cashflow lineage link back to distribution. |
| Client report is generated | Recallable and non-recallable cash are distinct. |

## 85. Property Appraisal Independence Dispute

Scenario:

A private real estate fund receives an updated appraisal from a valuer that also advised on the asset's refinancing. The valuation committee challenges whether the appraisal is independent enough for NAV use.

| Attribute | Value |
|---|---:|
| Prior appraised value | 48,000,000 |
| New appraised value | 51,500,000 |
| Client ownership share | 3.00% |
| Independence status | Disputed |

Client valuation impact:

```text
gross_appraisal_change = new_appraised_value - prior_appraised_value
gross_appraisal_change = 51,500,000 - 48,000,000 = 3,500,000

client_appraisal_impact = gross_appraisal_change x client_ownership_share
client_appraisal_impact = 3,500,000 x 3.00% = 105,000
```

Treatment:

- preserve prior appraisal, new appraisal, valuer relationship disclosure, committee minutes, independence challenge and approved NAV basis;
- keep disputed appraisal value separate from final NAV until governance approval is complete;
- report valuation sensitivity without treating the uplift as confirmed performance;
- track whether a second independent valuation is required;
- avoid using conflicted appraisal data for collateral or suitability decisions without approved override evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Valuer independence is disputed | Appraisal is flagged and NAV uplift remains provisional. |
| Committee approves use | NAV source changes with approval evidence and effective date. |
| Second valuation differs | Difference is captured as valuation governance evidence. |

## 86. REIT Index Deletion Event

Scenario:

A listed REIT is deleted from a benchmark index after failing liquidity and free-float criteria. Passive funds and benchmark-relative mandates must update expected flow, tracking-error and attribution treatment.

| Attribute | Value |
|---|---:|
| REIT units held | 75,000 |
| Pre-announcement price | 2.40 |
| Post-announcement price | 2.25 |
| Benchmark weight before deletion | 0.45% |
| Benchmark weight after deletion | 0.00% |

Market value impact:

```text
market_value_drop = units_held x (pre_announcement_price - post_announcement_price)
market_value_drop = 75,000 x (2.40 - 2.25) = 11,250

benchmark_weight_change = benchmark_weight_after_deletion - benchmark_weight_before_deletion
benchmark_weight_change = 0.00% - 0.45% = -0.45%
```

Treatment:

- preserve index deletion notice, effective date, free-float/liquidity reason, benchmark file and market price evidence;
- update benchmark-relative analytics from the effective date without rewriting prior periods;
- separate market price impact from benchmark deletion attribution;
- flag passive or benchmark-constrained mandates for rebalance review;
- avoid treating index deletion as a corporate action that changes legal holding quantity.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Index deletion is announced | Benchmark lineage and effective-date change are captured. |
| REIT price falls | Market impact is separated from benchmark weight change. |
| Mandate is benchmark-constrained | Rebalance or exception workflow opens. |

## 87. Concession Lender Consent Failure

Scenario:

An infrastructure concession operator seeks lender consent for a contract amendment. Consent fails because required majority lenders do not approve, delaying the amendment and associated refinancing benefit.

| Attribute | Value |
|---|---:|
| Required consent threshold | 66.67% |
| Consent received | 58.00% |
| Expected annual saving if approved | 2,400,000 |
| Client look-through share | 4.00% |

Consent shortfall and client sensitivity:

```text
consent_shortfall = required_consent_threshold - consent_received
consent_shortfall = 66.67% - 58.00% = 8.67%

client_annual_saving_at_risk = expected_annual_saving_if_approved x client_lookthrough_share
client_annual_saving_at_risk = 2,400,000 x 4.00% = 96,000
```

Treatment:

- preserve concession amendment, lender consent solicitation, vote tally, threshold rule, refinancing plan and revised timetable;
- keep failed consent separate from borrower default unless debt documents define a default event;
- update valuation, refinancing and cashflow scenarios from the failed-consent date;
- track whether a revised consent solicitation is active;
- avoid recognizing expected savings before lender consent is legally effective.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Consent falls below threshold | Consent failure state and shortfall are calculated. |
| Refinancing benefit was forecast | Expected savings move to at-risk scenario. |
| New solicitation opens | Prior failed vote remains auditable. |

## 88. Renewable Hedge Floor Termination

Scenario:

A renewable asset has a power-price hedge with a merchant-price floor. The hedge counterparty terminates the floor after a credit support failure, increasing merchant revenue exposure.

| Attribute | Value |
|---|---:|
| Forecast production | 180,000 MWh |
| Terminated floor price | 52 |
| Current forward price | 45 |
| Client look-through share | 2.50% |

Client revenue at risk:

```text
floor_revenue_protection_lost = max(floor_price - forward_price, 0) x forecast_production
floor_revenue_protection_lost = max(52 - 45, 0) x 180,000 = 1,260,000

client_revenue_at_risk = floor_revenue_protection_lost x client_lookthrough_share
client_revenue_at_risk = 1,260,000 x 2.50% = 31,500
```

Treatment:

- preserve hedge confirmation, termination notice, credit support failure evidence, production forecast, forward price source and replacement hedge plan;
- separate hedge termination from physical production availability;
- update downside revenue scenarios, valuation assumptions and mandate risk reporting;
- track replacement hedge execution and residual merchant exposure;
- avoid treating hedge termination as realized loss until production and market prices crystallize cashflow.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Hedge floor terminates | Floor protection lost and revenue-at-risk are calculated. |
| Production forecast changes | Revenue-at-risk recalculates from sourced forecast. |
| Replacement hedge executes | Residual exposure updates from new hedge terms. |

## 89. Data-Centre Service-Credit Settlement

Scenario:

A data-centre tenant receives service credits after repeated uptime SLA breaches. The property still has high occupancy, but net rental income is reduced by the service-credit settlement.

| Attribute | Value |
|---|---:|
| Monthly contracted rent | 1,200,000 |
| Service-credit percentage | 8.00% |
| Settlement months | 3 |
| Client ownership share | 1.50% |

Client income reduction:

```text
gross_service_credit = monthly_contracted_rent x service_credit_percentage x settlement_months
gross_service_credit = 1,200,000 x 8.00% x 3 = 288,000

client_income_reduction = gross_service_credit x client_ownership_share
client_income_reduction = 288,000 x 1.50% = 4,320
```

Treatment:

- preserve uptime SLA, incident records, tenant claim, settlement agreement, service-credit calculation and income adjustment booking;
- separate occupancy, gross rent, service-credit settlement and net rental income;
- update income yield, tenant concentration and operational-quality reporting;
- track recurring SLA risk where credits indicate supportability or infrastructure issues;
- avoid treating service credits as ordinary rent-free incentive or tenant default.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| SLA breach settlement is approved | Service-credit amount and income reduction are calculated. |
| Tenant remains in occupation | Occupancy remains unchanged while net income reduces. |
| Credits recur | Operational risk and valuation assumptions are flagged for review. |

## 90. Real-Estate Fund Recallable Distribution Overcall Dispute

Scenario:

A real-estate fund calls capital against prior recallable distributions. The requested call exceeds the recallable balance recorded by the investor, creating an overcall dispute.

| Attribute | Value |
|---|---:|
| Recorded recallable balance | 210,000 |
| New call referencing recallable distribution | 260,000 |
| Available commitment after recallable balance | 500,000 |
| Disputed overcall amount | 50,000 |

Overcall amount:

```text
overcall_amount = new_call_referencing_recallable_distribution - recorded_recallable_balance
overcall_amount = 260,000 - 210,000 = 50,000
```

Treatment:

- preserve original distribution notice, recallable balance ledger, capital-call notice, partnership terms, administrator reconciliation and dispute owner;
- split the call between recallable-supported amount and disputed overcall amount;
- reserve cash only according to approved funding policy while dispute is open;
- keep remaining commitment and recallable distribution ledger synchronized;
- avoid treating the full call as valid recallable capital when the investor ledger does not support it.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Call exceeds recallable ledger | Overcall amount is calculated and disputed. |
| Administrator corrects ledger | Dispute closes with adjusted recallable balance evidence. |
| Cash forecast is generated | Valid call, disputed overcall and remaining commitment are distinct. |

## 91. Property Valuation Reviewer Rotation Breach

Scenario:

A direct-property portfolio requires independent valuation reviewer rotation every three years. The same reviewer signs the fourth annual review, creating a governance breach even though the valuation amount is otherwise supportable.

| Attribute | Value |
|---|---:|
| Current appraised value | 24,800,000 |
| Prior appraised value | 24,200,000 |
| Reviewer tenure | 4 years |
| Rotation limit | 3 years |

Valuation change:

```text
valuation_change = current_appraised_value - prior_appraised_value
valuation_change = 24,800,000 - 24,200,000 = 600,000

reviewer_tenure_excess = reviewer_tenure - rotation_limit
reviewer_tenure_excess = 4 - 3 = 1 year
```

Treatment:

- preserve appraisal report, reviewer identity, tenure record, rotation policy, approval exception and second-review requirement;
- keep valuation amount separate from reviewer-independence breach status;
- mark valuation as governance-limited until an approved independent review or waiver exists;
- show reviewer rotation breach in operations and audit reporting;
- avoid suppressing valuation update solely because governance remediation is pending.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Reviewer exceeds rotation limit | Governance breach is flagged. |
| Valuation amount is otherwise valid | Value can be stored with governance-limited status. |
| Independent review is completed | Breach closes with review evidence. |

## 92. REIT Benchmark Reinclusion Watchlist

Scenario:

A REIT removed from a benchmark becomes eligible for reinclusion after liquidity and free-float recovery. The index provider places it on a watchlist before formal reinclusion.

| Attribute | Value |
|---|---:|
| REIT market value held | 1,850,000 |
| Benchmark weight before deletion | 0.45% |
| Watchlist probability weight | 0.30% |
| Portfolio benchmark value | 420,000,000 |

Watchlist benchmark exposure:

```text
watchlist_benchmark_exposure = portfolio_benchmark_value x watchlist_probability_weight
watchlist_benchmark_exposure = 420,000,000 x 0.30% = 1,260,000

overweight_vs_watchlist = reit_market_value_held - watchlist_benchmark_exposure
overweight_vs_watchlist = 1,850,000 - 1,260,000 = 590,000
```

Treatment:

- preserve index provider watchlist notice, eligibility criteria, liquidity evidence, free-float data and effective review date;
- keep watchlist state separate from actual benchmark inclusion;
- do not rewrite benchmark-relative attribution until formal reinclusion is effective;
- support scenario analysis for potential rebalance impact;
- label watchlist exposure as provisional in portfolio and risk reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Watchlist notice arrives | Provisional benchmark exposure is calculated but not treated as formal benchmark weight. |
| Formal reinclusion is confirmed | Benchmark history versions from effective date. |
| Watchlist is withdrawn | Provisional exposure is cleared with source evidence. |

## 93. Concession Reserve-Account Lockup

Scenario:

An infrastructure concession requires a reserve account to remain locked after a debt-service coverage trigger is breached. Cash is held in the project vehicle but is not distributable to investors.

| Attribute | Value |
|---|---:|
| Reserve-account cash | 6,400,000 |
| Required locked reserve | 5,750,000 |
| Excess potentially releasable | 650,000 |
| Client look-through share | 2.20% |

Client locked cash exposure:

```text
client_locked_cash = required_locked_reserve x client_lookthrough_share
client_locked_cash = 5,750,000 x 2.20% = 126,500

client_potential_release = excess_potentially_releasable x client_lookthrough_share
client_potential_release = 650,000 x 2.20% = 14,300
```

Treatment:

- preserve concession agreement, reserve-account statement, debt-service coverage test, lockup notice and release conditions;
- separate project cash from distributable cash;
- show locked reserve, excess reserve and release conditions in liquidity reporting;
- update income projection and distribution forecast while lockup remains active;
- avoid treating reserve-account cash as available fund liquidity.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Lockup trigger is active | Required reserve is excluded from distributable liquidity. |
| Excess reserve exists | Excess remains conditional until release evidence exists. |
| Coverage ratio cures | Lockup release follows concession terms and source evidence. |

## 94. Renewable Hedge Replacement Execution Slippage

Scenario:

A renewable power asset loses its price-floor hedge and executes a replacement hedge later than planned. During the gap, merchant price exposure remains unhedged.

| Attribute | Value |
|---|---:|
| Forecast production during gap | 95,000 MWh |
| Original floor price | 52 |
| Spot forward price during gap | 46 |
| Client look-through share | 2.50% |

Gap revenue at risk:

```text
gap_floor_protection_lost = max(original_floor_price - spot_forward_price_during_gap, 0) x forecast_production_during_gap
gap_floor_protection_lost = max(52 - 46, 0) x 95,000 = 570,000

client_gap_revenue_at_risk = gap_floor_protection_lost x client_lookthrough_share
client_gap_revenue_at_risk = 570,000 x 2.50% = 14,250
```

Treatment:

- preserve terminated hedge, replacement hedge plan, execution timestamps, gap period, production forecast and price source;
- separate execution slippage from hedge termination and from final realized merchant revenue;
- show unhedged gap exposure in risk and income forecast reporting;
- track whether replacement hedge execution met mandate and operating thresholds;
- avoid assuming replacement hedge coverage before execution is confirmed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Replacement hedge is delayed | Gap exposure is calculated for the unhedged period. |
| Forecast production changes | Gap exposure recalculates from sourced forecast. |
| Replacement hedge executes | Future exposure uses replacement terms from execution timestamp. |

## 95. Data-Centre SLA Cure-Period Expiry

Scenario:

A data-centre tenant gives the operator a cure period after repeated uptime SLA breaches. The cure period expires without evidence of remediation, increasing termination and service-credit risk.

| Attribute | Value |
|---|---:|
| Monthly contracted rent | 1,200,000 |
| Potential service-credit percentage | 10.00% |
| Cure-period months impacted | 2 |
| Client ownership share | 1.50% |

Client exposure after cure expiry:

```text
gross_potential_service_credit = monthly_contracted_rent x potential_service_credit_percentage x cure_period_months_impacted
gross_potential_service_credit = 1,200,000 x 10.00% x 2 = 240,000

client_potential_income_reduction = gross_potential_service_credit x client_ownership_share
client_potential_income_reduction = 240,000 x 1.50% = 3,600
```

Treatment:

- preserve SLA breach notices, cure-period start/end dates, remediation evidence, tenant communications and operator response;
- separate expired cure status from agreed service-credit settlement;
- update tenant risk, income risk and operational-quality reporting;
- escalate termination risk if lease terms allow termination after cure expiry;
- avoid booking service credits until settlement or contractual calculation is confirmed.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Cure period expires without evidence | Cure-expired state and potential credit exposure are visible. |
| Remediation evidence arrives late | Cure state is updated with timestamped evidence. |
| Tenant settlement is agreed | Potential exposure converts to confirmed service-credit treatment. |

## 96. Real-Estate Fund Recallable Distribution Funding Priority Dispute

Scenario:

A real-estate fund has both unfunded commitment and recallable distribution balances. A new capital call states that recallable distributions should be used first, but investor records show a side-letter priority requiring new commitment funding before recallable reuse.

| Attribute | Value |
|---|---:|
| Capital call amount | 420,000 |
| Recorded recallable balance | 210,000 |
| Unfunded commitment | 500,000 |
| Disputed priority amount | 210,000 |

Funding split under disputed priority:

```text
recallable_priority_amount = min(capital_call_amount, recorded_recallable_balance)
recallable_priority_amount = min(420,000, 210,000) = 210,000

new_commitment_funding_if_recallable_blocked = capital_call_amount
new_commitment_funding_if_recallable_blocked = 420,000
```

Treatment:

- preserve capital-call notice, recallable distribution ledger, side-letter priority terms, administrator position and investor dispute evidence;
- show call funding under both administrator and investor-priority views while dispute is open;
- reserve liquidity according to approved funding policy and escalation decision;
- keep recallable balance, unfunded commitment and disputed priority amount separately reconciled;
- avoid consuming recallable balance until the priority basis is resolved.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Funding priority conflicts | Disputed priority amount is calculated and routed for review. |
| Administrator accepts side-letter priority | Funding split updates and recallable balance remains intact. |
| Funding forecast is generated | Recallable-supported, commitment-funded and disputed amounts are distinct. |

## 97. Property Reviewer Remediation Waiver

Scenario:

A direct-property valuation reviewer exceeded the normal rotation limit. Governance grants a short remediation waiver while a second independent reviewer is appointed. The valuation can be used with a governance-limited status, but the waiver must not be treated as a permanent cure.

| Attribute | Value |
|---|---:|
| Current appraised value | 24,800,000 |
| Reviewer tenure | 4 years |
| Rotation limit | 3 years |
| Waiver period | 60 days |

Waiver excess:

```text
reviewer_tenure_excess = reviewer_tenure - rotation_limit
reviewer_tenure_excess = 4 - 3 = 1 year
```

Treatment:

- preserve appraisal report, reviewer identity, rotation breach, waiver approval, waiver expiry and second-review plan;
- keep valuation amount, reviewer breach and remediation waiver as separate states;
- label valuation as governance-limited until independent review is complete;
- escalate when waiver expires without remediation evidence;
- avoid treating the waiver as proof of reviewer independence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Waiver is approved | Valuation remains usable with governance-limited status. |
| Waiver expires | Breach escalates if second review is incomplete. |
| Independent review arrives | Governance limitation closes with source evidence. |

## 98. REIT Reinclusion Effective-Date Dispute

Scenario:

A REIT is formally reincluded in a benchmark, but the index provider and internal benchmark file show different effective dates. Portfolio attribution must not rewrite benchmark exposure before the accepted effective date.

| Attribute | Value |
|---|---:|
| REIT market value held | 1,850,000 |
| Index-provider effective date | Day 15 |
| Internal file effective date | Day 14 |
| Formal benchmark weight | 0.42% |
| Portfolio benchmark value | 420,000,000 |

Effective benchmark exposure:

```text
formal_benchmark_exposure = portfolio_benchmark_value x formal_benchmark_weight
formal_benchmark_exposure = 420,000,000 x 0.42% = 1,764,000
```

Treatment:

- preserve index-provider notice, internal benchmark file, effective dates, benchmark version and reconciliation owner;
- apply formal benchmark weight only from the accepted effective date;
- retain disputed-date attribution as provisional until benchmark source is resolved;
- avoid restating prior-day active weight without approved benchmark history correction;
- show effective-date dispute in performance and benchmark governance reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Effective dates conflict | Benchmark exposure is labelled disputed. |
| Provider date is confirmed | Benchmark history updates from confirmed date. |
| Attribution report is generated | Official and disputed-date exposures are separately traceable. |

## 99. Concession Reserve Release Approval

Scenario:

An infrastructure concession reserve-account lockup is partially released after debt-service coverage improves. The project vehicle holds more cash than required, but release requires lender approval before cash becomes distributable.

| Attribute | Value |
|---|---:|
| Reserve-account cash | 6,400,000 |
| Required locked reserve after cure | 5,200,000 |
| Lender-approved release | 900,000 |
| Client look-through share | 2.20% |

Client approved release:

```text
potential_release = reserve_account_cash - required_locked_reserve_after_cure
potential_release = 6,400,000 - 5,200,000 = 1,200,000

client_approved_release = lender_approved_release x client_lookthrough_share
client_approved_release = 900,000 x 2.20% = 19,800
```

Treatment:

- preserve reserve statement, coverage-ratio cure evidence, lender approval, release amount, release date and distribution policy;
- separate potential release from lender-approved release;
- treat unreleased excess as locked or conditional liquidity until approval exists;
- update distribution forecast only for approved release amounts;
- avoid classifying reserve release as operating income.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Coverage ratio cures | Potential release is calculated. |
| Lender approves partial release | Only approved amount becomes distributable. |
| Liquidity report is generated | Locked reserve, potential release and approved release are distinct. |

## 100. Renewable Replacement Hedge Price Slippage Claim

Scenario:

A renewable asset executes a replacement power-price hedge after a delayed approval. The replacement price is worse than the approved target price, and the manager files a slippage claim against the execution provider.

| Attribute | Value |
|---|---:|
| Hedged forecast production | 95,000 MWh |
| Approved target hedge price | 48 |
| Executed replacement hedge price | 46.50 |
| Client look-through share | 2.50% |

Client slippage exposure:

```text
gross_price_slippage = (approved_target_hedge_price - executed_replacement_hedge_price) x hedged_forecast_production
gross_price_slippage = (48 - 46.50) x 95,000 = 142,500

client_slippage_exposure = gross_price_slippage x client_lookthrough_share
client_slippage_exposure = 142,500 x 2.50% = 3,562.50
```

Treatment:

- preserve replacement hedge mandate, approval timestamp, execution record, price source, slippage calculation and provider response;
- separate market price movement from execution slippage;
- keep slippage claim as disputed exposure until settled or rejected;
- update income forecast from executed hedge terms, not target terms;
- avoid treating claim recovery as renewable operating revenue.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Execution price is below target | Slippage exposure is calculated. |
| Claim is filed | Claim remains disputed until provider response. |
| Forecast is generated | Future revenue uses executed hedge price. |

## 101. Data-Centre Tenant Termination Notice After Cure Expiry

Scenario:

A data-centre tenant issues a termination notice after an SLA cure period expires. The operator disputes whether the notice is valid, so lease income, service credits and occupancy risk must remain separately modelled.

| Attribute | Value |
|---|---:|
| Monthly contracted rent | 1,200,000 |
| Notice period months | 6 |
| Potential termination rent at risk | 7,200,000 |
| Client ownership share | 1.50% |

Client termination exposure:

```text
gross_termination_rent_at_risk = monthly_contracted_rent x notice_period_months
gross_termination_rent_at_risk = 1,200,000 x 6 = 7,200,000

client_termination_exposure = gross_termination_rent_at_risk x client_ownership_share
client_termination_exposure = 7,200,000 x 1.50% = 108,000
```

Treatment:

- preserve SLA breach history, cure-period expiry, termination notice, operator dispute, lease terms and legal response;
- keep tenant termination risk separate from service-credit settlement;
- update occupancy, income forecast and valuation sensitivity while dispute is open;
- avoid treating disputed termination notice as final vacancy until source outcome is confirmed;
- show notice validity, exposure and cure history in reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Termination notice is received | Termination exposure and dispute state are opened. |
| Operator disputes validity | Occupancy remains active but risk-labelled. |
| Termination becomes effective | Income forecast and occupancy update from effective date. |

## 102. Recallable Distribution Priority Settlement Reversal

Scenario:

A real-estate fund resolves a recallable distribution funding priority dispute by using recallable distributions first. Later, a side-letter review reverses that settlement and requires commitment-funded treatment instead.

| Attribute | Value |
|---|---:|
| Settled recallable-funded amount | 210,000 |
| Correct commitment-funded amount | 210,000 |
| Recallable balance to reinstate | 210,000 |
| Unfunded commitment reduction | 210,000 |

Priority reversal:

```text
recallable_balance_reinstatement = settled_recallable_funded_amount
recallable_balance_reinstatement = 210,000

correct_commitment_funding = correct_commitment_funded_amount
correct_commitment_funding = 210,000
```

Treatment:

- preserve original settlement, side-letter review, reversal approval, administrator correction and investor ledger update;
- reinstate recallable balance and reduce unfunded commitment according to corrected priority;
- keep reversal separate from a new capital call or distribution;
- update cash forecasts and commitment reporting with source-labelled correction;
- avoid losing the original settlement version.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Priority settlement is reversed | Recallable and commitment ledgers are corrected with lineage. |
| Cash already funded | Cash remains reconciled while funding classification changes. |
| Investor report is generated | Original settlement, reversal and corrected funding priority are visible. |

## 103. Property Reviewer Waiver Expiry Breach

Scenario:

A property valuation reviewer remediation waiver expires before the second independent review is completed. The valuation amount may still be the latest available appraisal, but it must be marked governance-breached until reviewer remediation evidence arrives.

| Attribute | Value |
|---|---:|
| Current appraised value | 24,800,000 |
| Waiver period | 60 days |
| Days since waiver approval | 75 |
| Client ownership share | 1.75% |

Expired waiver exposure:

```text
waiver_expiry_breach_days = days_since_waiver_approval - waiver_period
waiver_expiry_breach_days = 75 - 60 = 15

client_governance_limited_value = current_appraised_value x client_ownership_share
client_governance_limited_value = 24,800,000 x 1.75% = 434,000
```

Treatment:

- preserve appraisal, reviewer tenure breach, waiver approval, waiver expiry, second-review plan and escalation owner;
- keep appraised value, governance breach and valuation usability state separate;
- label valuation as governance-breached after waiver expiry until independent review completes;
- route reporting and advisory workflows when governance-limited value is material;
- avoid treating expired waiver as active reviewer independence evidence.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Waiver expires before second review | Valuation remains available but governance-breached. |
| Second review arrives | Breach closes with source evidence. |
| Report is generated during breach | Appraised value and governance limitation are both visible. |

## 104. REIT Reinclusion Backdated Benchmark Restatement

Scenario:

An index provider confirms that a REIT benchmark reinclusion effective date should be backdated. Performance attribution must restate benchmark exposure for the affected days without changing portfolio holdings.

| Attribute | Value |
|---|---:|
| Portfolio benchmark value | 420,000,000 |
| Formal benchmark weight | 0.42% |
| Backdated restatement days | 2 |
| Portfolio REIT holding value | 1,850,000 |

Backdated benchmark exposure:

```text
restated_benchmark_exposure = portfolio_benchmark_value x formal_benchmark_weight
restated_benchmark_exposure = 420,000,000 x 0.42% = 1,764,000

active_exposure_after_restatement = portfolio_REIT_holding_value - restated_benchmark_exposure
active_exposure_after_restatement = 1,850,000 - 1,764,000 = 86,000
```

Treatment:

- preserve index-provider correction, prior benchmark file, backdated effective date, restatement approval and attribution rerun evidence;
- restate benchmark exposure only for approved effective dates;
- keep portfolio holding history unchanged;
- label performance restatement as benchmark-history correction, not trade activity;
- retain original and restated attribution versions for audit.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Provider confirms backdated date | Benchmark history is restated from confirmed date. |
| Portfolio holdings are unchanged | Holding position and transaction history remain unchanged. |
| Attribution report is regenerated | Original and restated benchmark exposure are traceable. |

## 105. Concession Reserve Release Reversal Dispute

Scenario:

A concession reserve release was approved and forecast as distributable, but a lender later reverses part of the release because coverage-ratio evidence was restated. The platform must reverse distributable liquidity while preserving the reserve account history.

| Attribute | Value |
|---|---:|
| Lender-approved release | 900,000 |
| Reversed release amount | 250,000 |
| Client look-through share | 2.20% |
| Prior client approved release | 19,800 |

Client reversal:

```text
client_release_reversal = reversed_release_amount x client_lookthrough_share
client_release_reversal = 250,000 x 2.20% = 5,500

revised_client_approved_release = prior_client_approved_release - client_release_reversal
revised_client_approved_release = 19,800 - 5,500 = 14,300
```

Treatment:

- preserve original lender approval, restated coverage-ratio evidence, reversal notice, reserve statement and revised release file;
- reduce distributable liquidity only by source-confirmed reversal amount;
- keep reserve-account cash, released cash and reversed release separately reconciled;
- avoid treating reversal as operating expense or income loss;
- update client distribution forecasts and infrastructure liquidity reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Lender reverses part of release | Distributable release is reduced with source evidence. |
| Reserve cash remains held | Reserve account balance and release forecast reconcile. |
| Liquidity report is generated | Approved, reversed and revised release amounts are separate. |

## 106. Renewable Hedge Slippage Claim Settlement

Scenario:

A renewable replacement hedge slippage claim is partially settled by the execution provider. The settlement must reduce the disputed slippage exposure without changing the executed hedge price used for revenue forecasts.

| Attribute | Value |
|---|---:|
| Gross price slippage claim | 142,500 |
| Provider settlement | 80,000 |
| Client look-through share | 2.50% |
| Client original slippage exposure | 3,562.50 |

Client settlement:

```text
client_claim_settlement = provider_settlement x client_lookthrough_share
client_claim_settlement = 80,000 x 2.50% = 2,000

client_unrecovered_slippage = client_original_slippage_exposure - client_claim_settlement
client_unrecovered_slippage = 3,562.50 - 2,000 = 1,562.50
```

Treatment:

- preserve replacement hedge execution, target price approval, slippage claim, provider settlement and cash receipt;
- keep executed hedge price unchanged for future revenue forecasts;
- classify settlement as claim recovery, not renewable operating revenue;
- track unrecovered slippage until rejected, settled or written off;
- report claim settlement separately from market price movement.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Claim settlement is received | Disputed exposure reduces by settled amount. |
| Hedge forecast is generated | Forecast still uses executed hedge price. |
| Client report is generated | Claim settlement and unrecovered slippage are visible. |

## 107. Data-Centre Tenant Termination Withdrawal Notice

Scenario:

A data-centre tenant withdraws a termination notice after a commercial settlement. The platform must reverse termination exposure while preserving SLA cure-period history and any service-credit settlement.

| Attribute | Value |
|---|---:|
| Gross termination rent at risk | 7,200,000 |
| Settlement service credit | 350,000 |
| Client ownership share | 1.50% |
| Termination exposure withdrawn | 7,200,000 |

Client exposure reversal:

```text
client_termination_exposure_reversal = termination_exposure_withdrawn x client_ownership_share
client_termination_exposure_reversal = 7,200,000 x 1.50% = 108,000

client_service_credit_impact = settlement_service_credit x client_ownership_share
client_service_credit_impact = 350,000 x 1.50% = 5,250
```

Treatment:

- preserve original termination notice, withdrawal notice, settlement terms, service-credit agreement, SLA cure history and lease status;
- remove termination exposure from vacancy forecast when withdrawal is source-confirmed;
- keep service-credit settlement separate from rent continuation and termination reversal;
- retain SLA breach history for operational risk reporting;
- update income forecast and occupancy state from withdrawal effective date.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Withdrawal notice is confirmed | Termination exposure is reversed. |
| Service credit remains payable | Service-credit impact remains separate from rent forecast. |
| Report is generated | Cure history, withdrawal state and settlement impact are traceable. |

## 108. Recallable Priority Reversal Tax Reclassification

Scenario:

A recallable distribution priority settlement reversal changes the investor tax classification from recallable distribution usage to new commitment funding. The platform must update tax labels without changing the actual cash funded.

| Attribute | Value |
|---|---:|
| Reclassified funding amount | 210,000 |
| Original recallable distribution tax basis | 210,000 |
| Correct commitment-funded basis | 210,000 |
| Cash funded | 210,000 |

Tax reclassification:

```text
tax_basis_reclassified = original_recallable_distribution_tax_basis
tax_basis_reclassified = 210,000

cash_change_required = cash_funded - correct_commitment_funded_basis
cash_change_required = 210,000 - 210,000 = 0
```

Treatment:

- preserve original tax classification, side-letter reversal, administrator correction, investor ledger and amended tax statement;
- update tax labels and funding classification without changing cash funded;
- keep recallable balance reinstatement, commitment funding and tax basis correction separate;
- block final tax reporting until administrator confirmation is available;
- retain original and corrected tax classification versions.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Tax classification changes | Tax labels update without cash movement. |
| Administrator evidence is missing | Corrected tax output is blocked or provisional. |
| Investor report is generated | Original classification, reversal and corrected tax basis are visible. |

## 109. Property Waiver Duplicate Suppression

Scenario:

A property reviewer waiver expiry breach is reported twice by separate governance feeds. The platform must suppress the duplicate breach event while preserving both feed records for audit and reviewer-independence monitoring.

| Attribute | Value |
|---|---:|
| Current appraised value | 24,800,000 |
| Client ownership share | 1.75% |
| Duplicate waiver breach feeds | 2 |
| Active breach events allowed | 1 |

Duplicate suppression:

```text
suppressed_duplicate_breaches = duplicate_waiver_breach_feeds - active_breach_events_allowed
suppressed_duplicate_breaches = 2 - 1 = 1

client_governance_limited_value = current_appraised_value x client_ownership_share
client_governance_limited_value = 24,800,000 x 1.75% = 434,000
```

Treatment:

- preserve both governance feed records, appraisal id, waiver id, expiry date, reviewer id and suppression decision;
- keep one active breach event per waiver/appraisal combination;
- avoid duplicate valuation restrictions, duplicate client notices or duplicate remediation tasks;
- route mismatched duplicate feed values to governance review;
- show duplicate-suppressed count in control reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Duplicate breach feed arrives | No second active breach is created. |
| Duplicate has different expiry date | Event is routed for review instead of suppressed silently. |
| Report is generated | Active breach and suppressed duplicate evidence are traceable. |

## 110. REIT Restatement Attribution Reversal

Scenario:

A REIT benchmark reinclusion restatement is reversed by the index provider after attribution was rerun. The platform must reverse the benchmark-attribution adjustment without changing portfolio holdings or market value.

| Attribute | Value |
|---|---:|
| Restated benchmark exposure | 1,764,000 |
| Original benchmark exposure | 0 |
| Portfolio REIT holding value | 1,850,000 |
| Reversal effective days | 2 |

Attribution reversal:

```text
benchmark_exposure_reversal = restated_benchmark_exposure - original_benchmark_exposure
benchmark_exposure_reversal = 1,764,000 - 0 = 1,764,000

active_exposure_after_reversal = portfolio_REIT_holding_value - original_benchmark_exposure
active_exposure_after_reversal = 1,850,000 - 0 = 1,850,000
```

Treatment:

- preserve original benchmark file, restatement file, provider reversal notice, attribution rerun and reversal approval;
- reverse benchmark attribution only for the affected effective dates;
- keep portfolio holdings, transactions and market valuation unchanged;
- label performance delta as benchmark restatement reversal rather than investment return;
- retain original, restated and reversed attribution versions.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Provider reverses benchmark restatement | Attribution restatement is reversed from source-effective date. |
| Holdings are checked | Portfolio holdings remain unchanged. |
| Performance report is generated | Original, restated and reversed benchmark versions are visible. |

## 111. Concession Release Reversal Tax Treatment

Scenario:

A concession reserve release reversal changes the tax label on a previously expected distribution. The platform must update tax treatment without creating a new operating cashflow.

| Attribute | Value |
|---|---:|
| Prior client approved release | 19,800 |
| Client release reversal | 5,500 |
| Taxable distribution previously projected | 19,800 |
| Revised taxable distribution | 14,300 |

Tax treatment reversal:

```text
taxable_distribution_reversal = taxable_distribution_previously_projected - revised_taxable_distribution
taxable_distribution_reversal = 19,800 - 14,300 = 5,500

revised_client_approved_release = prior_client_approved_release - client_release_reversal
revised_client_approved_release = 19,800 - 5,500 = 14,300
```

Treatment:

- preserve lender reversal, reserve statement, tax classification rule, prior projection and revised tax output;
- reduce projected taxable distribution only for source-confirmed reversal amount;
- keep reserve-account cash and tax classification separate from operating income;
- block final tax output when tax treatment source evidence is missing;
- show release, reversal and revised tax label in investor reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Release reversal changes tax label | Taxable distribution projection is reduced. |
| Cash is not distributed | No new operating cashflow is created. |
| Investor report is generated | Prior and revised tax treatment are traceable. |

## 112. Renewable Slippage Settlement Clawback

Scenario:

An execution provider claws back part of a renewable hedge slippage claim settlement after discovering that part of the production volume was outside the approved replacement window. The platform must reopen disputed exposure without changing the executed hedge price.

| Attribute | Value |
|---|---:|
| Client claim settlement received | 2,000 |
| Provider clawback amount | 650 |
| Client original slippage exposure | 3,562.50 |
| Prior unrecovered slippage | 1,562.50 |

Clawback exposure:

```text
retained_claim_settlement = client_claim_settlement_received - provider_clawback_amount
retained_claim_settlement = 2,000 - 650 = 1,350

revised_unrecovered_slippage = client_original_slippage_exposure - retained_claim_settlement
revised_unrecovered_slippage = 3,562.50 - 1,350 = 2,212.50
```

Treatment:

- preserve provider settlement, clawback notice, approved replacement window, production evidence and cash reversal;
- reduce retained settlement by the source-confirmed clawback only;
- keep executed hedge price unchanged for revenue forecasts;
- report clawback as claim-settlement correction rather than market price movement;
- keep reopened slippage exposure until settled, rejected or written off.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Provider clawback arrives | Retained settlement and unrecovered slippage are recalculated. |
| Hedge forecast is generated | Forecast still uses executed hedge price. |
| Client report is generated | Settlement, clawback and reopened exposure are separate. |

## 113. Data-Centre Withdrawal Notice Delivery Failure

Scenario:

A data-centre tenant termination withdrawal notice is source-confirmed, but delivery to downstream reporting and advisor workflow fails for some portfolios. The platform must keep lease status updated while blocking completion of the notice-control workflow.

| Attribute | Value |
|---|---:|
| Portfolios requiring withdrawal notice update | 42 |
| Successful notice deliveries | 37 |
| Failed notice deliveries | 5 |
| Approved delivery exceptions | 1 |

Unresolved delivery failures:

```text
unresolved_delivery_failures = failed_notice_deliveries - approved_delivery_exceptions
unresolved_delivery_failures = 5 - 1 = 4

delivery_completion_rate = successful_notice_deliveries / portfolios_requiring_withdrawal_notice_update
delivery_completion_rate = 37 / 42 = 88.10%
```

Treatment:

- preserve tenant withdrawal notice, portfolio impact list, delivery batch, failed recipients, retry evidence and exceptions;
- update source-effective lease and occupancy state from the confirmed withdrawal;
- keep notice-control completion separate from lease-state update;
- block final client-ready reporting where required notice delivery remains unresolved;
- route failures to advisor workflow and reporting operations.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Withdrawal is source-confirmed | Lease status updates from source-effective date. |
| Notice delivery fails | Notice-control workflow remains open. |
| Exception is approved | Only approved exception reduces unresolved failures. |
| Portfolio report is generated | Delivery state is blocked or labelled according to policy. |

## 114. Recallable Tax Reclassification Amended Statement

Scenario:

A recallable priority reversal tax reclassification requires an amended investor statement. The amendment changes funding and tax labels but does not change the cash funded.

| Attribute | Value |
|---|---:|
| Reclassified funding amount | 210,000 |
| Cash funded | 210,000 |
| Prior statement recallable basis | 210,000 |
| Correct commitment-funded basis | 210,000 |

Amended statement check:

```text
statement_basis_reclassified = prior_statement_recallable_basis
statement_basis_reclassified = 210,000

cash_change_required = cash_funded - correct_commitment_funded_basis
cash_change_required = 210,000 - 210,000 = 0
```

Treatment:

- preserve original statement, administrator tax correction, side-letter reversal, amended statement and delivery evidence;
- update tax and funding labels without changing cash-funded amount;
- suppress duplicate active statements after amended version is approved;
- block final delivery when administrator correction or statement approval is missing;
- show original and amended statement lineage in investor reporting.

QA assertions:

| Scenario | Expected behavior |
|---|---|
| Amended statement is approved | Tax and funding labels are corrected. |
| Cash amount is unchanged | No cash movement is created. |
| Prior statement exists | Prior statement is superseded with lineage. |
| Delivery evidence is missing | Amended output remains blocked or provisional. |
