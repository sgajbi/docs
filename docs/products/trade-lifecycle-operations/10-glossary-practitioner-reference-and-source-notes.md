# 10. Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| Allocation | Assignment of executed quantity/amount to one or more accounts |
| Affirmation | Agreement by relevant party that confirmation details are correct |
| Available cash | Cash usable after blocks, pending trades and policy adjustments |
| Available position | Quantity that can be sold/transferred after blocks and pending movements |
| Booking date | Date the platform books the event |
| Cash-in-lieu | Cash paid instead of fractional security entitlement |
| CCP | Central counterparty for cleared trades |
| CSD | Central securities depository |
| Corporate action | Issuer or security event affecting holder entitlement |
| Custodian | Institution safekeeping assets and processing settlement/asset servicing |
| DVP | Delivery versus payment |
| Execution | Actual fill in market or with counterparty |
| Fail | Trade not settled on intended settlement date |
| FOP | Free of payment transfer |
| Matched | Settlement instruction/confirmation details agree |
| OMS | Order management system |
| PVP | Payment versus payment, commonly used in FX settlement risk mitigation |
| Record date | Date holders are determined for corporate action entitlement |
| Settlement date | Date cash/securities are due to exchange |
| SSI | Standing settlement instruction |
| Trade date | Date trade is executed/agreed |
| Unmatched | Counterparty/custodian details do not match |
| Value date | Date cash/security value is effective |

## 2. Practitioner Operating Checklist

### Order, trade and transaction are not the same

| Object | Business question |
|---|---|
| Order | What did client/PM ask to do? |
| Execution | What was filled, when and at what price? |
| Trade | What is the bookable economic deal? |
| Settlement | Did cash and securities actually move? |
| Transaction | What accounting postings changed positions/cash? |
| Position | What does the client currently hold under a defined basis? |

### Dates matter

| Date | Do not confuse with |
|---|---|
| Trade date | Settlement date |
| Settlement date | Booking date |
| Value date | Report generation date |
| Ex-date | Record date |
| Payable date | Announcement date |

### Basis matters

Always ask:

- Is this trade-date or settlement-date position?
- Is this ledger cash or available cash?
- Is this actual or projected cash?
- Is this settled or pending quantity?
- Is the valuation market, issuer, model or NAV based?

## 3. Practitioner Explanation

A strong answer:

> The trade lifecycle is the end-to-end journey from investment decision to order, execution, trade capture, confirmation, settlement, custody, asset servicing, reconciliation and reporting. In a wealth platform, this lifecycle also includes suitability, mandate checks, buying power, product eligibility, client consent and advisor reporting. The key design principle is to separate order intent, execution fills, bookable trade, settlement instruction, accounting transaction and derived position. This separation gives auditability, reconciliation capability and explainability. It is also important to expose different views such as trade-date position, settlement-date position and available position because advisory, custody, buying power and performance may require different bases.

## 4. Senior Platform Review Lens

A senior architect should emphasize:

- lifecycle lineage from proposal to report
- explicit state machines
- separation of trade-date, settlement-date and available views
- product-specific extensions
- idempotent event processing
- corporate action event modelling
- reconciliation and exception workflow
- recalculation and restatement controls
- advisory/DPM/reporting integration
- audit-ready design

## 5. Common Review Questions

### Q1. Why should order, trade and transaction be separate?

Because they represent different business facts. The order is intent, the execution is market fill, the trade is the agreed bookable deal, and the transaction is the accounting posting. Merging them makes partial fills, cancellations, corrections, allocations, settlement fails and audit lineage difficult.

### Q2. What is the difference between trade-date and settlement-date positions?

Trade-date position includes executed trades as economic exposure from trade date. Settlement-date position includes only assets that have actually settled at the custodian. Performance and risk often prefer trade-date view, while custody and some availability/collateral views require settlement-date view.

### Q3. How should corporate actions be modelled?

Use an event model: corporate action event, entitlement/election, then resulting transactions. Do not model only the final transaction, because you need announcement details, options, deadlines, election decisions, entitlement calculation, event revisions and audit trail.

### Q4. What causes settlement fails?

Common causes include insufficient securities, insufficient cash, incorrect SSI, unmatched trade details, missed cut-offs, market restrictions, corporate action complications, FX funding issues and system outages.

### Q5. How does lifecycle affect performance?

Performance depends on correct event classification and timing. A buy funded from existing cash is internal, while a client cash deposit is external. Dividends are investment income. Splits should not create artificial return. Late/corrected transactions require recalculation from the affected date.

### Q6. How does lifecycle affect buying power?

Buying power must consider settled cash, pending buys/sells, open-order blocks, fees, taxes, FX conversions, credit lines, collateral haircuts, pledged assets and product settlement cycles. Ledger cash alone is not enough.

### Q7. What is the most important control in trade lifecycle platforms?

Lineage plus reconciliation. Every position, cash balance, performance number and report figure should be traceable back to source transactions, prices, FX rates, corporate actions and settlement/custody records.

## 6. Product-specific pitfalls

| Product | Pitfall |
|---|---|
| Equity | Corporate actions not adjusting cost/quantity correctly |
| Bond | Accrued interest and clean/dirty price mishandled |
| Fund | Order date treated as NAV date |
| Note | Observation trigger booked as cash transaction too early |
| Option | Exercise/assignment not creating underlying transaction |
| Future | Variation margin not treated as daily cashflow/P&L |
| FX | Value-date and currency holiday mismatch |
| Private market | Commitment confused with funded NAV |
| Loan | Collateral availability not adjusted for pending orders |
| Insurance | Surrender value confused with death benefit/face amount |

## 7. Source notes and further reading

This pack uses general industry operating knowledge and is aligned with publicly available regulatory and market-infrastructure references.

Important references to keep in mind:

1. **SEC and U.S. T+1 settlement** - The U.S. moved to a T+1 standard settlement cycle for many securities transactions on 28 May 2024.
2. **DTCC T+1 materials** - DTCC states that U.S. T+1 implementation activities were completed and that the market operates on a T+1 settlement cycle.
3. **SGX Rulebook settlement chapter** - SGX rulebook indicates T+2 intended settlement for ready market securities other than wholesale corporate bonds and for wholesale corporate bonds, with details by market category.
4. **CPMI-IOSCO Principles for Financial Market Infrastructures** - PFMI provides core global principles for payment, clearing, settlement and central securities depository infrastructures.
5. **DTCC Corporate Actions Processing** - DTCC describes full lifecycle processing for distributions, redemptions and reorganizations, including announcements, entitlements, instructions, allocations and payments.
6. **ISO 20022 / SWIFT corporate action messaging** - ISO 20022 and related industry messaging standards support structured securities and corporate-action event communication.

## 8. Suggested next study areas

After this pack, the knowledge base can be strengthened with:

- investment accounting and general ledger for wealth platforms
- custody and securities services deep dive
- product master and reference data governance
- market data, pricing and valuation governance
- client reporting document generation and explainability
- operational risk and control framework
- data lineage, audit and regulatory evidence architecture

## 9. Final mental model

```text
A product explains what the client owns.
A lifecycle explains how it was bought, settled, serviced and reported.
A transaction explains what changed economically.
A position explains what is held under a defined basis.
A report explains the result to the client.
A strong platform links all of them.
```
