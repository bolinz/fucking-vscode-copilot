---
title: "Optimizing skill descriptions"
description: "How to improve your skill's description so it triggers reliably on relevant prompts."
source: "https://agentskills.io/skill-creation/optimizing-descriptions.md"
---

> ## Documentation Index
> Fetch the complete documentation index at: https://agentskills.io/llms.txt
> Use this file to discover all available pages before exploring further.

# Optimizing skill descriptions

> How to improve your skill's description so it triggers reliably on relevant prompts.

A skill only helps if it gets activated. The `description` field in your `SKILL.md` frontmatter is the primary mechanism agents use to decide whether to load a skill for a given task. An under-specified description means the skill won't trigger when it should; an over-broad description means it triggers when it shouldn't.

This guide covers how to systematically test and improve your skill's description for triggering accuracy.

## How skill triggering works

Agents use [progressive disclosure](/what-are-skills#how-skills-work) to manage context. At startup, they load only the `name` and `description` of each available skill — just enough to decide when a skill might be relevant. When a user's task matches a description, the agent reads the full `SKILL.md` into context and follows its instructions.

This means the description carries the entire burden of triggering. If the description doesn't convey when the skill is useful, the agent won't know to reach for it.

## Writing effective descriptions

Before testing, it helps to know what a good description looks like. A few principles:

* **Use imperative phrasing.** Frame the description as an instruction to the agent: "Use this skill when..." rather than "This skill does..."
* **Focus on user intent, not implementation.** Describe what the user is trying to achieve, not the skill's internal mechanics.
* **Err on the side of being pushy.** Explicitly list contexts where the skill applies.
* **Keep it concise.** A few sentences to a short paragraph is usually right. The [specification](/specification#description-field) enforces a hard limit of 1024 characters.

## Designing trigger eval queries

To test triggering, you need a set of eval queries — realistic user prompts labeled with whether they should or shouldn't trigger your skill.

```json
[
  { "query": "I've got a spreadsheet with revenue in col C and expenses in col D — can you add a profit margin column?", "should_trigger": true },
  { "query": "whats the quickest way to convert this json file to yaml", "should_trigger": false }
]
```

Aim for about 20 queries: 8-10 that should trigger and 8-10 that shouldn't.

### Should-trigger queries

These test whether the description captures the skill's scope. Vary them along several axes:

* **Phrasing**: some formal, some casual, some with typos or abbreviations.
* **Explicitness**: some name the skill's domain directly, others describe the need without naming it.
* **Detail**: mix terse prompts with context-heavy ones.
* **Complexity**: vary the number of steps and decision points.

### Should-not-trigger queries

The most valuable negative test cases are **near-misses** — queries that share keywords or concepts with your skill but actually need something different.

## Testing whether a description triggers

The basic approach: run each query through your agent with the skill installed and observe whether the agent invokes it.

A query "passes" if:
* `should_trigger` is `true` and the skill was invoked, or
* `should_trigger` is `false` and the skill was not invoked.

### Running multiple times

Model behavior is nondeterministic. Run each query multiple times (3 is a reasonable starting point) and compute a **trigger rate**: the fraction of runs where the skill was invoked.

## Avoiding overfitting with train/validation splits

If you optimize the description against all your queries, you risk overfitting. The solution is to split your query set:

* **Train set (~60%)**: the queries you use to identify failures and guide improvements.
* **Validation set (~40%)**: queries you set aside and only use to check whether improvements generalize.

## The optimization loop

1. **Evaluate** the current description on both train and validation sets.
2. **Identify failures** in the train set.
3. **Revise the description.**
4. **Repeat** steps 1-3 until all train set queries pass or you stop seeing meaningful improvement.
5. **Select the best iteration** by its validation pass rate.

Five iterations is usually enough. If performance isn't improving, the issue may be with the queries rather than the description.

**Tip:** The [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator) Skill automates this loop end-to-end.

## Applying the result

Once you've selected the best description:

1. Update the `description` field in your `SKILL.md` frontmatter.
2. Verify the description is under the [1024-character limit](/specification#description-field).
3. Verify the description triggers as expected. Try a few prompts manually as a quick sanity check.

## Next steps

Once your skill triggers reliably, you'll want to evaluate whether it produces good outputs. See [Evaluating skill output quality](/skill-creation/evaluating-skills) for how to set up test cases, grade results, and iterate.
