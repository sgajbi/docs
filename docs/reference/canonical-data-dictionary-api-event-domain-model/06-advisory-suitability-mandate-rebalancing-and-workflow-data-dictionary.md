# 06 - Advisory, Suitability, Mandate, Rebalancing and Workflow Data Dictionary

## 1. Advisory proposal

| Field | Type | Description |
|---|---|---|
| proposal_id | string | proposal ID |
| portfolio_id | string | portfolio |
| client_id | string | client |
| advisor_id | string | advisor |
| proposal_type | enum | rebalance, product_switch, new_investment, risk_reduction |
| status | enum | draft, checked, approved, client_accepted, rejected, expired |
| created_at | datetime | created |
| valid_until | date | expiry |
| rationale | text | recommendation rationale |
| ai_assisted_flag | boolean | AI was used |
| suitability_result_id | string | suitability check |
| mandate_check_result_id | string | mandate check |
| approval_workflow_id | string | approval workflow |

## 2. Recommendation line

| Field | Type | Description |
|---|---|---|
| recommendation_line_id | string | line ID |
| proposal_id | string | proposal |
| action | enum | buy, sell, hold, switch, reduce, increase |
| instrument_id | string | instrument |
| amount | decimal | recommended amount |
| currency | ISO currency | currency |
| quantity | decimal | quantity |
| reason_code | enum | rebalance, suitability, risk, income, liquidity, client_goal |
| expected_allocation_after | decimal | expected allocation |
| status | enum | proposed, approved, rejected, executed |

## 3. Suitability result

| Field | Type | Description |
|---|---|---|
| suitability_result_id | string | result ID |
| client_id | string | client |
| portfolio_id | string | portfolio |
| proposal_id | string | proposal |
| product_id | string | product |
| result | enum | pass, fail, warning, override_required |
| rule_results | array | individual rule outcomes |
| checked_at | datetime | check time |
| ruleset_version | string | rules version |
| explanation | text | result explanation |
| override_allowed | boolean | whether override is allowed |
| override_reason | text | override explanation |
| approved_by | string | approver |

## 4. Mandate

| Field | Type | Description |
|---|---|---|
| mandate_id | string | mandate ID |
| portfolio_id | string | portfolio |
| mandate_type | enum | advisory, discretionary, execution_only |
| model_portfolio_id | string | linked model |
| benchmark_id | string | benchmark |
| start_date | date | start |
| end_date | date | end |
| status | enum | active, suspended, terminated |
| investment_objective | string | objective |
| restrictions | array | restrictions |
| allocation_ranges | array | target/range rules |
| ruleset_version | string | rule version |

## 5. Mandate rule

| Field | Type | Description |
|---|---|---|
| mandate_rule_id | string | rule ID |
| mandate_id | string | mandate |
| rule_type | enum | allocation, concentration, prohibited_product, liquidity, leverage, ESG |
| dimension | enum | asset_class, issuer, currency, product, country, rating |
| min_value | decimal | min |
| max_value | decimal | max |
| hard_soft | enum | hard, soft |
| effective_from | date | start |
| effective_to | date | end |
| status | enum | active, retired |

## 6. Rebalance run

| Field | Type | Description |
|---|---|---|
| rebalance_run_id | string | run ID |
| portfolio_id | string | portfolio |
| model_portfolio_id | string | model |
| run_type | enum | scheduled, adhoc, drift_triggered |
| as_of_date | date | date |
| status | enum | calculated, proposed, approved, executed, cancelled |
| drift_summary | object | drift results |
| optimizer_version | string | optimizer version |
| generated_proposal_id | string | proposal generated |

## 7. Workflow task

| Field | Type | Description |
|---|---|---|
| task_id | string | task |
| workflow_id | string | workflow |
| task_type | enum | review, approve, remediate, notify, execute |
| assigned_to | string | user/team |
| status | enum | open, in_progress, approved, rejected, completed |
| due_date | date | due |
| priority | enum | low, medium, high, critical |
| related_entity_type | string | proposal, mandate_breach, report, vendor, incident |
| related_entity_id | string | linked entity |
| created_at | datetime | created |
| completed_at | datetime | completed |

## 8. Mandate breach

| Field | Type | Description |
|---|---|---|
| breach_id | string | breach |
| portfolio_id | string | portfolio |
| mandate_id | string | mandate |
| rule_id | string | violated rule |
| detected_at | datetime | detection time |
| breach_type | enum | allocation, concentration, liquidity, leverage, product |
| current_value | decimal | actual |
| limit_value | decimal | allowed |
| severity | enum | low, medium, high, critical |
| cause | enum | market_movement, trade, data_issue, corporate_action |
| status | enum | open, waived, remediated, closed |
| remediation_plan | text | plan |

## 9. Validation rules

- proposals must have current suitability check before client acceptance.
- DPM trade proposals must pass mandate check before execution.
- hard mandate breaches must block execution unless formal override is allowed.
- AI-assisted proposals must store AI context and human approval.
- workflow tasks must have owner and status.
- rule versions must be recorded for audit.

## 10. Key takeaway

Advisory and DPM data models must combine investment intent, rules, workflow, approval and evidence.
