# Kubernetes, OpenShift, and AKS Fundamentals

## Purpose

This file explains Kubernetes-style runtime concepts for application engineers and architects.

The goal is not to become a cluster administrator. The goal is to understand how applications run on container orchestration platforms and what engineers must design for.

---

## Core Concepts

| Concept | Meaning |
|---|---|
| Cluster | The platform running workloads. |
| Node | Machine or VM that runs pods. |
| Pod | Smallest deployable runtime unit. |
| Container | Process packaged in an image. |
| Deployment | Manages replicated pods and rollout. |
| ReplicaSet | Maintains pod replica count. |
| Service | Stable network identity for pods. |
| Ingress | HTTP entrypoint and routing. |
| Namespace | Logical boundary for resources. |
| ConfigMap | Non-sensitive configuration. |
| Secret | Sensitive configuration object. |
| ServiceAccount | Runtime identity for workload. |
| Job | One-time task. |
| CronJob | Scheduled task. |
| HPA | Horizontal pod autoscaler. |
| PVC | Persistent volume claim. |
| NetworkPolicy | Network access rules. |

---

## OpenShift and AKS

Kubernetes is the underlying orchestration model.

OpenShift adds enterprise platform features such as:

- stricter security defaults
- routes
- integrated registry/build options
- project abstractions
- enterprise governance patterns

AKS is Azure-managed Kubernetes, commonly integrated with:

- Azure networking
- Azure identity
- Azure Key Vault
- Azure Monitor
- private endpoints
- managed node pools
- Azure Policy

Concepts are similar, but platform controls differ.

---

## What Application Teams Must Understand

Application teams should understand:

- pod lifecycle
- readiness and liveness probes
- resource requests and limits
- environment variables
- secrets
- service discovery
- ingress routing
- rolling deployments
- config updates
- logs and metrics
- graceful shutdown
- horizontal scaling
- dependency connectivity

---

## Kubernetes Is Not Magic

Kubernetes will not automatically fix:

- bad readiness checks
- memory leaks
- missing timeouts
- broken migrations
- unsafe retries
- unbounded queues
- secrets in images
- poor logs
- no runbooks
- wrong resource limits
- bad application design

It automates running workloads. Applications still need engineering discipline.

---

## Kubernetes Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No readiness probe | Traffic sent before app ready. |
| Liveness probe too aggressive | Healthy pods restarted. |
| No resource requests | Scheduling instability. |
| No limits | Noisy-neighbor problems. |
| Secrets hardcoded | Security incident. |
| Stateful workload without backup plan | Data loss. |
| One namespace for everything | Weak isolation. |
| App ignores shutdown signals | Lost in-flight work. |

---

## Review Checklist

- Is workload type correct?
- Are probes defined?
- Are resources defined?
- Are secrets externalized?
- Is service identity clear?
- Is ingress required?
- Are network rules understood?
- Is shutdown safe?
- Is scaling safe?
- Are logs/metrics integrated?
- Are platform-specific constraints documented?

---

## Summary

Kubernetes provides the runtime control plane. Application teams must still design services to start, stop, scale, fail, recover, and explain themselves correctly.
