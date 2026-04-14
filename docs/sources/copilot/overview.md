---
title: "GitHub Copilot in VS Code"
description: "VS Code 中的 GitHub Copilot 概述：Agent、内联建议、Chat 和自定义配置"
source: "https://code.visualstudio.com/docs/copilot/overview"
---

# GitHub Copilot in VS Code

GitHub Copilot brings AI agents to Visual Studio Code. Describe what you want to build, and an agent plans the approach, writes the code, and verifies the result across your entire project. Choose from Copilot's built-in agents, third-party agents from providers like Anthropic and OpenAI, or your own custom agents, and run them locally, in the background, or in the cloud. For more targeted changes, inline suggestions and chat give you precise control directly in the editor.

## Agents

An agent is an AI assistant that works autonomously to complete a coding task. Unlike traditional code completion, which suggests the next few lines, an agent takes a goal, breaks it into steps, edits files across your project, runs commands, and self-corrects when something goes wrong.

Give an agent a high-level description of what you want to build and it gets to work. Each task runs inside an **agent session**, a persistent conversation you can track, pause, resume, or hand off to another agent.

> **Important**: Your organization might have disabled agents in VS Code. Contact your admin to enable this functionality.

### Plan before you build

Use the built-in **Plan** agent to break a task into a structured implementation plan before writing any code. The Plan agent analyzes your codebase, asks clarifying questions, and produces a step-by-step plan. When the plan looks right, hand it off to an implementation agent to execute it, locally, in the background, or in the cloud.

Learn more about [planning with agents](agents/planning).

### Run agents anywhere

Agents run where the work needs to happen. Run them locally in VS Code for interactive work, in the background for autonomous tasks, or in the cloud for team collaboration through pull requests. You can also use third-party agents from providers like Anthropic and OpenAI. At any point, hand off a task from one agent type to another and the relevant context carries over.

Learn more about [agent types and delegation](agents/overview) or follow the [agents tutorial](agents/agents-tutorial).

### Manage sessions from a central view

Run multiple agent sessions in parallel, each focused on a different task. The **Sessions** view in the **Chat** panel gives you a single place to monitor all active sessions, whether they run locally, in the background, or in the cloud. See the status of each session, switch between them, review file changes, and pick up where you left off.

Learn more about [managing agent sessions](chat/chat-sessions).

## What can you build

Agents handle coding tasks end-to-end, from a single file change to a full feature shipped as a pull request.

- **Build a feature end-to-end.** Describe a feature in natural language and the agent scaffolds the project, implements the logic across multiple files, and runs tests to verify the result.
- **Debug and fix failing tests.** Point an agent at a failing test and it reads the error, traces the root cause across your codebase, applies a fix, and re-runs the test to confirm. Learn more about [debugging with AI](guides/debug-with-copilot).
- **Refactor or migrate a codebase.** Ask an agent to plan a migration, for example, from one framework to another, and it applies coordinated changes across files while verifying with builds.
- **Test and interact with web apps.** *(Experimental)* Ask an agent to open your web app in the [integrated browser](https://code.visualstudio.com/docs/debugtest/integrated-browser), verify a feature works, check for layout issues, or take screenshots. Follow the [browser agent testing guide](guides/browser-agent-testing-guide).
- **Collaborate via pull requests.** Delegate a task to a cloud agent that creates a branch, implements the changes, and opens a pull request for your team to review. Learn more about [cloud agents](agents/cloud-agents).

## Getting started

### Step 1: Set up Copilot

1. Hover over the Copilot icon in the Status Bar and select **Set up Copilot**.
2. Choose a sign-in method and follow the prompts. If you don't have a Copilot subscription yet, you are signed up for the [Copilot Free plan](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-as-an-individual-subscriber/managing-copilot-free/about-github-copilot-free).

### Step 2: Start your first agent session

1. Open the **Chat** view (`Ctrl+Alt+I` on Windows/Linux, `Ctrl+Cmd+I` on macOS).
2. Enter a prompt that describes what you want to build, for example:

   ```
   Create a basic Node.js web app for sharing recipes. Make it look modern and responsive.
   ```

3. Review the generated code. The agent creates files, installs dependencies, and runs commands as needed.
4. Enter `/init` to configure your project for AI. This creates [custom instructions](customization/custom-instructions) that help the agent understand your codebase and generate better code.

For a full hands-on tutorial covering inline suggestions, agents, inline chat, and customization, see [Get started with GitHub Copilot in VS Code](getting-started).

## AI assistance as you type

For smaller changes or when you want more precise control, Copilot assists directly in the editor.

### Inline suggestions

Copilot provides code suggestions as you type, from single-line completions to full function implementations. Next edit suggestions predict the next logical change based on your current edits.

Learn more about [inline suggestions in VS Code](ai-powered-suggestions).

### Inline chat

Press `Ctrl+I` (Windows, Linux) or `Cmd+I` (macOS) to open a chat prompt directly in the editor. Describe a change, and Copilot suggests edits in place, so you stay in the flow of coding. Use it for targeted refactors, explanations, or quick fixes without switching context.

Learn more about [inline chat in VS Code](chat/inline-chat).

### Smart actions

VS Code includes predefined AI-powered actions for common tasks: generating commit messages, renaming symbols, fixing errors, and running semantic search across your project.

Learn more about [smart actions in VS Code](copilot-smart-actions).

## Customize AI for your workflow

Agents work best when they understand your project's conventions, have the right tools, and use a model suited to the task. VS Code gives you several ways to [tailor the AI](customization/overview) so it produces code that fits your codebase from the start, instead of requiring manual corrections after the fact.

- **[Custom instructions](customization/custom-instructions)**: Define project-wide coding conventions so the AI generates code that matches your style.
- **[Agent skills](customization/agent-skills)**: Teach Copilot specialized capabilities that work across VS Code, GitHub Copilot CLI, and GitHub Copilot cloud agent.
- **[Custom agents](customization/custom-agents)**: Create agents that assume a specific role, such as a code reviewer or documentation writer, with their own tools and instructions.
- **[MCP servers](customization/mcp-servers)**: Extend agents with tools from MCP servers or Marketplace extensions.
- **[Hooks](customization/hooks)**: Execute custom commands at specific events for automation and policy enforcement.

## Support

Support for GitHub Copilot Chat is provided by GitHub and can be reached at https://support.github.com.

To learn more about Copilot's security, privacy, compliance, and transparency, see the [GitHub Copilot Trust Center FAQ](https://copilot.github.trust.page/faq).

## Pricing

You can start using GitHub Copilot for free with monthly limits on inline suggestions and chat interactions. For more extensive usage, you can choose from various paid plans.

[View detailed GitHub Copilot pricing](https://docs.github.com/en/copilot/get-started/plans)
