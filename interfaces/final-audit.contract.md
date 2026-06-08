# final-audit Interface Contract

Version: 0.1.0

This contract lets any coding agent call the Project Final Audit workflow without needing Claude Code-specific skill support.

## Interface Name

`final-audit`

## Purpose

Run a final project audit covering release readiness, vulnerability analysis, API documentation generation/checking, API testing, interface nonconformance detection, blockers, remediation priorities, and retest guidance.

## Canonical Workflow

Agents must read and follow:

- `workflows/final-audit.md`

Supporting references:

- `skills/project-final-audit/references/security-checklist.md`
- `skills/project-final-audit/references/api-test-matrix-template.md`
- `skills/project-final-audit/references/final-audit-report-template.md`

## Inputs

| Field | Required | Type | Default | Description |
|---|---:|---|---|---|
| `scope` | Yes | string | - | Project path, subdirectory, service name, or audit boundary. |
| `base_url` | No | string | empty | API base URL for runtime/API tests. |
| `auth_notes` | No | string | empty | Authentication method, token location, test account notes, or login instructions. |
| `environment` | No | enum | `unknown` | `local`, `test`, `staging`, `production`, or `unknown`. |
| `write_outputs` | No | boolean | `false` | Whether the agent may write report/documentation files. |
| `language` | No | string | `zh-CN` | Preferred output language. |
| `allowed_actions` | No | string[] | `["read-only"]` | Allowed action categories. |
| `destructive_testing_allowed` | No | boolean | `false` | Must be explicitly true before destructive/data-mutating tests. |

Allowed action values:

- `read-only`
- `run-tests`
- `run-local-server`
- `write-reports`
- `api-smoke-test`

## Output Sections

Agents should return Markdown by default and may additionally emit JSON matching `final-audit.output.schema.json`.

Required sections:

1. `progress_board`
2. `security_findings`
3. `api_inventory`
4. `api_test_matrix`
5. `api_test_results`
6. `documentation_nonconformance`
7. `blocked_items`
8. `release_readiness`
9. `final_recommendation`

## Safety Rules

- Perform only authorized defensive review and testing.
- Do not invent results. State exactly what was and was not run.
- Mark missing base URL, auth, startup command, service availability, or permission as `BLOCKED`.
- Ask for confirmation before production testing, destructive/data-mutating tests, high-volume tests, external-service calls, real-account tests, or writing output files.
- Do not expose full secrets or credentials in evidence.
- Do not perform DoS, mass targeting, destructive exploitation, supply-chain compromise, or detection evasion.

## Minimal Generic Invocation Prompt

```text
Use the final-audit interface.
Read AGENTS.md, interfaces/final-audit.contract.md, and workflows/final-audit.md.

Input:
scope: <target project path>
base_url: <optional>
auth_notes: <optional>
environment: unknown
write_outputs: false
allowed_actions: ["read-only"]
destructive_testing_allowed: false

Run the workflow. Do not invent results. Mark missing inputs as BLOCKED.
```
