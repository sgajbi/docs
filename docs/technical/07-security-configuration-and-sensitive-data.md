# 07 - Security, Configuration And Sensitive Data

Security is part of engineering quality. In regulated wealth and banking platforms, sensitive-data handling, least privilege, secure workflow design and evidence filtering must be built into normal development.

## Secure Configuration

Configuration should be:

1. environment-specific,
2. typed,
3. validated at startup,
4. documented,
5. read through a central settings layer,
6. redacted in logs and diagnostics.

Avoid:

1. direct environment reads in business logic,
2. hardcoded hostnames or secrets,
3. default credentials,
4. logging full config objects,
5. local-machine paths in docs or artifacts.

## Secrets Handling

Secrets include:

1. API keys,
2. database passwords,
3. tokens,
4. private keys,
5. certificates,
6. connection strings,
7. cloud credentials.

Rules:

1. never commit secrets,
2. never put secrets in examples,
3. never log secrets,
4. never expose secrets in generated evidence,
5. rotate secrets after suspected exposure,
6. prefer managed secret stores in production,
7. keep local `.env` files out of version control.

## Authorization And Entitlements

Access control should model:

1. actor,
2. resource,
3. action,
4. scope,
5. policy decision,
6. audit event.

Example:

```text
actor: advisor_user
resource: portfolio_report
action: read
scope: entitled_portfolios_only
decision: allowed
audit_event: report_read_allowed
```

Do not let UI-only checks be the final authorization control. Backend APIs must enforce access decisions.

## Sensitive Observability Controls

Logs, metrics and traces must avoid sensitive labels and payloads.

Forbidden metric labels:

1. client name,
2. account id,
3. portfolio id,
4. raw entitlement reason,
5. document id,
6. prompt text,
7. model output,
8. trace id,
9. request body.

Use bounded values:

```text
operation=portfolio_summary
state=degraded
reason=source_stale
status_class=5xx
```

## CI Security Controls

Common controls:

1. dependency vulnerability audit,
2. source security scan,
3. secret scanning,
4. container image scan,
5. workflow permission audit,
6. OpenAPI and contract checks,
7. sensitive-observability guard,
8. generated-artifact leakage checks.

## Workflow Security

GitHub or CI workflows should:

1. use least-privilege permissions,
2. pin action major versions or approved versions,
3. avoid unsafe privilege elevation,
4. avoid secrets in pull-request contexts from untrusted forks,
5. keep deployment credentials separate from test credentials,
6. avoid soft-failing critical security checks.

## Evidence Filtering

Evidence artifacts should be useful but safe.

Before sharing evidence, remove:

1. local paths,
2. internal-only source file paths when not needed,
3. raw payloads,
4. client/account/portfolio identifiers unless explicitly synthetic and approved,
5. credentials,
6. provider tokens,
7. verbose stack traces,
8. raw telemetry dumps.

## Security Review Questions

1. What data classification applies?
2. Who can access the endpoint or artifact?
3. Are secrets externalized and redacted?
4. Are logs and metrics source-safe?
5. Does CI block known security findings?
6. Is permission denial product-safe?
7. Are generated artifacts safe to retain?
