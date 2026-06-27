# 07 - Exceptions, Disclosures, Degraded Data and Client Explanations

## 1. Why degraded-data handling matters

In wealth reporting, the platform will often face incomplete or imperfect data:

- private-market NAVs arrive late;
- structured-note valuations are issuer estimates;
- corporate actions are pending;
- fund NAVs are stale;
- bond prices are evaluated, not traded;
- derivatives require model valuations;
- insurance surrender values are periodic;
- FX rates may differ between sources;
- transactions may be pending settlement;
- client hierarchy or account ownership may change.

The correct response is not to hide the issue. The platform should provide controlled status, disclosure and explanation.

## 2. Data quality categories

| Category | Meaning | Client/report treatment |
|---|---|---|
| Final | Approved value available. | Show normally. |
| Provisional | Value likely to change. | Show with label and explanation. |
| Estimated | Model/issuer/vendor estimate. | Show with valuation source. |
| Stale | Value older than threshold. | Show valuation date and warning. |
| Missing | Required value absent. | Suppress, exclude or show N/A with reason. |
| Conflicting | Sources disagree beyond tolerance. | Use hierarchy or hold for review. |
| Corrected | Previously published value changed. | Show restatement/correction workflow. |
| Suppressed | Intentionally hidden due to policy. | Show blank/N/A with permitted label. |

## 3. Exception severity

| Severity | Description | Example |
|---|---|---|
| Informational | Does not materially impact report. | Minor label fallback. |
| Warning | Client interpretation may be affected. | Stale fund NAV. |
| Material | Report total or key metric affected. | Missing valuation for large holding. |
| Blocking | Report cannot be published. | Missing client/account scope, broken totals. |
| Regulatory/control | Required disclosure or control missing. | Missing fee disclosure or suitability status. |

## 4. Exception object model

| Field | Meaning |
|---|---|
| exception_id | Unique identifier. |
| report_run_id | Report impacted. |
| section | Holdings, performance, risk, etc. |
| entity_type | Account, position, instrument, transaction, price, metric. |
| entity_id | Impacted object. |
| severity | Informational/warning/material/blocking. |
| exception_type | Missing price, stale NAV, missing benchmark, etc. |
| description | Business-readable description. |
| client_display_flag | Whether shown to client. |
| advisor_display_flag | Whether shown to advisor. |
| resolution_status | Open, accepted, corrected, waived, closed. |
| owner | Team or system responsible. |
| materiality_amount | Optional impact amount. |
| materiality_pct | Optional impact percentage. |

## 5. Common reporting exceptions

| Exception | Cause | Reporting approach |
|---|---|---|
| Missing security price | Vendor/source unavailable. | Use fallback if allowed; otherwise flag and exclude/include at cost per policy. |
| Stale fund NAV | NAV not yet available. | Show latest NAV date and stale flag. |
| Missing FX rate | Currency conversion unavailable. | Use fallback source or block if material. |
| Pending corporate action | Entitlement not finalized. | Show provisional event note. |
| Unsettled trade | Trade executed but not settled. | Treat based on reporting basis. |
| Missing benchmark | Benchmark not mapped. | Show absolute performance only with warning. |
| Missing cost basis | Migration or source issue. | Show market value but suppress P&L or mark as unavailable. |
| Missing classification | Instrument taxonomy incomplete. | Assign temporary bucket with warning. |
| Incomplete look-through | Fund/structured product exposure unavailable. | Show legal holding and flag limited look-through. |
| Private-market NAV lag | Manager reporting delay. | Show NAV date and explain lag. |

## 6. Disclosure design

Disclosures should be specific, not generic walls of text.

Disclosure categories:

| Category | Examples |
|---|---|
| Valuation | Estimated price, stale NAV, model valuation, issuer quote. |
| Performance | Methodology, cashflow treatment, benchmark limitations. |
| Product risk | Structured products, derivatives, private markets, leverage. |
| Liquidity | Lock-up, notice period, secondary-market limitations. |
| Tax | Not tax advice, jurisdictional assumptions, withholding. |
| FX | Reporting-currency translation effects. |
| Data source | Third-party data dependency and timing. |
| Mandate | Breach, waiver, client restriction. |
| Fees | Gross/net, included/excluded charges. |

## 7. Good disclosure versus poor disclosure

| Poor | Better |
|---|---|
| Values may be inaccurate. | The NAV for Fund X is based on the latest available NAV dated 31 May 2026 and may change when the manager publishes the June NAV. |
| Performance may differ. | Performance is calculated using time-weighted return in USD and excludes external cashflows. |
| Prices are estimates. | The structured note valuation is an issuer-provided indicative bid price as of 30 June 2026 and may not be executable. |
| This is not tax advice. | Tax fields are provided for information and may not reflect your personal tax position; consult a tax adviser. |

## 8. Client-facing explanation patterns

### 8.1 Stale NAV

```text
The latest available NAV for this holding is dated [NAV date]. The value is included using that NAV because the latest period-end NAV has not yet been published by the fund manager. The valuation may change when updated NAV data becomes available.
```

### 8.2 Structured product valuation

```text
This structured product is valued using [issuer/vendor/model] data as of [valuation date]. The value depends on factors such as the underlying level, volatility, interest rates, time to maturity, issuer credit spread and liquidity. The valuation may differ from an executable sale price.
```

### 8.3 Missing cost basis

```text
Cost and unrealized gain/loss are not shown for this position because historical acquisition cost is unavailable or under review. Market value is still shown based on available valuation data.
```

### 8.4 Private-market NAV lag

```text
Private market valuations are typically reported with a lag by the fund manager. The value shown uses the latest available capital account or NAV statement and may not reflect recent market developments.
```

### 8.5 FX translation effect

```text
Part of the portfolio movement is due to currency translation between the investment currency and the report currency. This does not necessarily represent a realized gain or loss unless the position or currency was sold.
```

## 9. Suppression rules

Some values should be suppressed when quality is insufficient.

| Value | Suppress when |
|---|---|
| Unrealized P&L | Cost basis missing/unreliable. |
| Performance | Valuation history incomplete or cashflow classification invalid. |
| Attribution | Benchmark/segment mapping incomplete. |
| Yield | Income data incomplete or product yield not meaningful. |
| Risk metric | Market history insufficient or product not modelled. |
| Look-through exposure | Underlying composition unavailable or stale. |

Suppressing a metric is better than publishing a misleading metric.

## 10. Materiality policy

Materiality should consider:

- value impact;
- percentage of portfolio;
- client segment;
- product complexity;
- regulatory sensitivity;
- report type;
- whether the value affects decision-making;
- whether the error changes advisory recommendation.

## 11. Report release gates

Example release gates:

| Gate | Rule |
|---|---|
| Completeness | All required accounts included. |
| Valuation | No missing valuation above threshold. |
| Cash | Cash reconciled to custody/ledger. |
| Holdings | Position totals tie to book of record. |
| Performance | Returns calculated or explicitly suppressed. |
| Disclosures | Required product/valuation disclosures present. |
| Exceptions | Material exceptions approved before publication. |
| Delivery | Recipient entitlement validated. |

## 12. Exception analytics

Track exceptions over time:

- exception count by source;
- exception count by product type;
- stale price age;
- missing cost basis backlog;
- NAV-lag distribution;
- failed report count;
- manual override count;
- restatement count;
- client-impacting exception rate.

These metrics help improve the underlying platform, not just reporting.
