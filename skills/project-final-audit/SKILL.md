---
name: project-final-audit
description: This skill should be used when a programmer has finished a project, release, backend service, API service, microservice, or full-stack app and wants a practical final audit covering vulnerability analysis, API documentation generation, API testing, interface nonconformance detection, release readiness review, and clear terminal-visible progress reporting. Trigger for requests like “项目最终审核”, “发布前检查”, “漏洞分析”, “生成接口文档”, “接口测试”, “API 测试”, “上线前审计”, “帮我检查项目能不能交付”, “final audit”, “release readiness review”.
version: 0.1.0
---

# Project Final Audit Skill

Use this skill to run the repository's final project audit workflow.

## Source of Truth

Load and follow the canonical workflow:

- `workflows/final-audit.md`

Use the bundled references when deeper detail is needed:

- `skills/project-final-audit/references/security-checklist.md`
- `skills/project-final-audit/references/api-test-matrix-template.md`
- `skills/project-final-audit/references/final-audit-report-template.md`

## Required Behavior

- Output in Chinese by default unless the user asks otherwise.
- Show terminal-visible progress with `Audit Progress`, `Security Findings`, `API Test Issues`, `Documentation Nonconformance`, and `Blocked Items`.
- Separate security findings, API test failures, documentation mismatches, and blockers.
- Do not invent execution results. If testing cannot run, mark the relevant item as `BLOCKED`.
- Ask before production testing, destructive/data-mutating tests, high-volume tests, external-service calls, real-account tests, or writing output files.
- Perform only authorized defensive review and testing.

## Fallback Summary

If `workflows/final-audit.md` cannot be loaded, run this condensed flow:

1. Discover project type, stack, commands, routes, docs, tests, and blockers.
2. Review dependencies, configs, secrets, auth, authorization, validation, injection risks, uploads, CORS/CSRF/SSRF/XSS, logs, errors, and business logic.
3. Discover API endpoints from route/controller files, OpenAPI/Postman files, tests, and frontend clients.
4. Generate an API inventory and documentation coverage matrix.
5. Build an API test matrix for happy path, negative cases, auth boundaries, permission boundaries, status codes, and response schemas.
6. Run tests only when allowed and sufficiently configured; otherwise mark cases as `BLOCKED`.
7. Produce a final report with evidence, impact, remediation, retest steps, and release readiness: `Ready`, `Ready with risks`, or `Not ready`.
