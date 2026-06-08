# Verification

Use this checklist after editing the repository.

## Required Files

Confirm these exist:

```text
.claude-plugin/plugin.json
commands/final-audit.md
skills/project-final-audit/SKILL.md
workflows/final-audit.md
interfaces/final-audit.contract.md
interfaces/final-audit.input.schema.json
interfaces/final-audit.output.schema.json
README.md
LICENSE
```

## JSON Validity

Validate:

```text
.claude-plugin/plugin.json
interfaces/final-audit.input.schema.json
interfaces/final-audit.output.schema.json
interfaces/mcp-tool.final-audit.schema.json
```

## Consistency Checks

- Plugin name is `project-final-audit`.
- Slash command is `/final-audit`.
- Canonical workflow is `workflows/final-audit.md`.
- `commands/final-audit.md` points to the canonical workflow.
- `skills/project-final-audit/SKILL.md` points to the canonical workflow.
- Safety rules are consistent across README, workflow, command, skill, and interface contract.
- Adapters are thin and do not duplicate the full workflow.

## Claude Code Smoke Test

After loading the plugin, run:

```text
/final-audit scope=.
```

Expected:

- Chinese output by default.
- Audit Progress board appears.
- Project discovery starts.
- Missing API details are marked `BLOCKED`.
- No invented API test results.

## Generic Agent Smoke Test

Give an agent `examples/agent-prompts/generic-agent.md` and verify it can identify:

- `AGENTS.md`
- `interfaces/final-audit.contract.md`
- `workflows/final-audit.md`

Expected output includes:

- Security Findings
- API Test Issues
- Documentation Nonconformance
- Blocked Items
- Final Recommendation
