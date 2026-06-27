<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 02 - Kubernetes, AKS, OpenShift, Runtime and Workload Design

## 1. Kubernetes mental model

Kubernetes is a declarative platform. You declare desired state; the control plane continuously works to make actual state match desired state.

| Concept | Meaning |
|---|---|
| Cluster | Collection of control-plane and worker-node resources |
| Node | Machine that runs pods |
| Pod | Smallest deployable unit; one or more containers sharing network/storage context |
| Deployment | Desired-state controller for stateless replicated pods |
| StatefulSet | Controller for stateful replicated pods with stable identity |
| DaemonSet | Runs one pod per node, often for agents |
| Job | Runs finite work to completion |
| CronJob | Runs jobs on a schedule |
| Service | Stable network endpoint for pods |
| Ingress / Gateway | External HTTP routing into services |
| ConfigMap | Non-secret configuration |
| Secret | Sensitive configuration data |
| Namespace | Logical partition for resources and policy |
| RBAC | Role-based access control to Kubernetes API |

## 2. AKS in enterprise wealth platforms

Azure Kubernetes Service is a managed Kubernetes service. In wealth systems it is commonly used when the organization standardizes on Azure for cloud transformation, data platforms, AI services and managed networking.

Typical AKS concerns:

- private cluster versus public API server exposure
- VNET/subnet design
- network plugin and network policies
- Azure Container Registry integration
- managed identity / workload identity
- Azure Key Vault integration
- Azure Monitor / Log Analytics integration
- node pool design
- availability zones
- ingress controller and WAF
- private endpoints to data services
- backup and DR
- cluster upgrade policy
- cost governance

## 3. OpenShift in enterprise wealth platforms

OpenShift is an enterprise Kubernetes platform commonly used in banks due to strong platform governance, operator ecosystem, integrated developer tooling and enterprise support. Red Hat documentation currently provides OpenShift Container Platform 4.22 documentation, reflecting the continuing evolution of the platform.

Typical OpenShift concerns:

- projects/namespaces
- routes and ingress
- Security Context Constraints / pod security
- operators
- integrated registry
- build and deployment pipelines
- service mesh options
- OpenShift GitOps
- cluster logging and monitoring
- platform upgrades and operators
- multi-cluster management

## 4. AKS versus OpenShift

| Area | AKS | OpenShift |
|---|---|---|
| Platform type | Managed Kubernetes service | Enterprise Kubernetes platform |
| Control-plane management | Microsoft-managed | Depends on deployment model; enterprise platform controls |
| Developer UX | Azure-native + Kubernetes tools | Strong integrated console, routes, operators, developer tooling |
| Security defaults | Azure policy/identity integrations | Strong opinionated enterprise controls |
| Networking | Azure CNI, private endpoints, ingress/WAF | OpenShift networking, routes, SDN/OVN, service mesh |
| Best fit | Azure-aligned cloud modernization | Enterprise/hybrid platform standardization |

Both can be bank-grade if implemented with strong guardrails. The difference is operating model, not just technology.

## 5. Workload design patterns

### Stateless services

Examples:

- portfolio summary API
- holdings API
- advisory proposal API
- report metadata API
- product master API
- RAG retrieval API

Use Deployments with:

- multiple replicas
- readiness/liveness/startup probes
- resource requests and limits
- horizontal autoscaling
- graceful shutdown
- idempotent request handling
- externalized state

### Stateful workloads

Examples:

- PostgreSQL
- MongoDB/Cosmos-compatible workloads
- Redis
- Kafka brokers
- search indexes
- local cache nodes

In banks, many stateful platforms are consumed as managed services rather than run inside Kubernetes. If run inside Kubernetes, use mature operators, persistent storage, backup/restore controls, anti-affinity and DR design.

### Batch jobs

Examples:

- EOD portfolio valuation
- performance recalculation
- report generation
- reconciliation
- migration conversion
- market data enrichment
- AI embedding refresh

Use Jobs/CronJobs or orchestration tools. Ensure idempotency, restart safety, checkpointing and clear business-date semantics.

### Event consumers

Examples:

- transactions ingestion
- position events
- market data events
- product lifecycle events
- order/trade events

Design for:

- consumer group semantics
- idempotency keys
- retry and dead-letter queues
- ordering requirements
- partition strategy
- replay
- poison-message handling
- lag monitoring

## 6. Pod design checklist

| Area | Good practice |
|---|---|
| Resources | Set CPU/memory requests and limits |
| Probes | Define startup, readiness and liveness probes |
| Shutdown | Handle SIGTERM and graceful drain |
| Config | Externalize via ConfigMaps and environment config |
| Secrets | Use secret store integration, avoid hardcoding |
| Logging | Structured JSON logs with correlation IDs |
| Metrics | Expose `/metrics` for Prometheus-style scraping |
| Tracing | Propagate trace IDs across calls |
| Health | Separate technical health from business readiness |
| Identity | Use workload identity / service account |
| Security | Run non-root, read-only filesystem where possible |
| Storage | Avoid local state unless intentionally ephemeral |
| Resilience | Circuit breakers, retries with backoff, timeouts |
| Idempotency | Safe retry for writes/events |
| Versioning | Expose app version/build SHA |

## 7. Resource requests, limits and Python/Kotlin services

For JVM/Kotlin services, memory sizing must account for heap, metaspace, threads, direct buffers and container memory limit.

For Python services, there is no JVM heap flag equivalent, but sizing still matters:

- worker count
- async versus sync model
- request concurrency
- memory growth from pandas/numpy/dataframes
- batch payload size
- cache size
- model/embedding memory
- container memory limit
- Python garbage collection behavior
- Uvicorn/Gunicorn worker configuration

For portfolio analytics, batch size and memory growth are often more important than baseline API traffic.

## 8. Namespace design

Example namespace model:

| Namespace type | Purpose |
|---|---|
| app-dev | Development workloads |
| app-test | Integration testing |
| app-uat | User acceptance / business validation |
| app-prod | Production runtime |
| shared-observability | Logging/metrics/tracing agents |
| shared-ingress | Ingress controllers |
| shared-security | Policy/secrets/security agents |

Do not overload namespaces as only technical partitions. Use them for:

- access control
- network policy
- resource quotas
- deployment separation
- environment boundary
- operational ownership

## 9. Node pool design

| Node pool | Workloads |
|---|---|
| system | cluster system components |
| app-general | stateless APIs |
| batch-analytics | EOD/recalculation jobs |
| memory-optimized | valuation/reporting workloads |
| compute-optimized | analytics calculation |
| ai-gpu | ML/AI inference or training |
| isolated-sensitive | regulated/sensitive workloads |

Use taints, tolerations, node selectors and affinity to control placement.

## 10. Common failure patterns

| Failure | Cause | Prevention |
|---|---|---|
| CrashLoopBackOff | bad config, missing secret, startup failure | config validation, startup probes |
| OOMKilled | memory limit too low, batch too large | memory profiling, batch sizing |
| Readiness flapping | dependency or health check design issue | separate health checks and dependencies |
| Slow rollout | insufficient capacity or tight PDB | capacity buffer, surge strategy |
| Hidden data loss | non-idempotent consumer retries | idempotency and DLQ |
| Report job timeout | large portfolios / poor pagination | chunking, async jobs, progress tracking |
| API latency spike | DB/cache bottleneck | profiling, connection pools, autoscaling |
| Stale results | cache invalidation failure | source lineage and freshness markers |

## 11. Wealth platform workload examples

| Capability | Kubernetes pattern |
|---|---|
| Holdings API | Deployment + cache + DB |
| Transactions API | Deployment + pagination + DB indexes |
| Performance engine | Batch job + calculation services + result store |
| Report generation | Async job + queue + renderer + archive |
| Market data ingestion | Event consumers + idempotent enrichment |
| Buying power | Low-latency API + rule engine + cache |
| AI advisor copilot | API + RAG retriever + model gateway + audit log |
| Reconciliation | Scheduled jobs + break store + workflow |
