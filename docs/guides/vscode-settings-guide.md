---
title: "VS Code Copilot 配置详解"
description: "逐条说明 Copilot 各项设置的作用、解决的问题，以及不当配置会带来的后果"
---

# VS Code Copilot 配置详解

本文档逐条说明关键配置的用途、解决的问题和潜在风险，基于 `docs/sources/` 中的官方文档和实际踩坑经验整理。

## 1. 日志与调试

### `github.copilot.chat.agentDebugLog.enabled`

**作用**：开启 Agent 调试日志记录。

**解决的问题**：
- `Plan` 的推理过程像黑箱，不知道它为什么放弃了某个方案
- `Agent` 执行过程中读了什么文件、调了什么命令，无法追踪
- 问题发生后无法复盘，只能靠盲猜

**开启后的效果**：
- Chat 面板出现 `Agent Debug Log` 和 `Chat Debug View` 入口
- 每次响应的完整上下文使用情况可见
- 工具调用顺序和返回值可查

**风险**：日志量较大，影响性能，生产环境建议只在排查时开启。

```json
{
  "github.copilot.chat.agentDebugLog.enabled": true
}
```

### `github.copilot.chat.agent.enabled`

**作用**：启用 Agent 模式。

**解决的问题**：
- 新版 Copilot Chat 默认不一定开启 Agent
- 没有这个你根本看不到 `Plan` 和 `Agent` 按钮

**风险**：Agent 权限大，容易造成大范围改动，见坑 7。

---

## 2. 自定义指令

### `/.github/copilot-instructions.md`

**作用**：项目级指令文件，Copilot 在当前仓库下所有会话中自动加载。

**解决的问题**：
- 坑 10：给太少上下文，然后怪它不懂项目
- 项目风格、测试命令、禁止事项无法靠每次 prompt 传递
- 新开 session 或切换窗口后上下文丢失

**文件内容示例**：

```markdown
# 项目规范

## 架构约束
- 只修改 `src/` 目录，不要动 `lib/`、`config/`、`scripts/`
- 禁止修改数据库 schema 文件
- 不允许新增依赖，必须用已有依赖

## 命令规范
- 测试：`npm test`
- 构建：`npm run build`
- Lint：`npm run lint`
- 禁止：`yarn`、`pnpm`

## 输出要求
- 优先最小改动，不要重构
- 改动前先说明会改哪些文件
- 验证通过后再提交
```

**解决的问题**：
- Agent 不知道哪些是核心模块
- Agent 随意新增依赖、随意扩大改动范围
- 不同人对话风格不统一

**风险**：指令文件太长会被截断，优先写约束和命令，不要写成使用手册。

---

## 3. 上下文指令加载

### `chat.includeApplyingInstructions`

**作用**：控制 Copilot 是否自动加载当前仓库的 `.github/copilot-instructions.md` 指令文件。

**解决的问题**：
- 写了项目指令文件但 Copilot 不读取
- 不同仓库的指令互相污染

```json
{
  "chat.includeApplyingInstructions": true
}
```

**建议**：始终保持 `true`，项目级指令是团队约束的基础。

### `chat.includeReferencedInstructions`

**作用**：控制 Copilot 是否加载被指令文件引用的其他文件（如 `!INCLUDE path/to/file.md`）。

```json
{
  "chat.includeReferencedInstructions": false
}
```

**建议**：如无跨文件引用需求，保持 `false` 减少干扰。

---

## 4. MCP 服务器

### MCP 配置

MCP（Model Context Protocol）允许 Copilot 连接外部工具和数据集。

**解决的问题**：
- Copilot 原生能力有限，连接外部工具才能扩展
- 数据库查询、API 调用、文件系统操作需要额外工具

**配置位置**：VS Code 设置或 `settings.json`：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./"]
    }
  }
}
```

**常用 MCP 资源**：
- **文件系统**：读写本地文件
- **数据库**：直接查数据库结构
- **API**：连接内部 API 文档
- **Git**：查询 commit 历史、分支状态

**风险**：MCP 工具权限很大，只连接可信的数据源，不要把敏感目录暴露给不可信的 MCP 服务器。

---

## 5. 插件组合

### 必装

| 插件 | 用途 |
|------|------|
| GitHub Copilot | 主体，补全和 Chat |
| GitHub Copilot Chat | Agent 能力、Plan 模式 |

### 推荐

| 插件 | 用途 |
|------|------|
| MCP Explorer | 查看 MCP 服务状态、调试连接 |
| GitLens | 代码历史、引用查看，帮助理解上下文 |

---

## 配置清单

以下为经过官方验证的推荐配置：

```json
{
  // 核心功能
  "chat.agent.enabled": true,

  // 上下文指令
  "chat.includeApplyingInstructions": true,
  "chat.includeReferencedInstructions": false,

  // 调试
  "github.copilot.chat.agentDebugLog.enabled": true,

  // 会话：右上角 New Session 按钮随手点，不要在一个脏会话里持续工作
  // 补全：编辑器内补全默认已启用，无需额外配置
}
```

配合 `/.github/copilot-instructions.md` 项目指令文件使用。

---

## 配置与坑的对应表

| 坑编号 | 对应配置或操作 |
|--------|--------------|
| 坑 1-3 | 项目指令文件 + 提示时显式引用文件 |
| 坑 4 | 不要把 Plan 当长期记忆，及时落盘 |
| 坑 5-6 | 复杂任务先用 Plan，习惯 `chat.planAgent.defaultModel` |
| 坑 7 | 在 Chat 权限选择器里选 `Default Approvals`，关键操作手点确认 |
| 坑 8-9 | `Default Approvals` + 监督点，不要开 Bypass |
| 坑 10 | `.github/copilot-instructions.md` |
| 坑 11 | 定期点 `New Session`，不要在旧会话里持续工作 |
| 坑 12 | 提示时显式引用文件，不要盲猜 |
| 坑 13 | 不要在 prompt 里塞敏感信息 |
| 坑 14 | `github.copilot.chat.agentDebugLog.enabled: true` + 调试视图 |

## 后续

- MCP 详细配置和可信 MCP 资源列表
- 各语言的具体补全行为差异
- 企业场景下的管理策略（Copilot Enterprise）
