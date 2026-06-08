# Generic Agent Prompt

Use the Project Final Audit workflow.

Read these files first:

1. `AGENTS.md`
2. `interfaces/final-audit.contract.md`
3. `workflows/final-audit.md`

Input:

```yaml
scope: <target project path>
base_url: <optional API base URL>
auth_notes: <optional auth notes>
environment: unknown
write_outputs: false
language: zh-CN
allowed_actions:
  - read-only
destructive_testing_allowed: false
```

Run the final audit workflow. Show progress. Do not invent results. Mark missing inputs as `BLOCKED`. Separate Security Findings, API Test Issues, Documentation Nonconformance, and Blocked Items.
