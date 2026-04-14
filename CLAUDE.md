# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个 **VS Code Copilot 避坑知识库**，收集整理 Copilot 官方文档、Agent Skills 规范及相关资源，目标是通过了解 Copilot 的工作原理和限制条件，让这个"垃圾"能正常为你工作。

## 文档结构

```
docs/
├── sources/              # 抓取的官方文档（只读）
│   ├── copilot/         # VS Code Copilot 官方文档
│   │   ├── overview.md, setup.md, getting-started.md
│   │   ├── ai-powered-suggestions.md, copilot-smart-actions.md
│   │   ├── best-practices.md, troubleshooting.md, faq.md, security.md
│   │   ├── agents/, chat/, customization/, guides/
│   └── agentskills/      # Agent Skills 规范文档
│       ├── home.md, what-are-skills.md, specification.md
│       ├── skill-creation/, client-implementation/
│       └── llms.txt
└── guides/              # 本项目生成的避坑指南
    ├── README.md
    └── copilot-pitfalls.md  # 待编写
```

## 目录原则

- **`docs/sources/`**：抓取的官方文档，原则上只读，不直接修改。如需更新，重新抓取替换。
- **`docs/guides/`**：本项目原创内容，整理避坑经验和分析。

## 维护说明

- 所有 `docs/sources/` 下的文档均来自官方公开页面，保存为带 frontmatter 的 Markdown
- 每个 `.md` 文件顶部的 frontmatter 包含 `title`、`description`、`source` 字段
- 如需更新文档，修改 frontmatter 中的 `source` URL 后重新抓取

## 常用操作

添加新抓取的文档：
1. 确定官方文档 URL
2. 创建对应目录（如 `docs/sources/copilot/xxx/`）
3. 使用 `mcp__open-webSearch__fetchWebContent` 工具抓取，readability=true
4. 写入 `.md` 文件并补全 frontmatter

抓取格式：`mcp__open-webSearch__fetchWebContent`，readability=true，从返回的 `content` 字段取 Markdown 内容。
