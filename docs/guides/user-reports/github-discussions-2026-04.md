---
title: "GitHub Discussions 案例"
description: "来自 GitHub Community 的真实案例"
source: https://github.com/community
date: 2026-04-15
type: collection
---

# GitHub Discussions 案例

> 收集自 GitHub Community 的 Copilot Chat 相关讨论

---

## 案例 1：Chat 返回 410 错误（API 端点废弃）

**来源**：[GitHub Community Discussion #103051](https://github.com/orgs/community/discussions/103051)

**日期**：2024年2月

**类型**：`bug`

**标签**：`认证`, `API`, `废弃`

### 问题描述

Copilot Chat 无法工作，每次都返回：

```
unhandled status from server: 410 {
  "error": {
    "agent-version": "",
    "details": "https://github.blog/changelog/label/copilot/#%F0%9F%8C%90-upcoming-deprecation-of-copilot-chat-api-endpoints",
    "editor-plugin-version": "copilot/0.2.2",
    "editor-version": "vscode/1.79.2",
    "engine": "copilot-chat",
    "message": "copilot-proxy.githubusercontent.com is no longer accepting chat requests. Please update your IDE extension to continue using GitHub Copilot chat."
  }
}
```

### 原因

GitHub 在 2024 年 2 月废弃了旧的 Copilot Chat API 端点（`copilot-proxy.githubusercontent.com`），改为使用 `api.githubcopilot.com`。需要 Copilot Chat 扩展版本 >= 0.8.0 才能使用新端点。

### 解决方案

更新 VS Code 到最新版本（>= 1.86），然后更新 Copilot 扩展到最新版本。

---

## 案例 2：消息发送后立即消失

**来源**：[GitHub Community Discussion #156686](https://github.com/orgs/community/discussions/156686)

**日期**：2025年

**类型**：`bug`

**标签**：`认证`, `试用期`, `会话`

### 问题描述

Copilot Chat 功能出现问题：inline suggestions 正常工作，但 chat 不工作。发送消息后，Copilot 看起来在响应，但响应立即消失。另外，用户名或头像图标也出现问题。

### 可能原因

1. 试用期账户尚未完全解锁 Copilot Chat 后端
2. 存在 session/token 错误，需要干净刷新

### 解决方案

1. **完全退出并清除 Copilot Chat 缓存**
   - 关闭 VS Code
   - 删除文件夹：`%APPDATA%\Code\User\globalStorage\github.copilot-chat`
   - 重新打开 VS Code，重新登录测试

2. **检查 Copilot Chat 是否实际启用**
   ```json
   "gitHub.copilot.enableChat": true
   ```

3. **尝试使用干净的 Profile**
   - 命令面板 → "Profiles: Create Empty Profile"
   - 切换到新 profile，重新启用 Copilot Chat
   - 在新窗口中测试

### 注意

如果订阅尚未正式生效（仍在试用期），后端可能不会立即激活所有功能。有些用户说只有在付款阶段开始后才正常工作。

---

## 案例 3：GitHub Copilot 不存储聊天历史

**来源**：[GitHub Community Discussion #129888](https://github.com/orgs/community/discussions/129888)

**日期**：2025年2月

**类型**：`frustration`

**标签**：`会话`, `历史`, `缺失功能`

### 问题描述

用户询问在哪里可以找到 GitHub Copilot Chat 历史记录。

### 官方回答

GitHub Copilot **不存储** chat 或 agent mode 交互的历史记录。每次建议都是基于当前代码上下文实时生成的。

### 影响

这意味着：

- 无法回顾之前的对话
- 无法基于历史会话继续工作
- 不同 session 之间没有记忆

---

## 案例 4：聊天历史随工作区切换消失

**来源**：[GitHub Community Discussion #68362](https://github.com/orgs/community/discussions/68362)

**类型**：`bug`

**标签**：`会话`, `历史`, `工作区`

### 问题描述

当 VS Code 打开一个文件夹（称为"folder_1"）时与 Copilot 聊天。然后切换到"folder_2"做其他工作，之前的 Copilot 对话就消失了。切换回"folder_1"后，对话又出现了。

### 解决方案

1. 确保 VS Code Insiders 和 GitHub Copilot 扩展都是最新版本
2. 检查 GitHub Copilot 扩展的设置，看是否有保留聊天历史的选项
3. 检查是否按工作区配置了设置
4. 清除缓存/本地存储
5. 确保 GitHub Copilot 已正确通过 GitHub 账户认证

### 注意

切换文件夹可能被解释为切换工作区，如果历史记录不在工作区之间共享，则可能导致历史丢失。

---

## 案例 5：信息消失问题（已知 issue 未完全解决）

**来源**：[GitHub Community Discussion #169560](https://github.com/orgs/community/discussions/169560)

**类型**：`bug`

**标签**：`会话`, `历史`, `丢失`

### 问题描述

VS Code 中 GitHub Copilot Agent 聊天历史突然消失，这是许多用户遇到的令人沮丧的问题。

**已知问题**：

1. **Chat 持久性问题**：许多用户报告聊天历史在 VS Code 重启或空闲后消失，即使只有 Copilot 扩展处于活动状态。这已被正式记录为 issue 并标记为完成，但底层行为仍未解决
2. **工作区依赖性和更新**：聊天历史与单个工作区绑定，意味着在切换项目或扩展更新后可能会消失
3. **扩展更新后的问题**：多次报告 Copilot 扩展更新导致历史被删除或加载失败

### 如何恢复或保存过去的聊天历史

1. **查看 VS Code 的 workspaceStorage**
   - Windows: `C:\Users\<username>\AppData\Roaming\Code\User\workspaceStorage\<hash>\`
   - macOS: `~/Library/Application Support/Code/User/workspaceStorage/<hash>/`
   - 查找包含 JSON 文件的 `chatSessions` 文件夹

2. **检查 state.vscdb SQLite 数据库**
   ```
   sqlite3 ./state.vscdb "SELECT value FROM ItemTable WHERE key = 'interactive.sessions';" > sessions.json
   ```

3. **手动导出活动会话**
   - 命令面板（Ctrl+Shift+P）运行 `Chat: Export Session...`
   - 或在聊天窗口中右键选择"Copy All"

4. **使用第三方工具自动保存**
   - SpecStory 等扩展可自动将 Copilot 聊天历史保存到项目目录
