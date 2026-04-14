---
title: "Agent Skills Documentation"
description: "Overview of Agent Skills format - A simple, open format for giving agents new capabilities and expertise."
source: "https://agentskills.io/home.md"
---

> ## Documentation Index
> Fetch the complete documentation index at: https://agentskills.io/llms.txt
> Use this file to discover all available pages before exploring further.

# Overview

> A simple, open format for giving agents new capabilities and expertise.

Agent Skills are folders of instructions, scripts, and resources that agents can discover and use to do things more accurately and efficiently.

## Why Agent Skills?

Agents are increasingly capable, but often don't have the context they need to do real work reliably. Skills solve this by giving agents access to procedural knowledge and company-, team-, and user-specific context they can load on demand. Agents with access to a set of skills can extend their capabilities based on the task they're working on.

**For skill authors**: Build capabilities once and deploy them across multiple agent products.

**For compatible agents**: Support for skills lets end users give agents new capabilities out of the box.

**For teams and enterprises**: Capture organizational knowledge in portable, version-controlled packages.

## What can Agent Skills enable?

* **Domain expertise**: Package specialized knowledge into reusable instructions, from legal review processes to data analysis pipelines.
* **New capabilities**: Give agents new capabilities (e.g. creating presentations, building MCP servers, analyzing datasets).
* **Repeatable workflows**: Turn multi-step tasks into consistent and auditable workflows.
* **Interoperability**: Reuse the same skill across different skills-compatible agent products.

## Adoption

Agent Skills are supported by leading AI development tools.

## Open development

The Agent Skills format was originally developed by [Anthropic](https://www.anthropic.com/), released as an open standard, and has been adopted by a growing number of agent products. The standard is open to contributions from the broader ecosystem.

Come join the discussion on [GitHub](https://github.com/agentskills/agentskills) or [Discord](https://discord.gg/MKPE9g8aUy)!

## Get started

* [What are skills?](/what-are-skills) - Learn about skills, how they work, and why they matter.
* [Specification](/specification) - The complete format specification for SKILL.md files.
* [Add skills support](/client-implementation/adding-skills-support) - Add skills support to your agent or tool.
* [Example skills](https://github.com/anthropics/skills) - Browse example skills on GitHub.
* [Reference library](https://github.com/agentskills/agentskills/tree/main/skills-ref) - Validate skills and generate prompt XML.
