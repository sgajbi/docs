# 03 - Cross-Product Dependency Map and Learning Sequence

## 1. Dependency logic

Some topics depend on others.

For example:

- structured notes require bonds + derivatives
- ETFs require funds + equities + exchange trading
- private markets require funds + cashflow modelling + IRR
- DPM requires products + performance + mandates + rebalancing
- AI advisor copilot requires product knowledge + data lineage + suitability + governance
- reporting requires accounting + pricing + analytics + product treatment

## 2. Product dependency map

```text
Cash / FX
  |-- settlement
  |-- buying power
  |-- performance cashflows
  `-- multi-currency reporting

Bonds
  |-- notes
  |-- structured products
  |-- fixed-income attribution
  `-- collateral / Lombard lending

Equities
  |-- equity-linked notes
  |-- ETFs
  |-- options
  |-- corporate actions
  `-- concentration risk

Funds
  |-- ETFs
  |-- private markets
  |-- hedge funds
  |-- fund reporting
  `-- NAV-based valuation

Derivatives
  |-- structured products
  |-- hedging overlays
  |-- DCI
  |-- CLN
  `-- risk and Greeks

Private markets
  |-- commitments
  |-- capital calls
  |-- IRR / TVPI / DPI
  `-- liquidity planning
```

## 3. Platform dependency map

```text
Client/account master
  -> entitlements
  -> advisory
  -> reporting
  -> suitability
  -> mandate ownership

Instrument/product master
  -> holdings
  -> valuation
  -> suitability
  -> reporting labels
  -> product governance

Market data
  -> valuation
  -> P&L
  -> performance
  -> risk
  -> reporting

Transactions/ledger
  -> positions
  -> cash
  -> income
  -> fees
  -> P&L
  -> tax lots

Performance/risk
  -> portfolio review
  -> DPM monitoring
  -> client reporting
  -> advisor explanation

Data governance
  -> trust
  -> reconciliation
  -> audit
  -> AI grounding
```

## 4. Advisory dependency map

```text
Client profile
  -> suitability
  -> target market
  -> proposal
  -> client consent
  -> order workflow
  -> review and monitoring

Product governance
  -> approved product universe
  -> product risk rating
  -> disclosure
  -> product eligibility
  -> recommendation checks

Portfolio analytics
  -> gaps
  -> risks
  -> performance explanation
  -> rebalance suggestion
  -> client conversation
```

## 5. AI dependency map

```text
Approved knowledge base
  -> RAG retrieval
  -> grounded answer
  -> citation
  -> advisor review
  -> audit trail

Portfolio data
  -> analytics calculation
  -> AI narrative
  -> exception explanation
  -> client-ready draft

Workflow tools
  -> action proposal
  -> human approval
  -> controlled execution
  -> evidence record
```

## 6. Suggested full learning sequence

### Foundation

1. Cash / FX
2. Bonds
3. Equities
4. Funds

### Product complexity

5. Derivatives
6. Structured notes
7. Structured products
8. Private markets
9. Loans / collateral
10. Insurance / annuities

### Platform correctness

11. Market data
12. Investment accounting
13. Trade lifecycle
14. Data governance

### Wealth outcomes

15. Performance / risk / benchmarks
16. Reporting / client experience
17. Product governance
18. Model portfolios / suitability
19. Client/account master
20. Entitlements / workbench

### Enterprise architecture

21. Wealth platform architecture
22. Cloud / Kubernetes
23. Security
24. Delivery / SDLC
25. Vendor management

### Future growth

26. AI decision intelligence
27. wealth-platform suite AI strategy
28. Commercial SaaS
29. Wealth business strategy / TOM

## 7. Key takeaway

The best learning path moves from product mechanics to platform records to client/advisor outcomes to enterprise strategy.
