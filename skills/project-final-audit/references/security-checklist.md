# 漏洞分析清单

用于项目最终审核中的安全检查。按项目实际技术栈裁剪，不要机械套用。

## 1. 依赖与供应链

- 是否存在高危/Critical 依赖漏洞。
- lockfile 是否存在且与 manifest 一致。
- 是否使用废弃、无人维护或来源不明的包。
- 是否有 postinstall、build script、动态下载二进制等供应链风险。
- 容器基础镜像是否过旧。
- CI 是否固定版本，是否使用不可信 action/plugin。

常见工具：

- Node.js: `npm audit`, `pnpm audit`, `yarn audit`
- Python: `pip-audit`, `safety`, `uv pip audit`（如可用）
- Go: `govulncheck`, `go list -m -u all`
- Java: Maven/Gradle dependency check, OWASP Dependency-Check（如项目已有）
- Rust: `cargo audit`（如项目已有）

## 2. 密钥与敏感信息

- `.env`、配置文件、测试文件、日志、前端构建产物中是否存在 token、密码、API key、私钥。
- 是否提交了云服务凭据、数据库连接串、JWT secret、Webhook secret。
- 是否有默认密码、弱密码、硬编码管理员账号。
- 错误响应或日志是否暴露敏感字段。

证据记录时只展示最小必要片段，避免完整泄露密钥。

## 3. 认证

- 登录、刷新 token、注销流程是否完整。
- JWT 是否校验签名、过期时间、issuer/audience、算法。
- session/cookie 是否设置 HttpOnly、Secure、SameSite。
- 密码是否安全哈希：bcrypt/argon2/scrypt；禁止明文或快速 hash。
- 是否存在认证中间件漏挂、公开接口误暴露。

## 4. 授权与越权

- 管理员接口是否检查角色。
- 用户资源是否检查 owner/resource scope。
- IDOR：通过修改 ID 是否能访问他人数据。
- 多租户项目是否检查 tenant/org boundary。
- 前端隐藏按钮不能替代后端授权。

## 5. 输入校验与注入

- SQL/NoSQL 查询是否参数化。
- shell command 是否拼接用户输入。
- 模板渲染是否可能注入。
- 文件路径是否存在 path traversal。
- 正则是否可能 ReDoS。
- JSON/body/query/path 参数是否校验类型、长度、范围、枚举。

## 6. Web 常见风险

- XSS：输出是否转义，富文本是否消毒。
- CSRF：cookie/session 认证接口是否需要 CSRF 防护。
- SSRF：URL 抓取、webhook、图片代理是否限制内网地址。
- CORS：是否允许 `*` + credentials，是否过度信任 origin。
- Open Redirect：redirect 参数是否限制域名。
- Clickjacking：是否设置 frame 相关 header。

## 7. 文件上传与下载

- 上传是否校验大小、类型、扩展名、MIME、内容。
- 文件名是否安全处理，是否可能覆盖或路径穿越。
- 上传目录是否可执行。
- 下载接口是否校验权限。
- 公开 URL 是否泄露私有文件。

## 8. 错误处理与日志

- 生产环境是否暴露 stack trace。
- 错误响应是否暴露 SQL、内部路径、环境变量。
- 日志是否记录密码、token、身份证、手机号、银行卡等敏感信息。
- 是否有审计日志记录关键操作。

## 9. 配置与部署

- debug/dev mode 是否在生产打开。
- TLS/HTTPS 是否启用。
- 安全响应头是否合理。
- 数据库、Redis、管理面板是否暴露公网。
- Docker 是否 root 运行，是否挂载敏感目录。
- 环境变量示例是否完整但不包含真实秘密。

## 10. 业务逻辑

- 金额、库存、积分、优惠券、权限、状态流转是否只信任后端计算。
- 是否防重复提交、重复支付、重复领取。
- 是否检查订单/资源状态机合法性。
- 是否有速率限制或验证码等滥用防护。

## 11. OWASP Top 10 映射

对重要发现标注对应类别：

- A01 Broken Access Control
- A02 Cryptographic Failures
- A03 Injection
- A04 Insecure Design
- A05 Security Misconfiguration
- A06 Vulnerable and Outdated Components
- A07 Identification and Authentication Failures
- A08 Software and Data Integrity Failures
- A09 Security Logging and Monitoring Failures
- A10 Server-Side Request Forgery
