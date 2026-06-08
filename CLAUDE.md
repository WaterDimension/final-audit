# Claude Code Notes

Read `AGENTS.md` first.

This repository provides a Claude Code plugin-compatible final audit workflow:

- Slash command: `commands/final-audit.md` (`/final-audit`)
- Skill: `skills/project-final-audit/SKILL.md`
- Canonical workflow: `workflows/final-audit.md`
- Cross-agent contract: `interfaces/final-audit.contract.md`

When running `/final-audit`, follow `workflows/final-audit.md` as the source of truth. Use Chinese output by default unless the user requests otherwise.

Do not perform production, destructive, high-volume, external-service, real-account, or data-mutating tests without explicit confirmation.
