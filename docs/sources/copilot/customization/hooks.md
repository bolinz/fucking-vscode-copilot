---
title: "Agent hooks in Visual Studio Code (Preview)"
description: "Hooks enable you to execute custom shell commands at key lifecycle points during agent sessions. Use hooks to automate workflows, enforce security policies, validate operations, and integrate with external tools."
source: "https://code.visualstudio.com/docs/copilot/customization/hooks"
---

Hooks enable you to execute custom shell commands at key lifecycle points during agent sessions. Use hooks to automate workflows, enforce security policies, validate operations, and integrate with external tools.

For background on how hooks fit into the AI customization framework, see Customization concepts.

This article explains how to configure and use hooks in VS Code.

        Note
      Agent hooks are currently in Preview. The configuration format and behavior might change in future releases.

        Important
      Your organization might have disabled the use of hooks in VS Code. Contact your admin for more information. See enterprise policies for details.

        Tip
      Use the Chat Customizations editor (Preview) to discover, create, and manage all your chat customizations in one place. Run Chat: Open Chat Customizations from the Command Palette.

Hooks are designed to work across agent types, including local agents, background agents, and cloud agents. Each hook receives structured JSON input and can return JSON output to influence agent behavior.

Why use hooks?
Hooks provide deterministic, code-driven automation. Unlike instructions or custom prompts that guide agent behavior, hooks execute your code at specific lifecycle points with guaranteed outcomes:

Enforce security policies: Block dangerous commands like rm -rf or DROP TABLE before they execute, regardless of how the agent was prompted.

Automate code quality: Run formatters, linters, or tests automatically after file modifications.

Create audit trails: Log every operation for compliance and debugging.

Note: The full content for this page is 85.4KB and exceeds the preview limit. Please fetch the page directly for the complete content.
