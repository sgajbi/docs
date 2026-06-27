# 07 - Advisory, Mandate, Suitability, Estate and Reporting Deep Dive

## 1. Advisory purpose framework

Every insurance or annuity recommendation should start from purpose.

| Purpose | Suitable product families |
|---|---|
| Family protection | Term life, whole life, CI/disability riders |
| Wealth transfer | Whole life, universal life, trust-owned policies |
| Estate liquidity | Universal life, whole life, premium-financed policies |
| Retirement income | Annuities, endowments, income riders |
| Education funding | Endowment, savings plans, sometimes ILP |
| Investment-linked growth | ILP, VUL, variable annuity |
| Business protection | Key-person insurance, buy-sell funding |
| Long-term care planning | LTC riders, disability products |
| Collateral / financing | Cash-value policies, assigned policies |

## 2. Suitability questions

| Area | Questions |
|---|---|
| Need | What risk or goal is being addressed? |
| Affordability | Can client maintain premiums through stress? |
| Time horizon | Can client hold long enough to avoid poor surrender outcome? |
| Liquidity | Does client need access to capital? |
| Risk tolerance | For ILP/variable products, can client bear investment volatility? |
| Protection gap | Is coverage amount appropriate? |
| Existing cover | Does client already have similar coverage? |
| Complexity | Does client understand guaranteed vs non-guaranteed values? |
| Health/underwriting | Are exclusions/loadings acceptable? |
| Currency | Does policy currency match future liabilities? |
| Beneficiary/estate | Are ownership and nominations aligned to estate plan? |
| Tax/legal | Is jurisdiction-specific advice required? |
| Replacement risk | Is existing policy surrender/replacement justified? |

## 3. Advisory red flags

| Red flag | Why it matters |
|---|---|
| Client needs short-term liquidity but product has surrender charges | Mismatch |
| Product sold mainly as investment but has heavy insurance charges | Return drag |
| Client believes projected values are guaranteed | Misunderstanding |
| Premium obligation is high relative to income | Lapse risk |
| Client is elderly and product has long breakeven horizon | Suitability risk |
| Policy replacement without clear benefit | Churning/replacement risk |
| ILP sub-funds exceed risk profile | Market risk mismatch |
| Death benefit shown as net worth | Misleading reporting |
| Premium-financed policy depends on low rates | Financing risk |
| Beneficiaries not updated after life event | Estate risk |

## 4. DPM / mandate relevance

Insurance products often sit outside normal discretionary portfolio management.

| Product | DPM treatment |
|---|---|
| Term life | Usually outside investable mandate, shown in protection report |
| Whole life / endowment | Usually outside DPM, may be included in net worth/surrender value |
| ILP | May be non-discretionary or advisory-only; underlying funds may overlap mandate exposures |
| Variable annuity | Usually outside DPM, but account allocation may need risk visibility |
| Premium-financed policy | Important for leverage, collateral and liquidity monitoring |
| Annuity | Retirement-income planning asset, often outside DPM performance benchmark |

### Mandate controls

A DPM platform may still need controls:

- total illiquid exposure including insurance cash values;
- premium obligations versus portfolio liquidity;
- policy-financing loan exposure;
- pledged/assigned policy collateral;
- currency exposure from policy and benefits;
- look-through fund exposure for ILP/variable products;
- concentration in insurer credit exposure;
- reporting exclusions from benchmark performance.

## 5. Advisory versus execution-only

Insurance advice typically requires more fact-finding than simple execution of securities:

- dependents and liabilities;
- income and expense profile;
- health and underwriting disclosures;
- existing policies;
- estate objectives;
- retirement income needs;
- replacement analysis;
- premium sustainability.

A platform should capture advisory rationale and product suitability notes.

## 6. Estate planning and beneficiary reporting

Insurance products are heavily used in estate planning because benefits may be paid to named beneficiaries, subject to legal and jurisdictional rules.

Important data points:

- policy owner;
- life assured;
- beneficiary names/shares;
- revocable/irrevocable nomination;
- trust ownership;
- assignment to lender;
- contingent beneficiaries;
- currency of benefit;
- expected estate liquidity need;
- whether policy proceeds bypass or form part of estate, jurisdiction-specific.

Client reporting should avoid exposing sensitive beneficiary details where privacy rules or report audience do not permit it.

## 7. Client reporting sections

### Protection summary

| Field | Example |
|---|---|
| Insurer | ABC Life |
| Policy type | Whole Life |
| Life assured | Client |
| Sum assured | SGD 1,000,000 |
| Riders | Critical illness SGD 500,000 |
| Premium | SGD 20,000 annual |
| Next premium due | 2026-12-01 |
| Status | Active |

### Policy value summary

| Field | Example |
|---|---|
| Account/cash value | SGD 250,000 |
| Surrender value | SGD 230,000 |
| Guaranteed value | SGD 180,000 |
| Non-guaranteed value | SGD 70,000 |
| Policy loan | SGD 50,000 |
| Net value | SGD 180,000 |

### ILP allocation summary

| Fund | Allocation | Value | Risk |
|---|---:|---:|---|
| Global Equity Fund | 60% | 120,000 | High |
| Global Bond Fund | 30% | 60,000 | Medium |
| Money Market Fund | 10% | 20,000 | Low |

### Annuity income summary

| Field | Example |
|---|---|
| Income amount | SGD 3,000/month |
| Payout start | 2035-01-01 |
| Payout option | Joint life, 10-year guarantee |
| Escalation | 2% p.a. |
| Surrender value | Not available / as reported |

## 8. Analytics and reporting treatment

| Report | Recommended treatment |
|---|---|
| Portfolio valuation | Include only selected value basis, clearly labelled |
| Total wealth | Include surrender/net policy value, not death benefit |
| Protection report | Include coverage/death benefit/riders |
| Retirement plan | Include annuity income and maturity cashflows |
| Cashflow projection | Include future premiums, annuity income, maturity dates |
| Liquidity report | Use surrender value and restrictions |
| Risk report | Include insurer, market, currency, liquidity and lapse risks |
| Performance report | Separate policy performance from normal portfolio benchmark |
| Suitability report | Include purpose, affordability, risk and complexity rationale |
| Mandate report | Include whether policy is in/out of DPM scope |

## 9. Replacement analysis

When replacing or surrendering an existing policy, the advisor should compare:

- surrender charges;
- lost guarantees;
- lost bonuses/dividends;
- new underwriting risk;
- new exclusions/loadings;
- reset of surrender charge period;
- difference in premium obligations;
- difference in coverage amount;
- fund/charge differences for ILPs;
- whether the replacement genuinely improves client outcome.

## 10. Interview-ready advisory answer

"Insurance and annuity products should be assessed based on purpose first: protection, savings, investment-linked growth, retirement income, estate liquidity or legacy planning. Suitability depends not only on risk profile but also premium affordability, liquidity needs, time horizon, health underwriting, policy charges, surrender value, guaranteed versus non-guaranteed benefits and beneficiary structure. In a wealth platform, I would not report death benefit as portfolio market value; I would separately show policy value, surrender value, protection amount, premium obligations, annuity income and ILP look-through exposure where available."
