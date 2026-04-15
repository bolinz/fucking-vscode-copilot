---
title: "用户自己能把 VS Code Copilot Chat 调教到什么程度"
description: "从 settings、instructions、prompt files、custom agents、hooks、MCP、sessions 等层面整理普通用户能做的改善手段"
---

# 用户自己能把 VS Code Copilot Chat 调教到什么程度

如果你没有组织管理员权限，也不打算等 GitHub 或 VS Code 官方把 Copilot Chat 修好，那现实问题就变成了：

用户自己到底还能做什么，才能让这个东西少发癫一点？

答案是：能做的其实不少，但重点不是“把它变强”，而是把它的行为收紧、限制住、模板化、可观察化。

这篇文档只关注 **user level 可以自己完成的改善手段**。重点不是空泛建议，而是那些普通用户今天就能改、改了以后确实会影响体验的东西。

## 先定目标：不要试图把它变聪明，要先让它变老实

Copilot Chat 最烦的地方不是单点能力差，而是它太容易在错误理解上持续推进。所以用户级改善的目标应该按下面顺序排：

1. 减少上下文污染。
2. 限制错误执行的幅度。
3. 把重复任务模板化。
4. 让错误过程可观测。
5. 只在必要时再给更多工具和权限。

如果顺序反了，一上来先给更多工具、更多提示、更多自动化，通常只会让它更快地做错事。

## 第一层：先把 Chat 本身用得不那么脏

这是最便宜、收益也最高的一层，而且完全不需要额外插件或管理员权限。

### 1. 把 session 当隔离单元，不要当聊天记录

官方文档已经写得很明白：每个 session 都是独立上下文，而且新 session 会清掉历史上下文窗口。对 Copilot Chat 来说，这不是小功能，而是保命机制。

你应该主动这样用：

- 一个 session 只做一种任务。
- 调研、规划、执行、排障尽量拆开。
- 方向不对时优先 `Steer with Message` 或直接新开 session，不要硬聊。
- 要试不同方案时用 `/fork`，不要在原会话里不断回滚语义。

如果你不这么做，后面所有设置优化都会被上下文污染吃掉。

### 2. 把 sessions list 常驻出来

用户级最值得开的视图设置就是 sessions list。它能强迫你看见“我现在其实已经有 4 个不同任务混在一起了”。

推荐：

- `chat.viewSessions.enabled: true`
- `chat.viewSessions.orientation: "sideBySide"`

这不是 UI 偏好问题，而是上下文卫生问题。

### 3. 改默认排队动作，优先 steer

当 Copilot Chat 正在跑偏时，最有价值的不是再等它说完，而是中途扳方向。

可以考虑设置：

- `chat.requestQueuing.defaultAction: "steer"`

这样你更容易形成一个正确习惯：它一旦开始误读，你就立刻打断并重定向，而不是等它完整做错。

## 第二层：把项目知识写成它必须吞下去的东西

Copilot Chat 最大的幻觉之一，就是让你以为“它已经在 IDE 里了，所以应该天然懂项目”。解决办法不是相信它，而是强制喂上下文。

### 4. 先跑 `/init`，然后认真改 `.github/copilot-instructions.md`

`/init` 不只是新手功能，它是把“项目约束”从口头提示变成稳定输入的最快入口。

你至少应该把这些内容写进去：

- 项目架构边界
- 哪些目录允许改，哪些最好别动
- 测试和构建命令
- 命名、风格、依赖约束
- 典型禁令，例如“不要升级依赖”“不要改数据库 schema”

如果这些信息还停留在你脑子里，Copilot Chat 就一定会猜你想要什么。

### 5. 把 instructions 拆层，而不是全塞进一个文件

官方文档给的方向是对的：项目级规则用 always-on instructions，语言或目录差异用 `.instructions.md`。

用户级更合理的做法是：

- `.github/copilot-instructions.md`：只放全局规则
- `*.instructions.md`：只放目录或语言局部规则
- `AGENTS.md`：只放 agent 级的工作方式或约束

不要把所有东西都堆到一个总文件里，不然它最后只会变成另一种噪音。

### 6. 开启 instruction files 自动应用

如果你已经写了 instructions，却没让它自动应用，那基本等于白写。

推荐设置：

- `chat.includeApplyingInstructions: true`
- `github.copilot.chat.codeGeneration.useInstructionFiles: true`
- `chat.includeReferencedInstructions: false`

最后这一项建议先关，是因为“顺着 Markdown 链接继续自动带更多 instructions”很容易把上下文重新弄脏。

### 7. 如果你经常开子目录工作，打开父仓库定制发现

很多 monorepo 用户在子目录里开 VS Code，结果根目录的 instructions、prompt files、agents 全失效，然后开始骂 Copilot Chat 不认项目。这个锅一半是自己开的方式不对。

如果你的工作方式确实常常是“只开包目录”，可以考虑：

- `chat.useCustomizationsInParentRepositories: true`

前提是你知道自己想继承父仓库定制，而不是盲开这个设置。

## 第三层：把高频任务模板化，不要每次重新教它

如果一种任务你已经踩过两三次坑，就不应该继续每次靠自然语言重新解释。用户级真正高杠杆的改善，不是 prompt 技巧，而是把常见任务固定成模板。

### 8. 把成功会话导出成 `.prompt.md`

官方已经给了 `/savePrompt`。这不是锦上添花，是止损工具。

适合沉淀成 prompt file 的任务：

- 补测试
- 做 review
- 排查某类报错
- 生成变更摘要
- 规划某类固定结构的改动

你的目标不是“存聊天记录”，而是把一次还算成功的会话，抽成一个可重复模板。

### 9. 用 prompt files 固定输入、输出和限制

一个靠谱的 `.prompt.md` 应该至少固定这几样东西：

- 任务类型
- 必须先读哪些文件
- 不允许做什么
- 最终输出格式
- 验证步骤

这样你下一次运行同类任务时，不需要再重新和它讨价还价。

### 10. 把一类任务封成 custom agent，而不是继续靠口头人格设定

很多人会在 prompt 里写“你现在是一个严格 reviewer”“你现在先读代码不要修改”。这类口头人格提示稳定性很差。更靠谱的做法是直接做 custom agent。

适合做成 custom agent 的场景：

- 只读规划 agent
- 最小改动实现 agent
- 只做 code review 的 agent
- 只做日志诊断的 agent

custom agent 的价值，不是人格，而是：

- 工具可以限制
- 模型可以固定
- handoff 可以预设
- 工作方式可以反复复用

### 11. 给 custom agent 限工具，而不是只写“请不要”

这是 user level 最容易被低估的一点。

如果你真想让一个“Planner”只做规划，不要只在正文里写“不要改代码”，而是直接让它只拥有只读工具。  
如果你想让一个“Implementer”收敛，只给它必要的 edit/read/terminal 能力。

Copilot Chat 对“口头约束”的服从远不如对“工具根本不给”的服从。

## 第四层：控制工具，而不是只控制提示词

### 12. 不要手痒给 `Plan` 乱加额外工具

`github.copilot.chat.planAgent.additionalTools` 看起来很诱人，但大多数时候，`Plan` 更需要的是少一点乱探索，而不是多一点能力。

推荐保持：

- `github.copilot.chat.planAgent.additionalTools: []`

你真正想避免的是“它拿着更多工具，在错误前提上做更复杂的规划”。

### 13. 谨慎使用 MCP，而且先从 user profile 装

MCP 是用户级能加的强能力，但也是最容易把 Copilot Chat 变成“拿错工具的自动机”的入口。

更稳的方式是：

- 先在 user profile 安装，不要一上来写进 workspace
- 只装你明确会用的 MCP server
- 进 chat 里手动关掉不需要的工具
- 优先选有明确单一用途的 server

不要为了“更强”一次装一堆 MCP。工具一多，误调用和误规划都会明显变多。

### 14. 给 MCP 上沙箱，别默认全信

如果你用的是本地 stdio MCP server，而且运行环境支持沙箱，就尽量开。

目标很简单：

- 限它能写的目录
- 限它能访问的域名
- 缩小误调用的伤害面

这属于用户自己就能做的最低成本风险控制。

### 15. 需要读外部目录时，用 additional read access，别直接乱开工作区

有些上下文不在当前 workspace，但你又确实想让 agent 读。比起把整个东西乱塞进工作区，更稳的是显式给只读访问。

可考虑：

- `github.copilot.chat.additionalReadAccessFolders`

这样至少你是在声明“只能读这些额外路径”，而不是把边界彻底搞没。

## 第五层：用 hooks 把口头约束变成硬约束

### 16. 对高风险动作，用 hooks 拦，不要只靠提示词

官方 hooks 文档最值得注意的不是“能自动化”，而是“能 deterministic”。

这意味着你可以把这些事变成硬规则：

- 编辑后自动跑 formatter
- 改完代码自动跑最小测试
- 遇到危险命令直接拦
- 记录关键操作日志

如果一个约束你已经反复用口头提示说了三遍，那它更适合被做成 hook。

### 17. 但 hooks 要从最小闭环开始

不要上来就写一套复杂自动化链。更务实的顺序是：

1. 先做一个 PostToolUse formatter hook。
2. 再做一个轻量测试 hook。
3. 最后才考虑命令拦截和审计。

原因很简单：Copilot Chat 本身已经够不稳定了，hook 也写复杂，只会变成“双重不稳定”。

## 第六层：把设置调成“少发癫优先”，不是“能力全开”

### 18. 最值得改的几个设置

如果你只想从 settings 入手，优先级大概是这样：

- `github.copilot.chat.agentDebugLog.enabled: true`
- `github.copilot.chat.agentDebugLog.fileLogging.enabled: true`
- `chat.viewSessions.enabled: true`
- `chat.viewSessions.orientation: "sideBySide"`
- `chat.planAgent.defaultModel`
- `github.copilot.chat.implementAgent.model`
- `chat.agent.maxRequests`
- `github.copilot.chat.summarizeAgentConversationHistory.enabled: true`

其中：

- debug log 决定你出了事以后是不是还看得懂。
- sessions view 决定你是不是还会继续把上下文搅脏。
- maxRequests 决定它能在错误方向上连跑多远。

### 19. 这些设置建议先关着

如果目标是求稳，不是追求炫技，下面这些建议先保守：

- `github.copilot.chat.agent.thinkingTool: false`
- `github.copilot.chat.claudeAgent.allowDangerouslySkipPermissions: false`
- `github.copilot.chat.copilotMemory.enabled: false`

原因分别是：

- 实验能力越多，行为越漂。
- 先跳过权限，通常只是更快做错。
- 跨会话 memory 很容易让你对“它会记住”产生过高期待。

### 20. 一份偏保守的 `settings.json`

```json
{
  "chat.viewSessions.enabled": true,
  "chat.viewSessions.orientation": "sideBySide",
  "chat.requestQueuing.defaultAction": "steer",

  "chat.includeApplyingInstructions": true,
  "chat.includeReferencedInstructions": false,
  "github.copilot.chat.codeGeneration.useInstructionFiles": true,
  "chat.useCustomizationsInParentRepositories": true,

  "chat.planAgent.defaultModel": "Auto (Vendor Default)",
  "github.copilot.chat.implementAgent.model": "",
  "github.copilot.chat.planAgent.additionalTools": [],
  "chat.agent.maxRequests": 16,
  "github.copilot.chat.summarizeAgentConversationHistory.enabled": true,

  "github.copilot.chat.agentDebugLog.enabled": true,
  "github.copilot.chat.agentDebugLog.fileLogging.enabled": true,

  "github.copilot.chat.tools.memory.enabled": true,
  "github.copilot.chat.copilotMemory.enabled": false,

  "github.copilot.chat.claudeAgent.allowDangerouslySkipPermissions": false,
  "github.copilot.chat.agent.thinkingTool": false
}
```

这里故意没有塞太多“看起来很高级”的键，因为 user-level 真正有用的优化，本来就不该靠几十个开关堆出来。

## 最后一句话：用户级改善的核心，不是让它更自由，而是让它更窄

Copilot Chat 的问题，本质上不是“能力不够多”，而是“在边界不清时它也会主动推进”。

所以用户自己能做的最有效改善，不是继续给它更多入口，而是做这些事：

- 把 session 切干净
- 把 instructions 写清楚
- 把高频任务模板化
- 把 custom agent 工具收窄
- 把 hooks 用在最关键的硬约束上
- 把 settings 调成少发癫优先

换句话说，别试图把它调教成天才，先把它调教成一个没那么容易失控的工具。
