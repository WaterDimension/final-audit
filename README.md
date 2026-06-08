# Project Final Audit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-0.1.0-blue.svg)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-7C3AED.svg)](docs/claude-code-plugin.md)
[![Slash Command](https://img.shields.io/badge/slash%20command-%2Ffinal--audit-0EA5E9.svg)](commands/final-audit.md)
[![Agent Workflow](https://img.shields.io/badge/agent--workflow-cross--agent-10B981.svg)](interfaces/final-audit.contract.md)
[![Security](https://img.shields.io/badge/security-defensive%20only-red.svg)](SECURITY.md)

Project Final Audit 是一个开源的项目最终审核工作流，主要用于项目交付、上线或发布前的最后检查。

它不是一个独立扫描器，而是一套给编码 Agent 使用的审核流程。Claude Code、Trae、OpenCode 或其他 Agent 可以根据这套流程检查项目、发现风险、整理接口文档、执行接口测试，并输出清晰的最终审核报告。

默认输出语言是中文（`zh-CN`），也可以按用户要求切换为其他语言。

## Features

Project Final Audit 主要提供以下能力：

- **一键最终审核**：在 Claude Code 中安装后，可以直接使用 `/final-audit` 触发完整审核流程。
- **漏洞分析**：检查依赖、配置、密钥泄露、认证授权、输入校验、注入风险、上传下载、日志泄露等问题。
- **接口文档生成**：从路由、控制器、已有 OpenAPI / Swagger、Postman 集合、测试用例或前端调用中整理接口文档。
- **接口测试**：生成接口测试矩阵，并在获得授权和环境信息后执行测试；无法执行时会明确标记为 `BLOCKED`。
- **问题可视化**：在终端中持续展示审核进度、漏洞台账、接口测试问题、文档不一致项和阻塞项。
- **跨 Agent 调用**：提供 Markdown 调用契约和 JSON Schema，方便 Trae、OpenCode 或其他通用编码 Agent 复用同一套流程。
- **安全优先**：不会编造结果；生产环境测试、破坏性测试、外部服务调用或写文件前必须先确认。

## Quick Start: Claude Code

这个仓库已经按照 Claude Code 插件结构组织。安装或链接为 Claude Code 插件后，可以直接运行：

```text
/final-audit scope=.
```

如果你已经知道本地接口地址、认证方式或审核范围，可以把这些信息一起传入：

```text
/final-audit scope=backend base_url=http://localhost:3000 auth_notes="JWT test token is available in .env.test" environment=local
```

相关入口文件：

- `/final-audit` 命令：`commands/final-audit.md`
- 自动触发 skill：`skills/project-final-audit/SKILL.md`
- 规范审核流程：`workflows/final-audit.md`

## Installation

### Option 1: Clone for local development

如果你只是想本地查看、修改或二次开发，可以直接克隆仓库：

```bash
git clone https://github.com/<OWNER>/<REPO>.git
cd <REPO>
```

发布到 GitHub 后，把 `<OWNER>/<REPO>` 替换成你的真实仓库地址。

### Option 2: Use as a Claude Code plugin

如果你想在 Claude Code 中使用 `/final-audit`，需要把这个仓库放到 Claude Code 可以加载插件的位置。

插件根目录需要包含这些文件：

```text
.claude-plugin/plugin.json             插件清单
commands/final-audit.md                /final-audit 调用入口
skills/project-final-audit/SKILL.md    Claude Code skill 入口
workflows/final-audit.md               最终审核的规范流程
```

放好后，重启或刷新 Claude Code，然后运行：

```text
/final-audit scope=.
```

### Option 3: Use with Trae, OpenCode, or another coding agent

如果你使用的是 Trae、OpenCode 或其他编码 Agent，不需要依赖 Claude Code 的插件机制。让 Agent 先读取下面三个文件即可：

1. `AGENTS.md`
2. `interfaces/final-audit.contract.md`
3. `workflows/final-audit.md`

然后把审核参数传给 Agent，例如：

```yaml
scope: /path/to/project
base_url: http://localhost:3000
auth_notes: test account or token notes
environment: local
write_outputs: false
allowed_actions:
  - read-only
  - run-tests
destructive_testing_allowed: false
```

可直接复制使用的示例提示词在这里：

- `examples/agent-prompts/generic-agent.md`
- `examples/agent-prompts/trae.md`
- `examples/agent-prompts/opencode.md`

## Usage Examples

### Local read-only audit

只做本地只读审核，不运行测试、不写文件：

```text
/final-audit scope=. environment=local allowed_actions=read-only
```

### Backend API audit with local API server

审核后端 API，并允许在本地环境执行测试和少量接口冒烟测试：

```text
/final-audit scope=backend base_url=http://localhost:3000 environment=local allowed_actions="read-only,run-tests,api-smoke-test"
```

### Generate reports after confirmation

允许生成报告文件：

```text
/final-audit scope=. write_outputs=true allowed_actions="read-only,write-reports"
```

如果目标项目里已经存在同名报告文件，工作流应该先检查文件内容，并在必要时请求用户确认，避免误覆盖。

## Source of Truth

本项目的规范工作流是：

- `workflows/final-audit.md`

所有命令、skill、提示词和文档都应该指向这个文件。维护时也应优先修改这个文件，避免多个适配器各自复制一份流程，最后产生不一致。

## Expected Output

一次完整审核通常会包含以下内容：

- `Audit Progress`：当前审核进度和阶段状态。
- `Security Findings`：安全漏洞或安全风险。
- `API Inventory`：发现的接口清单。
- `API Test Matrix`：接口测试矩阵。
- `API Test Results`：接口测试结果。
- `Documentation Nonconformance`：接口文档与实现不一致的问题。
- `Blocked Items`：由于缺少环境、权限、认证或启动信息而无法继续的事项。
- `Remediation Priorities`：建议优先修复的问题。
- `Release Readiness`：发布建议，取值为 `Ready`、`Ready with risks` 或 `Not ready`。

当用户允许写入文件时，推荐生成这些文件：

- `FINAL_AUDIT_REPORT.md`
- `API_DOCUMENTATION.md`
- `openapi.yaml` 或 `openapi.json`
- `API_TEST_MATRIX.md`
- `API_TEST_RESULTS.md`
- `REMEDIATION_CHECKLIST.md`

## Safety Model

这个工作流只用于已授权的防御性审核和测试。

以下操作必须先获得用户确认：

- 生产环境测试；
- 破坏性测试或会修改数据的测试；
- 高频、大量请求或可能触发限流的测试；
- 调用外部服务；
- 使用真实账号测试；
- 写入或覆盖目标项目文件。

如果缺少必要信息，工作流应该把对应事项标记为 `BLOCKED`，并说明缺少什么，而不是猜测或编造结果。

## Repository Layout

```text
.claude-plugin/                 Claude Code 插件清单
commands/                       /final-audit 命令入口
skills/project-final-audit/      Claude Code skill 入口和参考模板
workflows/final-audit.md         最终审核的规范工作流
interfaces/                     跨 Agent 调用契约和 JSON Schema
docs/                           使用文档和维护文档
examples/                       示例请求、示例报告和 Agent 提示词
```

## Documentation

更多说明可以查看：

- `docs/installation.md`：安装方式。
- `docs/usage.md`：使用方式。
- `docs/claude-code-plugin.md`：Claude Code 插件说明。
- `docs/cross-agent-interface.md`：Trae、OpenCode 和其他 Agent 的调用方式。
- `docs/verification.md`：验证清单。
- `docs/maintainer-guide.md`：维护指南。

## Release Notes

当前第一个版本是 `v0.1.0`。

发布说明和变更记录：

- `RELEASE_NOTES.md`
- `CHANGELOG.md`

## Contributing

欢迎提交改进建议和贡献。详见 `CONTRIBUTING.md`。

维护时请注意：`workflows/final-audit.md` 是唯一事实来源，其他命令、skill 和示例提示词都应该保持轻量，只负责指向规范工作流。

## Security

安全策略详见 `SECURITY.md`。

请只在你拥有授权的项目中使用这个工作流，不要用于未授权测试、破坏性攻击、拒绝服务、大规模扫描或恶意用途。

## License

本项目使用 MIT License，详见 `LICENSE`。
