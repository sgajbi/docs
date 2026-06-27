<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 12 - Source Notes and Further Reading

## Official / authoritative references used

These sources are useful for ongoing study and future updates.

| Source | Why useful |
|---|---|
| Kubernetes Documentation - Concepts | Core Kubernetes concepts: objects, workloads, services, storage, configuration, security, policies, scheduling and observability. |
| Microsoft Learn - AKS Baseline Architecture | Production-oriented AKS architecture guidance, including networking, identity, security, observability and operational recommendations. |
| Red Hat OpenShift Container Platform Documentation | OpenShift platform documentation, including architecture, workloads, operators, networking, security and operations. |
| NIST SP 800-190 Application Container Security Guide | Container security risks and controls across images, registries, orchestrators, containers and host OS layers. |
| OpenTelemetry Documentation | Telemetry standards for traces, metrics and logs. |
| OWASP Kubernetes / Cloud Native security resources | Practical security controls and common risks for cloud-native applications. |
| CIS Kubernetes Benchmark | Kubernetes hardening and configuration benchmark. |
| SLSA Supply Chain Levels for Software Artifacts | Software supply-chain integrity model. |
| CNCF Cloud Native Landscape | Ecosystem reference for Kubernetes/cloud-native tooling. |

## Suggested official links

- Kubernetes Concepts: https://kubernetes.io/docs/concepts/
- Kubernetes Security: https://kubernetes.io/docs/concepts/security/
- Kubernetes Workloads: https://kubernetes.io/docs/concepts/workloads/
- Kubernetes Services and Networking: https://kubernetes.io/docs/concepts/services-networking/
- AKS Baseline Architecture: https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks/baseline-aks
- Azure Kubernetes Service Documentation: https://learn.microsoft.com/en-us/azure/aks/
- OpenShift Container Platform Documentation: https://docs.redhat.com/en/documentation/openshift_container_platform/
- NIST SP 800-190: https://csrc.nist.gov/pubs/sp/800/190/final
- OpenTelemetry: https://opentelemetry.io/docs/
- SLSA: https://slsa.dev/
- OWASP Kubernetes Top 10: https://owasp.org/www-project-kubernetes-top-ten/
- CIS Benchmarks: https://www.cisecurity.org/cis-benchmarks

## Notes on usage

This pack intentionally focuses on enterprise cloud/Kubernetes architecture and operating practices for wealth systems. It is not a replacement for official platform documentation or internal bank standards.

When applying this pack in a real project:

- defer to internal platform standards
- validate against the target cloud/provider implementation
- document environment-specific exceptions
- use ADRs for significant decisions
- keep security and data-risk teams involved
- test DR and restore rather than assuming it works
- prove controls through CI/CD and runtime evidence

## Knowledge-base enrichment ideas

Future addenda could cover:

- AKS-specific landing-zone checklist
- OpenShift-specific namespace/project design
- Kubernetes manifests template library
- Helm/Kustomize/GitOps examples
- production-readiness checklist template
- SRE runbook template
- disaster-recovery drill template
- AI workload Kubernetes deployment pattern
- portfolio analytics batch scaling examples
- cost-management dashboard examples
