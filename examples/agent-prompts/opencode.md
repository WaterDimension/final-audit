# OpenCode Prompt

Use Project Final Audit as a provider-neutral workflow.

Load:

1. `AGENTS.md`
2. `interfaces/final-audit.contract.md`
3. `workflows/final-audit.md`

Audit target:

```yaml
scope: <target project path>
base_url: <optional>
auth_notes: <optional>
environment: unknown
write_outputs: false
language: zh-CN
allowed_actions:
  - read-only
destructive_testing_allowed: false
```

Expected output:

- Audit Progress
- Security Findings
- API Inventory
- API Test Matrix
- API Test Results
- Documentation Nonconformance
- Blocked Items
- Release Readiness
- Final Recommendation

Do not invent execution results. Ask before production, destructive, high-volume, external-service, real-account, or data-mutating tests.
