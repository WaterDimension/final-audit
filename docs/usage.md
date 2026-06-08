# Usage

## Claude Code

Run:

```text
/final-audit scope=.
```

Recommended arguments:

```text
/final-audit scope=backend base_url=http://localhost:3000 auth_notes="test user exists" environment=local
```

The command will follow `workflows/final-audit.md` and show a live progress board.

## Generic Agent

Ask the agent to use the contract:

```text
Read AGENTS.md, interfaces/final-audit.contract.md, and workflows/final-audit.md.
Run final-audit for scope=<target-project>.
```

## Common Inputs

```yaml
scope: .
base_url: http://localhost:3000
auth_notes: JWT test token is available in .env.test
environment: local
write_outputs: false
language: zh-CN
allowed_actions:
  - read-only
  - run-tests
destructive_testing_allowed: false
```

## Output Files

If `write_outputs=true` or the user explicitly allows writing files, the workflow may create:

- `FINAL_AUDIT_REPORT.md`
- `API_DOCUMENTATION.md`
- `openapi.yaml` or `openapi.json`
- `API_TEST_MATRIX.md`
- `API_TEST_RESULTS.md`
- `REMEDIATION_CHECKLIST.md`

Before overwriting existing files, the agent should inspect them and ask if needed.

## Blocked Work

If runtime information is missing, the workflow should not guess. It should output a blocker such as:

```text
[BLOCKED] API testing cannot start
Missing: API base URL, auth token
Needed from user: provide local start command or test URL + credentials
```
