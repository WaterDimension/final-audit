# Maintainer Guide

## Architecture

Project Final Audit uses one canonical workflow and multiple thin adapters.

Canonical source:

- `workflows/final-audit.md`

Adapters:

- `commands/final-audit.md`
- `skills/project-final-audit/SKILL.md`
- `examples/agent-prompts/*.md`

Interfaces:

- `interfaces/final-audit.contract.md`
- `interfaces/final-audit.input.schema.json`
- `interfaces/final-audit.output.schema.json`

## Updating the Workflow

When changing audit behavior:

1. Update `workflows/final-audit.md`.
2. Update references under `skills/project-final-audit/references/` if needed.
3. Update contract and schemas if inputs/outputs changed.
4. Update docs and examples.
5. Update `CHANGELOG.md`.
6. Verify JSON files and structure.

## Versioning

When releasing a new version, update:

- `.claude-plugin/plugin.json`
- `skills/project-final-audit/SKILL.md`
- `CHANGELOG.md`

## Adapter Discipline

Do not copy the full workflow into adapters. Adapters should point to `workflows/final-audit.md` and include only tool-specific launch instructions.
