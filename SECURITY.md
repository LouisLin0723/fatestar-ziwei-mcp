# Security Policy / 安全政策

## 上报漏洞 (Reporting a Vulnerability)

发现安全漏洞请负责任地私下上报，**不要开公开 GitHub issue**。
If you find a security vulnerability, please report it privately — **do NOT open a public GitHub issue**.

### 上报方式 (How to Report)

发邮件到 **support@fatestar.top**，标题前缀 `[Security]`，附漏洞描述 / 复现步骤 / 潜在影响 / 修复建议（如有）。
Email **support@fatestar.top** with the subject prefixed `[Security]`, including a description, reproduction steps, potential impact, and a suggested fix (if any).

### 响应时间 (Response Timeline)

| 动作 / Action | 时限 / Timeframe |
|------|----------|
| 确认收到 / Acknowledgment | 48 小时内 / within 48h |
| 初步评估 / Initial assessment | 5 个工作日内 / within 5 business days |
| 发布修复 / Fix release | 视严重程度 / depends on severity |

### 范围 (Scope)

覆盖 / Covers:
- 本仓库的接入文档（`README.md`）/ This repo's integration docs
- npm 包 `@fatestar/ziwei-mcp` / The npm package

不在范围 (Out of Scope):
- FateStar API 后端（`www.fatestar.top`）/ The FateStar API backend
- 第三方 MCP 客户端 / Third-party MCP clients
- 用户自身的 API key 配置错误 / User misconfiguration of API keys

## 用户安全最佳实践 (Best Practices)

- `fs_live_` key 存环境变量 / MCP 配置的 env，绝不写进代码或贴聊天框
- Charting (`ziwei_chart` / `ziwei_transits`) 匿名即可，无需 key
- 定期轮换 key / Rotate keys periodically
