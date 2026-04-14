# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个 **专门拆解 VS Code Copilot Chat 的知识库**。核心目标不是泛泛整理 AI 文档，而是围绕四件事展开：

- 它有多烂
- 它具体烂在哪里
- 它为什么会这样烂
- 怎样让这个烂货至少能正常干活

官方文档和 Agent Skills 规范只是原材料；真正的输出重点应放在 `docs/guides/`，也就是对 Copilot Chat 的问题机制、使用误区、调教方法和排障路径做直接分析。

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
└── guides/              # 本项目生成的拆解与避坑指南
    ├── README.md
    └── copilot-pitfalls.md
```

## 目录原则

- **`docs/sources/`**：抓取的官方文档，原则上只读，不直接修改。如需更新，重新抓取替换。
- **`docs/guides/`**：本项目原创内容，重点写 VS Code Copilot Chat 的问题、原因、症状、规避方式和调教方法。

## 写作方向

- 默认聚焦 **VS Code Copilot Chat**，尤其是 `Ask`、`Plan`、`Agent`、上下文、会话、权限、调试这几个面向。
- 重点回答“为什么它会这样表现”，不要只罗列功能。
- 允许语气直接、尖锐，但要尽量落到具体机制和使用场景，不要只做情绪化吐槽。
- 除非必要，不要把文章主轴写成和其他产品的横向对比；本仓库重点是把 Copilot Chat 自己拆明白。
- 如果引用官方说法，优先在原创指南里补上“这在真实使用里意味着什么”。

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

## 分支管理

**所有变更必须通过 PR 合入 main 分支，禁止直接推送 main。**

工作流：
1. 从 `main` 创建新分支：`git checkout -b feat/xxx`
2. 在新分支上完成修改
3. 推送到远程：`git push -u origin feat/xxx`
4. 在 GitHub 上创建 Pull Request
5. PR 合并后删除分支
