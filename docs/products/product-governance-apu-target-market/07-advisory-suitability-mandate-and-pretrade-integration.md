# 07. Advisory, Suitability, Mandate and Pre-Trade Integration

## 1. How product governance connects to advisory

Product governance defines whether a product can be used. Advisory determines whether it should be recommended to a specific client.

The workflow should look like this:

```text
Product approved in APU
    ->
Client eligible under target-market and cross-border rules
    ->
Product compatible with client risk profile and knowledge
    ->
Product compatible with portfolio and mandate
    ->
Concentration and exposure checks pass
    ->
Disclosures delivered and acknowledged
    ->
Recommendation rationale captured
    ->
Order created and pre-trade checks pass
```

## 2. Advisory recommendation inputs

A recommendation should consider:

- client objective,
- risk profile,
- investment horizon,
- liquidity need,
- loss capacity,
- knowledge and experience,
- existing portfolio composition,
- concentration after trade,
- product risk and complexity,
- costs and alternatives,
- product target market,
- negative target market,
- tax/cross-border restrictions,
- mandate constraints,
- current market view,
- house view / research stance,
- conflicts and inducements,
- disclosure status.

## 3. Suitability hierarchy

Suitability should be checked at multiple levels.

| Level | Example check |
|---|---|
| Product-level | product approved and in target market |
| Client-level | client risk profile and knowledge match |
| Portfolio-level | no concentration or liquidity mismatch |
| Mandate-level | product allowed under strategy restrictions |
| Order-level | size, price, currency and exposure are acceptable |
| Post-trade level | portfolio remains suitable after execution |

## 4. Product governance inputs to suitability engine

| Input | Why needed |
|---|---|
| product risk rating | match to client risk profile |
| complexity level | determine knowledge/disclosure requirements |
| liquidity category | compare with client/mandate liquidity needs |
| target market | confirm client segment/objective/horizon alignment |
| negative target market | detect explicit mismatch |
| leverage flag | detect leverage suitability and mandate constraints |
| max loss profile | check loss capacity |
| currency risk | assess FX suitability |
| income/capital classification | compare with objective |
| issuer/manager ID | concentration and issuer limits |
| valuation transparency | disclosure and risk reporting |
| product status | approved/restricted/suspended |
| required disclosures | pre-trade evidence |

## 5. Suitability reason codes

A good platform should produce reason codes that advisors and auditors can understand.

Examples:

| Reason code | Meaning |
|---|---|
| PRODUCT_APPROVED_FOR_CLIENT_SEGMENT | product target market includes client type |
| RISK_PROFILE_MATCH | client risk profile meets product risk |
| OBJECTIVE_MATCH | product objective matches client objective |
| HORIZON_MATCH | client horizon sufficient |
| LIQUIDITY_MISMATCH | product liquidity too restrictive |
| KNOWLEDGE_GAP | client lacks required experience |
| COMPLEX_PRODUCT_DISCLOSURE_REQUIRED | additional disclosure required |
| CONCENTRATION_LIMIT_BREACH | post-trade concentration too high |
| CURRENCY_RISK_WARNING | product currency differs from client base currency |
| MANDATE_RESTRICTION_BREACH | portfolio mandate does not allow product |
| PRODUCT_NEGATIVE_TARGET_MARKET_MATCH | client falls into negative target market |

## 6. Advisory proposal integration

In proposal generation, product governance should determine:

- whether product can appear as a recommendation,
- whether it can appear as an alternative only,
- whether it requires enhanced explanation,
- whether advisor can generate proposal without supervisor review,
- what documents must be attached,
- what warnings appear in client pack,
- what suitability rationale is required,
- whether client acknowledgement is mandatory,
- whether digital signing is allowed.

## 7. Mandate integration

Mandates need eligibility rules beyond client suitability.

Examples:

| Mandate | Product governance consideration |
|---|---|
| conservative income | restrict equities, high-yield, complex notes, illiquids |
| balanced | allow diversified funds/bonds/equities within bands |
| growth | allow equities and alternatives within limits |
| alternatives mandate | allow hedge/private markets, restrict simple cash drag |
| capital preservation | restrict non-principal-protected products |
| ESG mandate | require sustainability classification/exclusions |
| Sharia mandate | require Sharia-compliant product classification |
| bespoke UHNW | allow exceptions with IPS documentation |

## 8. DPM model portfolio integration

For DPM, product governance should control:

- which instruments can enter model portfolios,
- whether a product is strategic or tactical,
- maximum position weight,
- liquidity limits,
- risk budget,
- issuer/manager exposure,
- turnover constraints,
- substitute product mapping,
- model versioning,
- exception approval,
- post-trade drift monitoring.

DPM may have products approved for discretionary use that are not suitable for direct advisory sale.

## 9. Pre-trade controls

Pre-trade checks should combine product governance and portfolio constraints.

Minimum checks:

| Check | Example |
|---|---|
| APU status | product approved and active |
| client eligibility | client segment/jurisdiction allowed |
| disclosure | required documents delivered/acknowledged |
| risk profile | client can hold product risk level |
| knowledge/experience | client understands complex product |
| concentration | issuer/asset/product limits |
| mandate | product allowed by mandate |
| liquidity | product not incompatible with needs |
| buying power | sufficient cash/collateral |
| restricted list | issuer/security not blocked |
| sanctions/cross-border | country/person/product restrictions |
| order size | min/max ticket, denomination, lot size |
| currency | FX risk and settlement currency handled |

## 10. Execution-only integration

Execution-only does not mean uncontrolled.

Controls may still include:

- product APU eligibility,
- client knowledge/experience assessment,
- complex-product warnings,
- disclosure delivery,
- acknowledgement capture,
- restricted-list check,
- order-size and lot-size validation,
- appropriateness check,
- target-market warnings,
- post-trade monitoring.

The system must distinguish:

- recommendation suitability,
- appropriateness / knowledge assessment,
- execution-only client instruction.

## 11. Exception and override handling

Overrides should be rare, controlled and explainable.

Override fields:

| Field | Meaning |
|---|---|
| override_id | unique ID |
| blocked_rule | rule being overridden |
| override_reason | business rationale |
| approving_person | supervisor/committee |
| client_confirmation | if required |
| expiry | one-time or time-limited |
| impacted_order_id | linked order |
| evidence | documents/call notes |
| post_review_required | yes/no |

Hard-stop rules should not be overrideable unless explicitly configured.

Examples of non-overrideable rules:

- product not legally offerable in jurisdiction,
- sanctions restriction,
- client classification not eligible by law,
- missing mandatory disclosure for retail sale,
- product globally suspended.

## 12. Post-sale monitoring

Product governance also supports post-sale monitoring.

Monitor:

- product held by clients outside target market,
- product status changed to hold-only/suspended,
- client risk profile changed,
- mandate changed,
- concentration increased due to market movement,
- product risk rating increased,
- liquidity deteriorated,
- issuer downgraded,
- fund gated,
- disclosure or regulatory classification changed.

Post-sale monitoring should generate advisor action lists and reporting caveats.

## 13. Client reporting integration

Client reports should reflect governance attributes:

- complex product indicator,
- product risk rating,
- liquidity category,
- issuer/manager exposure,
- maturity/lock-up date,
- income type,
- valuation source,
- stale price flag,
- product status warning,
- mandate breach warning,
- concentration warning,
- disclosure caveat.

## 14. Advisory workflow example

Client wants to buy a private credit fund.

1. Product is in APU for accredited clients only.
2. Client is accredited and has alternatives knowledge.
3. Product has quarterly NAV and 5-year lock-up.
4. Client liquidity need is moderate, not short-term.
5. Mandate allows alternatives up to 15%.
6. Post-trade alternatives exposure would be 18%.
7. System blocks order or requires portfolio rebalance.
8. Advisor explains liquidity and concentration issue.
9. Client either reduces order size or updates mandate/exception process.
10. Decision and rationale are stored.

## 15. Advisory-practitioner summary

> Product governance says what the firm is willing and allowed to distribute. Suitability says whether the product is right for this client. Mandate rules say whether it fits this portfolio strategy. Pre-trade controls ensure the specific order does not breach limits. Good platforms connect all four layers with clear reason codes, evidence and audit lineage.
