# Final Audit Workflow

Version: 0.1.0  
Default language: zh-CN

This is the canonical workflow for Project Final Audit. All adapters, including Claude Code slash commands, Claude Code skills, Trae prompts, OpenCode prompts, and generic agent prompts, should treat this file as the source of truth.

## Purpose

Run a practical final project audit after a project, release, backend service, API service, microservice, or full-stack app is considered feature-complete.

The workflow covers:

1. Vulnerability analysis.
2. API route and endpoint discovery.
3. API documentation generation or correction.
4. API test planning and execution where authorized.
5. Interface nonconformance detection.
6. Release readiness reporting.
7. Terminal-visible progress reporting.

## Safety Rules

- Perform only authorized defensive review and testing.
- Do not invent results. If a command, API test, scan, or runtime check was not run, say so.
- Mark missing inputs, unavailable services, denied permissions, and unknown credentials as `BLOCKED`.
- Ask for confirmation before:
  - production API testing,
  - destructive or data-mutating tests,
  - high-volume or rate-limit-sensitive tests,
  - external service calls,
  - tests using real user accounts,
  - deleting, overwriting, or changing target project files.
- Avoid destructive security techniques, DoS, mass targeting, supply-chain compromise, or detection evasion.
- Evidence should be minimal and safe. Do not print full secrets, tokens, private keys, or credentials.

## Inputs

Accept these inputs when available:

```yaml
scope: project path, subdirectory, service, or audit boundary
base_url: optional API base URL
auth_notes: optional auth method, token location, or test account notes
environment: local | test | staging | production | unknown
write_outputs: true | false
language: zh-CN by default
allowed_actions:
  - read-only
  - run-tests
  - run-local-server
  - write-reports
  - api-smoke-test
destructive_testing_allowed: false by default
```

If inputs are missing, infer from project files first. Ask only for details that block progress.

## Status Vocabulary

Use these status values consistently:

- `PENDING`: not started.
- `RUNNING`: in progress.
- `PASS`: completed without obvious issues.
- `WARN`: completed with medium/low risk, uncertainty, or manual follow-up.
- `FAIL`: high-risk finding, failed test, or clear nonconformance.
- `BLOCKED`: cannot proceed due to missing input, permission, environment, service, or credentials.

Severity values:

- `Critical`
- `High`
- `Medium`
- `Low`
- `Info`

Issue IDs:

- Security findings: `SEC-001`, `SEC-002`, ...
- API test issues: `API-001`, `API-002`, ...
- Documentation nonconformance: `DOC-001`, `DOC-002`, ...
- Blockers: `BLK-001`, `BLK-002`, ...

## Terminal Progress Format

Update progress after each major phase and whenever an important issue is found.

```text
Audit Progress
[######----] 60%

Stage                         Status      Findings
Project discovery             PASS        18 routes found
Dependency/config audit        WARN        3 vulnerable packages
Source security review        RUNNING     Checking auth boundaries
API documentation             PENDING     -
API testing                   PENDING     -
Final report                  PENDING     -
```

Maintain separate ledgers:

```text
Security Findings
ID       Severity   Status   Evidence                         Summary
SEC-001  High       Open     src/routes/admin.ts:42            Admin endpoint lacks role check

API Test Issues
ID       Severity   Status   Endpoint             Expected       Actual
API-001  High       Open     POST /users          400 invalid     200 created

Documentation Nonconformance
ID       Status   Endpoint       Problem
DOC-001  Open     POST /login    Response schema differs from implementation

Blocked Items
ID       Status    Missing                         Needed
BLK-001  BLOCKED   API base URL, auth token         Provide local start command or test URL
```

## Phase 1: Project Discovery and Scope Confirmation

Inspect the target project for:

- README, AGENTS.md, CLAUDE.md, project docs.
- Manifest files: `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle`, `.csproj`, `composer.json`.
- Lockfiles.
- Test configs.
- CI configs.
- Dockerfile and compose files.
- Route/controller/handler files.
- OpenAPI/Swagger/Postman/Insomnia files.
- Frontend API client calls.

Output:

- Project type.
- Detected stack.
- Audit scope.
- Out-of-scope areas.
- Known blockers.

## Phase 2: Command and Environment Discovery

Find existing commands before inventing new ones.

Record:

```text
Build command:
Test command:
Lint command:
Start command:
Security/audit command:
API base URL:
Authentication notes:
Known limitations:
```

Prefer documented commands from README, AGENTS.md, CLAUDE.md, package scripts, Makefile, CI, or framework conventions.

## Phase 3: Dependency and Configuration Security Audit

Use existing project tools when available. Otherwise perform static checks and state that automated audit was not run.

Check:

- vulnerable/outdated dependencies,
- lockfile consistency,
- suspicious install/build scripts,
- hardcoded secrets,
- `.env` leakage,
- debug mode,
- permissive CORS,
- unsafe cookies,
- exposed stack traces,
- insecure Docker/container settings,
- public management endpoints,
- unsafe CI or deployment settings.

Detailed checklist: `skills/project-final-audit/references/security-checklist.md`.

## Phase 4: Source-Level Vulnerability Analysis

Review code for:

- authentication bypass,
- missing authorization,
- IDOR/resource ownership bugs,
- tenant/org boundary failures,
- SQL/NoSQL/command/template/path injection,
- XSS/CSRF/SSRF/open redirect,
- unsafe upload/download,
- sensitive logs or response leaks,
- unsafe error handling,
- business logic abuse,
- missing rate limits where relevant,
- OWASP Top 10 mapping.

Format each finding:

```text
SEC-XXX | Severity | Status
Title: short title
Location: file:line or endpoint
Evidence: safe minimal evidence
Impact: concrete consequence
Reproduction: safe minimal reproduction or reasoning
Recommendation: specific fix direction
Retest: how to verify after fix
```

## Phase 5: API Route and Endpoint Discovery

Discover endpoints from:

- backend route/controller/handler files,
- framework conventions,
- OpenAPI/Swagger files,
- Postman/Insomnia collections,
- integration/e2e tests,
- frontend fetch/axios/request clients.

Output endpoint inventory:

```text
Method   Path              Auth   Source                  Covered By Test   Documented
GET      /health           No     src/routes/health.ts    Yes               Yes
POST     /users            Yes    src/routes/users.ts     Partial           No
```

## Phase 6: API Documentation Generation or Correction

Generate or update documentation with:

- method and path,
- auth requirement,
- headers,
- path/query/body parameters,
- validation rules,
- request examples,
- response examples,
- status codes,
- error responses,
- pagination/filter/sort rules,
- rate-limit or idempotency notes,
- implementation/documentation mismatches.

Preferred outputs:

- OpenAPI 3.0/3.1 YAML or JSON when enough information exists.
- Markdown API summary when full OpenAPI is not realistic.
- Endpoint coverage matrix in all cases.

## Phase 7: API Test Matrix

Create tests for:

- health checks,
- happy path,
- required parameter missing,
- invalid type/format/range,
- auth missing,
- auth invalid/expired,
- role and permission boundary,
- resource ownership boundary,
- duplicate creation,
- pagination/filtering/sorting,
- upload/download,
- schema conformance,
- status code correctness,
- response body correctness,
- safe error message behavior.

Detailed template: `skills/project-final-audit/references/api-test-matrix-template.md`.

## Phase 8: API Test Execution

Execution priority:

1. Existing test framework and project test command.
2. Existing Postman/Newman, OpenAPI validator, or contract tests.
3. Ecosystem tools already suitable for the project, such as pytest, Jest/Vitest/Supertest, JUnit, Go test, Playwright.
4. Minimal curl/httpie smoke tests only when appropriate and authorized.

Before running API tests, confirm:

- target environment,
- base URL,
- auth method,
- test account/token,
- data mutation impact,
- allowed actions.

For each failed case:

```text
API-XXX | Severity | Status
Endpoint: METHOD /path
Case: missing required field `email`
Expected: 400 with validation error
Actual: 200 and resource created
Evidence: command/test name/response excerpt
Likely cause: validation middleware not applied
Fix: add schema validation and negative test
Retest: rerun specific test case
```

## Phase 9: Classify Nonconformance

Keep categories separate:

- `Security Findings`: vulnerabilities and security risks.
- `API Test Issues`: failed API behavior, status code, schema, or business expectation.
- `Documentation Nonconformance`: missing, stale, or wrong documentation.
- `Blocked Items`: missing data, permissions, environment, auth, or service availability.

## Phase 10: Final Report

Final output must include:

- audit scope and limitations,
- commands/checks performed,
- risk summary by severity,
- security findings,
- API inventory and documentation coverage,
- API test results,
- documentation nonconformance,
- blocked items,
- top remediation priorities,
- retest checklist,
- release readiness recommendation.

Release readiness values:

- `Ready`: no release-blocking issues found.
- `Ready with risks`: known non-blocking risks exist.
- `Not ready`: Critical/High risks, key API failures, auth/authorization bugs, data integrity issues, or major blockers exist.

Detailed report template: `skills/project-final-audit/references/final-audit-report-template.md`.

## Recommended Output Files

When `write_outputs=true` or the user explicitly allows writing reports, create or update:

- `FINAL_AUDIT_REPORT.md`
- `API_DOCUMENTATION.md`
- `openapi.yaml` or `openapi.json`
- `API_TEST_MATRIX.md`
- `API_TEST_RESULTS.md`
- `REMEDIATION_CHECKLIST.md`

Before overwriting existing files, inspect them and confirm if they do not appear generated for this audit.

## Completion Criteria

The workflow is complete when:

- all feasible phases are `PASS`, `WARN`, or `FAIL`,
- all blocked phases have clear `BLOCKED` reasons,
- findings include evidence and remediation,
- API test issues include expected vs actual,
- documentation mismatches are separated from test failures,
- final release readiness is stated plainly.
