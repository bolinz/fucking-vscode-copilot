---
title: "GitHub Issues 案例"
description: "来自 VS Code / Copilot 官方 GitHub repo 的真实 issue"
source: https://github.com/microsoft/vscode/issues
date: 2026-04-18
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

---

## 案例 8：更新后 Copilot Chat 性能严重下降

**来源**：[microsoft/vscode#310734](https://github.com/microsoft/vscode/issues/310734)

**日期**：2026-04-16

**类型**：`bug`

**标签**：`性能`, `版本更新`

### 问题描述

用户反映在更新 GitHub Copilot Chat 扩展到 0.43.0 后，AI 变得极其缓慢，完全无法接受。回滚到旧版本后速度恢复正常。

### 影响

- Copilot Chat Extension v0.43.0
- Windows 10/11 系统都有报告

---

## 案例 9：使用 Copilot 时 VS Code 崩溃

**来源**：[microsoft/vscode#310875](https://github.com/microsoft/vscode/issues/310875)

**日期**：2026-04-16

**类型**：`bug`

**标签**：`崩溃`, `稳定性`

### 问题描述

使用 Copilot 时 VS Code 持续崩溃。VS Code 版本 1.115.0，Windows 11。用户怒评："Do I really have to go try Claude Code?"

---

## 案例 10：Copilot Pro+ 周限额未重置

**来源**：[microsoft/vscode#310828](https://github.com/microsoft/vscode/issues/310828)

**日期**：2026-04-16

**类型**：`bug`

**标签**：`订阅`, `配额`, `计费`

### 问题描述

Copilot Pro+ 订阅用户反映达到周限额后等待超过一小时，错误仍然持续。每周配额未重置，一直被阻塞。用户明确表示"与使用量无关"，配额已过时间却仍未恢复。

---

## 案例 11：Copilot 使用量计数不准确（缓存问题）

**来源**：[microsoft/vscode#310745](https://github.com/microsoft/vscode/issues/310745)

**日期**：2026-04-16

**类型**：`bug`

**标签**：`计费`, `使用量`, `缓存`

### 问题描述

Copilot 请求的使用量计数器不准确，似乎被缓存了，不能真实反映实际使用量。用户表示"以前一直是准的，现在像是被缓存了"。这对需要靠计数器做月度预算的用户造成困扰。

---

## 案例 12：删除代码时 Copilot 极度缓慢

**来源**：[microsoft/vscode#301178](https://github.com/microsoft/vscode/issues/301178)

**日期**：2026-04-08（3月12日创建，4月仍有活跃讨论）

**类型**：`bug`

**标签**：`性能`, `打字延迟`, `编辑`

### 问题描述

删除代码时 Copilot 会显示加载指示器，用户无法删除字符。Copilot Chat 扩展 v0.39.0 导致打字速度约 120ms/每个 keyevent。用户确认禁用 GitHub Copilot Chat 扩展后 VS Code"非常快"——直接证明是 Copilot 导致的问题。

---

## 案例 13：Agent 模式极慢 - 几乎无法使用

**来源**：[microsoft/vscode#276681](https://github.com/microsoft/vscode/issues/276681)

**日期**：2026-04-01（持续未解决）

**类型**：`bug`

**标签**：`性能`, `Agent`, `UI卡顿`

### 问题描述

Copilot 在 Agent 模式下变得"极其缓慢"，整个 UI 都在卡顿。DevTools 显示"potential listener LEAK detected, having 208 listeners already"错误。用户在 4 月 1 日确认同样的问题，简单提示也要等至少 20 分钟，"比拨号还慢"。

### 临时解决方案

- 禁用 VS Code 中的 Git
- 关闭 Explorer 面板
- 减少工作区文件夹数量（降低文件监视器负载）

---

## 案例 14：Agent Session 变成不可读

**来源**：[microsoft/vscode#310884](https://github.com/microsoft/vscode/issues/310884)

**日期**：2026-04-17

**类型**：`bug`

**标签**：`Agent`, `会话`, `远程`

### 问题描述

每次打开远程服务器窗口时，Agent Session 都会变成不可读状态。影响 Copilot Agent 连接远程 VS Code 使用时的功能。

---

## 案例 15：新版本 Copilot 表现极差

**来源**：[microsoft/vscode#310061](https://github.com/microsoft/vscode/issues/310061)

**日期**：2026-04-16

**类型**：`bug`

**标签**：`质量问题`, `版本更新`

### 问题描述

新版本 Copilot 思维极慢，把问题放到输出里回答，话题中途切换，不完整完成请求的任务。

### 表现

- 响应时间慢
- 把用户问题放到答案里
- 任务中途跑偏
- 不完整执行

---

## 案例 16：Copilot 彻底不工作

**来源**：[microsoft/vscode#310203](https://github.com/microsoft/vscode/issues/310203)

**日期**：2026-04-15

**类型**：`bug`

**标签**：`完全不工作`, `系统资源`

### 问题描述

Copilot 完全停止工作。无具体错误，但系统显示高负载（82, 99, 55）和低内存（0.31GB 可用）。

---

## 案例 17：Copilot Chat 显示"Language Model unavailable"

**来源**：[microsoft/vscode#307265](https://github.com/microsoft/vscode/issues/307265)

**日期**：2026-04-09

**类型**：`bug`

**标签**：`认证`, `LM不可用`

### 问题描述

在 VSCode 中尝试使用 Copilot 时收到"Language Model unavailable"错误，阻止所有 Copilot 功能。

---

## 案例 18：Copilot 在"evaluating"阶段卡住

**来源**：[microsoft/vscode#308246](https://github.com/microsoft/vscode/issues/308246)

**日期**：2026-04-07

**类型**：`bug`

**标签**：`Agent`, `冻结`, `文件扫描`

### 问题描述

Copilot 在"evaluating and looking through files"阶段挂起。用户不得不放弃 token，因为它一直停在文件扫描阶段。过夜放置仍然卡住。

---

## 案例 19：Copilot CLI 无响应

**来源**：[microsoft/vscode#306609](https://github.com/microsoft/vscode/issues/306609)

**日期**：2026-04-14

**类型**：`bug`

**标签**：`CLI`, `无响应`, `高级功能`

### 问题描述

使用多个高级请求后，终端没有任何输出。高级功能似乎消耗了请求但不产生任何可见结果。

---

## 案例 20：扩展提示更新但扩展已安装

**来源**：[microsoft/vscode#307841](https://github.com/microsoft/vscode/issues/307841)

**日期**：2026-04-13

**类型**：`bug`

**标签**：`版本`, `扩展`, `冲突`

### 问题描述

系统反复提示用户更新 VS Code 才能使用 Copilot 扩展，尽管扩展看起来已经安装好了。扩展版本不匹配提示。
