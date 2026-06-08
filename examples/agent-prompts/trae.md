# Trae Prompt

Use this repository as the Project Final Audit workflow package.

Before auditing the target project, read:

- `AGENTS.md`
- `interfaces/final-audit.contract.md`
- `workflows/final-audit.md`

Then run `final-audit` with:

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

Follow all safety rules. Do not run production, destructive, high-volume, external-service, real-account, or data-mutating tests without explicit confirmation. If information is missing, mark the item as `BLOCKED` instead of guessing.
