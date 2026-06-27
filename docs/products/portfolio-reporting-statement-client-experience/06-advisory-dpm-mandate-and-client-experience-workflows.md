# 06 - Advisory, DPM, Mandate and Client Experience Workflows

## 1. Reporting in the advisor workflow

Advisor reporting should support preparation, conversation, action and follow-up.

```text
Review trigger
  -> Advisor dashboard
  -> Portfolio diagnosis
  -> Client conversation
  -> Proposal / action plan
  -> Suitability and mandate checks
  -> Order execution
  -> Post-trade reporting
  -> Next review cycle
```

## 2. Advisor dashboard

The advisor dashboard should highlight what needs attention.

Recommended widgets:

| Widget | Purpose |
|---|---|
| Total wealth and change | High-level view of client value. |
| Performance versus benchmark | Conversation starter. |
| Allocation versus target | Drift and rebalancing trigger. |
| Concentration alerts | Single-name/issuer/sector/currency risk. |
| Upcoming events | Bond maturities, note observations, fund gates, loan reviews. |
| Liquidity view | Cash, liquid assets, lockups, unfunded commitments. |
| Income view | Received/projected income. |
| Suitability alerts | Product/risk mismatch. |
| Data exceptions | Stale prices, missing NAV, pending corporate actions. |

## 3. Client portfolio review conversation

A good portfolio review explains:

- what changed;
- why performance moved;
- whether objectives remain valid;
- where risk has increased;
- what action is recommended;
- what no-action risk exists;
- what costs, tax and liquidity consequences apply;
- what client approval is needed.

## 4. DPM mandate review

DPM reporting should show both outcome and process quality.

| Area | Reporting treatment |
|---|---|
| Objective | Mandate objective and client risk profile. |
| Benchmark | Linked benchmark and benchmark changes. |
| Allocation | Current, target and range. |
| Drift | Breach/near-breach indicators. |
| Performance | Absolute, relative, net/gross where applicable. |
| Attribution | What drove relative performance. |
| Risk | Volatility, drawdown, tracking error, concentration, liquidity. |
| Trading | Material changes, turnover, rebalancing. |
| Compliance | Breaches, waivers, client restrictions. |
| Outlook/actions | Portfolio manager commentary and planned actions. |

## 5. Mandate reporting principles

Mandate reports must avoid vague statements. For each mandate rule, show:

- rule name;
- threshold/range;
- current value;
- status;
- evaluation date;
- source/calculation method;
- breach duration;
- remediation owner;
- exception/waiver reference.

## 6. Advisory suitability reporting

Suitability reporting should not be reduced to a pass/fail flag. It should show the rationale.

Potential suitability dimensions:

| Dimension | Example |
|---|---|
| Risk profile | Product risk does not exceed client risk profile. |
| Knowledge/experience | Client has complex-product experience. |
| Investment objective | Product supports income/growth/preservation objective. |
| Time horizon | Product tenor fits client horizon. |
| Liquidity need | Lock-up does not conflict with liquidity needs. |
| Concentration | Issuer/product/asset exposure remains within threshold. |
| Currency | Client understands FX exposure. |
| Leverage | Margin/loan use is permitted and affordable. |
| Product governance | Product is approved for client segment and booking centre. |

## 7. Proposal-to-report continuity

A proposal should be reportable after execution. The system should connect:

```text
Proposal -> Recommended trades -> Suitability checks -> Orders -> Executions -> Transactions -> Positions -> Post-trade report
```

This enables the advisor to show the client whether the proposal achieved the intended allocation/risk change.

## 8. Client experience design

Client-facing reporting should be:

- clear without oversimplifying risk;
- consistent across mobile, web and PDF;
- explainable by the advisor;
- transparent about assumptions;
- accessible for non-expert clients;
- detailed enough for sophisticated clients;
- segment-aware for affluent, HNW, UHNW and family-office clients.

## 9. HNW/UHNW reporting needs

HNW/UHNW clients often require:

- consolidated reporting across entities and custodians;
- family-level reporting;
- trust/foundation/holding-company hierarchy;
- private-market commitments and capital calls;
- bespoke benchmark and mandate views;
- tax-aware reporting packages;
- alternative asset and direct investment reporting;
- liquidity planning;
- multi-currency wealth reporting;
- advisor commentary and CIO views;
- customized report templates.

## 10. RM/advisor notes and commentary

Advisor commentary should be governed.

Controls:

- distinguish system-generated commentary from advisor-entered text;
- store author and timestamp;
- maintain approval status where needed;
- avoid promissory or misleading language;
- include evidence for performance/risk claims;
- support comments at portfolio, product and action-plan level;
- retain commentary in archive if client-facing.

## 11. Portfolio review triggers

Examples:

| Trigger | Reason |
|---|---|
| Scheduled review | Annual/semiannual/quarterly review cycle. |
| Large market move | Portfolio risk or allocation materially changed. |
| Mandate breach | Portfolio outside permitted range. |
| Large cash balance | Cash drag or deployment opportunity. |
| Product event | Note maturity, bond maturity, capital call, corporate action. |
| Suitability change | Client risk profile or objective changed. |
| Concentration increase | Single-name or issuer exposure breached threshold. |
| Loan/collateral change | LTV or buying power changed materially. |

## 12. Digital client experience

Digital reporting should support:

- interactive drill-down from total wealth to holding to transaction;
- period selection;
- currency toggle;
- benchmark toggle;
- product-level explanation;
- event timeline;
- downloadable PDF and data export;
- alerts and tasks;
- secure messaging with advisor;
- evidence of report acknowledgement where required.

## 13. Advisor-ready explanation templates

For each reporting issue, prepare an advisor-friendly explanation:

| Issue | Explanation pattern |
|---|---|
| Performance down | Market movement, allocation, product drivers, currency effect. |
| High cash | Liquidity retained, pending investment, opportunity cost. |
| Stale NAV | Latest available NAV date and expected update cycle. |
| Structured note loss | Underlying movement, barrier/strike, issuer valuation. |
| Bond price decline | Yield rise, spread widening, duration effect. |
| FX loss | Reporting-currency translation effect. |
| Mandate drift | Market movement or flows moved allocation outside target. |
| Private-market distribution | Return of capital versus gain/income classification. |

## 14. Client reporting maturity model

| Level | Capability |
|---|---|
| 1 | Static statement with holdings and transactions. |
| 2 | Product-aware valuation, income and activity reporting. |
| 3 | Performance, risk, benchmark and mandate sections. |
| 4 | Advisor dashboard, exceptions and proposal continuity. |
| 5 | Interactive digital reporting with lineage and personalized explanations. |
