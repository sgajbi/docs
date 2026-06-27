# 17 - Security Worked Examples And Implementation Patterns

Security controls become easier to implement when they are expressed as concrete engineering behavior, tests and evidence.

## Example 1. Portfolio API Object-Level Denial

### Scenario

An advisor has a valid token and advisor role but is not assigned to the requested portfolio.

### Expected API behavior

```text
http_status: 403
error_type: access-denied
reason_code: NO_ACTIVE_CLIENT_RELATIONSHIP
correlation_id: present
response_contains_client_data: false
audit_event: portfolio_read_denied
```

### Required tests

| Test | Expected result |
|---|---|
| Advisor role without relationship requests portfolio | 403 with safe error. |
| Same advisor requests assigned portfolio | 200 with allowed fields only. |
| Denied response log scan | No client name, account number or raw portfolio id. |
| Audit event validation | Actor, resource type, action, decision and reason code present. |

## Example 2. Report Export Control

### Scenario

A monthly statement is generated for a client and exported through a portal.

### Required controls

1. entitlement check at generation,
2. immutable snapshot id,
3. encrypted document storage,
4. entitlement recheck at download,
5. watermark or recipient binding,
6. export audit event,
7. rate limit or anomaly signal for bulk download,
8. revocation handling.

### Failure behavior

If entitlement is revoked after generation but before download, the document remains stored but download is denied and audited.

## Example 3. Security Release Gate

### Scenario

A change adds a new endpoint that creates proposal drafts using portfolio holdings and product eligibility data.

### Gate requirements

| Control | Evidence |
|---|---|
| Threat model delta | New endpoint, actors, resources and misuse cases captured. |
| API authorization tests | No-token, wrong-role and wrong-portfolio tests pass. |
| Suitability and product policy | Ineligible product and unsupported client segment blocked. |
| Sensitive telemetry | Logs and metrics contain no raw holdings or client identifiers. |
| Dependency scan | No unapproved critical dependency exposure. |
| Runbook | Support knows denial, degraded and dependency-failure states. |

## Example 4. Suspicious Download Detection

### Scenario

A user downloads a high volume of client reports outside their normal pattern.

### Detection design

| Dimension | Signal |
|---|---|
| Actor | User role, team, privileged status and normal behavior. |
| Resource | Report type, client count, booking centre and sensitivity. |
| Action | Download count, export format and time window. |
| Context | Device, network, location, time and case reference. |
| Decision | Alert, challenge, block or case creation. |

### Review question

Would the detection distinguish legitimate quarterly reporting activity from unusual targeted exfiltration? If not, improve context and thresholds.

## Example 5. AI Retrieval Boundary

### Scenario

An assistant answers advisor questions using portfolio data, product notes and prior report commentary.

### Required tests

| Test | Expected result |
|---|---|
| Advisor asks for non-assigned client | Retrieval returns no client context and answer is denied. |
| Prompt-injection text appears in a source document | Instruction is ignored or source is flagged. |
| Source is stale | Answer is degraded or asks for refreshed data. |
| Tool asks to submit order | Tool call is unavailable or blocked pending governed workflow. |
| Logs are scanned | Raw prompt and retrieved client details are absent. |

## Example 6. Privileged Access Review

### Scenario

An operations user needs temporary access to correct a production reference-data issue.

### Correct flow

1. request privileged access with business reason,
2. approve access with time limit,
3. grant only the required role and environment,
4. record session or action evidence,
5. require maker-checker for the correction,
6. revoke access automatically after expiry,
7. review the correction and access evidence,
8. add a regression or control test if the correction exposed a process gap.

### QA assertions

| Test | Expected result |
|---|---|
| User attempts privileged correction without approved access | Blocked and audited. |
| Same user attempts maker and checker approval | Checker action denied. |
| Access expires before correction is submitted | Submit action blocked. |
| Evidence pack is generated | Contains bounded action details, no secrets and no raw client identifiers. |

## Review Checklist

1. Does the example prove both allowed and denied behavior?
2. Are reason codes bounded and supportable?
3. Is sensitive data absent from logs and evidence?
4. Are tests close to the boundary where authority is used?
5. Is the operational response clear when the control blocks an action?
