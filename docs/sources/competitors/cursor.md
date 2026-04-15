---
title: "Cursor 官方文档"
description: "Cursor 编辑器文档"
source: "https://www.cursor.com/cn/docs"
---

# Cursor 文档

Cursor 是一款 AI 编辑器和编码智能体。用它来理解你的代码库、规划和构建功能、修复错误、审查更改，以及配合你已在使用的工具工作。

## 快速开始

本指南将带你从安装开始，到在 Cursor 中完成第一次有用的改动。你将登录，让 Cursor 讲解你的代码库，做一个小改动，并审查结果。

### 1. 安装 Cursor 并登录

下载 Cursor。打开应用并登录。然后选择一个文件夹，从一个小任务开始。

### 2. 让 Cursor 讲解你的代码库

选择文件夹后，使用 **Cmd I** 打开 Agent。让 Cursor 讲解这个代码库，并指出应先阅读的主要部分。

> "Explain this codebase. Point me to the main entry points, key modules, and anything I should read before making changes."

Cursor 会搜索你的仓库、阅读相关文件，并总结项目是如何组织在一起的。这是快速熟悉陌生代码库的最快方式之一。

### 3. 做一个小改动

了解项目后，向 Cursor 询问几个安全的小改进建议。选一个，并让它做出这个改动。

> "Suggest three small, safe improvements in this codebase. Explain the tradeoffs and wait for me to choose one."

适合作为入门的任务通常风险较低，比如改进一些文案或修复小的 UI 问题。

### 4. 审查 diff 并验证结果

现在你可以观察 Cursor 工作。diff 视图会显示 Agent 所做的改动。完成后，审查 diff，并让 Cursor 运行你项目中已经在使用的检查。这可能包括测试、类型检查、lint 或本地构建。

### 5. 对较大的改动使用 Plan Mode

现在你已经了解了基础知识，可以在更大的改动中使用 Plan Mode。当任务跨多个文件、需要调研，或在编码前需要审批时，它会很好用。

在 Agent 输入框中按 **Shift+Tab** 以切换 **Plan Mode**。Cursor 不会立即编写代码，而是会：
- 研究你的代码库以找到相关文件
- 就你的需求提出澄清问题
- 创建详细的实现计划
- 在开始构建前等待你的批准

## Agent 概览

Agent 是 Cursor 的助手，能够独立完成复杂的编码任务、运行终端命令并编辑代码。可在侧边栏按 **Cmd+I** 打开。

### 智能体的工作原理

一个智能体由三个组件构成：
- **Instructions**：引导智能体行为的 system prompt 和 [rules](https://www.cursor.com/cn/docs/rules)
- **Tools**：文件编辑、代码库搜索、终端执行等
- **Model**：你为该任务选择的智能体模型

Cursor 的智能体会为我们支持的每个模型编排这些组件，并针对每个前沿模型专门调整指令和工具。随着新模型的发布，你可以专注于构建软件，而由 Cursor 负责处理模型特定的优化。

### 工具

工具是构建 Agent 的基础模块。它们用于搜索你的代码库和网络以查找相关信息、编辑文件、运行终端命令等。

Agent 在单个任务中可发起的工具调用次数没有上限。

可用的工具包括：
- 语义搜索
- 搜索文件和文件夹
- Web 搜索
- 获取规则
- 读取文件
- 编辑文件
- 运行 shell 命令
- 浏览器
- 图像生成
- 提问

### 检查点

检查点会在一次 Agent 会话期间保存你的代码库快照。Agent 会在进行重大更改前自动创建检查点，记录所有已修改文件的状态。

如果 Agent 走偏了，你可以在对话时间线中点击任意检查点来预览当时的文件状态，然后执行恢复，将所有文件回滚到该状态。

> 检查点存储在本地，并且独立于 Git。只将它们用于撤销 Agent 的更改；永久版本控制请使用 Git。

### 排队消息

在 Agent 处理当前任务时，将后续消息加入队列。你的指令会依次等待，并在就绪后自动执行。

**使用队列：**
- 当 Agent 正在工作时，输入你的下一条指令
- 按 **Enter** 将其添加到队列
- 消息会按顺序显示在当前任务下方
- 按需拖动以重新排序队列中的消息
- Agent 完成当前任务后会按顺序依次处理队列中的消息

**键盘快捷键：**
- 按 **Enter** 将消息排入队列（会等待 Agent 完成当前任务后再发送）
- 按 **Cmd+Enter** 立即发送，跳过队列

### 即时发送消息

当你使用 **Cmd+Enter** 即时发送时，你的消息会附加到当前对话中最近的一条用户消息后面，并立即处理，而无需在队列中等待。

## 模型

Cursor 支持多种 AI 模型：

| 名称 | 默认上下文 | 最大模式 | 能力 |
|------|-----------|---------|------|
| Claude 4.6 Opus | 200k | 1M | Agent、Thinking、Image |
| Claude 4.6 Sonnet | 200k | 1M | Agent、Thinking、Image |
| Composer 2 | 200k | - | Agent、Thinking、Image |
| Gemini 3.1 Pro | 200k | 1M | Agent、Thinking、Image |
| GPT-5.3 Codex | 272k | - | Agent、Thinking、Image |
| GPT-5.4 | 272k | 1M | Agent、Thinking、Image |
| Grok 4.20 | 200k | 2M | Agent、Thinking、Image |

## 规则

规则为 Agent 提供系统级指令。它们将提示、脚本等内容整合在一起，使你能够轻松地在团队中管理和共享工作流。

Cursor 支持四种类型的规则：
- **项目规则**：存储在 `.cursor/rules` 中，纳入版本控制，并限定在你的代码库范围内
- **用户规则**：在你的 Cursor 环境中全局生效
- **团队规则**：从仪表盘管理的团队范围规则
- **AGENTS.md**：采用 markdown 格式的 Agent 指令

### 规则如何工作

大语言模型在各次补全之间不会保留记忆。规则在提示层面提供持久、可复用的上下文。应用规则时，其内容会被加入到模型上下文的开头。

### 项目规则

项目规则存放在 `.cursor/rules` 目录中的 Markdown 文件里，并纳入版本控制。它们可以通过路径模式限定作用范围、手动触发，或按相关性自动应用。

使用项目规则可以：
- 固化与你代码库相关的领域知识
- 自动化项目特定的工作流或模板
- 统一风格或架构决策

### 规则文件结构

```
.cursor/rules/
  react-patterns.mdc    # 带有前置元数据的规则（description, globs）
  api-guidelines.md     # 简单的 markdown 规则
  frontend/
    components.md       # 在文件夹中组织规则
```

### 规则类型

| 类型 | 描述 |
|------|------|
| Always Apply | 应用于每个聊天会话 |
| Apply Intelligently | 当 Agent 根据描述判断其相关时应用 |
| Apply to Specific Files | 当文件匹配指定模式时应用 |
| Apply Manually | 在聊天中被 @ 提及时应用 |

### 创建规则

有两种方式可以创建规则：
1. 在对话中使用 `/create-rule`：在 Agent 中输入 `/create-rule`，然后描述你的需求
2. 在设置中创建：打开 `Cursor Settings > Rules, Commands`，然后点击 `+ Add Rule`

### 最佳实践

- 将规则控制在 500 行以内
- 将大型规则拆分为多个可组合的规则
- 提供具体示例或引用相关文件
- 避免模糊的指导，像撰写清晰的内部文档那样来写规则
- 在聊天中重复提示时复用规则
- 引用文件而不是复制其内容

### 编写规则时应避免的事项

- 整份照搬风格指南：请改用 linter
- 把每一个可能的命令都写进文档：Agent 了解常用工具
- 为极少出现的边缘情况添加说明
- 重复你代码库中已有的内容

### 团队规则

Team 和 Enterprise 方案可以通过 Cursor 仪表盘在整个组织范围内创建并强制执行规则。管理员可以配置每条规则对团队成员来说是否为必需。

团队规则会与其他类型的规则一同生效，并具有更高优先级。

优先级顺序：**团队规则 → 项目规则 → 用户规则**

### AGENTS.md

AGENTS.md 是一个用于定义 agent 指令的简单 markdown 文件。将它放在项目根目录中，可作为 `.cursor/rules` 的一种替代方式。

```markdown
# Project Instructions

## Code Style
- Use TypeScript for all new files
- Prefer functional components in React
- Use snake_case for database columns

## Architecture
- Follow the repository pattern
- Keep business logic in service layers
```

### 用户规则

用户规则是在 `Cursor Settings → Rules` 中定义的全局首选项，适用于所有项目。

## MCP (模型上下文协议)

模型上下文协议 (MCP) 使 Cursor 能够连接到外部工具和数据源。

### 为什么使用 MCP？

MCP 可将 Cursor 连接到外部系统和数据。无需反复解释您的项目结构，直接与您的工具集成即可。

您可以使用任何能够输出到 stdout 或提供 HTTP 端点的语言来编写 MCP 服务器，例如 Python、JavaScript、Go 等。

### 工作原理

MCP 服务器通过该协议提供能力，将 Cursor 连接到外部工具或数据源。

Cursor 支持三种传输方式：

| 传输方式 | 执行环境 | 部署 | 用户 | 输入 | 认证 |
|---------|---------|------|------|------|------|
| stdio | 本地 | 由 Cursor 管理 | 单用户 | Shell 命令 | 手动 |
| SSE | 本地/远程 | 部署为服务器 | 多用户 | SSE 端点 URL | OAuth |
| Streamable HTTP | 本地/远程 | 部署为服务器 | 多用户 | HTTP 端点 URL | OAuth |

### 协议和扩展支持

| 功能 | 支持情况 | 描述 |
|------|---------|------|
| 工具 | 支持 | 供 AI 模型调用的函数 |
| Prompts | 支持 | 面向用户的模板化消息和工作流 |
| Resources | 支持 | 可读取和引用的结构化数据源 |
| Roots | 支持 | 由服务器发起、用于确定 URI 或文件系统边界的查询 |
| Elicitation | 支持 | 由服务器发起、向用户请求更多信息的请求 |
| 应用 (扩展) | 支持 | 由 MCP 工具返回的交互式 UI 视图 |

### MCP 应用

Cursor 支持 MCP 应用扩展。MCP 工具除了标准工具输出外，还可以返回交互式 UI。

### 安装 MCP 服务器

**一键安装**：在插件市场浏览支持一键安装的官方插件。

**使用 mcp.json 配置**：

CLI Server (Node.js):
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "mcp-server"],
      "env": {
        "API_KEY": "value"
      }
    }
  }
}
```

CLI Server (Python):
```json
{
  "mcpServers": {
    "server-name": {
      "command": "python",
      "args": ["mcp-server.py"],
      "env": {
        "API_KEY": "value"
      }
    }
  }
}
```

Remote Server:
```json
{
  "mcpServers": {
    "server-name": {
      "url": "http://localhost:3000/mcp",
      "headers": {
        "API_KEY": "value"
      }
    }
  }
}
```

### 配置位置

- **项目配置**：在项目中创建 `.cursor/mcp.json`，用于项目专属工具
- **全局配置**：在主目录中创建 `~/.cursor/mcp.json`，用于在任何地方都可用的工具

### 配置插值

支持以下变量语法：
- `${env:NAME}` - 环境变量
- `${userHome}` - 主目录路径
- `${workspaceFolder}` - 项目根目录
- `${workspaceFolderBasename}` - 项目根目录名称
- `${pathSeparator}` 或 `${/}` - 操作系统路径分隔符

### 在聊天中使用 MCP

智能体会在相关时自动使用 Available Tools 下列出的 MCP 工具。

**工具批准**：默认情况下，智能体在使用 MCP 工具前会请求批准。

**自动运行**：启用自动运行后，智能体无需询问即可使用 MCP 工具。

## GitHub 集成

Cursor GitHub 应用会连接到你的仓库，让你可以使用 Cloud Agents 和 Bugbot 等功能。

### 设置

1. 需要具备 Cursor 管理员权限和 GitHub 组织管理员权限
2. 前往仪表盘中的集成
3. 点击 GitHub 旁的连接
4. 选择所有仓库或选定的仓库
5. 返回仪表盘，配置仓库功能

### 权限

GitHub 应用请求以下权限：

| 权限 | 目的 |
|------|------|
| Repository access | 克隆你的代码并创建工作分支 |
| PR | 创建 PR 并留下评审意见 |
| Issues | 跟踪评审期间发现的缺陷和任务 |
| Checks and statuses | 报告代码质量和测试结果 |
| Actions and workflows | 监控 CI/CD 流水线和部署状态 |

所有权限都遵循最小权限原则。

### IP 允许列表配置

如果你的组织使用 GitHub 的 IP 允许列表功能来限制对你的仓库的访问，可以将 Cursor 配置为使用托管的出站代理。

**为已安装的 GitHub 应用启用 IP 允许列表配置（推荐）**：
1. 前往你组织的 Security 设置
2. 打开 IP allow list 设置
3. 勾选 "Allow access by GitHub Apps"

## Cloud Agent

Cloud Agent 是云端运行的 AI 智能体，能够在你的仓库上执行任务。

### 功能

- 在云端你的仓库上运行
- 与 GitHub 集成
- Bugbot 自动化 PR 审查
- 安全与网络配置

## CLI (命令行界面)

Cursor 提供命令行界面工具，支持多种功能：

- **安装**：支持多种安装方式
- **能力**：文件操作、终端命令等
- **Shell 模式**：直接在终端中使用 Cursor
- **ACP**：高级命令行功能
- **无头模式 / CI**：支持在 CI 环境中使用

## 更多功能

- **插件**：扩展 Cursor 功能
- **技能**：自定义 Agent 能力
- **子代理**：创建专用 Agent
- **钩子**：自动化工作流
- **Bugbot**：自动化 PR 审查
- **自动化**：自定义自动化任务

## 集成

Cursor 支持与多种工具集成：
- Slack
- Linear
- GitHub
- GitLab
- JetBrains
- Xcode
- 深度链接
