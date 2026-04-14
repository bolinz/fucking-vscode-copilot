# Copilot 生存指南

让 VS Code Copilot 这个垃圾正常工作的知识库。

## 目录结构

```
docs/
├── sources/              # 抓取的官方文档（只读）
│   ├── copilot/          # VS Code Copilot 官方文档
│   └── agentskills/      # Agent Skills 规范文档
└── guides/               # 本项目生成的避坑指南
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

### docs/guides/ — 本项目避坑指南

| 文件 | 说明 |
|------|------|
| `copilot-pitfalls.md` | Copilot 常见坑汇总（待编写） |

## 背景

VS Code Copilot 文档散落在各处，且官方教程往往回避了实际使用中的痛点。本项目通过整理官方文档 + Agent Skills 规范 + 踩坑经验，让 Copilot 从"人工智障"变成真正可用的工具。

## 来源

所有 `docs/sources/` 下的文档均来自公开官方页面：
- https://code.visualstudio.com/docs/copilot/
- https://agentskills.io/
