    # Family Office, Holding Vehicles, VCC and Ownership Structures

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. What is a family office?

A family office is an operating model for managing the financial, investment, administrative, governance and sometimes lifestyle needs of a wealthy family. It may or may not be the legal owner of assets. Often, assets are held by trusts, companies, funds, VCC sub-funds, partnerships, insurance wrappers or direct individual accounts, while the family office coordinates strategy, governance, reporting and service providers.

## 2. Single-family office versus multi-family office

| Dimension | Single-family office | Multi-family office |
|---|---|---|
| Serves | One family | Multiple families |
| Ownership | Usually controlled by one family | External/private bank/independent provider |
| Customisation | High | Standardised service model |
| Cost | Higher setup and operating cost | Shared cost base |
| Governance | Family-specific | Provider governance plus family-specific reporting |
| Platform need | Deep hierarchy and custom reports | Multi-client segregation and scalable workflows |

## 3. Family office functions

| Function | Examples |
|---|---|
| Investment governance | IPS, asset allocation, manager selection, DPM oversight |
| Consolidated reporting | Multi-bank, multi-custody, multi-entity reporting |
| Risk management | Concentration, liquidity, leverage, currency, counterparty risk |
| Estate and succession | Wills, trusts, family governance coordination |
| Tax/legal coordination | Advisors, filings, entity maintenance |
| Philanthropy | Foundations, grants, impact reporting |
| Administration | Payments, expenses, document management |
| Education | Next-generation education and family meetings |
| Operating businesses | Business ownership and liquidity planning |

## 4. Common holding structures

| Structure | Purpose | Platform treatment |
|---|---|---|
| Holding company | Owns investments/business assets | Corporate account, shareholder hierarchy |
| Private investment company | Family investment vehicle | Entity-level mandate and reporting |
| Trust-owned company | Trustee owns shares in company | Two-layer relationship graph |
| Partnership / LP | Private investment ownership | Capital account and commitment model |
| Foundation | Purpose/beneficiary governance | Legal entity with board/council |
| VCC | Fund vehicle, standalone or umbrella/sub-fund | Fund accounting and investor registry |
| Insurance wrapper | Policy holds investment exposure | Policy valuation and beneficiary rules |

## 5. Variable Capital Company / fund structures

Singapore’s VCC framework was launched by MAS and ACRA as a corporate structure for investment funds, providing operational flexibility for fund managers. A VCC can be used as a standalone fund or umbrella fund with sub-funds; current compliance is handled through ACRA/MAS requirements and regulated fund-manager arrangements.

Platform implications:

- the VCC is not just a normal company account;
- sub-funds may require ring-fenced reporting;
- NAV, subscriptions, redemptions and share classes may apply;
- investors may be family branches, trusts or companies;
- the family office may instruct or monitor, but fund manager/legal vehicle rules govern;
- regulatory and tax-incentive status may drive investment scope, spending and reporting controls.

## 6. Singapore family office tax incentive context

MAS has fund tax incentive schemes for funds managed by family offices under sections such as 13O, 13OA and 13U, with eligibility criteria that fund vehicles must meet throughout the incentive period. This pack does not reproduce detailed tax requirements because they change and require professional tax advice; the platform should store tax-incentive status and constraints as configurable attributes, not hardcoded assumptions.

Suggested platform fields:

| Field | Purpose |
|---|---|
| tax_scheme_type | 13O / 13OA / 13U / other / none |
| tax_scheme_status | Applied / approved / expired / revoked / not applicable |
| approval_date | Governance and audit |
| qualifying_investment_rules_ref | Link to policy/rules engine |
| local_spending_requirement_ref | Compliance tracking |
| investment_professional_requirement_ref | Compliance tracking |
| reporting_deadline | Operations/compliance calendar |

## 7. Family governance documents

| Document | Purpose |
|---|---|
| Family constitution | Principles, decision rights, values |
| Investment policy statement | Asset allocation, risk and liquidity objectives |
| Shareholder agreement | Ownership, voting, transfer restrictions |
| Trust deed | Trustee powers and beneficiary rules |
| Letter of wishes | Settlor guidance to trustees |
| Board/committee charter | Governance of investment decisions |
| Delegation of authority | Who can approve what |
| Philanthropy policy | Giving objectives and controls |

## 8. Family reporting hierarchy

Example:

```text
Family Group: Tan Family
  ├── Branch A
  │   ├── Trust A
  │   ├── Holding Company A
  │   └── Individual accounts
  ├── Branch B
  │   ├── Foundation B
  │   └── VCC Sub-Fund B
  └── Consolidated Family Office View
```

Reports may be needed at each level:

- individual account report;
- legal entity report;
- trust report;
- family branch report;
- consolidated family report;
- investment committee report;
- beneficiary report with restricted detail;
- philanthropic/impact report.

## 9. Family office analytics

| Analytics | Description |
|---|---|
| Strategic asset allocation | Target versus actual across all entities |
| Look-through exposure | Funds, private markets, notes and wrappers |
| Liquidity runway | Cash versus capital calls, distributions, expenses, loans |
| Currency exposure | Family base currency and entity-level currencies |
| Manager concentration | Exposure by bank, manager, issuer, custodian |
| Product complexity | Structured notes, derivatives, private assets |
| Beneficiary allocation | Economic exposure by family branch |
| Tax-sensitive income | Income categorisation for advisors |
| Leverage/collateral | Loans, pledged assets, LTV and cross-pledging |
| ESG/impact | Sustainability or philanthropy targets |

## 10. Platform design rule

A family office view should be a **reporting and governance aggregation**, not necessarily a legal owner. Keep legal ownership and reporting aggregation separate.

```text
Legal owner: Trust A
Custodian: Bank X
Investment advisor: Family Office Y
Beneficiary: Child Z
Reporting group: Tan Family Consolidated
```

If these are collapsed into one “client,” controls and reporting will break.
