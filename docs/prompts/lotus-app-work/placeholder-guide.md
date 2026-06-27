# Placeholder Guide

Use this file before sending any prompt from this pack.

## Standard Placeholders

| Placeholder | Replace With | Example |
|---|---|---|
| `<APP_NAME>` | Lotus app or repo name | `lotus-risk` |
| `<APP_PATH>` | Local absolute repo path | `C:\Users\Sandeep\projects\lotus-risk` |
| `<RFC_ID>` | RFC or work identifier | `RFC-0047` |
| `<RFC_PATH>` | RFC file or folder | `docs/rfcs/RFC-0047-risk-review.md` |
| `<SOURCE_DOCS_PATH>` | External or local docs to review | `C:\Users\Sandeep\Downloads\composite-performance-docs` |
| `<FEATURE_BRANCH>` | Branch name | `feature/rfc-0047-risk-review-hardening` |
| `<SCOPE>` | Short task scope | `risk review API certification` |
| `<ENDPOINT_OR_FAMILY>` | API endpoint or endpoint group | `/api/v1/risk/review` |
| `<TARGET_APPS>` | One or more apps in scope | `lotus-gateway and lotus-workbench` |
| `<EVIDENCE_PATH>` | Evidence output folder | `C:\Users\Sandeep\AppData\Local\Temp\lotus-risk-module-shots` |
| `<PR_NUMBER>` | GitHub PR number | `#123` |
| `<ISSUE_REPO>` | Repo where an issue should be opened | `sgajbi/lotus-gateway` |

## Replacement Rules

1. Replace every placeholder before sending the prompt.
2. Keep paths absolute when the receiving agent may start in another repo.
3. Keep RFC IDs exact, including leading zeros when used by the repo.
4. If there is no RFC, replace `<RFC_ID>` with the feature or issue identifier.
5. If multiple apps are in scope, name each app explicitly and state which app owns which part.
6. If you want planning only, include "Do not implement yet" near the top.
7. If you want implementation, include the branch and validation expectations.

## Quick Examples

Backend RFC implementation:

```text
<APP_NAME> = lotus-risk
<APP_PATH> = C:\Users\Sandeep\projects\lotus-risk
<RFC_ID> = RFC-0047
<RFC_PATH> = docs/rfcs/RFC-0047-risk-review.md
<FEATURE_BRANCH> = feature/rfc-0047-risk-review
```

Workbench UI audit:

```text
<APP_NAME> = lotus-workbench
<APP_PATH> = C:\Users\Sandeep\projects\lotus-workbench
<SCOPE> = portfolio review and DPM command center UI audit
<EVIDENCE_PATH> = C:\Users\Sandeep\AppData\Local\Temp\workbench-ui-audit
```

Gateway and Workbench downstream realization:

```text
<TARGET_APPS> = lotus-gateway and lotus-workbench
<SCOPE> = Manage campaign-definition retire/supersede downstream realization
<FEATURE_BRANCH> = feature/manage-campaign-definition-retire-supersede
```
