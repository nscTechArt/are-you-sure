# Are You Sure?

English | [简体中文](./README.zh.md)

**Challenge a proposed engineering solution before adopting it.**

## The problem

Coding agents often hand you a concrete plan and keep going. They make assumptions without checking, overbuild when a smaller fix would do, and sometimes change unrelated code as a side effect — without a dissenting second pass before you adopt the idea.

## The three dimensions

This skill challenges that plan across:

| Dimension | What it asks |
| --- | --- |
| **Effectiveness** | Does this actually achieve the stated goal under the hard constraints? |
| **Simplicity** | Is the added complexity necessary versus the strongest credible lower-complexity baseline? |
| **Consequences** | What material new liabilities would adoption create (regressions, lifecycle cost)? |

It treats the proposal as a hypothesis to falsify, then returns an evidence-backed decision — including *keep it* when the plan survives.

## Install

**Option A: Ask your agent (recommended)**

Tell your coding agent something like:

```text
Install the are-you-sure skill from https://github.com/nscTechArt/are-you-sure
```

**Option B: npx**

```bash
npx skills add nscTechArt/are-you-sure
```

**Option C: Manual copy**

Clone or download this repo, then copy it into your agent's skills directory (for example `~/.agents/skills/are-you-sure`, `~/.claude/skills/are-you-sure`, or `.cursor/skills/are-you-sure`, depending on the tool). The skill entrypoint is `SKILL.md`.

After install, start a **new** agent task so the skill directory reloads.

## Usage

### Example prompts

```text
Use $are-you-sure to challenge your most recent technical recommendation.
```

```text
Before we implement that plan, run are-you-sure on it.
```

### When to use it

- The agent has given a **concrete** plan that claims to solve the goal — not loose brainstorming
- The work is **software engineering or technical-project** scoped
- You want a second pass **before** adopting or implementing

### When not to use it

- There is no concrete proposal yet — only exploration or unselected alternatives
- The question is outside software / technical-project work
- You only want execution, not a challenge
- You need the agent to **implement** changes in the same pass (this skill stays read-only on purpose)

### What you get back

A self-contained recommendation in this shape:

1. **Decision** — one of: Retain, Simplify, Modify, Replace, Reject, Defer
2. **Effectiveness / Simplicity / Consequences** — findings with decisive evidence for each dimension
3. **Revised recommendation** — what to adopt next, without implementing it

## How to know it's working

- You get an explicit **Decision**, including Retain when the plan survives
- Each dimension has a finding tied to **evidence**, not vibes
- It **pushes back** (Simplify / Modify / Replace / Reject) when warranted — not only agreement
- It **stops at the recommendation** and does not start implementing

## License

[MIT](./LICENSE)
