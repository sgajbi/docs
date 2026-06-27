# 10 - Advisory, Mandate, Reporting and Client Experience Integration

## 1. Why architecture must support advisory workflows

Product data and analytics are only valuable when they support decisions and explanations. In wealth management, architecture must help advisors and portfolio managers answer:

- Is this product suitable for this client?
- Is this product approved in this booking centre?
- What is the portfolio impact?
- Does the trade breach the mandate?
- How does this affect risk, liquidity and concentration?
- What will the client see in the statement or portfolio review?
- What explanation and disclosure must be provided?

## 2. Advisory workflow architecture

```text
Client context
  -> product universe / idea
  -> portfolio diagnostics
  -> proposal construction
  -> suitability and target-market checks
  -> mandate and risk impact checks
  -> cost/tax/liquidity review
  -> client explanation and disclosure
  -> approval / consent
  -> order generation
  -> execution and settlement
  -> post-trade monitoring
  -> reporting and review
```

## 3. Data required at point of advice

| Data | Why required |
|---|---|
| Client risk profile | Suitability and risk alignment. |
| Investment objectives | Product and strategy fit. |
| Time horizon | Liquidity/tenor/product appropriateness. |
| Knowledge and experience | Complex product eligibility. |
| Product risk rating | Match product risk to client profile. |
| Target market | Product governance distribution control. |
| Portfolio holdings | Concentration and diversification impact. |
| Cash/buying power | Funding feasibility. |
| Mandate rules | DPM/advisory restrictions. |
| Product costs | Fee and cost disclosure. |
| Scenario/downside | Client explanation and suitability evidence. |
| Reporting treatment | Advisor can explain how it will appear to client. |

## 4. Mandate integration

Mandate architecture should support:

- target allocation
- asset-class bands
- single-security concentration limits
- issuer/counterparty limits
- currency limits
- liquidity limits
- product restrictions
- country/booking-centre restrictions
- ESG/restricted-list constraints
- leverage and borrowing limits
- derivative exposure limits
- minimum cash levels
- client-specific exclusions

## 5. Pre-trade and post-trade checks

| Stage | Purpose |
|---|---|
| Pre-proposal | Filter obviously unsuitable or unavailable products. |
| Proposal | Evaluate suitability, risk and portfolio impact. |
| Pre-trade | Validate cash, buying power, product eligibility, mandate rules. |
| Post-trade | Confirm actual outcome after execution/settlement. |
| EOD | Full portfolio monitoring using official data. |
| Reporting | Display mandate status and exceptions where required. |

## 6. Portfolio impact simulation

Advisory architecture should support what-if simulation:

| Simulation | Output |
|---|---|
| Buy product | New allocation, risk, concentration, liquidity. |
| Sell product | Realized P&L, tax/cost estimate, allocation impact. |
| Rebalance | Drift reduction, trades, cash usage, costs. |
| Add leverage | LTV, collateral usage, margin risk. |
| Structured product scenario | Payoff, downside, issuer exposure. |
| FX conversion | Currency allocation and settlement impact. |

Simulation should be clearly marked as estimated and not confused with official book-of-record results.

## 7. Reporting integration

Every advisory decision should consider reporting impact:

| Product/event | Reporting consideration |
|---|---|
| Structured note | Show issuer, underlying, maturity, coupon, barrier status. |
| Bond | Show yield, maturity, accrued interest, credit quality. |
| Fund | Show NAV, units, distribution, share class, fees. |
| Derivative | Show notional, market value, margin/collateral, risk exposure. |
| Private market | Show commitment, funded, unfunded, NAV lag, IRR/multiples. |
| Lending | Show outstanding loan, collateral, LTV, available credit. |
| Mandate breach | Show or suppress based on report type and policy with explanation. |

## 8. Advisor dashboard architecture

Advisor dashboards should combine:

- client profile
- portfolio allocation
- top holdings
- performance summary
- risk summary
- concentration alerts
- income and cashflow view
- cash/buying power
- mandate status
- recommendations/proposals
- open tasks and approvals
- upcoming events: maturities, coupon dates, capital calls, reviews
- report readiness/exceptions

## 9. Client experience principles

Client-facing outputs should be:

- understandable
- consistent with advisor screens
- transparent about data limitations
- timely
- traceable to official snapshots
- compliant with disclosure rules
- safe for cross-border and entitlement constraints

## 10. Exception explanation design

When data is degraded, explain in business terms.

| Technical issue | Client/advisor explanation |
|---|---|
| No price for instrument | Market value is unavailable because no approved price was received for valuation date. |
| NAV is provisional | Fund value is based on provisional NAV and may change. |
| Performance unavailable | Return cannot be calculated because required historical valuation/cashflow data is incomplete. |
| Mandate check pending | Mandate status is pending because official EOD holdings are not yet available. |
| Structured note value estimated | Value is based on issuer/evaluated quote and may not equal executable sale price. |

## 11. Workflow integration

Architecture should support workflows for:

- product approval
- proposal approval
- client consent
- order approval
- exception resolution
- mandate breach review
- report correction/restatement
- data override approval
- migration sign-off

Workflow should capture who did what, when, why and with what evidence.

## 12. Client interaction history

Important interactions:

- meeting notes
- proposal discussion
- suitability rationale
- client consent
- risk disclosure acknowledgement
- order instruction
- complaint or query
- report delivery
- document viewing
- mandate breach communication

This history supports client servicing, audit, disputes and continuity across relationship managers.

## 13. Architecture anti-patterns

| Anti-pattern | Impact |
|---|---|
| Advisory tool uses different product taxonomy than reporting | Client/advisor inconsistency. |
| Mandate checks only after trade | Preventable breaches. |
| Proposal simulation not linked to actual order | Weak audit lineage. |
| Report values differ from dashboard without explanation | Trust issue. |
| Product restrictions hardcoded in UI | Controls bypassed through other channels. |
| Consent captured outside workflow | Weak evidence. |

## 14. Integration checklist

- Advisory uses governed product universe.
- Suitability uses current client profile and product attributes.
- Mandate checks are available pre-trade and post-trade.
- Proposal simulation is separated from official accounting results.
- Client consent and disclosures are captured with audit trail.
- Orders link back to proposal and suitability evidence.
- Reports can explain product and mandate outcomes.
- Dashboard and report values use consistent snapshots or explain differences.
- Exceptions have workflow and owner.
