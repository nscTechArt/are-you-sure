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

## How it differs from related projects

`Are You Sure?` overlaps with projects that reduce agent over-engineering, but it operates at a different layer of the workflow.

| Project | Core question | Main intervention point | Primary output |
| --- | --- | --- | --- |
| **Are You Sure?** | **Should this solution be adopted?** | After a concrete proposal exists, before implementation | Evidence-backed Decision and revised recommendation |
| [**Ponytail**](https://github.com/DietrichGebert/ponytail) | **Given the task, what is the smallest correct implementation?** | During task understanding and implementation | Smaller implementation, diff, and dependency surface |
| [**Karpathy Guidelines**](https://github.com/multica-ai/andrej-karpathy-skills) | **How should the coding agent behave while working?** | Throughout coding, editing, and verification | More cautious, simple, surgical, and verifiable execution behavior |

### Compared with Ponytail

Ponytail is primarily about **implementation minimization**. After understanding the problem, it prefers existing code, the standard library, native platform capabilities, and already-installed dependencies before writing the minimum custom code. It can also challenge unnecessary requirements or abstractions through YAGNI, but its main objective is to make the **implementation itself smaller and more direct**.

`Are You Sure?` is primarily about **solution evaluation**. It does not optimize for the shortest code path. Before a Candidate solution is adopted, it independently asks whether the proposal is effective, whether its complexity is justified, and what consequences adoption would create. A complex proposal can legitimately receive **Retain** when the evidence supports it.

In short:

> **Ponytail asks: what is the simplest way to implement this?**  
> **Are You Sure? asks: should we adopt this solution at all?**

Their simplicity reasoning can overlap, but the role is different: Ponytail uses lower-complexity options to **guide implementation**; `Are You Sure?` uses them as baselines to **challenge the proposal**.

### Compared with Karpathy Guidelines

Karpathy Guidelines is a set of **behavioral guidelines** that applies throughout coding work. Its main principles are to surface assumptions and ambiguity before coding, prefer simplicity, make surgical changes, and drive execution with verifiable success criteria. Its goal is to reduce common LLM coding mistakes across the whole path from task understanding to verification.

`Are You Sure?` is a narrower, explicit **independent second-pass review**. It requires a complete, concrete Candidate solution, treats that solution as a hypothesis to falsify, evaluates it across Effectiveness, Simplicity, and Consequences, and returns one explicit **Retain / Simplify / Modify / Replace / Reject / Defer** decision.

In short:

> **Karpathy Guidelines asks: how should a coding agent work?**  
> **Are You Sure? asks: should this specific solution be adopted?**

The two are therefore complementary: Karpathy Guidelines can improve how a solution is proposed and executed; `Are You Sure?` inserts a dedicated **dissenting second pass between proposal and implementation**.

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
