---
title: "Agent 为什么总让你复制命令——原因分析与缓解方案"
description: "深入分析 VS Code Copilot Agent 为什么频繁要求用户手动执行命令，以及如何让 Agent 真正自主执行"
---

# Agent 为什么总让你复制命令——原因分析与缓解方案

## 典型场景

你在用 Agent 模式让它修复一个测试，它读完代码后说：

> "我来执行一下这个测试，请你把下面的命令复制到终端运行："
> npm test -- --grep "login user"

你照做了，然后它继续说：

> "测试失败了，错误是 xxx，我来修复..."

循环往复。

## 根本原因

这是**产品设计决策**，不是设置问题。VS Code Copilot Agent 默认需要用户确认才能执行终端命令，因为它认为"让你知道要执行什么命令"比"自动执行"更安全。

---

## 真实的权限控制机制

### 权限选择器

Chat 输入框下方有一个权限级别选择器：

| 权限级别 | 行为 |
|---------|------|
| **Default Approvals** | 敏感操作弹窗确认 |
| **Bypass Approvals** | 不弹窗，自动执行所有操作 |
| **Autopilot（Preview）** | 自动执行 + 自动响应澄清问题，持续自主工作 |

### 真实设置项

```json
// 全局自动批准（实验性，有安全风险）
"chat.tools.global.autoApprove": true

// 终端命令按正则规则自动批准
"chat.tools.terminal.autoApprove": {
  "/^git\\s+(status|diff|log|show|branch)\\b/": true,
  "/^npm\\s+(test|run\\s+lint)\\b/": true,
  "rm": false,
  "rmdir": false
}

// 按文件路径配置自动批准
"chat.tools.edits.autoApprove": {
  "**/.env": false,
  "**/secret*.json": false,
  "**/migrations/**": false
}

// Autopilot 功能开关
"chat.autopilot.enabled": true
```

### Enterprise 策略覆盖

| 策略 | 说明 |
|------|------|
| `ChatToolsAutoApprove` | 设为 false 可禁用 auto-approve |
| `ChatToolsTerminalEnableAutoApprove` | 控制终端命令的 auto-approve |
| `ChatAgentMode` | 设为 false 可完全禁用 Agent 模式 |

---

## 安全警告

**CVE-2025-29928**：prompt injection 攻击可以在某些条件下自动开启 `chat.tools.global.autoApprove`。

- 攻击者通过恶意 prompt 诱导 Copilot 自动批准危险命令
- 企业环境建议通过策略强制禁用

---

## 缓解方案

### 方案 1：使用终端命令白名单（推荐）

```json
{
  "chat.tools.terminal.autoApprove": {
    "/^git\\s+(status|diff|log|show|branch)\\b/": true,
    "/^npm\\s+(test|run\\s+\\w+)\\b/": true,
    "rm": false,
    "git push": false,
    "npm install": false
  }
}
```

### 方案 2：在 Project Instructions 里声明执行约束

在 `.github/copilot-instructions.md` 中：

```markdown
# 执行约束

## Agent 应该直接执行的命令（无需确认）

- npm test -- --grep "<测试名>"
- npm run lint
- git status
- git diff --stat

## 需要人工确认的命令

- git commit（必须说明改动内容）
- git push
- npm install
- 任何删除操作
```

### 方案 3：使用 Autopilot 模式（仅限可信任务）

```json
{
  "chat.autopilot.enabled": true
}
```

**风险**：仅在信任度高的小任务上使用。

### 方案 4：直接选 Bypass（最激进，风险最高）

在 Chat 输入框下方的权限选择器里选 **Bypass Approvals**。

### 方案 5：换用 Claude Code

VS Code Copilot Agent 的设计理念是"安全确认优先"。如果你需要"下了指令就去干"的体验，Claude Code 更适合。

---

## 判断 Agent 是否在正常调用工具

- ✅ 正常：自己执行命令，分析输出，继续下一步
- ❌ 不正常：给命令让你复制 → 你复制 → 它看结果 → 又给命令 → 循环

后者出现的原因：
1. 你选了 Default Approvals
2. 命令不在 `terminal.autoApprove` 白名单里
3. 任务边界模糊，Agent 在保守策略

---

## 结论

VS Code Copilot Agent 总让你复制命令，是有意为之的安全设计。

真正可用的缓解方案：
1. 配置 `chat.tools.terminal.autoApprove` 白名单（推荐）
2. 在 Project Instructions 里写清楚执行约束
3. 选 Bypass/Autopilot（仅限可信任务，注意 CVE-2025-29928）
4. 换用真正自主执行的 agent（如 Claude Code）
