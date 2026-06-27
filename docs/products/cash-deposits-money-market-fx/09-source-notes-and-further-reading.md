# 09 — Source Notes and Further Reading

This knowledge pack is written for wealth-management learning, advisory, portfolio analytics and platform design. It is not legal, tax, accounting or investment advice. Always align implementation with local regulation, bank policy, product termsheets, accounting policy and source-system contracts.

## Key references used

### Deposit insurance and Singapore cash/deposit treatment

- MoneySense Singapore — Understanding Deposit Insurance: https://www.moneysense.gov.sg/understanding-deposit-insurance/

- Singapore Deposit Insurance Corporation — Deposit Insurance Scheme FAQs: https://www.sdic.org.sg/di_faq/

### Money market funds and liquidity products

- Investor.gov / SEC — Money Market Funds Investor Bulletin: https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins/updated-12

- SEC — Money Market Fund Reforms: https://www.sec.gov/rules-regulations/2023/07/s7-22-21

- MAS — Code on Collective Investment Schemes: https://www.mas.gov.sg/regulation/codes/code-on-collective-investment-schemes

### FX market structure and settlement risk

- BIS — 2025 Triennial Central Bank Survey of foreign exchange and OTC derivatives markets: https://www.bis.org/statistics/rpfx25.htm

- MAS — Foreign Exchange & Derivatives market information: https://www.mas.gov.sg/development/foreign-exchange

- CLS — CLSSettlement and payment-versus-payment settlement: https://www.cls-group.com/products/settlement/clssettlement/

- Bundesbank — Continuous Linked Settlement overview: https://www.bundesbank.de/en/tasks/payment-systems/oversight/continuous-linked-settlement-626502

### Investor protection and FX risk

- FINRA — Retail foreign exchange risk / regulatory notice: https://www.finra.org/rules-guidance/notices/08-66

- MAS — Investor Alert List: https://www.mas.gov.sg/investor-alert-list

## Further study topics

To deepen the knowledge base further, consider separate packs or addenda for:

1. **Credit lines, Lombard lending, margin and collateral**
   Buying power, pledged assets, haircut, loan-to-value, in-flight orders and cross-pledging.

2. **Portfolio settlement and corporate-action cashflows**
   Unified treatment of settlements, income, tax, fee, CA entitlements and failed trades.

3. **FX overlays and currency attribution**
   Hedge ratio, local return versus currency return, forward carry and hedge effectiveness.

4. **Treasury and ALM concepts for wealth platforms**
   Deposit funding, transfer pricing, interest-rate sensitivity and liquidity management.

5. **Cash forecasting and liquidity planning**
   Capital calls, DPM cash buffers, withdrawals, income projections and stress scenarios.

6. **Regulatory and jurisdictional treatment**
   Deposit protection, client money rules, MAS/SFA/CIS treatment, product eligibility and disclosure.

## Suggested review questions

1. What is the difference between ledger cash, settled cash and available cash?
2. How would you model a fixed deposit in a portfolio platform?
3. Why is a money market fund not the same as cash?
4. How should FX spot be modelled as transactions?
5. How is an FX forward different from FX spot from a position perspective?
6. How would you calculate projected cash by value date?
7. How should foreign currency cash affect performance?
8. What controls prevent settlement failure?
9. How should pledged cash affect buying power?
10. How would you report liquidity in a DPM portfolio?
11. How would you explain foreign currency deposit risk to a client?
12. What test cases would you create for cash and FX migration?

## Knowledge-base quality standard

Every future product pack should continue to include:

- business explanation;
- product taxonomy;
- lifecycle and transactions;
- position modelling;
- valuation and income;
- performance and risk analytics;
- advisory and mandate treatment;
- reporting treatment;
- platform data model;
- controls and reconciliation;
- QA/test scenarios;
- glossary and practitioner reference;
- source notes.
