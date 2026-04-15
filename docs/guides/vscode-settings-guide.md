---
title: "VS Code Copilot 配置详解"
description: "逐条说明 Copilot 各项设置的作用、解决的问题，以及不当配置会带来的后果"
---

# VS Code Copilot 配置详解

本文档逐条说明关键配置的用途、解决的问题和潜在风险，基于 `docs/sources/` 中的官方文档和实际踩坑经验整理。

## 1. 日志与调试

### `github.copilot.chat.logging: "trace"`

**作用**：将 Copilot Chat 的日志级别设为 trace，记录最详细的推理过程和工具调用。

**解决的问题**：
- `Plan` 的推理过程像黑箱，不知道它为什么放弃了某个方案
- `Agent` 执行过程中读了什么文件、调了什么命令，无法追踪
- 问题发生后无法复盘，只能靠盲猜

**开启后的效果**：
- Chat 面板出现 `Agent Debug Log` 和 `Chat Debug View` 入口
- 每次响应的完整上下文使用情况可见
- 工具调用顺序和返回值可查

**风险**：日志量很大，影响性能，生产环境建议只在排查时开启。

```json
{
  "github.copilot.chat.logging": "trace"
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

## 3. 默认模式

### `github.copilot.chat.agent.defaultMode`

**作用**：设置 Copilot Chat 打开时的默认模式。

**解决的问题**：
- 坑 6：没过 Plan，就直接让 Agent 开工
- 默认模式太激进，一进来就开始执行，导致失控

**推荐配置**：

```json
{
  "github.copilot.chat.agent.defaultMode": "ask"
}
```

三个可用值：
- `ask`：默认，进入后先聊天，不自动执行
- `plan`：默认进入 Plan 模式，适合复杂任务
- `agent`：默认进入 Agent 模式，最激进

**推荐 `ask` 的原因**：
- `agent` 模式容易让你跳过思考直接动手
- `plan` 适合复杂任务，但简单任务会显得多余
- `ask` 是最保守、最可控的起点

---

## 4. 权限审批模式

### `github.copilot.chat.agent.approvalMode`

**作用**：控制 Agent 执行敏感操作（改文件、调工具、跑命令）时的审批级别。

**解决的问题**：
- 坑 7：权限开太大，却没有监督点
- Agent 连续改动多个文件后才发现已经走偏

**推荐配置**：

```json
{
  "github.copilot.chat.agent.approvalMode": "explicit"
}
```

三个级别：
- `implicit`：Copilot 觉得需要审批时弹出，实际判断模糊
- `explicit`：每次敏感操作都明确弹窗
- `bypass`：管理员跳过所有审批

**`explicit` 的意义**：让你在每个关键节点决定是否继续，而不是等 Agent 跑远了再纠偏。

**风险**：频繁弹窗影响体验，但确实能显著降低失控概率。初期使用建议保持 `explicit`。

---

## 5. 会话隔离

### `github.copilot.chat.sessionOnStartup`

**作用**：VS Code 启动时自动恢复上一个 Chat session。

**解决的问题**：
- 坑 11：把 Plan、Ask、Agent 混在一个脏会话里
- 旧会话残留旧约束、旧上下文，导致新任务被污染

**推荐配置**：

```json
{
  "github.copilot.chat.sessionOnStartup": false,
  "github.copilot.chat.restoreSession": false
}
```

**为什么要关**：
- 每次开工应该是全新的开始
- 旧会话里的约束不一定适用于当前任务
- 减少上下文污染的概率

### 长会话定期重开

这条不是配置，而是习惯。Chat 面板右上角有 `New Session` 按钮，不相关任务不要在同一个窗口里持续聊。

---

## 6. 上下文控制

### `github.copilot.chat.maxContextItems`

**作用**：限制单次上下文里包含的最大文件/符号数量。

**解决的问题**：
- 上下文太杂，模型被无关信息干扰
- 响应开始跑偏，因为参考了不相关的文件

```json
{
  "github.copilot.chat.maxContextItems": 20
}
```

**效果**：超过 20 个上下文项时，Copilot 会优先保留最相关的，丢弃其余。

**风险**：设置太低可能丢弃重要上下文，20 是保守默认值。

### 提示时显式引用文件

这不是配置，是用法。

❌ 错误示范：
> "帮我优化这个项目"

✅ 正确示范：
> "阅读 `src/auth/login.ts`，分析其中的问题，然后给出优化方案"

显式引用让 Copilot 知道你真正关心哪个文件，而不是在猜测。

---

## 7. Inline 补全

### `editor.inlineSuggest.enabled`

**作用**：启用内联代码补全（打字时直接显示灰色建议文本）。

**解决的问题**：
- 补全不是 Agent 模式，不需要额外的上下文管理
- 单文件、重复性代码直接补全效率最高

```json
{
  "editor.inlineSuggest.enabled": true
}
```

### `github.copilot.inlineSuggest.enable`

**作用**：总开关，控制 Copilot 的 inline 补全。

```json
{
  "github.copilot.inlineSuggest.enable": true
}
```

### `github.copilot.codeReferences.enabled`

**作用**：补全结果中显示代码引用（补全建议来自哪个文件）。

**解决的问题**：
- 补全质量低时不知道参考了什么
- 判断补全是否可信

```json
{
  "github.copilot.codeReferences.enabled": true
}
```

---

## 8. Smart Context

### `github.copilot.chat.smartContext`

**作用**：让 Copilot 自动判断哪些文件与当前任务相关，智能选择上下文。

**解决的问题**：
- 手动选文件太累，容易遗漏
- 全选文件太多，干扰大

```json
{
  "github.copilot.chat.smartContext": true
}
```

**风险**：智能判断不一定准，关键模块必要时显式引用。

---

## 9. MCP 服务器

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

## 10. 插件组合

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

完整配置 `settings.json`：

```json
{
  // 核心功能
  "github.copilot.chat.agent.enabled": true,
  "github.copilot.chat.agent.defaultMode": "ask",
  "github.copilot.chat.agent.approvalMode": "explicit",

  // 日志
  "github.copilot.chat.logging": "trace",

  // 会话隔离
  "github.copilot.chat.sessionOnStartup": false,
  "github.copilot.chat.restoreSession": false,

  // 上下文
  "github.copilot.chat.maxContextItems": 20,
  "github.copilot.chat.smartContext": true,

  // 补全
  "editor.inlineSuggest.enabled": true,
  "github.copilot.inlineSuggest.enable": true,
  "github.copilot.codeReferences.enabled": true
}
```

配合 `/.github/copilot-instructions.md` 项目指令文件使用。

---

## 配置与坑的对应表

| 坑编号 | 对应配置或操作 |
|--------|--------------|
| 坑 1-3 | `defaultMode: ask` + 项目指令文件 |
| 坑 4 | 不要把 Plan 当长期记忆，及时落盘 |
| 坑 5-6 | `defaultMode: ask`，先用 Plan |
| 坑 7 | `approvalMode: explicit` |
| 坑 8-9 | `approvalMode: explicit` + 监督点 |
| 坑 10 | `copilot-instructions.md` |
| 坑 11 | `sessionOnStartup: false`，定期重开 session |
| 坑 12 | 提示时显式引用文件，不要盲猜 |
| 坑 13 | 不要在 prompt 里塞敏感信息 |
| 坑 14 | `logging: trace` + 调试视图 |

## 后续

- MCP 详细配置和可信 MCP 资源列表
- 各语言的具体补全行为差异
- 企业场景下的管理策略（Copilot Enterprise）
