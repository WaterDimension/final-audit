# Claude Code Plugin

This repository is structured as a Claude Code plugin.

## Files

```text
.claude-plugin/plugin.json          Plugin manifest
commands/final-audit.md             Slash command: /final-audit
skills/project-final-audit/SKILL.md Skill wrapper
workflows/final-audit.md            Canonical workflow
```

## Slash Command

After the plugin is loaded, run:

```text
/final-audit scope=.
```

The slash command is intentionally thin. It points Claude Code to `workflows/final-audit.md` and passes `$ARGUMENTS` as context.

## Skill

The skill is defined at:

```text
skills/project-final-audit/SKILL.md
```

Claude Code can use the skill automatically when the user asks for final audit, release readiness review, vulnerability analysis, API documentation generation, or API testing.

## Expected Behavior

- Chinese output by default.
- Visible `Audit Progress` board.
- Separate Security Findings, API Test Issues, Documentation Nonconformance, and Blocked Items.
- No invented test results.
- Confirmation before production, destructive, high-volume, external-service, or data-mutating tests.
