# Release Notes

## v0.1.0 - Initial Open-Source Release

Project Final Audit v0.1.0 is the first public release of the final project audit workflow.

### Highlights

- Added a Claude Code-compatible `/final-audit` slash command.
- Added a Claude Code `project-final-audit` skill wrapper.
- Added a provider-neutral canonical workflow at `workflows/final-audit.md`.
- Added a cross-agent interface contract for Trae, OpenCode, and generic coding agents.
- Added JSON schemas for standard input and output shapes.
- Added defensive security checklist, API test matrix template, and final audit report template.
- Added example prompts for generic agents, Trae, and OpenCode.
- Added GitHub-ready documentation, license, security policy, and contribution guide.

### What It Does

This release helps coding agents perform a practical final review before release or delivery:

- vulnerability analysis,
- dependency and configuration review,
- source-level security review,
- API endpoint discovery,
- API documentation generation or correction,
- API test planning and execution where authorized,
- documentation mismatch detection,
- blocker tracking,
- release readiness recommendation.

### Claude Code Usage

After installing or linking the repository as a Claude Code plugin, run:

```text
/final-audit scope=.
```

With more context:

```text
/final-audit scope=backend base_url=http://localhost:3000 auth_notes="JWT test token available" environment=local
```

### Cross-Agent Usage

For Trae, OpenCode, or another coding agent, ask the agent to read:

1. `AGENTS.md`
2. `interfaces/final-audit.contract.md`
3. `workflows/final-audit.md`

Example input:

```yaml
scope: /path/to/project
base_url: http://localhost:3000
auth_notes: optional test auth details
environment: local
write_outputs: false
allowed_actions:
  - read-only
  - run-tests
destructive_testing_allowed: false
```

### Safety

This workflow is for authorized defensive testing only.

It requires confirmation before:

- production testing,
- destructive or data-mutating tests,
- high-volume tests,
- external service calls,
- real-account tests,
- writing or overwriting files.

The workflow must mark missing inputs as `BLOCKED` instead of inventing results.

### Known Limitations

- No standalone CLI in v0.1.0.
- No implemented MCP server in v0.1.0.
- `interfaces/mcp-tool.final-audit.schema.json` is a future MCP tool contract template only.
- Claude Code plugin installation may vary by environment; see `docs/installation.md`.

### Files Added

- `.claude-plugin/plugin.json`
- `commands/final-audit.md`
- `skills/project-final-audit/SKILL.md`
- `workflows/final-audit.md`
- `interfaces/final-audit.contract.md`
- `interfaces/final-audit.input.schema.json`
- `interfaces/final-audit.output.schema.json`
- `docs/*`
- `examples/*`
- `README.md`
- `LICENSE`
- `CONTRIBUTING.md`
- `SECURITY.md`
- `CHANGELOG.md`

### Upgrade Notes

This is the first release, so there are no upgrade steps.
