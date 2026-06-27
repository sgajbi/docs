# 02 — Private Equity, Venture Capital, Growth, Buyout and Secondaries

## 1. What is private equity?

Private equity is investment in companies that are not publicly listed, usually through a fund managed by a General Partner. The fund invests in private companies, works to improve or scale them, and exits through sale, IPO, recapitalisation or secondary transaction.

Typical fund flow:

```text
LP commits capital -> GP calls capital -> GP invests in companies -> companies grow/restructure -> GP exits -> distributions to LPs
```

Private equity is not one product. It includes several strategies with different risk/return profiles.

## 2. Private equity strategy taxonomy

| Strategy | Stage/company type | Main return source | Key risks |
|---|---|---|---|
| Venture capital | Early-stage/startups | Large growth from successful winners | High failure rate, long duration, write-offs |
| Growth equity | Scaling private companies | Revenue growth and multiple expansion | Valuation risk, execution risk |
| Buyout | Mature companies, often control stakes | Operational improvement, leverage, exit multiple | Leverage, cyclicality, exit timing |
| Turnaround | Underperforming companies | Restructuring and recovery | Execution, legal, liquidity |
| Distressed-for-control | Distressed companies/debt-to-equity | Recovery and control outcomes | Legal/restructuring uncertainty |
| Sector specialist | Healthcare, tech, consumer, infra-related | Specialist sourcing/operating edge | Sector concentration |
| Co-investment | Single deal alongside sponsor | Deal-level exposure with often lower fees | Concentration and sponsor dependence |
| Secondaries | Existing fund interests/deal stakes | Discount to NAV, earlier cashflows | Valuation lag, transfer risk, residual commitments |

## 3. Venture capital

Venture capital invests in young companies with high growth potential and high failure risk.

### Sub-stages

| Stage | Description | Risk profile |
|---|---|---|
| Seed | Product idea/early validation | Very high risk |
| Series A | Early product-market fit | High risk |
| Series B/C | Scaling revenue | High but improving visibility |
| Late-stage venture | Pre-IPO/private growth | Valuation and exit risk |

### Platform implications

VC investments create many edge cases:

- frequent write-ups and write-downs;
- total write-offs for failed companies;
- follow-on rounds and dilution;
- bridge rounds and down rounds;
- exits through IPO, acquisition or secondary sale;
- in-kind distribution of public shares after IPO;
- long tail positions in small private companies;
- vintage-year and manager dispersion analysis.

### Advisory points

VC is suitable only for clients who can tolerate:

- long illiquidity;
- high dispersion between managers;
- multiple failed investments;
- long periods without distributions;
- valuation uncertainty;
- concentration in technology/growth themes.

## 4. Growth equity

Growth equity invests in established private companies that need capital to scale. These companies may have revenue and customer traction but are not yet public.

### Characteristics

| Dimension | Growth equity |
|---|---|
| Company stage | Scaling private company |
| Control | Often minority or significant minority |
| Leverage | Usually less than buyout |
| Return driver | Growth, margin expansion, exit multiple |
| Risk | Valuation, competition, execution, exit market |
| Liquidity | Fund-life / exit driven |

### Platform treatment

Growth equity funds are normally modelled like PE funds:

- commitment;
- capital calls;
- NAV updates;
- distributions;
- unfunded commitments;
- vintage/strategy classification.

If held as a direct private company share, it needs direct private-security modelling with valuation events, dilution and transfer restrictions.

## 5. Buyout funds

Buyout funds acquire controlling or influential stakes in mature companies. They often use leverage at the portfolio-company level.

### Return sources

| Source | Meaning |
|---|---|
| Earnings growth | Improve revenue/profitability |
| Margin expansion | Operational efficiency |
| Multiple expansion | Exit valuation higher than entry valuation |
| Deleveraging | Debt paid down from cashflows |
| Strategic repositioning | M&A, divestment, professionalisation |

### Risks

| Risk | Explanation |
|---|---|
| Leverage risk | Debt magnifies downside |
| Interest-rate risk | Higher financing costs affect leveraged companies |
| Exit risk | IPO/M&A markets may be closed or weak |
| Cyclical risk | Portfolio company earnings may decline in recession |
| Manager risk | Value creation depends heavily on sponsor quality |
| Valuation risk | NAV may lag public-market downturns |

### Analytics

Buyout reporting should support:

- vintage-year comparison;
- sector exposure;
- geography exposure;
- EBITDA/enterprise-value metrics if available;
- leverage exposure if available;
- contribution to portfolio return;
- IRR and multiple-based performance;
- realised versus unrealised value split.

## 6. Co-investments

A co-investment is a direct investment alongside a sponsor, often into a specific portfolio company or asset.

### Why clients like co-investments

- direct deal access;
- lower/no management fee or carry in some structures;
- greater transparency;
- potential to concentrate behind high-conviction opportunities.

### Why they are risky

- single-name concentration;
- adverse selection risk;
- compressed due diligence timeframe;
- sponsor dependency;
- no diversification of a fund;
- difficult valuation and liquidity.

### Platform model

Co-investments can be modelled in two ways:

| Legal wrapper | Suggested model |
|---|---|
| Fund/SPV interest | Alternative fund-like position with deal-level metadata |
| Direct private share | Private equity security with company-level valuation |
| Note/loan into company | Private credit/security model |

Key fields:

- deal_id;
- sponsor_id;
- target_company;
- sector/geography;
- investment date;
- entry valuation;
- ownership percentage if available;
- expected exit horizon;
- valuation source;
- transfer restrictions.

## 7. Secondaries

Secondaries involve buying an existing interest in a private fund, portfolio or company from another investor.

### Types

| Type | Explanation |
|---|---|
| LP-led secondary | Investor sells fund interest to buyer |
| GP-led continuation vehicle | GP transfers assets to new vehicle with optional liquidity for existing LPs |
| Direct secondary | Sale of private company shares |
| Secondary fund | Fund specialising in buying secondary interests |
| Preferred equity secondary | Financing structure with priority economics |

### Why secondaries matter

Secondaries often have:

- shorter remaining fund life;
- existing portfolio visibility;
- potentially earlier distributions;
- purchase price at discount or premium to reported NAV;
- transferred unfunded commitments;
- complex settlement and transfer approval.

### Modelling secondary purchase

Important fields:

| Field | Explanation |
|---|---|
| reported_nav_at_reference_date | Seller's reported NAV |
| purchase_price | Actual price paid |
| discount_premium_to_nav | Purchase price vs NAV |
| transferred_commitment | Original commitment transferred |
| transferred_unfunded | Future obligation assumed by buyer |
| effective_date | Date economics transfer |
| closing_date | Legal transfer date |
| interim cashflows | Calls/distributions between effective and closing |
| transfer approval status | GP/administrator consent |

### Transaction example

Buyer purchases a fund interest with reported NAV 1,000,000 at 90% of NAV and assumes 200,000 unfunded commitment.

| Transaction | Amount | Effect |
|---|---:|---|
| ALT_SECONDARY_BUY | -900,000 cash | Create interest at cost |
| ALT_COMMITMENT_TRANSFER_IN | 1,200,000 commitment base | Record original/transferred commitment |
| ALT_UNFUNDED_ASSUMED | 200,000 | Record future obligation |
| ALT_NAV_ESTABLISH | 1,000,000 NAV | Establish reported value |

The difference between NAV and purchase price should be tracked for performance and unrealised gain/loss policy.

## 8. Direct private company shares

Sometimes wealth clients hold direct private company shares, founder shares, employee shares, pre-IPO shares or private placements.

### Modelling requirements

| Area | Requirement |
|---|---|
| Instrument master | Private company identifier, share class, rights, restrictions |
| Position | Shares/units, cost basis, valuation source |
| Valuation | Latest funding round, independent valuation, manager estimate, transaction price |
| Corporate actions | Split, recapitalisation, conversion, down round, merger, IPO |
| Liquidity | Transfer restrictions, lock-up, ROFR, board approval |
| Reporting | Valuation date, stale price flag, concentration warning |

### IPO transition

When a private company goes public:

```text
Private share position -> listed share position
```

Possible transaction sequence:

| Transaction | Effect |
|---|---|
| PRIVATE_EQUITY_CONVERSION_OUT | Close private share/security |
| EQUITY_DELIVERY_IN | Create listed share position |
| LOCKUP_STATUS_UPDATE | Restrict tradability if lock-up applies |
| COST_BASIS_CARRYOVER | Preserve original basis unless policy requires revaluation |

## 9. Private equity performance metrics

### IRR

IRR is the discount rate that makes cashflows and ending NAV equal zero. It is timing-sensitive and is widely used in private markets.

```text
Capital calls = negative cashflows
Distributions = positive cashflows
Ending NAV = positive terminal value
IRR = rate that makes NPV = 0
```

### TVPI

```text
TVPI = (Distributions + Residual NAV) / Paid-in Capital
```

### DPI

```text
DPI = Distributions / Paid-in Capital
```

### RVPI

```text
RVPI = Residual NAV / Paid-in Capital
```

### PIC

```text
PIC = Paid-in Capital / Commitment
```

### Why multiple metrics matter

| Metric | What it tells you | Limitation |
|---|---|---|
| IRR | Timing-weighted cashflow efficiency | Can be distorted by early distributions/subscription lines |
| TVPI | Total value multiple | Does not show timing |
| DPI | Realised cash returned | Ignores remaining NAV |
| RVPI | Unrealised value left | Depends on valuation estimates |
| PIC | How much commitment is drawn | Does not show performance |

## 10. J-curve

Private equity often shows negative early performance because fees and setup costs are incurred before investments mature.

```text
Early years: fees + investment costs -> negative or low NAV
Middle years: portfolio companies mature -> NAV appreciation
Later years: exits -> distributions and realised gains
```

Reporting must explain that early negative performance may be normal but is not automatically harmless. It still must be monitored against expectations, peers, vintage year and manager updates.

## 11. Advisory checklist for PE/VC

Before recommending or approving PE/VC exposure, check:

| Area | Question |
|---|---|
| Eligibility | Is the client eligible for private/complex/illiquid products? |
| Risk profile | Can the client tolerate capital loss and valuation uncertainty? |
| Liquidity | Can the client lock up capital for the expected fund life? |
| Unfunded calls | Can the client meet future capital calls? |
| Concentration | Is strategy/sponsor/vintage exposure controlled? |
| Time horizon | Does fund life match client planning horizon? |
| Knowledge | Does client understand J-curve, fees, carry, illiquidity and no guaranteed exit? |
| Tax | Are reporting and tax consequences understood? |
| Documentation | Have PPM/LPA/subscription docs and risks been reviewed? |
| Portfolio fit | Does exposure complement or overconcentrate existing assets? |
