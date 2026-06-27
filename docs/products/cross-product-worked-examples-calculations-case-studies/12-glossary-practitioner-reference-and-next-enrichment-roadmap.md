# 12 - Glossary, Practitioner practitioner reference and Next Enrichment Roadmap

## 1. Glossary

| Term | Meaning |
|---|---|
| Worked example | Numerical or workflow scenario with expected results |
| Golden scenario | Approved regression case with known expected output |
| Lifecycle event | Business/product event, not always a transaction |
| Transaction | Economic posting affecting cash, position, income, fee or P&L |
| Position impact | Change to quantity, nominal, cost, MV or status |
| Performance classification | External/internal/income/expense classification for return calculation |
| Reporting snapshot | Reproducible report-ready data set |
| QA assertion | Expected condition that must pass |
| Degraded state | Output with limitation due to missing/stale/unsupported data |
| Lineage | Traceability from output to source/calculation |
| RAG grounding | AI answer based on retrieved approved sources |
| Human-in-the-loop | human approval before important action |

## 2. Practitioner practitioner reference

When you see any financial event, ask:

1. Is this a lifecycle event or transaction?
2. Does it move cash?
3. Does it change position?
4. Does it create income?
5. Does it create fee or tax?
6. Does it realize P&L?
7. Does it affect cost basis?
8. Does it affect performance?
9. Does it affect risk/exposure?
10. Does it require client/advisor reporting?
11. Does it create a control or audit requirement?
12. Does AI need access to this information safely?

## 3. Example review checklist

For each worked example, verify:

- numerical calculation correct
- transaction records complete
- position movement correct
- cash movement correct
- performance classification correct
- reporting impact explained
- edge cases included
- QA assertions testable
- source assumptions explicit
- platform relevance clear

## 4. Next enrichment roadmap

High-value follow-up packs:

1. **Canonical Data Dictionary and Field Catalog**
   - instrument, product, transaction, position, valuation, performance, report, AI fields

2. **API and Event Contract Example Library**
   - sample REST APIs, AsyncAPI events, schemas, error responses

3. **Architecture Diagram Pack**
   - Mermaid/PlantUML diagrams for major product and platform flows

4. **Practitioner Question Bank**
   - product, analytics, architecture, AI, DPM, reporting, strategy

5. **Office Presentation Deck Pack**
   - reusable session outlines and slide-ready content

6. **wealth platform Golden Test Pack**
   - test cases directly usable by Codex/engineering agents

7. **Advisor Client Explanation Library**
   - plain-English scripts for products, performance, risk and reports

8. **AI Prompt and Evaluation Library**
   - approved prompts, RAG patterns, refusal tests, benchmark sets

## 5. Recommended next pack

The strongest next pack should be:

```text
Canonical Data Dictionary, API Contracts, Event Schemas and Domain Model Catalog for Wealth Platforms
```

Reason:

The knowledge base is now very rich. The next step is to convert it into more implementation-ready specifications: fields, schemas, APIs, events and data contracts.

## 6. Key takeaway

Worked examples are the bridge between knowledge and implementation. They turn "I understand it" into "I can build, test, explain and operate it."
