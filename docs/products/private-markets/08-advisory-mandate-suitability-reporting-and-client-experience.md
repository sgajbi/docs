# 08 — Advisory, Mandate, Suitability, Reporting and Client Experience

## 1. Why private markets matter in advisory

Private markets are often positioned as long-term portfolio diversifiers, return enhancers and access products for HNW/UHNW clients. They also create some of the most important suitability and portfolio-construction challenges in wealth management.

Advisory must look beyond expected return:

```text
Expected return
+ investment horizon
+ illiquidity
+ unfunded commitments
+ valuation uncertainty
+ manager risk
+ fee/carry complexity
+ concentration
+ client sophistication
+ regulatory eligibility
```

## 2. Advisory suitability framework

| Area | Questions |
|---|---|
| Client eligibility | Is the client legally eligible for private/complex/illiquid products? |
| Sophistication | Does the client understand capital calls, lock-ups, NAV lag and possible loss? |
| Risk profile | Can the client tolerate high risk, illiquidity and valuation uncertainty? |
| Liquidity needs | Does the client need access to capital during fund life? |
| Cash-call capacity | Can the client meet unfunded calls in stress? |
| Time horizon | Does the fund life match the client’s planning horizon? |
| Concentration | Is exposure acceptable by asset class, manager, strategy, vintage and geography? |
| Currency | Can the client tolerate capital calls/distributions in fund currency? |
| Fees | Are management fee, performance fee/carry and expenses understood? |
| Tax | Has tax impact been considered? |
| Documentation | Has PPM/LPA/subscription agreement/risk disclosure been reviewed? |
| Portfolio fit | Does the product complement or overconcentrate existing holdings? |

## 3. Client explanation: simple but complete

Advisor explanation:

> A private-market fund is not a normal daily-traded fund. You usually commit a total amount, but the manager calls the money over time. The investment may last many years, and you may not be able to redeem when you want. The reported value may be updated quarterly and can lag current market conditions. Returns are usually assessed using IRR and multiples such as TVPI and DPI, not only daily price return. The investment can lose money, and you must be able to meet future capital calls.

## 4. Mandate treatment

Private markets can appear in different mandate types:

| Mandate type | Treatment |
|---|---|
| Advisory | Client approves each commitment/investment; advisor suitability applies |
| DPM/discretionary | Mandate must explicitly allow illiquid/alternative exposure and unfunded commitments |
| Execution-only | Strong product classification and risk acknowledgement needed |
| Family office/advisory overlay | Consolidated exposure and liquidity planning are key |
| Model portfolio | Private markets may be represented as strategic allocation with pacing plan |

## 5. Mandate rules and constraints

A robust mandate engine should support rules such as:

| Rule | Example |
|---|---|
| Max alternatives allocation | Alternatives <= 25% of portfolio NAV |
| Max private equity | PE <= 15% |
| Max unfunded commitment | Unfunded <= 20% of liquid assets |
| Max manager exposure | Single GP <= 10% |
| Max vintage exposure | One vintage <= 30% of private-market allocation |
| Max strategy exposure | VC <= 5% for balanced mandate |
| Max illiquid exposure | Locked assets <= threshold |
| Min liquidity buffer | Cash/liquid assets >= expected 12-month calls |
| Eligible product list | Only approved funds/managers |
| Currency constraint | Non-base currency commitment <= limit |
| Complexity constraint | Private placements only for eligible profiles |

## 6. Pacing model

Private-market allocation should consider pacing, not just current NAV.

Example:

```text
Target private market allocation = 15% of portfolio
Current NAV allocation = 8%
Unfunded commitments = 6%
Expected future NAV = current NAV + future calls - distributions +/- valuation
```

If the system only checks current NAV, it may recommend too much additional commitment.

A pacing model should include:

- target allocation;
- current NAV;
- unfunded commitments;
- expected call schedule;
- expected distribution schedule;
- existing vintage concentration;
- expected maturity/harvest;
- client liquidity buffer.

## 7. Liquidity planning

Liquidity planning is central.

| Liquidity item | Why it matters |
|---|---|
| Unfunded commitment | Future payment obligation |
| Capital call notice period | Short notice may create stress |
| Liquid asset buffer | Needed to meet calls |
| Borrowing availability | May be used but should not be assumed risk-free |
| Distribution uncertainty | Cannot rely on expected distributions |
| Redemption restrictions | Exits may not be possible |
| Secondary market discount | Liquidity may require selling below NAV |
| Currency mismatch | Calls may be in USD/EUR while client assets are SGD/etc. |

Stress example:

```text
All funds call 30-50% of unfunded commitment over 12 months.
No distributions arrive.
Public markets decline 20%.
Can the client still meet calls without forced sale of unsuitable assets?
```

## 8. Advisory red flags

| Red flag | Why it matters |
|---|---|
| Client needs short-term liquidity | Product may be unsuitable |
| Client focuses only on target return | Risk misunderstood |
| Large unfunded commitment relative to liquid assets | Cash-call stress |
| Too many funds from same GP/vintage | Concentration |
| Distribution misunderstood as income | Reporting/suitability issue |
| NAV date very stale | Valuation uncertainty |
| Fund uses high leverage | Loss magnification |
| Complex side letters/terms | Operational/legal complexity |
| Client cannot read/understand documents | Knowledge gap |
| Cross-border tax uncertainty | Post-sale issue |

## 9. Advisory product comparison

| Product | Good for | Not good for |
|---|---|---|
| PE buyout fund | Long-term growth/diversification | Short-term liquidity need |
| VC fund | High-risk growth exposure | Conservative or income-focused clients |
| Private credit fund | Income-oriented alternatives | Clients assuming bond-like safety |
| Core real estate | Income/real-asset allocation | Clients needing daily liquidity |
| Opportunistic real estate | Higher-risk real asset growth | Low-risk mandates |
| Infrastructure fund | Long-term real asset exposure | Clients sensitive to regulatory/political risk |
| Hedge fund | Diversification/absolute return | Clients needing full transparency/liquidity |
| Co-investment | Concentrated deal access | Clients needing diversification |
| Secondary fund | Private-market exposure with potentially earlier cashflows | Clients assuming discount guarantees return |

## 10. Portfolio analytics and reporting for advisors

Advisory screens should show:

- total alternatives exposure;
- private-market NAV;
- commitments;
- unfunded commitments;
- paid-in capital;
- distributions;
- expected calls;
- liquidity bucket;
- manager concentration;
- vintage concentration;
- strategy concentration;
- NAV date and stale flag;
- IRR, TVPI, DPI, RVPI;
- realised versus unrealised value;
- client eligibility and product-risk status.

## 11. Client statement design

A client statement should avoid presenting private markets like daily-traded funds without context.

Recommended fields:

| Field | Include? | Why |
|---|---|---|
| Commitment amount | Yes | Shows legal exposure |
| Paid-in capital | Yes | Shows funded amount |
| Unfunded commitment | Yes | Shows future obligation |
| NAV | Yes | Shows current reported value |
| NAV date | Yes | Shows valuation freshness |
| NAV status | Yes | Estimated/final/stale/restated |
| Distributions since inception | Yes | Shows realised cash returned |
| IRR | Yes, if reliable | Private-market performance |
| TVPI/DPI/RVPI | Yes | Complement IRR |
| Liquidity terms | Yes | Client understands lock-up/redemption |
| Strategy/vintage | Yes | Context |
| Risk note | Yes | Illiquidity/valuation uncertainty |

## 12. Performance reporting notes

Private markets should have a dedicated performance section rather than being forced into public-market reporting only.

Suggested layout:

```text
Private Markets Summary
- Commitment: X
- Paid-in: Y
- Unfunded: Z
- Current NAV: A as of valuation date
- Distributions: B
- TVPI: (A+B)/Y
- DPI: B/Y
- RVPI: A/Y
- IRR: money-weighted return
- NAV status: final/estimated/stale
```

## 13. DPM/mandate reporting

For DPM mandates, report:

- strategic allocation target;
- current NAV allocation;
- commitment allocation;
- unfunded commitments;
- future call forecast;
- illiquid exposure;
- liquidity buffer;
- rule breaches/exceptions;
- manager/vintage diversification;
- performance by strategy and vintage.

## 14. Suitability evidence

For auditability, store:

| Evidence | Why |
|---|---|
| client eligibility status | Regulatory control |
| product classification | Complex/illiquid/private |
| risk profile at recommendation date | Suitability evidence |
| explanation/disclosure acknowledgement | Client understanding |
| commitment affordability check | Future call suitability |
| liquidity stress result | Mandate/advisory evidence |
| concentration result | Portfolio suitability |
| document version | Audit trail |
| advisor recommendation rationale | Defensible advice |

## 15. Reporting language examples

### Capital call

> The fund has issued a capital call of USD X, representing Y% of your commitment. After this call, your paid-in capital will be USD A and your unfunded commitment will be USD B, subject to any recallable capital terms.

### NAV lag

> The latest reported NAV is as of 31 March 2026 and was received on 20 May 2026. The value may not reflect market or portfolio developments after the valuation date.

### Unfunded commitment

> Your unfunded commitment is not a current market value, but it is a future funding obligation. You should maintain sufficient liquidity to meet future capital calls.

### Distribution classification

> The distribution may include income, realised gain and/or return of capital. Until the manager provides final classification, the amount is shown as unclassified distribution.

## 16. Interview-ready explanation

> Private markets require a different platform and advisory model from public securities. The client may have a legal commitment, funded capital, unfunded future obligations, periodic capital calls, manager-reported NAVs, distributions and long lock-ups. Performance is usually assessed using money-weighted metrics like IRR and multiples such as TVPI, DPI and RVPI, while portfolio reporting also needs NAV, valuation date, liquidity terms, concentration and cash-call exposure. For advisory and DPM, the key is not only expected return but whether the client can tolerate illiquidity, valuation uncertainty, manager risk, concentration and future capital-call obligations.
