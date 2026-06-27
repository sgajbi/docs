# 03. Target Market, Client Segmentation and Distribution Strategy

## 1. What target market means

Target market defines the type of clients for whom a product is intended, based on the product's features, risks, complexity, investment objective, liquidity, time horizon and expected client needs.

It should identify both:

- positive target market: who the product may be suitable for,
- negative target market: who the product should not be sold to or recommended to.

Target market is a product-level concept. Suitability is a client-specific concept.

## 2. Why target market matters

Target-market classification prevents products from being distributed too broadly.

Examples:

| Product | Appropriate target-market considerations |
|---|---|
| cash deposit | capital preservation, short-term liquidity |
| investment-grade bond | income, moderate risk, interest-rate tolerance |
| high-yield bond | income, credit-risk tolerance, higher risk profile |
| equity fund | growth objective, equity volatility tolerance |
| private equity fund | long horizon, illiquidity tolerance, eligible investor status |
| worst-of autocallable note | sophisticated investor, complex payoff understanding, downside equity tolerance |
| FX option | FX knowledge, derivative approval, margin/settlement capability |
| insurance ILP | protection/investment mix, long-term horizon, fee/surrender understanding |

## 3. Target-market dimensions

A robust target-market framework should include:

| Dimension | Examples |
|---|---|
| client classification | retail, accredited, professional, institutional |
| risk tolerance | conservative, balanced, growth, aggressive |
| investment objective | income, growth, preservation, hedging, speculation, diversification |
| investment horizon | short, medium, long, very long |
| liquidity need | daily, monthly, quarterly, locked-up |
| knowledge/experience | basic, funds, bonds, derivatives, alternatives, private markets |
| loss capacity | no capital loss, partial loss, full loss, loss beyond initial investment |
| product complexity | non-complex, complex, highly complex |
| leverage tolerance | none, embedded, explicit margin, unlimited/uncapped |
| currency tolerance | base currency only, multi-currency, FX conversion risk |
| income need | regular income, accumulation, no income |
| tax/reporting sensitivity | FATCA/CRS, withholding, tax document needs |
| ESG/preference | exclusions, sustainability objective, transition risk |
| legal/regulatory eligibility | country, residency, investor status, age restrictions |

## 4. Negative target market

Negative target market is essential because it prevents inappropriate distribution even when a product is commercially attractive.

Examples:

| Product | Negative target market example |
|---|---|
| leveraged ETF | clients seeking long-term buy-and-hold capital preservation |
| private equity fund | clients needing short-term liquidity |
| high-yield bond | clients unable to tolerate credit loss |
| autocallable note | clients who do not understand conditional capital risk |
| perpetual bond | clients with short horizon or unable to accept extension risk |
| commodity futures | clients unable to meet margin calls |
| insurance ILP | clients seeking low-cost pure investment exposure without protection needs |
| dual-currency investment | clients unwilling to hold alternate currency |

A good system should not only check positive target market; it should explicitly detect negative target-market conflict.

## 5. Client classification

Client classification often drives regulatory obligations and product eligibility.

Common classes:

| Classification | Typical implication |
|---|---|
| Retail client | highest protection, more disclosures, stricter suitability/appropriateness |
| Accredited investor | broader product access, reduced protections if opted-in where applicable |
| Professional client | broader product universe, more responsibility assumed |
| Institutional client | institution-level sophistication, negotiated terms |
| Selected/vulnerable client | enhanced safeguards, call-backs, documentation, supervisory review |
| US person / EEA resident / cross-border client | additional restrictions and tax/regulatory documents |

Client classification should be time-versioned. A client can change status.

## 6. Distribution strategy

Distribution strategy defines how the product can be sold or recommended.

| Distribution channel | Typical controls |
|---|---|
| DPM | mandate rules, investment committee approval, periodic reporting |
| Advisory | suitability assessment, advisor explanation, client consent |
| Execution-only | appropriateness/knowledge check, no recommendation language |
| Online self-directed | digital disclosure, knowledge assessment, product limits |
| RM-assisted execution | interaction record, disclosure capture, order rationale |
| Institutional desk | counterparty onboarding, dealing limits, negotiated documentation |

A product may be approved for DPM but not for self-directed online trading.

## 7. Advisory model eligibility

Product governance should distinguish advisory models.

| Advice model | Product governance implication |
|---|---|
| Discretionary | PM acts under mandate; client-specific pre-trade suitability may be mandate-based |
| Advisory | recommendation must be suitable for client and portfolio |
| Execution-only | no recommendation; appropriateness/disclosure may still apply |
| Research-only | product may be discussed but not transacted |
| Hold-only | no new buy recommendation; existing holding monitored |

## 8. Booking-centre and cross-border eligibility

Cross-border eligibility is often more complex than product risk.

Relevant dimensions:

- client residence,
- citizenship/nationality,
- tax residence,
- booking centre,
- advisor location,
- product domicile,
- issuer domicile,
- offer jurisdiction,
- regulatory registration status,
- marketing restrictions,
- prospectus availability,
- language/document requirements,
- US-person restrictions,
- EU/UK PRIIPs/KID rules,
- local complex-product rules.

System design should avoid a single `country_allowed` field. It needs a structured cross-border matrix.

## 9. Target-market compatibility matrix

Example:

| Product | Retail | Accredited | Professional | DPM | Execution-only | Online |
|---|---:|---:|---:|---:|---:|---:|
| SGD deposit | Yes | Yes | Yes | Yes | Yes | Yes |
| investment-grade bond | Conditional | Yes | Yes | Yes | Conditional | Conditional |
| high-yield bond | Restricted | Yes | Yes | Conditional | Restricted | No |
| UCITS fund | Yes | Yes | Yes | Yes | Yes | Yes |
| hedge fund | No | Conditional | Yes | Conditional | No | No |
| private equity fund | No | Conditional | Yes | Conditional | No | No |
| autocallable note | No / Restricted | Conditional | Yes | Conditional | No | No |
| listed option | Conditional | Yes | Yes | Conditional | Conditional | Conditional |
| OTC derivative | No | Conditional | Yes | Conditional | No | No |

## 10. Target-market attributes for platform implementation

Recommended attributes:

```json
{
  "productId": "FUND123",
  "positiveTargetMarket": {
    "clientSegments": ["ACCREDITED_INVESTOR", "PROFESSIONAL_CLIENT"],
    "riskProfiles": ["BALANCED", "GROWTH", "AGGRESSIVE"],
    "objectives": ["INCOME", "DIVERSIFICATION"],
    "horizonMonthsMin": 36,
    "liquidityNeed": "MONTHLY_OR_LONGER",
    "knowledgeRequired": ["FUNDS", "CREDIT_RISK"]
  },
  "negativeTargetMarket": {
    "riskProfiles": ["CONSERVATIVE"],
    "objectives": ["CAPITAL_GUARANTEE"],
    "liquidityNeed": "DAILY",
    "clientSegments": ["RETAIL_EXECUTION_ONLY"]
  },
  "distributionStrategy": {
    "allowedChannels": ["ADVISORY", "DPM"],
    "blockedChannels": ["ONLINE_SELF_DIRECTED"],
    "requiredDisclosures": ["PROSPECTUS", "PHS", "RISK_DISCLOSURE"]
  }
}
```

## 11. Target-market lifecycle

Target market should be reviewed when:

- product risk changes,
- issuer/manager downgraded,
- liquidity deteriorates,
- market conditions change materially,
- product performance deviates from expected behaviour,
- client complaints increase,
- distribution outside target market is detected,
- product documents change,
- regulation changes,
- new share class or product variant is launched,
- product moves from open to closed / gated / suspended status.

## 12. Target-market controls

| Control | Purpose |
|---|---|
| target market required before approval | no product without intended client definition |
| negative target market required for complex products | prevents broad mis-selling |
| channel restrictions | prevents unsuitable digital or execution-only use |
| booking-centre restrictions | avoids cross-border breach |
| review date | prevents stale classification |
| post-sale distribution monitoring | detects sales outside intended market |
| exception reporting | evidences why exceptions were allowed |
| complaints linkage | identifies target-market failure |
| product outcome monitoring | compares actual outcomes against expected product design |

## 13. Practical advisory interpretation

Advisor-friendly explanation:

> Target market tells us who the product was designed for. Suitability tells us whether it is right for this client and this portfolio. APU eligibility tells us whether our firm allows the product to be used in this jurisdiction, channel and mandate. All three checks must align before a recommendation should proceed.
