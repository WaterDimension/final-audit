# Contributing

Thank you for improving Project Final Audit.

## Principles

- Keep `workflows/final-audit.md` as the canonical source of truth.
- Keep provider-specific adapters thin.
- Keep safety rules consistent across workflow, README, skill, command, and interface contract.
- Prefer practical, reproducible audit guidance over theoretical lists.
- Do not add real secrets, credentials, tokens, or private endpoints to examples.

## Changing the Workflow

When changing the workflow:

1. Update `workflows/final-audit.md`.
2. Update `interfaces/final-audit.contract.md` if inputs or outputs change.
3. Update JSON schemas when structured fields change.
4. Update examples and docs.
5. Add a `CHANGELOG.md` entry.

## Adapter Rules

- `commands/final-audit.md` should stay a launcher.
- `skills/project-final-audit/SKILL.md` should stay a wrapper plus fallback summary.
- Prompt examples should point to the canonical workflow.

## Security Contributions

Security-related changes must preserve the defensive testing boundary described in `SECURITY.md`.
