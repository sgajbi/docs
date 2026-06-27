# Workload Design: Deployments, Pods, Jobs, Workers, and Schedulers

## Purpose

This file explains how to choose the right workload type for different application responsibilities.

Not every process should be a web API deployment. Enterprise platforms often need APIs, workers, scheduled jobs, retention jobs, and operator tasks.

---

## Workload Types

| Workload | Use |
|---|---|
| Deployment | Long-running API or worker process with replicas. |
| Job | One-time task such as migration or batch execution. |
| CronJob | Scheduled task such as retention or periodic ingestion. |
| Worker | Background consumer or async executor. |
| Init container | Setup before main container starts. |
| Sidecar | Supporting process, such as proxy or log helper, where appropriate. |

---

## API Deployment

API deployments should define:

- replicas
- service port
- health/liveness/readiness
- resource requests/limits
- config/secrets
- graceful shutdown
- metrics
- autoscaling
- rolling deployment strategy

---

## Worker Deployment

Workers need:

- queue/topic/source
- concurrency settings
- retry policy
- lease model
- idempotency
- dead-letter path
- health/readiness semantics
- metrics
- shutdown handling
- replay controls
- scaling rules

---

## Scheduled Work

Scheduled jobs should define:

- schedule
- timezone/business calendar assumptions
- idempotency
- missed-run behavior
- concurrency policy
- retry behavior
- observability
- output/evidence
- runbook

---

## Migration Jobs

Database migrations can run:

- manually before deploy
- as CI/CD controlled job
- as init job
- outside cluster through release process

Choose based on risk and governance.

Migration jobs need:

- version control
- lock or concurrency protection
- rollback/compensation
- evidence output
- owner
- runbook

---

## Graceful Shutdown

Workloads should handle shutdown:

- stop accepting new work
- finish or checkpoint current work
- release locks/leases
- flush logs/metrics
- close DB connections
- exit within grace period

Workers especially need safe shutdown.

---

## Workload Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Long-running work inside API request | Timeouts and poor UX. |
| Worker has no idempotency | Duplicate side effects. |
| Cron job not idempotent | Duplicate scheduled output. |
| Migration runs in every pod | Race condition. |
| No shutdown handling | Lost or duplicated work. |
| Scheduler uses local timezone unintentionally | Wrong business timing. |
| Worker readiness always true | Broken worker appears healthy. |

---

## Review Checklist

- Is workload type appropriate?
- Is scaling safe?
- Is idempotency defined?
- Are retries bounded?
- Is shutdown safe?
- Are metrics present?
- Are schedules documented?
- Are migrations controlled?
- Are operator actions audited?
- Is runbook available?

---

## Summary

Runtime architecture includes choosing the right process model.

APIs, workers, jobs, and schedulers each need different operational controls.
