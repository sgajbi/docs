<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 05 - Storage, Stateful Workloads, Databases, Cache and Data Resilience

## 1. Stateless by default, stateful by design

Most business services should be stateless:

- scale horizontally
- recover quickly
- avoid local mutable state
- persist business state in controlled stores
- use queues/events for asynchronous work
- use idempotency for safe retries

But wealth platforms still need state:

- positions
- transactions
- cash balances
- valuations
- performance results
- report snapshots
- audit logs
- entitlements
- product master
- market data
- workflow state
- RAG indexes
- caches

## 2. Storage types

| Storage type | Use |
|---|---|
| Ephemeral container storage | temporary files, not durable |
| Persistent volume | stateful pods requiring durable storage |
| Object storage | reports, files, archives, raw feeds, exports |
| Relational database | transactions, positions, ledger, workflow |
| Document database | flexible portfolio snapshots, reports, product docs |
| Cache | Redis or equivalent for hot reads/session/rules |
| Search/vector store | document search, RAG, semantic retrieval |
| Queue/stream | asynchronous processing and replay |
| Data lake | historical data, analytics, raw/curated zones |

## 3. Kubernetes persistent volumes

Use Kubernetes persistent volumes only when the platform operating model supports:

- backup
- restore
- volume expansion
- zone affinity
- data encryption
- storage-class governance
- performance tiers
- snapshot policy
- failover testing
- operator lifecycle

For regulated financial systems, managed databases are often preferable unless there is a strong reason to run the database inside Kubernetes.

## 4. Database placement decision

| Option | Strength | Risk |
|---|---|---|
| Managed cloud database | operational simplicity, HA/backup integrations | cloud service dependency, configuration discipline |
| Database in Kubernetes | portability, operator-driven control | operational complexity, backup/DR burden |
| External enterprise DB | existing bank standard | network latency, change coordination |
| Data lake only | large analytical scale | not usually suitable for low-latency transactional APIs |

## 5. Caching

Cache is useful but dangerous when freshness is unclear.

Use cache for:

- portfolio summaries
- product metadata
- entitlement snapshots
- reference data
- expensive calculation results
- market-data lookups
- session state only if required

Cache controls:

- TTL
- source timestamp
- business-date marker
- invalidation rules
- version key
- cache-warm strategy
- stale-value policy
- cache-bypass for break investigation
- metrics: hit ratio, stale hits, evictions

## 6. Portfolio analytics data stores

A typical analytics platform may use:

| Store | Purpose |
|---|---|
| Operational DB | job status, calculations, metadata |
| Time-series store | daily market values, returns, risk metrics |
| Object store | snapshots, reports, audit files |
| Cache | latest summaries |
| Queue/stream | reprocessing jobs/events |
| Data lake | historical raw/curated data |
| Vector store | product knowledge and RAG |

## 7. Backup and restore

Backup is not useful unless restore is tested.

For each store define:

| Field | Example |
|---|---|
| RPO | maximum data loss tolerated |
| RTO | maximum recovery time |
| Backup frequency | hourly/daily/business-date |
| Retention | 35 days, 7 years, etc. |
| Encryption | at rest and in transit |
| Restore test | monthly/quarterly |
| Business-date restore | restore to known EOD snapshot |
| Audit evidence | test result and sign-off |

## 8. Data resilience patterns

| Pattern | Use |
|---|---|
| Idempotent writes | safe retries |
| Outbox pattern | reliable event publication |
| Inbox/dedup table | consumer idempotency |
| Checkpointing | long-running batch jobs |
| Snapshotting | report/business-date reproducibility |
| Event replay | rebuild projections |
| Versioned data | support corrections and backdated events |
| Soft delete | recover accidental deletes |
| Immutable archive | audit and statement reproducibility |

## 9. Stateful workload anti-patterns

Avoid:

- storing business data only in pod filesystem
- relying on single Redis node without persistence or recovery plan
- using cache as source of truth
- running databases in Kubernetes without operator expertise
- mixing production and test data stores
- backups without restore tests
- unversioned report snapshots
- deleting historical data needed for audit/performance
- using local time rather than business date/cut-off rules

## 10. Wealth-specific storage examples

| Capability | Data storage concern |
|---|---|
| Client reporting | immutable report snapshot and PDF archive |
| Performance | historical daily MV/cashflow snapshots |
| Buying power | low-latency current collateral/exposure cache with source timestamps |
| Advisory proposal | proposal version, consent, suitability evidence |
| AI RAG | approved document corpus, index version, retrieval audit |
| Reconciliation | break store, source snapshots, certification |
| Migration | parallel-run snapshots, mapping evidence |

## 11. Data deletion and retention

Financial data retention varies by regulation, policy and client agreement. Design retention as policy-driven:

- transactional records
- report snapshots
- advisor notes
- suitability evidence
- order and consent records
- data lineage
- AI prompts/responses
- access logs
- audit events
- incident records

## 12. Practitioner summary

A strong answer:

> For Kubernetes-based wealth platforms, application services should be stateless wherever possible, but the platform must have a disciplined data-resilience model for databases, object stores, caches, streams and report snapshots. Cache must never become an uncontrolled source of truth. Backup, restore, business-date reproducibility, event replay, idempotency and audit lineage are essential for financial correctness.
