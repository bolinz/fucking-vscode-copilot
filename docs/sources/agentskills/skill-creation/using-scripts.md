---
title: "Using scripts in skills"
description: "How to run commands and bundle executable scripts in your skills."
source: "https://agentskills.io/skill-creation/using-scripts.md"
---

> ## Documentation Index
> Fetch the complete documentation index at: https://agentskills.io/llms.txt
> Use this file to discover all available pages before exploring further.

# Using scripts in skills

> How to run commands and bundle executable scripts in your skills.

Skills can instruct agents to run shell commands and bundle reusable scripts in a `scripts/` directory. This guide covers one-off commands, self-contained scripts with their own dependencies, and how to design script interfaces for agentic use.

## One-off commands

When an existing package already does what you need, you can reference it directly in your `SKILL.md` instructions without a `scripts/` directory. Many ecosystems provide tools that auto-resolve dependencies at runtime.

**Tips for one-off commands in skills:**

* **Pin versions** (e.g., `npx eslint@9.0.0`) so the command behaves the same over time.
* **State prerequisites** in your `SKILL.md` (e.g., "Requires Node.js 18+") rather than assuming the agent's environment has them. For runtime-level requirements, use the [`compatibility` frontmatter field](/specification#compatibility-field).
* **Move complex commands into scripts.** A one-off command works well when you're invoking a tool with a few flags. When a command grows complex enough that it's hard to get right on the first try, a tested script in `scripts/` is more reliable.

## Referencing scripts from `SKILL.md`

Use **relative paths from the skill directory root** to reference bundled files. The agent resolves these paths automatically — no absolute paths needed.

List available scripts in your `SKILL.md` so the agent knows they exist:

```markdown
## Available scripts

- **`scripts/validate.sh`** — Validates configuration files
- **`scripts/process.py`** — Processes input data
```

Then instruct the agent to run them:

```markdown
## Workflow

1. Run the validation script:
   ```bash
   bash scripts/validate.sh "$INPUT_FILE"
   ```

2. Process the results:
   ```bash
   python3 scripts/process.py --input results.json
   ```
```

**Note:** The same relative-path convention works in support files like `references/*.md` — script execution paths (in code blocks) are relative to the **skill directory root**, because the agent runs commands from there.

## Self-contained scripts

When you need reusable logic, bundle a script in `scripts/` that declares its own dependencies inline. The agent can run the script with a single command — no separate manifest file or install step required.

Several languages support inline dependency declarations:

### Python (PEP 723)

Declare dependencies in a TOML block inside `# ///` markers:

```python
# /// script
# dependencies = [
#   "beautifulsoup4",
# ]
# ///

from bs4 import BeautifulSoup
```

Run with [uv](https://docs.astral.sh/uv/) (recommended):

```bash
uv run scripts/extract.py
```

* Pin versions with [PEP 508](https://peps.python.org/pep-0508/) specifiers.
* Use `requires-python` to constrain the Python version.
* Use `uv lock --script` to create a lockfile for full reproducibility.

### Deno

Deno's `npm:` and `jsr:` import specifiers make every script self-contained by default:

```typescript
#!/usr/bin/env -S deno run

import * as cheerio from "npm:cheerio@1.0.0";
```

```bash
deno run scripts/extract.ts
```

* Use `npm:` for npm packages, `jsr:` for Deno-native packages.
* Dependencies are cached globally. Use `--reload` to force re-fetch.

### Bun

Bun auto-installs missing packages at runtime when no `node_modules` directory is found:

```typescript
#!/usr/bin/env bun

import * as cheerio from "cheerio@1.0.0";
```

```bash
bun run scripts/extract.ts
```

* No `package.json` or `node_modules` needed. TypeScript works natively.
* If a `node_modules` directory exists anywhere up the directory tree, auto-install is disabled.

### Ruby

Use `bundler/inline` to declare gems directly in the script:

```ruby
require 'bundler/inline'

gemfile do
  source 'https://rubygems.org'
  gem 'nokogiri'
end
```

```bash
ruby scripts/extract.rb
```

## Designing scripts for agentic use

When an agent runs your script, it reads stdout and stderr to decide what to do next. A few design choices make scripts dramatically easier for agents to use.

### Avoid interactive prompts

This is a hard requirement of the agent execution environment. Agents operate in non-interactive shells — they cannot respond to TTY prompts, password dialogs, or confirmation menus. A script that blocks on interactive input will hang indefinitely.

Accept all input via command-line flags, environment variables, or stdin:

```
# Bad: hangs waiting for input
$ python scripts/deploy.py
Target environment: _

# Good: clear error with guidance
$ python scripts/deploy.py
Error: --env is required. Options: development, staging, production.
```

### Document usage with `--help`

`--help` output is the primary way an agent learns your script's interface. Include a brief description, available flags, and usage examples.

### Write helpful error messages

When an agent gets an error, the message directly shapes its next attempt. An opaque "Error: invalid input" wastes a turn. Instead, say what went wrong, what was expected, and what to try.

### Use structured output

Prefer structured formats — JSON, CSV, TSV — over free-form text. Structured formats can be consumed by both the agent and standard tools (`jq`, `cut`, `awk`), making your script composable in pipelines.

**Separate data from diagnostics:** send structured data to stdout and progress messages, warnings, and other diagnostics to stderr.

### Further considerations

* **Idempotency.** Agents may retry commands. "Create if not exists" is safer than "create and fail on duplicate."
* **Input constraints.** Reject ambiguous input with a clear error rather than guessing. Use enums and closed sets where possible.
* **Dry-run support.** For destructive or stateful operations, a `--dry-run` flag lets the agent preview what will happen.
* **Meaningful exit codes.** Use distinct exit codes for different failure types and document them in your `--help` output.
* **Safe defaults.** Consider whether destructive operations should require explicit confirmation flags.
* **Predictable output size.** Many agent harnesses automatically truncate tool output beyond a threshold. If your script might produce large output, default to a summary or a reasonable limit.
