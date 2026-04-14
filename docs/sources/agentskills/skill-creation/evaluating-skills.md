---
title: "Evaluating skill output quality"
description: "How to test whether your skill produces good outputs using eval-driven iteration."
source: "https://agentskills.io/skill-creation/evaluating-skills.md"
---

> ## Documentation Index
> Fetch the complete documentation index at: https://agentskills.io/llms.txt
> Use this file to discover all available pages before exploring further.

# Evaluating skill output quality

> How to test whether your skill produces good outputs using eval-driven iteration.

You wrote a skill, tried it on a prompt, and it seemed to work. But does it work reliably — across varied prompts, in edge cases, better than no skill at all? Running structured evaluations (evals) answers these questions and gives you a feedback loop for improving the skill systematically.

## Designing test cases

A test case has three parts:

* **Prompt**: a realistic user message — the kind of thing someone would actually type.
* **Expected output**: a human-readable description of what success looks like.
* **Input files** (optional): files the skill needs to work with.

Store test cases in `evals/evals.json` inside your skill directory:

```json
{
  "skill_name": "csv-analyzer",
  "evals": [
    {
      "id": 1,
      "prompt": "I have a CSV of monthly sales data in data/sales_2025.csv. Can you find the top 3 months by revenue and make a bar chart?",
      "expected_output": "A bar chart image showing the top 3 months by revenue, with labeled axes and values.",
      "files": ["evals/files/sales_2025.csv"]
    }
  ]
}
```

**Tips for writing good test prompts:**

* **Start with 2-3 test cases.** Don't over-invest before you've seen your first round of results.
* **Vary the prompts.** Use different phrasings, levels of detail, and formality.
* **Cover edge cases.** Include at least one prompt that tests a boundary condition.
* **Use realistic context.** Real users mention file paths, column names, and personal context.

## Running evals

The core pattern is to run each test case twice: once **with the skill** and once **without it** (or with a previous version). This gives you a baseline to compare against.

### Workspace structure

Organize eval results in a workspace directory alongside your skill directory. Each pass through the full eval loop gets its own `iteration-N/` directory.

```
csv-analyzer-workspace/
└── iteration-1/
    ├── eval-top-months-chart/
    │   ├── with_skill/
    │   └── without_skill/
    └── benchmark.json         # Aggregated statistics
```

### Spawning runs

Each eval run should start with a clean context — no leftover state from previous runs or from the skill development process.

For each run, provide:
* The skill path (or no skill for the baseline)
* The test prompt
* Any input files
* The output directory

### Capturing timing data

Timing data lets you compare how much time and tokens the skill costs relative to the baseline. When each run completes, record the token count and duration:

```json
{
  "total_tokens": 84852,
  "duration_ms": 23332
}
```

## Writing assertions

Assertions are verifiable statements about what the output should contain or achieve. Add them after you see your first round of outputs.

Good assertions:
* `"The output file is valid JSON"` — programmatically verifiable.
* `"The bar chart has labeled axes"` — specific and observable.
* `"The report includes at least 3 recommendations"` — countable.

Weak assertions:
* `"The output is good"` — too vague to grade.
* `"The output uses exactly the phrase 'Total Revenue: $X'"` — too brittle.

## Grading outputs

Grading means evaluating each assertion against the actual outputs and recording **PASS** or **FAIL** with specific evidence.

The simplest approach is to give the outputs and assertions to an LLM and ask it to evaluate each one. For assertions that can be checked by code (valid JSON, correct row count, file exists), use a verification script.

**Grading principles:**
* **Require concrete evidence for a PASS.** Don't give the benefit of the doubt.
* **Review the assertions themselves, not just the results.** Notice when assertions are too easy, too hard, or unverifiable.

## Aggregating results

Once every run in the iteration is graded, compute summary statistics per configuration and save them to `benchmark.json`:

```json
{
  "run_summary": {
    "with_skill": {
      "pass_rate": { "mean": 0.83, "stddev": 0.06 },
      "time_seconds": { "mean": 45.0, "stddev": 12.0 },
      "tokens": { "mean": 3800, "stddev": 400 }
    },
    "without_skill": {
      "pass_rate": { "mean": 0.33, "stddev": 0.10 },
      "time_seconds": { "mean": 32.0, "stddev": 8.0 },
      "tokens": { "mean": 2100, "stddev": 300 }
    },
    "delta": {
      "pass_rate": 0.50,
      "time_seconds": 13.0,
      "tokens": 1700
    }
  }
}
```

The `delta` tells you what the skill costs (more time, more tokens) and what it buys (higher pass rate).

## Analyzing patterns

After computing the benchmarks:

* **Remove or replace assertions that always pass in both configurations.**
* **Investigate assertions that always fail in both configurations.**
* **Study assertions that pass with the skill but fail without.** This is where the skill is clearly adding value.
* **Tighten instructions when results are inconsistent across runs.**
* **Check time and token outliers.**

## Reviewing results with a human

Assertion grading and pattern analysis catch a lot, but they only check what you thought to write assertions for. A human reviewer brings a fresh perspective. Record specific feedback for each test case.

## Iterating on the skill

After grading and reviewing, you have three sources of signal:

* **Failed assertions** point to specific gaps.
* **Human feedback** points to broader quality issues.
* **Execution transcripts** reveal *why* things went wrong.

The most effective way to turn these signals into skill improvements is to give all three — along with the current `SKILL.md` — to an LLM and ask it to propose changes.

**Tips for iteration:**
* **Generalize from feedback.** Fixes should address underlying issues broadly.
* **Keep the skill lean.** Fewer, better instructions often outperform exhaustive rules.
* **Explain the why.** Reasoning-based instructions work better than rigid directives.
* **Bundle repeated work.** If every test run independently wrote a similar helper script, that's a signal to bundle it.

**The loop:**
1. Give the eval signals and current `SKILL.md` to an LLM and ask it to propose improvements.
2. Review and apply the changes.
3. Rerun all test cases in a new `iteration-<N+1>/` directory.
4. Grade and aggregate the new results.
5. Review with a human. Repeat.

**Tip:** The [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator) Skill automates much of this workflow.
