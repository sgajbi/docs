# 10 - Infrastructure, Containers And Deployment

Enterprise services should be designed so they can run consistently across local development, CI, test environments and production-like runtime. Infrastructure is not an afterthought; it is the execution environment for reliability, security, observability and release evidence.

For deeper runtime, container, Kubernetes, deployment and platform-readiness guidance, use [`runtime-infrastructure-kubernetes/`](runtime-infrastructure-kubernetes/README.md).

## Runtime Environment Model

Think in layers:

| Layer | Responsibility |
|---|---|
| Application process | Runs the service code, configuration, health endpoints and metrics endpoint. |
| Container image | Packages runtime dependencies, application files, startup command and non-root runtime posture where possible. |
| Container runtime | Starts containers, captures logs, applies resource limits and health checks. |
| Orchestration | Schedules workloads, handles restarts, service discovery, scaling and rollout strategy. |
| Platform services | Provide ingress, identity, secrets, metrics, logs, traces, storage and policy controls. |
| Release pipeline | Builds, scans, validates, deploys and records evidence. |

## Container Design Principles

Good container images:

1. are reproducible,
2. use minimal base images where practical,
3. install only required runtime dependencies,
4. avoid embedded secrets,
5. expose explicit ports,
6. define clear startup commands,
7. support health checks,
8. run as non-root where possible,
9. keep build-time and runtime concerns separate,
10. produce logs to stdout/stderr for platform collection.

Avoid:

1. installing development tools in production images without need,
2. copying local cache directories into images,
3. relying on host-specific paths,
4. using mutable latest tags for controlled releases,
5. hiding startup failures behind long sleep loops,
6. writing logs only to local files inside containers.

## Dockerfile Review Checklist

Before accepting a service Dockerfile, check:

1. base image is approved and maintained,
2. dependency installation is deterministic,
3. application code is copied intentionally,
4. secrets and local files are excluded,
5. image exposes only required ports,
6. startup command matches documented runtime,
7. non-root user is used when compatible,
8. health or readiness behavior is externally testable,
9. image build is part of CI,
10. vulnerability scan posture is understood.

## Local Compose Pattern

Local compose files are useful for:

1. developer startup,
2. integration tests,
3. dependency simulation,
4. local observability stack,
5. smoke testing runtime contracts.

Compose should define:

1. service image or build context,
2. ports,
3. environment variables,
4. dependency order where useful,
5. health checks,
6. volumes only when needed,
7. named networks,
8. log rotation policy where supported.

Do not treat local compose as proof of production readiness. It is a useful parity layer, not a replacement for orchestrated deployment design.

## Kubernetes And Orchestration Concepts

Key concepts:

| Concept | Practical meaning |
|---|---|
| Deployment | Desired replica set and rollout behavior for stateless workloads. |
| Service | Stable network identity for pods. |
| Ingress / gateway | External routing, TLS and host/path mapping. |
| ConfigMap | Non-secret configuration. |
| Secret | Sensitive configuration, managed with stricter controls. |
| Readiness probe | Determines whether traffic should be sent to a pod. |
| Liveness probe | Determines whether a pod should be restarted. |
| Resource request | Scheduler capacity reservation. |
| Resource limit | Runtime protection from runaway usage. |
| Horizontal scaling | Adds replicas based on metrics or policy. |

## Deployment Strategy

Common strategies:

| Strategy | Use |
|---|---|
| Rolling deployment | Default for stateless services with backward-compatible changes. |
| Blue/green | Useful when fast rollback and full environment switch are needed. |
| Canary | Useful for controlled exposure to a small percentage of traffic. |
| Shadow traffic | Useful for observing new behavior without serving user responses. |
| Feature flag | Useful when code deploy and user enablement should be separated. |

Choose based on risk, data migration impact, user exposure, rollback complexity and observability coverage.

## Deployment Readiness

A service is deployment-ready when:

1. container build passes,
2. startup command is documented,
3. required configuration is declared,
4. health and readiness endpoints are implemented,
5. metrics endpoint is available where expected,
6. migration posture is known,
7. secrets are externalized,
8. resource requirements are reasonable,
9. rollback path is documented,
10. CI retains release evidence.

## Environment Configuration

Use environment profiles:

| Environment | Typical purpose |
|---|---|
| local | Developer feedback and focused debugging. |
| CI | Deterministic validation and artifact generation. |
| integration | Cross-service validation. |
| staging | Production-like release rehearsal. |
| production | Controlled user-facing runtime. |

Configuration must not rely on local machine paths or hidden shell state. Required variables should fail fast when missing.

## Database And Migration Deployment

Database-backed services need:

1. migration smoke tests,
2. backward-compatible schema changes where possible,
3. rollback or forward-fix plan,
4. data retention policy,
5. query-plan review for critical paths,
6. backup and restore validation,
7. startup checks that do not accidentally mutate schema.

Safe migration sequence:

```text
add compatible schema -> deploy code that writes both or reads both -> backfill -> switch readers -> remove old schema later
```

## Runtime Evidence

Deployment evidence should include:

1. image tag or digest,
2. build timestamp,
3. source commit,
4. test and gate summary,
5. vulnerability scan result,
6. migration result,
7. health/readiness result,
8. smoke-test output,
9. rollback decision or link,
10. known limitations.

## Infrastructure Anti-Patterns

Avoid:

1. production configuration that is only known by one engineer,
2. deployments without rollback plan,
3. health checks that always return success,
4. readiness checks that ignore critical dependencies,
5. containers that require manual shell steps,
6. unbounded resource use,
7. environment-specific code branches,
8. infrastructure diagrams that do not match runtime,
9. release evidence that cannot be tied to a commit.

## Engineering Review Questions

1. Can the service run from a fresh checkout?
2. Can CI build the image?
3. Can an orchestrator tell live from ready?
4. Are secrets externalized?
5. Are resource limits and scaling assumptions documented?
6. Is rollback possible?
7. Does deployment evidence map to the exact commit?
