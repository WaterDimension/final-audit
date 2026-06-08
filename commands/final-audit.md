---
name: final-audit
description: Run the canonical final project audit workflow for release readiness, vulnerability analysis, API documentation, API testing, and interface nonconformance reporting.
argument-hint: "[scope/base-url/auth-notes]"
---

Run the canonical Project Final Audit workflow.

User arguments: `$ARGUMENTS`

## Instructions

- Use Chinese output by default unless the user explicitly requests another language.
- Treat `workflows/final-audit.md` as the source of truth.
- If the `project-final-audit` skill is available, use it; otherwise follow `workflows/final-audit.md` directly.
- Keep visible progress updated with the workflow's `Audit Progress` board and issue ledgers.
- Do not invent vulnerability, documentation, or API test results.
- Mark missing base URL, auth, start command, service availability, or permissions as `BLOCKED`.
- Ask for confirmation before production testing, destructive/data-mutating tests, high-volume tests, external service calls, real-account tests, or writing output files.

## Initial Output

Start by printing:

```text
Audit Progress
[----------] 0%

Stage                         Status      Findings
Project discovery             RUNNING     Starting final audit
Dependency/config audit        PENDING     -
Source security review        PENDING     -
API documentation             PENDING     -
API testing                   PENDING     -
Final report                  PENDING     -
```

Then execute the workflow at `workflows/final-audit.md` using `$ARGUMENTS` as the initial input context.
