# 10 - Source Notes and Further Reading

## 1. Purpose

This file lists external standards, investor-education references and practitioner sources that support the concepts in this pack. It is not a legal, tax, compliance or investment advice document. Always apply local regulatory, business and firm policy requirements.

## 2. Core performance standards

### CFA Institute / GIPS Standards

The Global Investment Performance Standards (GIPS) are voluntary ethical standards for calculating and presenting investment performance, built around fair representation and full disclosure. They are important for understanding performance presentation, composites, pooled funds, policies, calculation consistency and disclosures.

References:

- CFA Institute / GIPS: https://rpc.cfainstitute.org/gips-standards
- GIPS Standards for Firms: https://www.gipsstandards.org/standards/gips-standards-for-firms/
- 2020 GIPS Standards for Firms PDF: https://www.gipsstandards.org/wp-content/uploads/2021/03/2020_gips_standards_firms.pdf

Key learning points for platform design:

- Define calculation methodology.
- Define composites and pooled-fund treatment.
- Apply policies consistently.
- Present performance fairly with adequate disclosure.
- Maintain records and calculation support.

## 3. Performance evaluation and attribution

CFA Institute performance and attribution materials are useful for understanding attribution, contribution, risk and return evaluation.

References:

- CFA Institute topic page: https://rpc.cfainstitute.org/topics/performance-attribution
- CFA Institute Portfolio Performance Evaluation refresher: https://www.cfainstitute.org/insights/professional-learning/refresher-readings/2026/portfolio-performance-evaluation
- CFA Institute Research Foundation Performance Attribution literature review: https://rpc.cfainstitute.org/sites/default/files/-/media/documents/book/rf-lit-review/2019/rflr-performance-attribution.pdf
- CFA Institute CIPM Program: https://www.cfainstitute.org/programs/cipm

Key learning points:

- Performance measurement answers what return was achieved.
- Attribution helps identify sources of return and active return.
- Risk evaluation and manager selection are closely related to performance evaluation.

## 4. Benchmark governance

IOSCO's Principles for Financial Benchmarks are relevant for understanding benchmark governance, quality, methodology and accountability. In a wealth platform, the same principles support benchmark data governance and effective-dated benchmark assignment.

References:

- IOSCO Principles for Financial Benchmarks PDF: https://www.iosco.org/library/pubdocs/pdf/ioscopd415.pdf
- IOSCO guidance on benchmark principles implementation: https://www.iosco.org/library/pubdocs/pdf/ioscopd549.pdf

Key learning points:

- Benchmarks need governance and methodology quality.
- Benchmark administrators should have accountability mechanisms.
- Users of benchmarks need to understand benchmark construction and limitations.

## 5. Marketing and performance presentation

The SEC Investment Adviser Marketing Rule is relevant when performance is used in advertisements or marketing material. Even if a wealth platform is not directly producing US adviser advertising, the rule is useful as a discipline for avoiding misleading performance presentation.

References:

- SEC Marketing Compliance FAQs: https://www.sec.gov/rules-regulations/staff-guidance/division-investment-management-frequently-asked-questions/marketing-compliance-frequently-asked-questions
- SEC Investment Adviser Marketing page: https://www.sec.gov/investment/marketing-rule

Key learning points:

- Performance presentation can be misleading if basis, time period and methodology are not clear.
- Gross and net performance treatment must be controlled where required.
- Hypothetical, extracted or model performance needs special care and governance.

## 6. Suitability, concentration and investor risk education

FINRA's investor and rule materials are useful for understanding suitability, concentration risk and communication of investment risks.

References:

- FINRA Rule 2111 Suitability: https://www.finra.org/rules-guidance/rulebooks/finra-rules/2111
- FINRA Suitability FAQ: https://www.finra.org/rules-guidance/key-topics/suitability/faq
- FINRA Concentration Risk investor education: https://www.finra.org/investors/insights/concentration-risk
- FINRA Risk investor education: https://www.finra.org/investors/investing/investing-basics/risk

Key learning points:

- Recommendations should be suitable given customer profile and product risk/reward.
- Concentration can exist across asset class, sector, issuer, security, maturity, currency or product type.
- Investor-facing risk explanations should be clear and not limited to volatility.

## 7. Risk analytics references

Risk concepts such as volatility, VaR, expected shortfall, duration, tracking error and stress testing are common across CFA, risk-management, regulatory and industry materials. For platform implementation, the key is to define methodology, inputs and limitations rather than relying on a metric label alone.

Suggested areas for further study:

- CFA risk management and portfolio management materials
- CIPM performance and risk evaluation materials
- BIS/IOSCO market and risk publications
- Vendor documentation for benchmark, risk factor and fixed-income analytics data

## 8. Product pack cross-references

Use this pack together with the earlier product packs:

| Product pack | Analytics relevance |
|---|---|
| Notes | Structured product performance, issuer risk, barriers and autocalls |
| Bonds | Accrual, yield, duration, spread and maturity analytics |
| Funds | NAV, distributions, look-through and fund fees |
| Equities | Corporate actions, dividends, sector/country/factor analytics |
| Derivatives | Greeks, notional exposure, margin and hedge analytics |
| Private markets | IRR, multiples, commitments and NAV lag |
| Cash/FX | Liquidity, FX translation and settlement analytics |
| Loans/collateral | Buying power, LTV, leverage and collateral analytics |
| Insurance | Cash value, premiums, surrender value and estate reporting |
| Tax/reporting | Income classification, tax lots and reporting disclosures |

## 9. Platform caution

This pack intentionally distinguishes between:

- Calculation methodology
- Regulatory reporting requirement
- Client-facing disclosure
- Advisory interpretation
- Internal risk monitoring

A single metric such as return, income, exposure or risk can have different definitions depending on business purpose. Platform design should therefore make definitions explicit, configurable, versioned and auditable.

## 10. Recommended study path

1. Understand TWR and MWR deeply.
2. Understand cashflow classification.
3. Learn contribution by asset class/security.
4. Learn Brinson attribution.
5. Learn currency attribution.
6. Learn fixed income attribution.
7. Learn benchmark construction and composite policy.
8. Learn risk metrics and limitations.
9. Learn mandate analytics and breach lifecycle.
10. Learn reporting controls, lineage and restatement governance.
