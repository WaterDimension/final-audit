# Cross-Agent Interface

Project Final Audit is not Claude-only. Version 0.1.0 exposes a lightweight documentation contract that any coding agent can read.

## Contract Files

```text
interfaces/final-audit.contract.md
interfaces/final-audit.input.schema.json
interfaces/final-audit.output.schema.json
interfaces/mcp-tool.final-audit.schema.json
```

## Calling Pattern

A generic agent should:

1. Read `AGENTS.md`.
2. Read `interfaces/final-audit.contract.md`.
3. Read `workflows/final-audit.md`.
4. Use `interfaces/final-audit.input.schema.json` to understand inputs.
5. Return Markdown sections or structured output matching `interfaces/final-audit.output.schema.json`.

## Example Input

```json
{
  "scope": "/path/to/project",
  "base_url": "http://localhost:3000",
  "auth_notes": "JWT test token available",
  "environment": "local",
  "write_outputs": false,
  "language": "zh-CN",
  "allowed_actions": ["read-only", "run-tests"],
  "destructive_testing_allowed": false
}
```

## Trae and OpenCode

No private Trae or OpenCode configuration format is required in this release. Use the prompt examples:

- `examples/agent-prompts/trae.md`
- `examples/agent-prompts/opencode.md`

The key requirement is that the agent reads the contract and canonical workflow before auditing a target project.

## Future MCP Support

`interfaces/mcp-tool.final-audit.schema.json` documents a future MCP-style tool contract. This repository does not implement an MCP server in version 0.1.0.
