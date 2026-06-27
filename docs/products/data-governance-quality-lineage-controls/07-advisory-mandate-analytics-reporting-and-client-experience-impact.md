# 07 - Advisory, Mandate, Analytics, Reporting and Client Experience Impact

## 1. Why data governance matters to front-office outcomes

Advisors, portfolio managers and clients usually do not see data pipelines. They see recommendations, portfolios, reports, mandate breaches, risk scores, performance numbers and explanations. Poor data quality becomes a front-office problem when it changes decisions or reduces trust.

## 2. Advisory impact

| Data issue | Advisory impact |
|---|---|
| Wrong client risk profile | Product recommendation may be unsuitable. |
| Product not mapped to complexity class | Complex product sold without required checks. |
| Missing liquidity flag | Advisor may recommend illiquid product to client needing liquidity. |
| Wrong product currency | Currency risk not explained. |
| Stale price | Proposal allocation and trade size are wrong. |
| Missing concentration exposure | Advisor misses issuer/sector/currency concentration. |
| Fund share class mismatch | Wrong fee, distribution, hedging or eligibility. |
| Structured note payoff terms missing | Downside risk cannot be explained properly. |

## 3. Mandate impact

Mandates depend on clean data.

| Mandate rule | Data dependency |
|---|---|
| Maximum equity exposure | Asset class, look-through, market value. |
| Maximum issuer exposure | Issuer mapping, group hierarchy, structured note issuer. |
| Minimum cash buffer | Cash balance, unsettled trades, deposits, money-market funds policy. |
| Maximum illiquid assets | Liquidity classification, private-market commitments, structured products. |
| No complex products | Product complexity and regulatory classification. |
| ESG restriction | ESG flags, issuer/fund classification, source date. |
| Rating floor | Bond rating, source, watchlist, effective date. |
| Currency exposure limit | Instrument currency, FX derivatives, fund hedge class. |
| Leverage cap | Loans, margin, derivatives notional, collateral and exposure. |

If data is incomplete, mandate engine should not silently pass. It should return pass, fail, warning, degraded or unable-to-evaluate.

## 4. Analytics impact

| Analytics output | Data dependencies | Common failure |
|---|---|---|
| TWR | Market values, external cashflows, transaction classification | Internal trades misclassified as external cashflows. |
| MWR/XIRR | Cashflows, ending value, date precision | Missing capital call or distribution. |
| Contribution | Returns, weights, classification | Product mapped to wrong asset class. |
| Attribution | Portfolio/benchmark weights and returns | Benchmark link or composition stale. |
| Risk | Time series, exposures, covariance, price history | Stale/missing price history. |
| Concentration | Issuer/counterparty/group hierarchy | Structured note issuer not aggregated. |
| Drawdown | Daily market values | Missing valuation day or late NAV. |
| Collateral availability | Market value, haircut, pledge, exposure | Wrong pledged portfolio mapping. |

## 5. Reporting impact

Client reports need data-quality awareness.

| Report section | Data-quality issue | Reporting behaviour |
|---|---|---|
| Holdings | Missing/stale price | Flag holding and exclude from certain totals only if policy allows. |
| Allocation | Missing classification | Show "Unclassified" rather than force wrong bucket. |
| Performance | Missing cashflow classification | Block or show performance unavailable. |
| Income | Missing dividend/coupon | Show pending/unconfirmed income status. |
| Risk | Insufficient history | Show metric unavailable with reason. |
| Structured products | Missing barrier status | Show limited payoff status and source note. |
| Private markets | NAV lag | Show NAV date and valuation lag disclosure. |
| Tax | Missing withholding classification | Avoid tax-specific statement or flag provisional. |

## 6. Client experience principles

A good client experience is transparent without being alarming.

| Principle | Example |
|---|---|
| Explain limitation clearly | "Price unavailable as of report date; last available price used." |
| Avoid technical blame | Do not say "batch failed." Say "valuation pending from source." |
| Show date and basis | NAV as of 31 May, report as of 30 Jun. |
| Separate estimate from final | Preliminary valuation vs certified valuation. |
| Use consistent labels | Same product category across advisor UI and client report. |
| Avoid false precision | Do not show 6 decimal places if source is indicative. |
| Show material impact | If missing price affects 0.2% of portfolio, say so internally; externally follow policy. |

## 7. Degraded-state design for front office

| State | Advisor UI | Client report |
|---|---|---|
| Complete | Normal. | Normal. |
| Warning | Badge and details. | Usually no external disclosure unless material. |
| Degraded | Show affected metrics and reason. | Exception note if externally released. |
| Blocked | Prevent proposal/report approval. | Report not generated/published. |
| Preliminary | Internal use only or watermark. | Not final unless policy allows. |
| Restated | Show change explanation and lineage. | Reissue/restatement protocol. |

## 8. Advisory workflow controls

Before recommendation:

- client profile current;
- product approved for booking centre;
- product target market matches client segment;
- risk rating not above client tolerance unless exception workflow;
- liquidity need checked;
- concentration impact calculated;
- mandate restrictions evaluated;
- price and FX not stale beyond threshold;
- report/proposal includes product-specific disclosure;
- client consent and rationale captured.

## 9. DPM workflow controls

For discretionary portfolio management:

| Stage | Data quality control |
|---|---|
| Model design | Target asset classes and benchmarks mapped to product taxonomy. |
| Portfolio monitoring | Current positions, prices and classifications certified. |
| Drift detection | Market values and FX current. |
| Rebalance proposal | Restrictions, liquidity, costs, tax lots and trading constraints available. |
| Trade generation | Instrument eligibility, lot size, minimum subscription and liquidity checked. |
| Post-trade review | Actual positions and cash reconciled. |
| Client reporting | Mandate performance, breaches and exceptions documented. |

## 10. Reporting governance decisions

Common decisions to document:

| Decision | Options |
|---|---|
| Missing price | Block report, use last price with stale flag, use model estimate, show unavailable. |
| Late NAV | Use last official NAV, use estimated NAV, exclude from performance, disclose lag. |
| Unclassified product | Show "Other/Unclassified," block allocation, map manually with approval. |
| Performance restatement | Reissue report, show revised analytics next cycle, internal adjustment only. |
| Source conflict | Use source hierarchy, escalate, manual override with expiry. |
| Corporate action pending | Show entitlement pending, withhold affected calculation, or publish with note. |

## 11. Stakeholder explanations

### For business users

"Data quality controls are not technical checks only. They protect client recommendations, mandate compliance, portfolio reviews and client statements. The same missing field can be harmless in one report and critical in a suitability decision."

### For architects

"Data quality state must travel with the data through APIs, events, snapshots and reports. Downstream systems should not have to guess whether data is certified, preliminary, stale, estimated or overridden."

### For senior stakeholders

"High-quality data governance reduces client-impacting errors, improves speed of issue resolution and provides audit evidence for migration, reporting, suitability and operational controls."

## 12. Practitioner takeaway

Data quality becomes valuable when it changes behaviour. If a price is stale, a report should know. If a mandate cannot be evaluated, the proposal should know. If a performance result excludes a holding, the advisor should know. Governance succeeds when front-office decisions become safer, clearer and more explainable.
