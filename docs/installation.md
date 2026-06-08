# Installation

Project Final Audit can be used in two ways:

1. As a Claude Code plugin with `/final-audit`.
2. As a provider-neutral Markdown workflow for other coding agents.

## Claude Code Plugin Installation

Clone or copy this repository to a location Claude Code can load as a plugin.

The plugin root must contain:

```text
.claude-plugin/plugin.json
commands/final-audit.md
skills/project-final-audit/SKILL.md
workflows/final-audit.md
```

After installing or linking the plugin, restart or refresh Claude Code and run:

```text
/final-audit scope=.
```

With API context:

```text
/final-audit scope=backend base_url=http://localhost:3000 auth_notes="JWT test token available" environment=local
```

If your Claude Code environment uses a plugin manager, install this repository according to that environment's plugin workflow.

## Generic Agent Installation

For Trae, OpenCode, or any other coding agent:

1. Clone this repository.
2. Tell the agent to read:
   - `AGENTS.md`
   - `interfaces/final-audit.contract.md`
   - `workflows/final-audit.md`
3. Provide the target project path through `scope`.

Example:

```text
Use the Project Final Audit workflow from this repository.
Read AGENTS.md, interfaces/final-audit.contract.md, and workflows/final-audit.md.
Run final-audit with scope=/path/to/my/project, environment=local, write_outputs=false.
```

## No CLI or MCP Server in 0.1.0

Version 0.1.0 exposes a lightweight documentation interface: Markdown contract, JSON schemas, and prompt examples.

It does not ship a real CLI or MCP server yet. `interfaces/mcp-tool.final-audit.schema.json` is a future tool contract template only.
