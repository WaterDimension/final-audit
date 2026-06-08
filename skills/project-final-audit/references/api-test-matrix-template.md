# API 测试矩阵模板

用于项目最终审核阶段生成接口测试计划和记录结果。

## Endpoint Inventory

```text
Method   Path             Auth   Source File           Documented   Test Coverage   Notes
GET      /health          No     src/routes/health     Yes          Yes             -
POST     /users           Yes    src/routes/users      Partial      No              missing negative cases
```

## Test Matrix

```text
ID       Method   Path          Case Type        Expected                  Actual    Status
TC-001   GET      /health       happy path       200 + service status      -         PENDING
TC-002   POST     /users        happy path       201 + user object         -         PENDING
TC-003   POST     /users        missing field    400 validation error      -         PENDING
TC-004   POST     /users        invalid auth     401 unauthorized          -         PENDING
TC-005   GET      /users/{id}   no permission    403 forbidden             -         PENDING
```

## 每个接口至少考虑的测试类型

### 1. 基础可用性

- 服务健康检查。
- happy path。
- 常用查询参数组合。
- 正常分页、筛选、排序。

### 2. 参数校验

- 必填字段缺失。
- 类型错误。
- 格式错误：email、URL、UUID、date、phone 等。
- 长度边界：空字符串、超长字符串。
- 数值边界：负数、0、最大值、超范围。
- 枚举非法值。
- 多余字段是否被拒绝或安全忽略。

### 3. 认证与授权

- 无 token/cookie。
- 无效 token。
- 过期 token。
- token 签名错误。
- 权限不足。
- 访问他人资源。
- 跨租户访问。
- 普通用户访问管理端接口。

### 4. 状态码与响应结构

- 成功状态码是否正确：200/201/204。
- 客户端错误是否为 400/401/403/404/409/422。
- 服务端不应因普通非法输入返回 500。
- 响应 JSON schema 是否与文档一致。
- 错误响应是否包含安全且可理解的信息。

### 5. 数据一致性

- 创建后可查询。
- 更新后字段正确变化。
- 删除后不可查询或状态正确。
- 重复提交是否幂等或返回合理错误。
- 并发或重复请求是否导致重复数据。

### 6. 特殊接口

- 上传：文件类型、大小、空文件、恶意文件名、权限。
- 下载：权限、文件不存在、路径穿越。
- Webhook：签名校验、重放防护、无效 payload。
- 回调/重定向：域名白名单。
- 搜索：注入字符、特殊字符、分页上限。

## 问题记录格式

```text
API-XXX | Severity | Status
Endpoint: METHOD /path
Case: 描述测试用例
Expected: 预期状态码和响应
Actual: 实际状态码和响应摘要
Evidence: 测试命令、测试文件、响应片段
Likely cause: 可能原因
Fix: 修复建议
Retest: 复测命令或步骤
```

## 覆盖率统计

```text
API Test Coverage
Total endpoints: 0
Documented endpoints: 0
Endpoints with tests: 0
Happy path covered: 0
Negative cases covered: 0
Auth boundary covered: 0
Failed cases: 0
Blocked cases: 0
```
