<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 07 - Resilience, Scaling, Performance, Capacity, Cost and Disaster Recovery

## 1. Resilience mindset

A resilient wealth platform assumes failure:

- pods restart
- nodes fail
- zones degrade
- downstream systems time out
- market data arrives late
- reports fail to render
- caches become stale
- jobs need replay
- client/advisor demand spikes
- country rollout increases volume

The platform must fail safely and recover predictably.

## 2. Availability concepts

| Concept | Meaning |
|---|---|
| HA | continue operation despite local failures |
| DR | recover after site/region-scale disruption |
| RTO | recovery time objective |
| RPO | recovery point objective |
| SLO | measurable service level objective |
| SLI | metric used to evaluate SLO |
| Error budget | acceptable unreliability budget |
| Graceful degradation | reduced service instead of incorrect service |
| Backpressure | limit work to avoid collapse |
| Idempotency | retry safely without duplicate effect |

## 3. Scaling types

| Scaling type | Description |
|---|---|
| Horizontal Pod Autoscaling | scale pod replicas |
| Vertical Pod Autoscaling | adjust resource requests |
| Cluster autoscaling | add/remove nodes |
| Queue-based scaling | scale workers based on backlog |
| Scheduled scaling | pre-scale for known business windows |
| Manual capacity override | temporary controlled scaling |
| Partition-based scaling | scale consumers by partitions |
| Sharding | divide data/workload by key |

## 4. Wealth workload scaling patterns

| Workload | Scaling driver |
|---|---|
| Holdings API | advisor/client traffic |
| Transactions API | large history and pagination |
| Portfolio review | report-generation spikes |
| Performance engine | business-date calculation volume |
| Market data ingestion | feed arrival windows |
| Reconciliation | source-file arrival and break volume |
| Buying power | pre-trade traffic and low-latency needs |
| AI copilot | advisor usage and model latency |
| RAG indexing | document ingestion and embedding refresh |

## 5. Resilience patterns

| Pattern | Use |
|---|---|
| Timeouts | prevent indefinite waits |
| Retries with backoff | handle transient failures |
| Circuit breaker | avoid hammering failing systems |
| Bulkheads | isolate failures by pool/service |
| Rate limiting | protect downstream systems |
| Dead-letter queue | isolate poison messages |
| Checkpointing | resume long-running jobs |
| Saga/orchestration | manage multi-step workflows |
| Outbox/inbox | reliable event processing |
| Read replicas | separate read traffic |
| Caching | reduce load, with freshness controls |
| Degraded mode | show partial but honest data |

## 6. Performance engineering

Measure:

- API p50/p95/p99 latency
- DB query time
- cache hit ratio
- queue lag
- job duration
- report rendering time
- calculation throughput
- memory growth
- CPU saturation
- pod restarts
- GC pauses for JVM services
- Python worker saturation
- downstream dependency latency

## 7. Capacity planning

Capacity planning must include:

- business-date windows
- monthly/quarterly statements
- annual reporting spikes
- country onboarding
- migration backfills
- reprocessing/recalculation waves
- worst-case large portfolios
- high transaction history accounts
- AI embedding/index rebuilds
- DR failover capacity

## 8. Cost controls

Cloud cost is a design concern.

| Cost driver | Control |
|---|---|
| over-requested CPU/memory | right-sizing |
| always-on batch capacity | autoscaling/scheduled pools |
| excessive logs | log sampling/retention policy |
| inefficient queries | indexing/profiling |
| report re-renders | snapshot/archive reuse |
| AI model calls | caching, batching, smaller models |
| vector store growth | retention and index lifecycle |
| cross-region traffic | locality and architecture review |
| orphaned environments | lifecycle automation |

## 9. Disaster recovery

For every critical capability define:

| Field | Example |
|---|---|
| Criticality | high/medium/low |
| RTO | 1 hour / 4 hours / next business day |
| RPO | near-zero / 15 minutes / EOD |
| Dependency map | DB, cache, stream, vendor feed |
| Restore procedure | documented and tested |
| DR environment | warm/cold/active-active |
| Data recovery | snapshot, log replay, event replay |
| Validation | reconciliation and smoke tests |
| Business sign-off | operational acceptance |

## 10. Business-date recovery

Wealth platforms must recover by business date, not only timestamp.

Example recovery questions:

- What was the official EOD portfolio value for business date D?
- Which market prices were used?
- Which FX rates were used?
- Which transactions were included?
- Which reports were generated and archived?
- Can we rerun performance without changing the original statement?
- Can we explain differences after a correction?

## 11. Resilience testing

Test:

- pod restart
- node drain
- zone failure
- DB failover
- cache failure
- queue backlog
- downstream timeout
- stale market data
- failed report renderer
- partial source feed arrival
- bad message in stream
- deployment rollback
- secret rotation
- certificate rotation
- DR restore

## 12. Practitioner summary

A strong answer:

> For wealth platforms, resilience is not only uptime. It is the ability to preserve financial correctness, business-date lineage and client-report reproducibility under failure. Kubernetes gives primitives for scaling and self-healing, but real resilience comes from idempotent processing, timeout discipline, replay, checkpoints, backup/restore, DR testing, observability and safe degraded modes.
