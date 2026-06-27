# 10. Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| AUM | Assets under management; usually discretionary/advised assets managed by the firm. |
| AUA | Assets under administration; assets administered/custodied but not necessarily managed. |
| Base currency | Currency in which the portfolio report is presented. |
| Book of record | Authoritative book for positions, cash, accounting or performance. |
| Client statement | Periodic official report of holdings, cash and activity. |
| Data cut | Controlled set of input data used for a report. |
| DPM | Discretionary portfolio management. |
| Exception | Data/process issue affecting report quality or interpretation. |
| Final value | Approved value not expected to change under normal process. |
| Gross performance | Performance before certain fees/taxes, depending methodology. |
| Net performance | Performance after specified fees/taxes. |
| NAV lag | Delay between valuation date and receipt of latest fund/private asset NAV. |
| Portfolio review | Advisory or PM review document explaining portfolio status and actions. |
| Provisional value | Value expected to be finalized later. |
| Report basis | Trade-date, settlement-date, accounting, performance or tax basis. |
| Report run | A single production instance of a report. |
| Reporting dataset | Structured data used to generate report outputs. |
| Restatement | Replacement or correction of a previously published report. |
| Stale price | Price older than accepted policy threshold. |
| TWR | Time-weighted return. |
| MWR | Money-weighted return/internal rate of return. |

## 2. Practitioner Explanations

### What is portfolio reporting?

Portfolio reporting is the controlled presentation of holdings, cash, activity, income, fees, taxes, performance, risk and mandate status for a client, account or reporting group over a defined period. In a wealth platform, reporting is not just PDF generation. It requires curated positions, transactions, pricing, FX, product master, performance, risk, disclosures, data-quality flags and lineage so that the report is client-friendly, reproducible and auditable.

### What makes wealth reporting difficult?

Wealth reporting is difficult because portfolios contain many product types with different lifecycle behavior: bonds have accrued interest, funds have NAVs and share classes, structured notes have payoff conditions, derivatives have notional and margin, private markets have commitments and NAV lag, loans have collateral and LTV, and insurance has policy values. The report must consolidate these into a consistent client view without losing product-specific meaning.

### How would you design a reporting platform?

I would separate source data, calculation, reporting dataset, rendering and archive. Reports should be generated from a controlled data cut, with section-level data contracts, quality flags, source lineage, template versioning, disclosure mapping and immutable archive. The same reporting dataset should support PDF, portal and API outputs. Report cells should be traceable to source records and calculation versions.

### How do you handle missing or stale data?

Missing or stale data should be handled explicitly through an exception model. Depending on severity and materiality, the report can use a fallback source, show the latest available value with a valuation-date warning, suppress the affected metric, or block publication. The client and advisor should receive clear explanations where interpretation is affected.

### What is the difference between statement and portfolio review?

A statement is usually an official periodic record of holdings, cash and activity. A portfolio review is more advisory and analytical: it explains performance, allocation, risk, mandate alignment, product events and recommended actions. A good platform should support both from consistent underlying data but with different templates and commentary workflows.

## 3. Practitioner Operating Checklist

| Design decision | Recommended approach |
|---|---|
| Report output | Generate structured dataset first; render PDF/UI from it. |
| Data cut | Use explicit EOD/month-end/intraday cut IDs. |
| Basis | Record trade-date, settlement-date, accounting, performance or tax basis. |
| Valuation | Show source, date and quality flag for material values. |
| Performance | Show methodology, period, currency and benchmark. |
| Exceptions | Store structured exceptions with severity and client/advisor visibility. |
| Disclosures | Resolve by product, jurisdiction, client segment and report type. |
| Corrections | Supersede, do not silently overwrite published reports. |
| Archive | Store rendered output, dataset, run metadata and approval evidence. |
| QA | Test product-specific lifecycle scenarios and report rendering edge cases. |

## 4. Common mistakes

| Mistake | Impact |
|---|---|
| Treating PDF as the system of record. | Hard to test, audit and replay. |
| Not storing report run metadata. | Cannot explain historical values. |
| Silent stale-price fallback. | Misleading client valuation. |
| Showing P&L when cost basis is missing. | Incorrect gain/loss reporting. |
| Mixing trade-date and settlement-date basis. | Inconsistent holdings and cash. |
| Showing private-market NAV without NAV date. | False precision. |
| Showing structured-note coupon as bond-like yield. | Misleading risk explanation. |
| No product-specific sections. | Complex assets are misunderstood. |
| No disclosure mapping. | Regulatory and conduct risk. |
| No entitlement model. | Privacy and confidentiality risk. |

## 5. Source notes and further reading

These sources are useful for grounding reporting, performance, benchmark and investor-disclosure design. They should be treated as reference material, not as a substitute for local legal, tax, regulatory or compliance review.

| Topic | Source | Notes |
|---|---|---|
| Performance presentation | CFA Institute GIPS Standards | GIPS are voluntary ethical standards for calculating and presenting investment performance based on fair representation and full disclosure. |
| Benchmark governance | IOSCO Principles for Financial Benchmarks | Provides benchmark governance, methodology, transparency and conflict-control principles. |
| Investor education | Investor.gov / SEC investor education | Useful for client-friendly disclosure concepts, fees, diversification and risk explanations. |
| Fair dealing / conduct | MAS Fair Dealing Guidelines and FAA notices | Useful for Singapore wealth-advisory conduct and disclosure context; check latest official MAS material before implementation. |
| Settlement and operations | SEC / DTCC / SGX / CPMI-IOSCO | Useful for settlement cycle, custody, corporate actions and financial-market-infrastructure controls. |
| Accounting / valuation | IFRS 9 and IFRS 13 | Useful for financial instrument classification, fair-value measurement and valuation hierarchy concepts. |
| Tax reporting | IRAS / OECD CRS / IRS FATCA resources | Useful for cross-border reporting and tax-documentation concepts. |

## 6. How this pack fits the knowledge base

This pack should sit after:

- product-family packs;
- market data and instrument master;
- investment accounting and ledger;
- trade lifecycle and operations;
- performance, attribution, risk and benchmarks;
- tax/regulatory reporting;
- advisory and mandate workflows.

It is the client-facing integration layer that makes all upstream platform design visible and accountable.
