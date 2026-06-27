# 10. Source Notes and Further Reading

This pack is educational and implementation-oriented. It uses regulatory and investor-education sources to inform the framework, but does not provide legal or regulatory advice.

## 1. Singapore / MAS references

### MAS Guidelines on Fair Dealing

Source: Monetary Authority of Singapore, updated Guidelines on Fair Dealing, 30 May 2024.

Relevance:

- fair dealing outcomes,
- product/service suitability for target customer segments,
- board and senior management responsibilities,
- conduct and customer-outcome lens.

URL:

- https://www.mas.gov.sg/regulation/guidelines/guidelines-on-fair-dealing---board-and-senior-management-responsibilities-for-delivering-fair-dealing-outcomes-to-customers
- https://www.mas.gov.sg/news/media-releases/2024/mas-expands-application-of-fair-dealing-guidelines

### MAS Notice FAA-N16: Recommendations on Investment Products

Relevance:

- standards for financial advisers and representatives when making recommendations on investment products,
- suitability/recommendation controls,
- advisory-process relevance.

URL:

- https://www.mas.gov.sg/regulation/notices/notice-faa-n16

### MAS Notice SFA 04-N12: Sale of Investment Products

Relevance:

- customer account review and customer knowledge assessment requirements for specified investment products with retail customers,
- complex-product access controls.

URL:

- https://www.mas.gov.sg/regulation/notices/notice-sfa-04-n12

### MoneySense: Specified Investment Products

Relevance:

- investor-friendly explanation of SIPs,
- CAR/CKA context,
- retail complex-product safeguards.

URL:

- https://www.moneysense.gov.sg/understanding-specified-investment-products/

## 2. Europe / UK product governance references

### ESMA Guidelines on MiFID II product governance

Relevance:

- target-market definition,
- product governance expectations,
- manufacturer/distributor responsibilities,
- product clustering considerations,
- distribution strategy.

URL:

- https://www.esma.europa.eu/sites/default/files/2023-08/ESMA35-43-3448_Guidelines_on_product_governance.pdf
- https://www.esma.europa.eu/press-news/esma-news/esma-updates-its-guidance-product-governance

### FCA Product Governance Sourcebook

Relevance:

- target market,
- distribution strategy,
- product governance controls,
- UK product-manufacturer/distributor lens.

URL:

- https://handbook.fca.org.uk/handbook/prod3

### PRIIPs Key Information Document framework

Relevance:

- pre-contract disclosure for packaged retail and insurance-based investment products,
- KID contents and retail investor protection.

URL:

- https://finance.ec.europa.eu/consumer-finance-and-payments/retail-financial-services/key-information-documents-packaged-retail-and-insurance-based-investment-products-priips_en
- https://eur-lex.europa.eu/EN/legal-content/summary/key-information-about-investment-products.html

## 3. US references

### FINRA Rule 2111 Suitability

Relevance:

- suitability framework,
- customer investment profile,
- recommendation-specific analysis,
- client risk/time horizon/liquidity/objective inputs.

URL:

- https://www.finra.org/rules-guidance/rulebooks/finra-rules/2111
- https://www.finra.org/rules-guidance/key-topics/suitability

### Regulation Best Interest

Relevance:

- broker-dealer best-interest standard,
- care, disclosure, conflict and compliance obligations,
- recommendation governance.

URL:

- https://www.finra.org/rules-guidance/key-topics/regulation-best-interest
- https://www.sec.gov/about/divisions-offices/division-trading-markets/broker-dealers/staff-bulletin-standards-conduct-broker-dealers-investment-advisers-conflicts-interest

## 4. IOSCO references

### Suitability Requirements with respect to the Distribution of Complex Financial Products

Relevance:

- global principles for distribution of complex products,
- suitability and disclosure focus,
- lessons from financial crisis and conduct-risk context.

URL:

- https://www.iosco.org/library/pubdocs/pdf/ioscopd400.pdf
- https://www.fsb.org/2013/01/cos_130122/

## 5. How to use these sources in platform design

Convert regulatory principles into configurable system controls:

| Principle | Platform implementation |
|---|---|
| product suitable for target segment | target-market attributes and eligibility rules |
| clear disclosure | document store, version, delivery and acknowledgement controls |
| recommendation suitability | suitability engine inputs and reason codes |
| client knowledge/experience | CAR/CKA or equivalent knowledge profile fields |
| product governance | APU approval workflow and status model |
| ongoing review | review dates, event triggers and watchlist/suspension states |
| conflict management | cost/fee/share-class fields and advisor disclosure workflow |
| auditability | decision snapshot and rule versioning |

## 6. Practical caution

Regulatory language differs by jurisdiction. A global wealth platform should separate:

- universal product-governance concepts,
- local jurisdiction rules,
- firm policy,
- client classification,
- product-type rules,
- channel-specific controls.

Do not hardcode jurisdiction-specific legal conclusions into product master. Use policy/rule layers so that updates can be made with governance and audit control.
