    # Lifecycle, Transactions, Ownership, Position and Reporting Modelling

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. Why lifecycle modelling is different for wealth structures

Product lifecycle modelling tells you how an asset changes. Wealth-structure lifecycle modelling tells you how **ownership, authority, entitlement and reporting** change.

A trust may continue holding the same bond, but a beneficiary may die, a trustee may change, a protector approval may become necessary, or assets may be transferred to a new trust. These are structural lifecycle events, not product lifecycle events.

## 2. Structural lifecycle stages

| Stage | Description | Platform impact |
|---|---|---|
| Prospect/advisory | Discuss structure objectives | Advisory case, suitability inputs |
| Structure design | Legal/tax/trust advisors define setup | Document checklist, draft relationships |
| Onboarding | Entity KYC, UBO, FATCA/CRS/tax status | Client/entity records |
| Account opening | Custody/cash/loan/advisory/DPM accounts | Account and mandate setup |
| Funding | Cash/securities transferred into structure | Transfer transactions |
| Investment operation | Trading, DPM, advisory, reporting | Normal portfolio activity |
| Governance event | Trustee/director/protector/beneficiary change | Authority and relationship update |
| Distribution | Asset/cash transferred to beneficiary | Distribution transaction |
| Restructure | Transfer to new entity/trust/fund | Ownership transfer event |
| Succession | Death/incapacity/estate activation | Restriction and authority change |
| Termination | Structure closes | Final distributions and closure |

## 3. Structural event types

| Event type | Description |
|---|---|
| STRUCTURE_CREATED | Trust/company/foundation/family group created |
| STRUCTURE_ONBOARDED | Bank/platform onboarding completed |
| PARTY_ROLE_ASSIGNED | Trustee/director/beneficiary/protector/executor added |
| PARTY_ROLE_REMOVED | Role removed/resigned/deceased |
| AUTHORITY_CHANGED | Signing/approval authority changed |
| BENEFICIARY_ENTITLEMENT_CHANGED | Income/capital/reporting entitlement changed |
| DOCUMENT_UPDATED | Trust deed/will/board resolution updated |
| STRUCTURE_FUNDED | Assets transferred in |
| DISTRIBUTION_APPROVED | Trustee/board approves distribution |
| DISTRIBUTION_PAID | Distribution transaction posted |
| INCAPACITY_ACTIVATED | LPA/POA/donee authority activated |
| DEATH_REPORTED | Death event recorded |
| ESTATE_OPENED | Estate administration begins |
| STRUCTURE_TERMINATED | Structure closed |

## 4. Transaction types

| Transaction type | Asset/position impact | Cash impact | Performance treatment |
|---|---|---|---|
| STRUCTURE_FUNDING_CASH | Cash increases | Cash in | External inflow to structure |
| STRUCTURE_FUNDING_SECURITY | Security position increases | None or linked cash | External contribution / transfer in |
| STRUCTURE_TRANSFER_IN | Asset received from another structure/account | Maybe | Transfer logic |
| STRUCTURE_TRANSFER_OUT | Asset leaves structure | Maybe | Transfer/distribution |
| BENEFICIARY_INCOME_DISTRIBUTION | No product sale unless funded by cash | Cash out to beneficiary | External outflow from structure after income recognition |
| BENEFICIARY_CAPITAL_DISTRIBUTION | Asset/cash leaves capital | Cash/security out | External outflow/distribution |
| TRUSTEE_FEE | Expense | Cash out | Fee/expense |
| FAMILY_OFFICE_FEE | Expense | Cash out | Fee/expense |
| LEGAL_TAX_ADMIN_FEE | Expense | Cash out | Fee/expense |
| ESTATE_SETTLEMENT_PAYMENT | Liability paid | Cash out | Estate expense/debt settlement |
| STRUCTURE_REORG_TRANSFER | Move assets between related structures | Internal/external depending consolidation | Must avoid double-counting |
| CHARITABLE_GRANT | Donation/distribution | Cash/security out | External outflow/expense |

## 5. Position ownership model

A product position should have legal ownership attributes and optional beneficial allocation attributes.

| Field | Example |
|---|---|
| position_id | POS123 |
| instrument_id | BOND456 |
| account_id | Trust custody account |
| legal_owner_party_id | Trustee Co as trustee of ABC Trust |
| beneficial_owner_group_id | Beneficiaries of ABC Trust |
| reporting_group_id | Family Group Consolidated |
| mandate_id | DPM Conservative Income Mandate |
| restriction_set_id | Trust deed investment restrictions |
| cost_basis_owner | Trust / settlor carryover / estate |
| income_classification | Trust income / capital / retained |
| distributable_flag | Yes/no/conditional |
| pledged_flag | Yes/no |
| collateral_pool_id | Lombard line collateral pool |

## 6. Beneficial allocation model

For fixed entitlements:

| Beneficiary | Income entitlement | Capital entitlement | Reporting access |
|---|---:|---:|---|
| Beneficiary A | 50% | 40% | Summary only |
| Beneficiary B | 50% | 60% | Summary only |

For discretionary trusts:

| Beneficiary | Entitlement |
|---|---|
| Beneficiary A | Discretionary, not fixed |
| Beneficiary B | Discretionary, not fixed |
| Charitable class | Discretionary class |

Do not force discretionary trusts into fixed percentages. Model them as classes unless distributions are actually approved.

## 7. Reporting hierarchy model

Use separate objects for:

- legal entity hierarchy;
- reporting hierarchy;
- beneficial entitlement hierarchy;
- mandate hierarchy;
- tax/reporting hierarchy;
- access-control hierarchy.

Example:

```text
Product position -> Account -> Legal entity -> Family branch -> Consolidated family group
                 -> Mandate -> Risk profile -> Investment objective
                 -> Beneficiary entitlement -> Income/capital allocation
                 -> Reporting access group -> Portal user permissions
```

## 8. Consolidation and double-counting

Common double-counting situations:

| Scenario | Risk | Correct treatment |
|---|---|---|
| Family owns holding company that owns portfolio | Count company shares and underlying assets together | Choose one consolidation basis or eliminate intercompany layer |
| Trust owns company shares and company has bank account | Trust report shows shares; family report may need look-through | Use look-through with elimination controls |
| Family member is beneficiary of trust | Counting full trust value as personal asset may overstate wealth | Use entitlement/reporting rules |
| Insurance policy holds fund units | Count policy value and underlying funds together | Use policy value as accounting position; underlying as look-through only |
| VCC sub-fund holds assets and investor holds shares | Fund investor report uses shares/NAV; manager report uses assets | Separate investor accounting and fund accounting views |

## 9. Cost basis and transfer considerations

Transfers into trusts, estates, companies or beneficiaries can have different tax/accounting treatments depending on jurisdiction. Platform should capture cost basis method, not assume.

| Method | Meaning |
|---|---|
| Carryover basis | Recipient inherits original cost basis |
| Fair value basis | Transfer booked at market value |
| Probate/date-of-death basis | Estate administration value basis |
| Nominal/internal transfer | Used for non-tax internal reporting |
| Jurisdiction-specific | Driven by tax/legal advice |

## 10. Portfolio performance treatment

| Activity | Portfolio performance treatment |
|---|---|
| Funding trust with outside cash | External inflow |
| Transfer securities from settlor into trust | External inflow/transfer-in at chosen valuation basis |
| Coupon/dividend received | Investment income |
| Trustee fee paid | Expense |
| Distribution to beneficiary | External outflow from trust |
| Transfer between accounts within same consolidated reporting group | Internal transfer if consolidation basis includes both |
| Transfer from trust to beneficiary personal account | External outflow from trust, external inflow to beneficiary, eliminated only in family consolidated view if both included |
| Estate distribution | External outflow from estate, inflow to beneficiary |

## 11. Mandate interaction

A DPM mandate on a trust or family office account should consider:

- legal client risk profile;
- trust deed restrictions;
- beneficiary time horizon and liquidity needs;
- family investment policy statement;
- borrowing/leverage permission;
- derivatives permission;
- ESG/religious/ethical exclusions;
- tax-sensitive income preferences;
- currency base and distribution currency;
- future capital calls and planned distributions.
