# 最终审计报告模板

用于生成 `FINAL_AUDIT_REPORT.md` 或对话内最终报告。

## 1. Executive Summary

```text
Release Readiness: Ready | Ready with risks | Not ready
Overall Risk: Critical | High | Medium | Low
Audit Date: <date>
Project: <name/path>
Audited Scope: <scope>
```

简要说明：

- 是否建议交付/上线。
- 最大风险是什么。
- 必须先修复哪些问题。
- 哪些检查未覆盖，为什么。

## 2. Scope

### Covered

- 源码路径：
- API 范围：
- 依赖/配置：
- 执行的测试：
- 生成/检查的文档：

### Not Covered / Limitations

- 缺少 base URL：
- 缺少认证凭据：
- 无法启动服务：
- 生产环境限制：
- 用户未授权的破坏性测试：

## 3. Commands and Evidence

```text
Command / Check                         Result      Notes
Project discovery                       PASS        -
Dependency audit                        WARN        3 medium issues
Unit/integration tests                  PASS        -
API smoke tests                         FAIL        2 failed cases
OpenAPI generation/check                WARN        5 undocumented endpoints
```

## 4. Risk Summary

```text
Severity    Count
Critical    0
High        0
Medium      0
Low         0
Info        0
```

## 5. Security Findings

```text
SEC-001 | High | Open
Title: <short title>
Location: <file:line or endpoint>
OWASP: <category if applicable>
Evidence: <minimal evidence>
Impact: <what can happen>
Reproduction: <safe reproduction steps>
Recommendation: <specific fix>
Retest: <how to verify after fix>
```

## 6. API Documentation Coverage

```text
Total endpoints discovered: 0
Documented endpoints: 0
Undocumented endpoints: 0
Documentation mismatches: 0
OpenAPI generated: Yes/No
Markdown API summary generated: Yes/No
```

### Documentation Nonconformance

```text
DOC-001 | Open
Endpoint: METHOD /path
Problem: <missing/mismatch/wrong example>
Implementation Evidence: <source file or response>
Expected Doc Fix: <what to update>
```

## 7. API Test Results

```text
Total test cases: 0
Passed: 0
Failed: 0
Blocked: 0
Not run: 0
```

### API Test Issues

```text
API-001 | High | Open
Endpoint: METHOD /path
Case: <test case>
Expected: <expected behavior>
Actual: <actual behavior>
Evidence: <test output or response excerpt>
Likely Cause: <likely implementation issue>
Recommendation: <fix>
Retest: <specific retest>
```

## 8. Remediation Priorities

按上线风险排序，不按发现时间排序：

1. 修复 Critical/High 安全问题。
2. 修复认证授权绕过、数据越权、敏感信息泄露。
3. 修复会导致 500、数据错误、状态码错误的接口问题。
4. 补齐关键接口文档和测试。
5. 处理 Medium/Low 配置和质量问题。

```text
Priority   Item      Reason                 Owner   Suggested Deadline
P0         SEC-001   Admin auth bypass       -       before release
P1         API-002   invalid input returns 500 -     before release
```

## 9. Retest Checklist

```text
ID       Retest Step                       Expected Result      Status
SEC-001  Rerun admin endpoint auth test    403 for non-admin     PENDING
API-001  Rerun missing email case          400 validation error  PENDING
DOC-001  Compare OpenAPI response schema   schema matches impl   PENDING
```

## 10. Final Recommendation

选择一个：

- `Ready`：没有阻塞发布的问题。
- `Ready with risks`：可发布，但存在已知中低风险或非核心接口问题。
- `Not ready`：存在 Critical/High 风险、关键接口失败、认证授权问题、数据一致性问题或重要测试阻塞。

说明推荐理由和下一步。
