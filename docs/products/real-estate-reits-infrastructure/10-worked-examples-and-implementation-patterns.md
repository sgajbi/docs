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

## 70. Advisory And Mandate Checklist

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

## 71. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed REIT | security position, trades, price, distributions, corporate actions, gearing where sourced | property-sector look-through and detailed operating-metric analytics |
| Private real estate fund | commitment, calls, NAV, distributions, valuation date, queues, holdbacks, development drawdowns, capex reserves and tenant/default indicators where sourced | advanced liquidity queue simulation, project-level cashflow and manager-level stress modelling |
| Direct property | ownership record, appraisal value, source date, documents, ownership percentage, sale state, committee override, zoning, insurance, environmental and covenant status where sourced | document-backed ownership graph and property-level cashflow model |
| Infrastructure exposure | fund/security/private fund position, sector tags, concession, revenue model, clawback terms, availability deductions and regulated asset base reset terms where sourced | advanced concession, inflation-linkage, offtake, clawback and regulatory-risk analytics |
| Reporting | wrapper, exposure, value, income, source date, liquidity label, operating metrics, development state and governance overrides where sourced | consolidated real-asset income and stress dashboard |

## 72. Regression Test Pack

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
