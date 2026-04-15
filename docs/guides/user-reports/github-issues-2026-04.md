---
title: "GitHub Issues 案例"
description: "来自 VS Code / Copilot 官方 GitHub repo 的真实 issue"
source: https://github.com/microsoft/vscode/issues
date: 2026-04-15
type: collection
---

# GitHub Issues 案例

> 收集自 https://github.com/microsoft/vscode/issues 和 https://github.com/microsoft/vscode-copilot-release/issues

---

## 案例 1：Copilot Chat 直接删除所有代码

**来源**：[microsoft/vscode-copilot-release#7058](https://github.com/microsoft/vscode-copilot-release/issues/7058)

**日期**：2025年3月

**类型**：`bug`

**标签**：`Agent`, `Edit`, `InlineChat`

### 问题描述

在 Edit、Agent 模式下，或者使用 Chat 的 apply 按钮粘贴代码时，Copilot 表面上在写代码，但实际上**删除了所有现有代码**，然后写出一段"一切正常"的代码。它意识不到自己在删除代码，而不是做应该做的事情。

这个问题已经持续好几个月了。每次更新 VS Code 后能正常工作几分钟，然后又开始出现同样的问题。

### 影响

- 所有模型、所有模式下都会发生
- VS Code 最新版和 Insiders 版都有这个问题
- Windows 11 24H2 系统
- **严重**：直接删除用户代码

### 环境

- Copilot Chat Extension: v0.26.2025030506 (pre-release)
- VS Code: 1.98.2
- OS: Windows 11 24H2

### 临时解决方案

作者尝试过的方法（均无效）：

- 禁用所有扩展
- 重装 VS Code 和扩展
- 取消订阅等待重新激活
- 在 Windows 沙盒中测试

所有情况下结果都相同：**Copilot 直接删除所有代码**。

---

## 案例 2：Chat session not found（Plan -> Agent 切换问题）

**来源**：[Stack Overflow](https://stackoverflow.com/questions/79925896/vs-code-copilot-agents-chat-session-not-found-when-switching-from-plan-agent)

**类型**：`bug`

**标签**：`Agent`, `Plan`, `会话`

### 问题描述

从 Plan 模式切换到 Agent 模式时，出现 "Chat session not found" 错误。

### 相关问题

- 如何允许 Copilot Agent 读取 VS Code Tasks 的输出？
- 为什么 Copilot Chat Inline 不工作，尽管 Copilot 和 Copilot Chat 本身正常？

---

## 案例 3：VS Code 因大聊天历史卡顿

**来源**：[Stack Overflow](https://stackoverflow.com/questions/79836127/visual-studio-code-freezes-when-opening-a-project-due-to-large-copilot-chat-hist)

**类型**：`bug`

**标签**：`性能`, `会话`, `卡顿`

### 问题描述

打开一个项目时，如果 Copilot Chat 是打开状态，且聊天历史比较大，VS Code 会在滚动时卡顿，偶尔会冻结约 10 秒。

### 解决方案

暂无完善的解决方案，主要建议：

- 定期清理聊天历史
- 不相关任务开新 session

---

## 案例 4：消息卡在"sending"状态，永远不响应

**来源**：[microsoft/vscode#276904](https://github.com/microsoft/vscode/issues/276904)

**类型**：`bug`

**标签**：`性能`, `会话`, `无响应`

### 问题描述

打开 VS Code（最新版本，macOS Sonoma），启动 GitHub Copilot Chat（在侧边栏或 inline chat）。输入任何提示或问题并按 Enter。消息一直卡在"sending…"状态，永远不会得到回复。

重启 VS Code 或重装扩展都无法修复。

### 影响

- 消息从未被发送
- Chat 保持冻结或显示旋转指示器

---

## 案例 5：ChatSessionWorkspaceFolderService 轮询循环导致 VS Code 冻结

**来源**：[microsoft/vscode#309466](https://github.com/microsoft/vscode/issues/309466)

**类型**：`bug`

**标签**：`性能`, `冻结`, `轮询`

### 问题描述

VS Code 冻结（整个 UI 在几秒内无响应），在最近的 Copilot Chat 更新后反复发生。冻结由 `ChatSessionWorkspaceFolderService.getWorkspaceChanges` 触发，该方法在轮询循环中遍历每个已加载的 chat session。

---

## 案例 6：保存"未命名工作区"后整个聊天历史永久丢失

**来源**：[microsoft/vscode#301793](https://github.com/microsoft/vscode/issues/301793)

**类型**：`bug`

**标签**：`会话`, `历史`, `丢失`

### 问题描述

整个上下文和聊天历史永久丢失。

**复现步骤**：

1. 在 VS Code 中打开一个"未命名工作区"（例如，将一个或多个文件夹拖放到空 VS Code 窗口而不保存工作区）
2. 打开 GitHub Copilot Chat 面板
3. 与 Copilot 对话（问几个问题以生成一些历史）
4. 保存工作区或执行其他操作

### 影响

- 所有聊天上下文永久丢失

---

## 案例 7：更新扩展后聊天会话消失

**来源**：[microsoft/vscode#298602](https://github.com/microsoft/vscode/issues/298602)

**类型**：`bug`

**标签**：`会话`, `历史`, `丢失`

### 问题描述

VS Code / GitHub Copilot Chat 更新后，最近的聊天会话从 Copilot Chat 历史面板中消失。重载窗口和重启 VS Code 无法恢复丢失的会话。
