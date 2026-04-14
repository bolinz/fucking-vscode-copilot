# Copilot 生存指南

一个专门研究 VS Code Copilot Chat 到底有多烂、烂在哪里、为什么会烂，以及怎么让这个烂货勉强正常工作的知识库。

## 目录结构

```
docs/
├── sources/              # 抓取的官方文档（只读）
│   ├── copilot/          # VS Code Copilot 官方文档
│   └── agentskills/      # Agent Skills 规范文档
└── guides/               # 本项目生成的拆解与避坑指南
    └── copilot-pitfalls.md
```

## 内容

### docs/sources/copilot/ — VS Code Copilot 官方文档

| 分类 | 文件 | 说明 |
|------|------|------|
| 入门 | `setup.md` | 安装配置 |
| | `getting-started.md` | 快速上手 |
| | `overview.md` | 功能概述 |
| 核心功能 | `ai-powered-suggestions.md` | 内联代码补全 |
| | `chat/inline-chat.md` | 编辑器内 Chat |
| | `copilot-smart-actions.md` | 智能操作 |
| Agent | `agents/overview.md` | Agent 架构与类型 |
| | `agents/planning.md` | Plan Agent 用法 |
| | `agents/cloud-agents.md` | 云端 Agent |
| | `agents/agents-tutorial.md` | Agent 实战教程 |
| 会话 | `chat/chat-sessions.md` | 多会话管理 |
| 配置 | `customization/*.md` | 自定义指令、Hook、MCP 等 |
| 指南 | `guides/debug-with-copilot.md` | AI 辅助调试 |
| | `guides/browser-agent-testing-guide.md` | 浏览器 Agent 测试 |
| 参考 | `best-practices.md` | 最佳实践 |
| | `troubleshooting.md` | 常见问题 |
| | `faq.md` | FAQ |
| | `security.md` | 安全与隐私 |

### docs/sources/agentskills/ — Agent Skills 规范

| 文件 | 说明 |
|------|------|
| `what-are-skills.md` | 什么是 Agent Skills |
| `specification.md` | SKILL.md 格式规范 |
| `skill-creation/quickstart.md` | 创建第一个 Skill |
| `skill-creation/best-practices.md` | Skill 创作最佳实践 |
| `skill-creation/using-scripts.md` | 在 Skill 中运行脚本 |
| `client-implementation/adding-skills-support.md` | 给 Agent 添加 Skills 支持 |

### docs/guides/ — 本项目原创指南

| 文件 | 说明 |
|------|------|
| `copilot-pitfalls.md` | 专注分析 VS Code Copilot Chat 为什么难用、难用在哪、怎么尽量用顺 |

## 背景

VS Code Copilot Chat 的官方文档会告诉你功能入口，但很少正面回答几个更实际的问题：

- 为什么它经常一副很懂的样子，结果越做越偏
- 为什么 `Plan` 看起来很完整，落地时却不断漏前提
- 为什么 `Agent` 已经开始干活了，但你还是不放心
- 为什么它明明集成在 VS Code 里，用起来却还是一股半成品味

这个仓库就是围绕这些问题建立的。这里不打算替它洗地，也不打算做泛 AI 工具百科，而是专门拆 VS Code Copilot Chat：

- 它具体烂在哪里
- 这些问题为什么会发生
- 哪些任务最容易把它用崩
- 在接受它就这水平的前提下，怎样把它调教到至少能干活

`docs/sources/` 保留官方文档原文，`docs/guides/` 负责把这些文档和实际踩坑体验重新组织成更直接、更不客气、也更有操作性的说明。

## 来源

所有 `docs/sources/` 下的文档均来自公开官方页面：
- https://code.visualstudio.com/docs/copilot/
- https://agentskills.io/
