# Agent Instructions

This repository contains Project Final Audit, a reusable final-audit workflow for coding agents.

## Source of Truth

The canonical workflow is:

- `workflows/final-audit.md`

All provider-specific adapters must remain thin wrappers over that file. Do not create a second divergent copy of the audit workflow in commands, skills, prompts, or docs.

## How Agents Should Use This Repository

1. Read this file.
2. Read `interfaces/final-audit.contract.md` for inputs, outputs, and safety rules.
3. Read `workflows/final-audit.md` and follow it as the execution procedure.
4. Use reference templates under `skills/project-final-audit/references/` when detailed security, API test, or report structure is needed.
5. Return visible progress and separate findings into Security Findings, API Test Issues, Documentation Nonconformance, and Blocked Items.

## Safety Rules

- Perform only authorized defensive review and testing.
- Do not invent results.
- Mark missing environment, base URL, auth, permissions, service availability, or commands as `BLOCKED`.
- Ask before production testing, destructive/data-mutating tests, high-volume tests, external service calls, real-account tests, or writing files.
- Do not expose complete secrets or credentials in evidence.

## Adapter Rules

- `commands/final-audit.md` is the Claude Code slash command adapter.
- `skills/project-final-audit/SKILL.md` is the Claude Code skill adapter.
- `examples/agent-prompts/` contains prompt adapters for generic agents, Trae, and OpenCode.
- Keep adapters short and point them to `workflows/final-audit.md`.
