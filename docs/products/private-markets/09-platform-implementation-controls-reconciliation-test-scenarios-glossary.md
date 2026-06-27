# 09 — Platform Implementation, Controls, Reconciliation, Test Scenarios, Glossary and Practitioner Reference

## 1. Implementation architecture

Private markets should be implemented with clear domain separation:

```text
Product / Instrument Master
  -> stores fund/security terms

Commitment Service
  -> manages commitment, unfunded, recallable balances

Notice Processing Service
  -> capital calls, distributions, NAV notices

Transaction Service
  -> posts accounting-grade cash/security transactions

Valuation Service
  -> NAV, adjusted NAV, stale valuation flags

Performance Service
  -> IRR, TVPI, DPI, RVPI, TWR integration

Risk / Mandate Service
  -> concentration, liquidity, unfunded stress, eligibility

Reporting Service
  -> client statements, portfolio review, advisory dashboards
```

## 2. Core implementation rules

| Rule | Reason |
|---|---|
| Separate lifecycle events from transactions | Notices are not always settled transactions |
| Separate commitment from NAV | Commitment is obligation; NAV is value |
| Preserve notice-level classification | Income/ROC/gain/recallable classification matters |
| Store valuation date and received date | NAV lag is material |
| Support restatements | NAVs and notices can be revised |
| Support multiple currencies | Commitment, NAV, calls and reporting may differ |
| Do not force daily pricing | Many private assets are periodic-valued |
| Keep audit trail | Private assets are document-driven |
| Treat estimates as estimates | Avoid false precision |
| Support manual enrichment | Source data often arrives from PDFs/portals |

## 3. Recommended tables/domains

| Domain | Tables/entities |
|---|---|
| Instrument | fund_master, share_class, manager, administrator, domicile |
| Terms | commitment_terms, liquidity_terms, fee_terms, waterfall_terms |
| Position | alt_position, capital_account, commitment_balance |
| Notices | capital_call_notice, distribution_notice, nav_notice |
| Transactions | transaction, transaction_leg, cash_ledger, security_ledger |
| Valuation | valuation, valuation_component, nav_history |
| Performance | private_market_cashflow, private_market_metric_snapshot |
| Risk | exposure, liquidity_bucket, concentration_result, stress_result |
| Documents | document_store, document_version, disclosure_acknowledgement |
| Controls | reconciliation_result, data_quality_issue, exception_queue |

## 4. Reconciliation controls

### 4.1 Commitment reconciliation

| Check | Formula/logic |
|---|---|
| Paid-in reconciliation | Sum contributions = paid-in capital |
| Unfunded reconciliation | Commitment - called + recallable +/- adjustments |
| Call settlement | Capital call notice amount = contribution payment |
| Overdue calls | Due date passed and unpaid amount > 0 |
| Transfer reconciliation | Transferred commitment/unfunded matches transfer docs |

### 4.2 Distribution reconciliation

| Check | Formula/logic |
|---|---|
| Gross distribution split | Income + ROC + gain + recallable + other = gross |
| Net cash | Gross - tax/withholding/fees = net cash received |
| Recallable balance | Recallable distributions increase recallable amount |
| In-kind distribution | Securities delivered reconcile to distribution notice |
| Unclassified queue | Distributions without classification require follow-up |

### 4.3 NAV reconciliation

| Check | Formula/logic |
|---|---|
| NAV freshness | Report date - NAV date <= allowed stale threshold |
| NAV roll-forward | Prior NAV + calls - distributions +/- change = current NAV |
| Restatement audit | Restated NAV links to prior valuation |
| Unitised NAV | Units × NAV per unit = reported market value |
| Capital account | Capital account balance reconciles to admin statement |

### 4.4 Performance reconciliation

| Check | Logic |
|---|---|
| IRR cashflow set | Contributions/distributions/NAV match ledger |
| TVPI components | TVPI = DPI + RVPI |
| DPI/RVPI denominator | Paid-in capital denominator consistent |
| FX basis | Metrics in local and reporting currency use correct FX |
| Realised/unrealised split | Distributions and NAV correctly classified |

## 5. Data-quality issue taxonomy

| Issue | Severity |
|---|---|
| Missing NAV date | High |
| NAV older than threshold | Medium/high depending client report |
| Missing unfunded commitment | High |
| Distribution not classified | Medium |
| Capital call notice loaded but no payment | High if due date near/passed |
| Currency mismatch | High |
| Negative unfunded commitment without explanation | High |
| Duplicate capital call | High |
| Duplicate NAV update | Medium/high |
| Missing manager/strategy/vintage | Medium |
| Unknown liquidity terms | High for advisory |
| Missing eligibility classification | High for suitability |

## 6. Test scenarios

### 6.1 Basic PE fund commitment and call

| Step | Expected result |
|---|---|
| Commit USD 1,000,000 | Commitment = 1,000,000; unfunded = 1,000,000 |
| Capital call USD 200,000 | Pending payable = 200,000 |
| Pay call | Paid-in = 200,000; unfunded = 800,000; cash down |
| NAV reported USD 190,000 | Market value = 190,000; unrealised loss/fees reflected |

### 6.2 Distribution split

| Step | Expected result |
|---|---|
| Distribution gross 100,000 | Cash increases |
| Classified: 20,000 income, 50,000 ROC, 30,000 gain | Income/gain/ROC reporting correct |
| 40,000 recallable | Recallable amount increases if terms allow |

### 6.3 NAV restatement

| Step | Expected result |
|---|---|
| Load Q1 NAV 500,000 final | Valuation stored |
| Receive restated Q1 NAV 520,000 | Prior valuation not overwritten silently; restatement link exists |
| Performance recalculated | Audit trail preserved |

### 6.4 Secondary purchase

| Step | Expected result |
|---|---|
| Buy interest at 90% of NAV | Cost = purchase price; NAV = reported value |
| Assume unfunded commitment | Unfunded obligation created |
| Interim cashflows between effective and close | Allocated according to agreement |

### 6.5 In-kind distribution

| Step | Expected result |
|---|---|
| PE fund distributes listed shares | Private fund distribution and equity delivery created |
| Shares under lock-up | Equity position restricted from liquidity |
| Cost basis allocated | Policy-applied and auditable |

### 6.6 Hedge fund gate

| Step | Expected result |
|---|---|
| Client requests 100% redemption | Redemption request pending |
| Fund applies 25% gate | 25% redeemed; 75% remains pending/held |
| NAV changes before final redemption | Remaining position revalued |

### 6.7 Stale NAV report

| Step | Expected result |
|---|---|
| Report generated in June with December NAV | Stale NAV flag shown |
| Advisory dashboard | Warning raised for valuation uncertainty |

### 6.8 Capital call stress

| Step | Expected result |
|---|---|
| Unfunded commitment = 2,000,000 | Obligation recorded |
| Stress assumes 40% called in 12 months | Required liquidity = 800,000 |
| Liquid assets only 500,000 | Liquidity risk breach/alert |

## 7. Migration considerations

Private-market migration needs special care.

| Area | Migration question |
|---|---|
| Commitments | Were original commitments migrated or only current NAV? |
| Paid-in | Is historical paid-in available? |
| Distributions | Are historical distributions split by type? |
| NAV history | Are NAV dates and restatements preserved? |
| Unfunded | Is unfunded calculated or source-provided? |
| Recallable | Are recallable amounts available? |
| Performance | Can IRR be calculated from full history? |
| Documents | Are call/distribution/NAV notices available? |
| FX | Are historical FX rates available? |
| Closed positions | Are fully liquidated funds included for since-inception performance? |

If historical cashflows are missing, show metrics as partial or unavailable rather than fabricating IRR/TVPI.

## 8. Production triage guide

| Symptom | Likely cause | Investigation |
|---|---|---|
| Negative unfunded | Duplicate calls, missing commitment, recallable logic error | Reconcile commitment ledger |
| IRR impossible/extreme | Missing/duplicated cashflow, wrong sign, wrong NAV date | Inspect cashflow set |
| Market value stale | NAV not loaded or delayed | Check NAV feed/document queue |
| Income overstated | ROC distribution booked as income | Check classification notice |
| Liquidity report wrong | Lock-up/gate terms missing | Check liquidity terms master |
| Mandate breach false positive | Commitment vs NAV basis mismatch | Check rule basis |
| Performance jump | NAV restatement or FX issue | Compare valuation history |
| Cash mismatch | Capital call/distribution settlement date error | Reconcile cash ledger |

## 9. Glossary

| Term | Definition |
|---|---|
| Alternative investments | Investments outside traditional public stocks/bonds/cash |
| Private market | Non-publicly traded investment market |
| GP | General Partner / manager |
| LP | Limited Partner / investor |
| Commitment | Legal amount committed |
| Capital call | Request for capital contribution |
| Paid-in capital | Amount contributed |
| Unfunded commitment | Commitment not yet called |
| NAV | Net asset value / residual value |
| Distribution | Cash/securities returned |
| DPI | Distributions divided by paid-in capital |
| RVPI | Residual value divided by paid-in capital |
| TVPI | Total value divided by paid-in capital |
| IRR | Money-weighted return |
| PME | Public Market Equivalent |
| Vintage year | Year used to group/compare funds |
| J-curve | Early negative performance followed by later value creation pattern |
| Carry | Manager performance incentive |
| Hurdle | Minimum return before carry |
| Catch-up | Waterfall phase favouring GP after hurdle |
| Clawback | Mechanism to reverse overpaid carry |
| Side letter | Investor-specific agreement |
| Subscription line | Fund-level borrowing facility bridging capital calls |
| Recallable capital | Distribution that may be called again |
| Recycling | Reuse of returned capital |
| Gate | Redemption limit |
| Lock-up | Period when investor cannot redeem |
| Side pocket | Segregated illiquid asset pool |
| Secondaries | Purchase/sale of existing private fund interests |
| Co-investment | Direct deal investment alongside sponsor |

## 10. Practitioner-ready explanations

### What are private markets?

> Private markets are investments that are not primarily traded on public exchanges, such as private equity, private credit, private real estate, infrastructure and hedge funds. They typically involve commitments, capital calls, distributions, manager-reported NAVs, long lock-ups and limited liquidity. In wealth management, they can provide diversification and return potential, but they require strong suitability, liquidity planning, valuation controls and performance metrics such as IRR, TVPI, DPI and RVPI.

### How would you model private markets on a wealth platform?

> I would not model them as simple daily-priced securities only. I would model a commitment layer, a cashflow layer and a valuation/position layer. The commitment layer tracks committed, paid-in, unfunded and recallable amounts. The cashflow layer tracks capital calls, contributions, distributions, taxes, fees and secondary transfers. The valuation layer tracks NAV, valuation date, NAV status and market value. Performance uses money-weighted metrics and multiples, while total portfolio reporting also needs TWR integration, liquidity buckets, concentration and cash-call stress.

### Why is unfunded commitment important?

> Unfunded commitment is a future funding obligation. A client may show a small current NAV but still owe large future capital calls. This affects suitability, buying power, liquidity planning, mandate limits and stress testing. Ignoring unfunded commitment materially understates portfolio risk.

### Why is NAV date important?

> Private-market NAV is often quarterly and lagged. A report may use NAV from several months ago. Without the valuation date, the client may think the value is current. NAV date and stale flags are essential for transparent reporting.

### Why use IRR and TVPI/DPI/RVPI?

> Private-market cashflows occur over many years and are controlled by the manager. IRR captures timing, TVPI captures total value multiple, DPI captures realised cash returned and RVPI captures remaining unrealised value. No single metric is enough.

## 11. Practitioner reference table

| Topic | Remember |
|---|---|
| Position | NAV is not commitment |
| Exposure | Include unfunded commitments |
| Cashflow | Calls negative, distributions positive for IRR |
| Valuation | Always show NAV date |
| Reporting | Use IRR + TVPI/DPI/RVPI |
| Advisory | Check liquidity and call capacity |
| Mandates | Monitor illiquid allocation and manager/vintage concentration |
| Performance | Do not classify all distributions as income |
| Risk | Valuation uncertainty and illiquidity are core risks |
| Controls | Notices, cash and NAV must reconcile |
