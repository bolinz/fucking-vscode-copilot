---
title: "Planning with agents in VS Code"
description: "Learn how to use the plan agent for autonomous planning and task management with the todo list in VS Code chat."
source: "https://code.visualstudio.com/docs/copilot/agents/planning"
---

The plan agent enables you to create detailed implementation plans before starting the implementation to ensure all requirements are met. With todo lists, the agent can ensure it stays focused on the overall goals and tracks progress effectively.

For background on how the plan agent fits into the agent architecture, see [Agents concepts](https://code.visualstudio.com/docs/copilot/concepts/agents#_planning).

This article explains how to use the plan agent and todo lists in VS Code.

## How to plan a task

To plan a task, use the built-in **Plan** agent in the Chat view, describe your task, and iterate on the generated plan.

1. Open the Chat view by pressing ⌃⌘I (Windows, Linux Ctrl+Alt+I) and select **Plan** from the agents dropdown
   Alternatively, type `/plan` followed by your task description to switch to the Plan agent and start planning in one step.

2. Enter a high-level task (feature, refactoring, bug, etc.) and submit it. For example:

   ```
   Implement a user authentication system with OAuth2 and JWT
   ```

   Use the `/plan` slash command to start planning directly from the chat input box:

   ```
   /plan Add unit tests for all API endpoints
   ```

3. Answer any clarifying questions the agent asks after researching your task.

4. The Plan agent generates a high-level plan summary, implementation and verification steps. Review the plan draft and submit follow-up prompts to iterate on the plan until it meets your requirements.

5. When the plan is finalized, choose to start the implementation or open the planning prompt in the editor for further review.
   To implement the plan, you can continue in the same session or start a new [Copilot CLI session](https://code.visualstudio.com/docs/copilot/agents/copilot-cli) to implement the plan in the background.

> **Tip** The Plan agent automatically saves its implementation plan to a session memory file (`/memories/session/plan.md`). To access this file, run the **Chat: Show Memory Files** command and select `plan.md` from the list. Session memory is cleared when the conversation ends, so the plan is not available in subsequent sessions.

## Customize planning

You can tailor the planning process to fit your team's workflow:

- **Create a custom planning agent.** Define a [custom agent](https://code.visualstudio.com/docs/copilot/customization/custom-agents) with specific instructions for your planning process, such as enforcing architectural guidelines or requiring specific planning deliverables.

- **Choose models for planning and implementation.** Use the **chat.planAgent.defaultModel** setting to select a default model for the plan agent, and **github.copilot.chat.implementAgent.model** for the implementation step.

- **Add extra tools to the plan agent (experimental).** Use the **github.copilot.chat.planAgent.additionalTools** setting to give the plan agent access to additional tools during the research and planning phases. For example, use an MCP server to connect to internal data sources or tools.

- [Memory in VS Code agents](https://code.visualstudio.com/docs/copilot/agents/memory)
- [Configure tools for agents](https://code.visualstudio.com/docs/copilot/agents/agent-tools)
- [Context engineering user guide](https://code.visualstudio.com/docs/copilot/guides/context-engineering-guide)

*4/8/2026*
