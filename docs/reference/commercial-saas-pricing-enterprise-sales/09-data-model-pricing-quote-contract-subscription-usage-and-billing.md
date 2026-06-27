# 09 - Data Model: Pricing, Quote, Contract, Subscription, Usage and Billing

## 1. Why a commercial data model matters

A SaaS vendor needs systemized commercial operations.

The platform should track:

- products
- packages
- prices
- quotes
- contracts
- subscriptions
- entitlements
- usage
- invoices
- renewals
- discounts
- SLAs
- support plans
- customer success metrics

## 2. Product catalog

```json
{
  "productId": "performance analytics services",
  "productName": "wealth-platform suite Performance",
  "category": "Portfolio Analytics",
  "modules": ["TWR", "MWR", "Contribution", "Attribution", "Benchmarks"],
  "pricingMetrics": ["portfolio_count", "calculation_volume"],
  "status": "active"
}
```

## 3. Package entity

| Field | Example |
|---|---|
| package_id | enterprise-analytics |
| name | Enterprise Analytics |
| included_products | performance, risk, benchmark |
| included_limits | 100k portfolios |
| support_tier | premium |
| environments | prod, uat, dev |
| ai_included | no |

## 4. Price book

| Field | Example |
|---|---|
| price_id | PB-001 |
| product_id | performance analytics services |
| metric | portfolio_count |
| currency | USD |
| unit_price | 1.00 |
| billing_frequency | annual |
| tier_min | 0 |
| tier_max | 10000 |
| effective_from | 2026-01-01 |

## 5. Quote entity

| Field | Example |
|---|---|
| quote_id | Q-001 |
| customer_id | Bank A |
| package | Enterprise Analytics |
| term_months | 36 |
| annual_fee | 750000 |
| implementation_fee | 300000 |
| discount_pct | 15 |
| status | approved |
| valid_until | 2026-09-30 |

## 6. Contract entity

| Field | Example |
|---|---|
| contract_id | C-001 |
| customer_id | Bank A |
| start_date | 2026-10-01 |
| end_date | 2029-09-30 |
| auto_renewal | false |
| renewal_notice_days | 120 |
| governing_law | Singapore |
| sla_tier | mission-critical |
| dpa_signed | true |
| ai_addendum_signed | true |

## 7. Subscription entity

| Field | Example |
|---|---|
| subscription_id | SUB-001 |
| contract_id | C-001 |
| product_id | reporting services |
| quantity_metric | reports_per_year |
| committed_quantity | 500000 |
| included_quantity | 500000 |
| overage_rate | 0.02 |
| status | active |

## 8. Usage event

Usage event should be auditable.

```json
{
  "usageEventId": "USE-001",
  "customerId": "BankA",
  "productId": "AI copilot services",
  "metric": "ai_task",
  "quantity": 1,
  "timestamp": "2026-06-27T10:30:00Z",
  "environment": "prod",
  "workflow": "portfolio_review_explanation",
  "billable": true
}
```

## 9. Billing rules

Billing logic must handle:

- recurring subscription
- one-time implementation
- usage overage
- minimum commitments
- free allowances
- non-production exclusions
- discounts
- credit notes
- tax
- currency
- proration
- mid-term expansion
- true-up
- renewal uplift

## 10. Entitlement integration

Commercial entitlements should connect to product entitlements:

- module access
- user limits
- portfolio limits
- report limits
- AI task limits
- API quotas
- environment access
- feature flags
- premium support

## 11. Key takeaway

Commercial data must be as auditable as product data, especially for enterprise customers.
