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

## 33. Advisory And Mandate Checklist

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

## 34. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed REIT | security position, trades, price, distributions, corporate actions, gearing where sourced | property-sector look-through and detailed operating-metric analytics |
| Private real estate fund | commitment, calls, NAV, distributions, valuation date, queues, holdbacks, development drawdowns, capex reserves and tenant/default indicators where sourced | advanced liquidity queue simulation, project-level cashflow and manager-level stress modelling |
| Direct property | ownership record, appraisal value, source date, documents, ownership percentage, sale state, committee override, zoning, insurance, environmental and covenant status where sourced | document-backed ownership graph and property-level cashflow model |
| Infrastructure exposure | fund/security/private fund position, sector tags, concession, revenue model, clawback terms, availability deductions and regulated asset base reset terms where sourced | advanced concession, inflation-linkage, offtake, clawback and regulatory-risk analytics |
| Reporting | wrapper, exposure, value, income, source date, liquidity label, operating metrics, development state and governance overrides where sourced | consolidated real-asset income and stress dashboard |

## 35. Regression Test Pack

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
