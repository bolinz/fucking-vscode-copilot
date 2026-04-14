---
title: "Use custom instructions in VS Code"
description: "Learn how to create custom instructions for GitHub Copilot Chat in VS Code to ensure AI responses match your coding practices, project requirements, and development standards."
source: "https://code.visualstudio.com/docs/copilot/customization/custom-instructions"
---

Custom instructions enable you to define common guidelines and rules that automatically influence how AI generates code and handles other development tasks. Instead of manually including context in every chat prompt, specify custom instructions in a Markdown file to ensure consistent AI responses that align with your coding practices and project requirements.

You can configure custom instructions to apply automatically to all chat requests or to specific files only. Alternatively, you can manually attach custom instructions to a specific chat prompt.

        Tip
      Use the Chat Customizations editor (Preview) to discover, create, and manage all your chat customizations in one place. Run Chat: Open Chat Customizations from the Command Palette.

        Note
      Custom instructions are not taken into account for inline suggestions as you type in the editor.

Types of instruction files
VS Code supports two categories of custom instructions. If you have multiple instruction files in your project, VS Code combines and adds them to the chat context, no specific order is guaranteed.

Always-on instructions
Always-on instructions are automatically included in every chat request. Use them for project-wide coding standards, architecture decisions, and conventions that apply to all code.

A single .github/copilot-instructions.md file

Automatically applies to all chat requests in the workspace
Stored within the workspace

One or more AGENTS.md files

Useful if you work with multiple AI agents in your workspace
Automatically applies to all chat requests in the workspace or to specific subfolders (experimental)
Stored in the root of the workspace or in subfolders (experimental)

Organization-level instructions

Share instructions across multiple workspaces and repositories within a GitHub organization
Defined at the GitHub organization level

CLAUDE.md file

For compatibility with Claude Code and other Claude-based tools
Stored in the workspace root, .claude folder, or user home directory

File-based instructions
File-based instructions are applied when files that the agent is working on match a specified pattern or if the description matches the current task. Use file-based instructions for language-specific conventions, framework patterns, or rules that only apply to certain parts of your codebase.

One or more .instructions.md files

Conditionally apply instructions based on file type or location by using glob patterns
Stored in the workspace or user profile

        Tip
      Which approach should you use? Start with a single .github/copilot-instructions.md file for project-wide coding standards. Add .instructions.md files when you need different rules for different file types or frameworks. Use AGENTS.md if you work with multiple AI agents in your workspace.

Note: The full content for this page is 57.4KB and exceeds the preview limit. Please fetch the page directly for the complete content.
