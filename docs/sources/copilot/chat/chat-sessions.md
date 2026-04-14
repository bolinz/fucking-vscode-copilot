---
title: "Manage chat sessions"
description: "Learn how to create and manage chat sessions in Visual Studio Code, including the sessions list, opening chat in editor tabs, separate windows, and using chat session history."
source: "https://code.visualstudio.com/docs/copilot/chat/chat-sessions"
---

Use chat in Visual Studio Code to have conversation-based AI interactions. A chat session consists of the sequence of prompts and responses between you and the AI, along with any relevant context from your code or files. This article describes how to create and manage chat sessions, use the sessions list, and organize your sessions.

What is a chat session?
A chat session is a single conversation with the AI, including all prompts, responses, and context. Each session is independent, so context from one session does not carry over to another.
Key things to know about chat sessions:

Context window: as you chat, the session accumulates context. Creating a new session clears the history and starts a fresh context window. You can monitor context window usage in the chat input box.
Checkpoints: at any time, you can roll back to a previous state or edit a previous prompt to change direction. Learn more about checkpoints.
Agent types: sessions can run locally, in the background, or in the cloud. Learn more about agent types.
Multiple sessions: you can run multiple sessions in parallel, each focused on a different task. Use the sessions list to monitor ongoing sessions and switch between them.

        Tip
      Start a new chat session when you want to change topics to help the AI provide more relevant responses.
Start a new chat session
When you start a new chat session, you begin a new conversation with the AI. Each session has its own context window and can run with a different agent type and permission level. You can run multiple sessions in parallel, each focused on a different task or topic. Use the sessions list to monitor and switch between your active sessions.
To start a new chat session:

Use the New Chat (+) button in the Chat view, or use the keyboard shortcut ⌘N (Windows, Linux Ctrl+N).

Select where you want to run the agent by using the Agent Target dropdown. For example, select Local to run the agent interactively in the editor with full access to your workspace, tools, and models.

Select an agent from the Agent dropdown to choose the role or persona for the session. For example, select Plan to have the agent create a structured implementation plan before writing any code. You can switch between agents at any time during a session.

Optionally, select a permission level from the Permissions dropdown to control how much autonomy the agent has over tool approvals.

Type your prompt in the chat input box and press Enter to submit it.

Learn more about agent targets, agents, and permission levels in the agents overview.
Where to open a chat session
You can open chat sessions in different views, depending on how you prefer to work.

Side bar (default): select New Chat (+) > New Chat, or run the Chat: New Chat command. Best for keeping chat visible alongside your code.

Editor tab: select New Chat (+) > New Chat Editor, or run the Chat: New Chat Editor command. Best for giving chat more space or comparing sessions side by side.

Separate window: select New Chat (+) > New Chat Window, or run the Chat: New Chat Window command. Best for multi-monitor setups.

Maximized: select the Maximize button in the Chat view title bar, or run the View: Toggle Maximized Panel command. The chat takes over the full editor area, giving it maximum space. Select the button again to restore the previous layout.

You can move a chat session between views at any time. Select the ... menu in the Chat view, editor tab, or chat window and choose one of the Move Chat into... options. Alternatively, use the corresponding Chat: Move Chat commands from the Command Palette.
Sessions list
The Chat view gives you a unified view to manage all your sessions, regardless of where they run. It shows your sessions with information about their status, type, and file changes.
Use the find and filter options to find specific sessions. You can also pin important sessions to keep them at the top of the list for easy access. Hover over a session and select the pin icon to pin or unpin it.
The list of sessions is scoped to your workspace. If you don't have a workspace open, the list shows all sessions across your workspaces. The sessions are grouped by time periods, such as Today or Last Week.

Right-click a session in the list to see additional actions, such as different options to open the session details, archive the session, or agent-type specific actions like checking out a pull request (for cloud agent sessions).
To hide the session list from the Chat view, right-click in an empty chat and unselect Show Sessions (

      chat.viewSessions.enabled

  ).
The Chat view operates in two modes: compact and side-by-side. You can manually switch between compact and side-by-side mode by using the toggle control in the top-right corner of the Chat view.

Compact:
In compact view, the list of sessions is embedded in the Chat view. When you select a session from the list, the Chat view switches to that session. Use the back button to return to the sessions list.

Side-by-side
In side-by-side view, the list of sessions is shown side-by-side with the Chat view. Select a session from the list to view its details in the Chat view.

When you make the Chat view wider, it automatically switches to side-by-side mode. Right-click on the sessions list and select Sessions Orientation to change this behavior (

      chat.viewSessions.orientation

  ). You can also use the toggle button.

        Note
      Extension developers can learn how to integrate with the sessions view by using the proposed API chatSessionsProvider. The API is currently in a proposed state and subject to change.
Session status indicator (Experimental)
The session status indicator provides quick access to your sessions directly from the command center in the title bar. The indicator displays visual badges for unread messages and in-progress sessions, so you can stay informed about AI activity without switching views.

The indicator shows:

Unread sessions badge: shows the count of chat sessions with new messages. Select the badge to filter the sessions list to show only unread sessions.
In-progress sessions badge: shows the count of sessions with running agents. Select the badge to filter the sessions list to show only in-progress sessions.
Sparkle icon: provides quick access to chat and session management options.

You can configure the status indicator's visibility by using the

      chat.agentsControl.enabled

   setting.
View sessions on the welcome page
The VS Code welcome page can act as your startup experience for working with chat sessions. It provides quick access to your recent chat sessions, an embedded chat widget for starting new tasks, and quick actions for common tasks.
To configure the VS Code welcome page as your startup experience, set

      workbench.startupEditor

   to agentSessionsWelcomePage.
Archive sessions
To keep the sessions list organized, archive completed or inactive sessions. Archiving a session does not delete it but moves it out of the active sessions list. At any time, you can unarchive a session to restore it to the active sessions list.
To archive a session, hover over the session in the sessions list and select Archive. After you archive a session, it disappears from the list. Inversely, you can also unarchive a session in the same way.

To view your archived sessions, use the filter options in the sessions list and select the Archived filter.
Delete sessions
To permanently delete a session, right-click the session in the sessions list and select Delete. Deleting a session removes it permanently and can't be undone. For Copilot CLI sessions, deleting the session also removes any associated worktrees created for that session.

        Caution
      Deleting a session is irreversible. If you just want to hide a session, consider archiving it instead.
Send messages while a request is running
You don't have to wait for a response to finish before sending your next message. While a request is in progress, the Send button changes to a dropdown that gives you three options for how to handle the new message.

Add to Queue: your message waits and sends automatically after the current response completes. The current response finishes uninterrupted.
Steer with Message: signals the current request to yield after finishing the current tool execution. The current response stops and your new message processes immediately. Use this to redirect the agent when it's heading in the wrong direction.
Stop and Send: cancels the current request entirely and sends your new message right away.

The default action for the Send button is configurable. Use

      chat.requestQueuing.defaultAction

   to set it to steer (default) or queue.
Reorder pending messages
When you have multiple pending messages (queued or steering), you can drag and drop them to change the order in which they are processed. A drag handle appears on hover when more than one message of the same type is pending.

Fork a chat session
Forking a chat session creates a new, independent session that inherits the conversation history from the original session. The forked session is fully separate from the original, so changes in one session do not affect the other. The new session title is prefixed with "Forked:" to help you identify it.
Forking is useful when you want to explore an alternative approach, ask a side question, or branch a long conversation in a different direction without losing the original context.
There are two ways to fork a chat session:

Fork the entire session: type /fork in the chat input box and press Enter. A new session opens with the full conversation history copied from the current session.

Fork from a checkpoint: hover over a chat request in the conversation and select the Fork Conversation button. A new session opens that includes only the requests up to and including that checkpoint.

Get notified about chat responses
When you're working in another window or application, VS Code can send you OS notifications to let you know about important chat events, so you don't have to keep checking back.
Use

      chat.notifyWindowOnResponseReceived

   to configure when you receive an OS notification when a chat response is received. The notification includes a preview of the response, and selecting it brings focus to the chat session.
Use

      chat.notifyWindowOnConfirmation

   to configure when you receive an OS notification when the agent needs your input or confirmation to continue.
Both settings have three possible values:

off: never show notifications
windowNotFocused (default): show notifications only when the VS Code window is not focused
always: show notifications even when the VS Code window is in focus

        Tip
      Set the value to always if you want to stay aware of chat activity while working in other parts of VS Code, such as when running long agent tasks in the background.
Navigate between prompts in a chat session
Use the following keyboard shortcuts to navigate between prompts in a chat session:

⌥⌘↑ (Windows, Linux Ctrl+Alt+Up): Go to the previous prompt in the chat session.
⌥⌘↓ (Windows, Linux Ctrl+Alt+Down): Go to the next prompt in the chat session.
⌥⌘PageUp (Windows, Linux Ctrl+Alt+PageUp): Go to the previous code block in the chat session.
⌥⌘PageDown (Windows, Linux Ctrl+Alt+PageDown): Go to the next code block in the chat session.

Save and export chat sessions
You can save chat sessions to preserve important conversations or reuse them later for similar tasks.
Export a chat session as a JSON file
You can export a chat session to save it for later reference or share it with others. Exporting a chat session creates a JSON file that contains all prompts and responses from the session.
To export a chat session:

Open the chat session you want to export in the Chat view.

Run the Chat: Export Chat... command from the Command Palette (⇧⌘P (Windows, Linux Ctrl+Shift+P)).

Choose a location to save the JSON file.

Save a chat session as a reusable prompt
You can save a chat session as a reusable prompt to reuse for similar tasks.
To save a chat session as a reusable prompt:

Open the chat session you want to save in the Chat view.

Type /savePrompt in the chat input box and press Enter.
The command creates a .prompt.md file, which is a reusable prompt file that generalizes your current chat conversation into a template with placeholders. You can use prompt files to run the same type of task across different projects or codebases.

Review and edit the generated prompt file as needed, then save it to your workspace.

Copy chat messages as Markdown
The Chat view supports different options for copying chat messages as Markdown to the clipboard, available through the context menu when you right-click a message or the chat background.

Copy: Copy an individual prompt or response to the clipboard - the Markdown contains the response text, thinking steps, and tool calls.

Copy All: Copy the entire chat session in Markdown format, including all prompts, responses, thinking steps, and tool calls.

Copy Final Response: Copy just the final Markdown section of the agent's response, after the last tool call. This is useful for sharing or reusing the final output without the intermediate steps.

Chat overview
Agents overview
Manage context for AI
Best practices for using AI

                4/8/2026