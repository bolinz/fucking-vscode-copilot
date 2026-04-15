---
title: "What is Continue?"
description: "Continue runs AI checks on every pull request. Each check is a markdown file in your repo that shows up as a GitHub status check."
source: "https://docs.continue.dev/"
---

Continue runs AI checks on every pull request. Each check is a markdown file in your repo that shows up as a GitHub status check — green if the code looks good, red with a suggested fix if not.

## Quickstart

Go to [continue.dev/check](https://continue.dev/check) to run checks on a pull request.

## How it works

You define checks as markdown files in `.continue/checks/`. Each file has a name, a description, and a prompt that tells the AI what to look for.

```markdown
---
name: Security Review
description: Flag hardcoded secrets and missing input validation
---

Review this pull request for security issues.

Flag as failing if any of these are true:

- Hardcoded API keys, tokens, or passwords in source files
- New API endpoints without input validation
- SQL queries built with string concatenation
- Sensitive data logged to stdout

If none of these issues are found, pass the check.
```

When a PR is opened, Continue runs each check against the diff and reports the result as a GitHub status check. If a check fails, it suggests a fix you can accept or reject directly from GitHub.