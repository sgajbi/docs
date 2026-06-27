# 02. Product Due Diligence, Approval and Approved Product Universe Framework

## 1. Product due diligence overview

Product due diligence is the structured assessment performed before a product enters the Approved Product Universe. It is the bank's internal challenge process over the product's economics, risks, costs, issuer/manager quality, operational feasibility, distribution scope and client suitability conditions.

A good product due-diligence process should produce evidence, not opinions.

## 2. Product approval funnel

```text
Product sourcing
    ->
Initial screening
    ->
Due diligence pack creation
    ->
Risk, legal, compliance, tax, operations and data checks
    ->
Target market and distribution approval
    ->
Committee decision
    ->
APU activation
    ->
Ongoing monitoring and review
```

## 3. Initial screening

Before full due diligence, the firm should determine whether the product is even eligible for review.

Typical screening checks:

| Check | Examples |
|---|---|
| Product type allowed? | fund, note, bond, ETF, derivative, private fund |
| Jurisdiction allowed? | SG, HK, EU, UK, US, Middle East booking centre |
| Client segment relevant? | retail, accredited, professional, private banking, institutional |
| Issuer/manager accepted? | approved issuer, approved asset manager, approved counterparty |
| Minimum data available? | termsheet, prospectus, PHS/KID, risk rating, pricing source |
| Operationally supportable? | custody, settlement, pricing, lifecycle, tax, reporting |
| Regulatory feasibility? | local product licensing, restrictions, cross-border rules |
| Strategic fit? | product shelf, house view, asset allocation, mandate relevance |

A product failing screening should not proceed to advisory availability.

## 4. Due diligence dimensions

### 4.1 Product economics

Assess:

- payoff formula,
- income source,
- principal repayment conditions,
- embedded leverage,
- downside exposure,
- upside cap/participation,
- optionality,
- fees and charges,
- expected holding period,
- liquidity terms,
- early exit cost,
- fair value / value-for-money.

### 4.2 Issuer / manager / counterparty assessment

Different product types require different assessment.

| Product | Key party to review |
|---|---|
| Bond | issuer, guarantor, security trustee |
| Structured note | issuer, arranger, guarantor, calculation agent |
| Fund | manager, fund board, custodian, administrator |
| ETF | issuer, manager, index provider, market maker |
| Derivative | counterparty, clearing broker, CCP |
| Private fund | GP, investment manager, administrator, auditor |
| Insurance product | insurer, reinsurer, distributor |
| Lombard/margin product | lending entity, collateral agent |

Review items:

- financial strength,
- rating/credit quality,
- regulatory status,
- track record,
- litigation/regulatory history,
- governance framework,
- risk-management process,
- operational resilience,
- conflicts of interest,
- pricing transparency,
- reporting quality.

### 4.3 Risk assessment

Product risk assessment should cover:

| Risk type | Examples |
|---|---|
| Market risk | equity, rates, FX, commodity, volatility |
| Credit risk | issuer default, counterparty default, reference entity default |
| Liquidity risk | lock-up, bid/ask spread, gating, no secondary market |
| Complexity risk | embedded derivative, path dependency, nonlinear payoff |
| Leverage risk | margin, futures, options, inverse/leveraged ETP |
| Concentration risk | single issuer, sector, country, theme, manager |
| Operational risk | manual lifecycle, unavailable price, untested settlement |
| Legal risk | enforceability, documentation, jurisdiction conflicts |
| Tax risk | withholding, tax reporting, reclassification |
| Conduct risk | mis-selling, unclear disclosure, fee conflict |
| Model risk | model-based valuation, Greeks, scenario assumptions |

### 4.4 Cost and value assessment

Product approval should include cost transparency:

- explicit fees,
- embedded margin,
- management fee,
- performance fee,
- trailer/rebate,
- bid/ask spread,
- structuring cost,
- distribution fee,
- custody/admin cost,
- early redemption fee,
- surrender charge,
- FX spread,
- financing cost.

For similar products, the due-diligence team should compare available alternatives. For example:

- active fund vs ETF,
- retail share class vs institutional share class,
- structured note vs bond + option strategy,
- listed REIT vs private real estate fund,
- money market fund vs deposit,
- share class A vs share class I.

## 5. Approval decision types

| Decision | Meaning |
|---|---|
| Approved | product can be distributed within defined scope |
| Approved with restrictions | product can be used only under specified conditions |
| Approved for advisory only | execution-only blocked |
| Approved for execution-only only | no advisory recommendation allowed |
| Approved for DPM only | discretionary manager may use, but advisor cannot sell directly |
| Approved for professional/accredited clients only | retail blocked |
| Hold-only approval | existing positions allowed; new purchases blocked |
| Rejected | product cannot be distributed |
| Deferred | missing information or pending committee decision |
| Suspended | temporarily blocked after event |
| Withdrawn | removed from platform |

## 6. Approved Product Universe (APU)

The APU is the controlled set of products available for use by a firm under defined rules.

It should not be a simple product list. It should be a rules-driven eligibility framework.

APU core dimensions:

| Dimension | Example |
|---|---|
| product_id | ISIN / internal instrument ID |
| product_status | approved, restricted, suspended |
| booking_centre | Singapore, Hong Kong, Switzerland, UK |
| client_segment | retail, accredited, professional, institutional |
| channel | advisor-assisted, execution-only, online, DPM |
| advice_model | advisory, discretionary, execution-only |
| mandate_type | income, balanced, growth, alternatives, bespoke |
| jurisdiction_of_client | Singapore resident, EEA resident, US person |
| risk_profile_minimum | moderate, growth, aggressive |
| knowledge_required | options, derivatives, private markets |
| disclosure_required | prospectus, KID/PHS, termsheet, risk disclosure |
| review_expiry_date | next due diligence review date |
| approval_owner | product committee / shelf owner |

## 7. Product shelf versus APU

| Concept | Meaning |
|---|---|
| Product shelf | commercial product range the bank wants to offer |
| APU | controlled set of approved products with eligibility rules |
| Product master | reference data describing instruments |
| House view | investment research opinion or recommendation stance |
| Model portfolio universe | subset usable in model portfolios |
| Mandate universe | subset allowed under a DPM mandate |
| Advisor watchlist | tactical/research list used by advisors |

A product can be in product master but not in APU.

A product can be in APU but not in model portfolios.

A product can be approved for buy in one booking centre and hold-only in another.

## 8. APU decision engine

A platform should compute eligibility using a transparent decision engine.

Example query:

```text
Can client C buy product P in account A through channel X on date D?
```

Inputs:

- product approval status,
- client classification,
- client country/residency,
- booking centre,
- advisor location,
- channel,
- portfolio mandate,
- client risk profile,
- client knowledge/experience,
- disclosure status,
- restricted-list status,
- product risk rating,
- liquidity status,
- concentration after trade,
- order size,
- currency,
- issuer exposure,
- cross-border controls.

Output:

```json
{
  "eligibility": "BLOCKED",
  "reason_codes": ["CLIENT_SEGMENT_NOT_ALLOWED", "DISCLOSURE_NOT_ACCEPTED"],
  "required_actions": ["complete_KID_acknowledgement", "senior_approval_required"],
  "rule_version": "APU_RULES_2026_06_01",
  "decision_time": "2026-06-27T10:00:00+08:00"
}
```

## 9. APU hierarchy

A mature APU has multiple layers:

```text
Global product approval
    ->
Region / booking-centre approval
    ->
Client-segment approval
    ->
Channel approval
    ->
Advice-model approval
    ->
Mandate approval
    ->
Account-level eligibility
    ->
Order-level decision
```

Do not collapse all of these into a single `approved = true` flag.

## 10. Minimum APU controls

| Control | Purpose |
|---|---|
| four-eye approval | no single person can approve product alone |
| maker-checker data activation | product data is reviewed before use |
| effective dating | future-dated and historical decisions are supported |
| expiry/review date | products cannot remain approved indefinitely without review |
| suspension override | urgent market/regulatory events can block trading |
| exception workflow | manual overrides are controlled and auditable |
| source lineage | system knows where data came from |
| rule versioning | historical decisions can be reconstructed |
| downstream acknowledgement | advisory/order systems confirm rule consumption |
| stale-data control | product not tradable if critical fields are missing/stale |

## 11. Example product approval record

```json
{
  "productId": "XS1234567890",
  "productType": "STRUCTURED_NOTE",
  "approvalStatus": "APPROVED_WITH_RESTRICTIONS",
  "approvalDate": "2026-06-01",
  "nextReviewDate": "2026-12-01",
  "approvedBookingCentres": ["SG", "HK"],
  "clientSegmentsAllowed": ["ACCREDITED_INVESTOR", "PROFESSIONAL_CLIENT"],
  "clientSegmentsBlocked": ["RETAIL"],
  "channelsAllowed": ["ADVISORY", "DPM"],
  "channelsBlocked": ["EXECUTION_ONLY_ONLINE"],
  "minimumRiskProfile": "GROWTH",
  "complexityLevel": "COMPLEX",
  "liquidityCategory": "LIMITED_SECONDARY_MARKET",
  "requiredDisclosures": ["TERMSHEET", "RISK_DISCLOSURE", "SCENARIO_ANALYSIS"],
  "requiredKnowledge": ["STRUCTURED_PRODUCTS"],
  "concentrationLimitPct": "10.00",
  "issuerLimitPct": "20.00",
  "ruleVersion": "APU-2026-06-01"
}
```

## 12. Product approval checklist

Before activation, confirm:

- product identity is unique,
- legal documents are stored,
- product type and subtype are correct,
- issuer/manager/counterparty is mapped,
- risk rating is approved,
- complexity classification is approved,
- liquidity classification is approved,
- target market and negative target market are defined,
- client segment restrictions are defined,
- booking-centre restrictions are defined,
- channel restrictions are defined,
- disclosure documents are linked,
- pricing/valuation source is configured,
- lifecycle event support is confirmed,
- tax/reporting classification is configured,
- custody/settlement support is confirmed,
- suitability rule inputs are complete,
- mandate eligibility is configured,
- next review date is populated,
- approval evidence is linked,
- downstream systems receive the approved record.
