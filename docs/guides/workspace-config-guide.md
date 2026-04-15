---
title: "Workspace 级 Copilot 配置指南"
description: "如何通过 .github/copilot-instructions.md 和项目文件为团队所有成员配置 Copilot 行为约束"
---

# Workspace 级 Copilot 配置指南

User-level 配置（`settings.json`）只影响你自己。Workspace 级配置则能让整个团队在同一个项目里使用 Copilot 时都遵循统一的约束和规范。

## 核心机制：`.github/copilot-instructions.md`

这是 Workspace 级 Copilot 配置的主要载体。

文件路径：`.github/copilot-instructions.md`

Copilot 在当前仓库下所有会话中自动加载此文件，不需要手动指定。

### 基础结构

```markdown
# 项目规范

## 技术栈
- 语言：TypeScript + Node.js
- 框架：Express.js
- 数据库：PostgreSQL (禁止直接操作 migration 以外的表)

## 代码规范
- 所有 async 函数必须有 try/catch
- 禁止使用 var，只用 const 和 let
- 公共 API 必须写 JSDoc

## 测试要求
- 覆盖所有公共函数
- 测试文件放在 `__tests__/` 目录
- 禁止提交未通过的测试

## 禁止事项
- 不要修改 `config/production.ts`
- 不要升级依赖版本
- 不要删除测试文件
```

### 触发 Copilot 加载指令

在 Chat 中提示时，Copilot 会自动读取此文件。但你也可以显式引用：

> "根据 .github/copilot-instructions.md 的规范，帮我审查这段代码"

---

## Workspace 级 `.vscode/`

### `settings.json` 团队共享配置

`.vscode/settings.json` 纳入版本控制后，团队成员克隆项目即可获得统一的编辑器配置。

```json
{
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  },
  "chat.includeApplyingInstructions": true,
  "chat.includeReferencedInstructions": false
}
```

**注意**：敏感配置（如认证信息、个人 token）不要放在这里。

### `.vscode/extensions.json`

推荐团队使用的插件：

```json
{
  "recommendations": [
    "github.copilot",
    "github.copilot-chat",
    "mcp-explorer"
  ]
}
```

成员打开项目时 VS Code 会提示安装推荐插件。

---

## `AGENTS.md` — 多 Agent 协作文件

Cursor 和部分 Agent 实现支持 `AGENTS.md` 文件，定义多个 Agent 的角色和职责。

```markdown
# Agent Roles

## Frontend Agent
负责：
- `src/components/` 下的所有组件
- React 组件编写和样式
- 不允许修改 API 路由

## Backend Agent
负责：
- `src/routes/` 下的 API 路由
- 数据库操作和迁移
- 不允许修改前端组件

## Review Agent
负责：
- 审查 Frontend 和 Backend Agent 的改动
- 检查是否违反项目规范
- 在 PR 中添加审查意见
```

---

## `.github/copilot-instructions.md` 进阶用法

### 分语言配置

```markdown
# TypeScript 规范

## 类型要求
- 禁止使用 `any`，必须用 `unknown` 替代
- 所有函数参数必须有类型标注
- 接口命名：I开头（如 IUserRepository）

## 测试
- 单元测试必须使用 Jest
- 测试文件命名：`<模块>.test.ts`
```

### 分任务配置

```markdown
# 任务类型指南

## 排障任务
1. 先读取相关日志文件
2. 说明可能原因（不超过3个）
3. 提供修复步骤
4. 不要在没有确认前修改代码

## 重构任务
1. 先说明重构范围
2. 列出所有受影响的文件
3. 获取确认后再执行
4. 保留原有测试，更新测试覆盖率

## 新功能开发
1. 先确认接口设计
2. 更新相关类型定义
3. 补充测试用例
4. 验证通过后再提交
```

### 与安全相关的配置

```markdown
# 安全约束

## 禁止的输入处理方式
- 禁止直接将 user input 拼 SQL
- 禁止在 console.log 中打印敏感数据
- 禁止在代码中硬编码密钥或连接串

## 敏感操作确认
- 数据库写操作前必须人工确认
- 删除操作必须二次确认
- 部署相关命令需要说明环境
```

---

## 与 User-Level 配置的优先级

| 配置类型 | 文件位置 | 作用范围 | 优先级 |
|---------|---------|---------|-------|
| User | `~/.config/Code/User/settings.json` | 个人所有项目 | 可覆盖 Workspace |
| Workspace | `.vscode/settings.json` | 项目内所有成员 | 可覆盖系统默认 |
| Workspace | `.github/copilot-instructions.md` | 项目内 Copilot Chat | 最高优先级 |

**推荐原则**：
- Workspace 配置只写团队共识，不写个人偏好
- User 配置用于个人习惯（如字体、快捷键）
- 两者冲突时，以 Workspace 配置为准

---

## 维护建议

### 定期 review

指令文件需要随项目演进更新，建议：
- 在 PR 模板中检查是否需要更新 copilot-instructions.md
- 新成员加入时 review 此文件
- 每次 Copilot 失控后分析是否需要新增约束

### 避免指令文件过长

Copilot 对指令文件有最大长度限制，过长会被截断。

**优先写**：
- 禁止事项（简洁、明确）
- 测试和构建命令
- 目录结构约束

**不要写**：
- 代码风格详细说明（交给 linter 处理）
- 使用手册
- 历史背景
