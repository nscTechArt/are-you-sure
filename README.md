# Are You Sure?

English | [简体中文](./README.zh.md)

**Challenge a proposed engineering solution before adopting it.**

## The problem

Coding agents often move from a concrete recommendation directly into implementation. The recommendation may rely on unchecked assumptions, add unnecessary machinery, or create lifecycle costs that were never compared with a simpler baseline. `Are You Sure?` inserts a dissenting second pass before adoption.

## The three dimensions

| Dimension | What it asks |
| --- | --- |
| **Effectiveness** | Does the proposal actually achieve the stated goal under the hard constraints? |
| **Simplicity** | Is its added complexity necessary versus the strongest credible lower-complexity baseline? |
| **Consequences** | What material new liabilities would adoption create? |

The skill treats the proposal as a hypothesis to falsify, gathers the smallest evidence likely to change the outcome, and returns one evidence-backed Decision: **Retain / Simplify / Modify / Replace / Reject / Defer**.

## Review behavior

The review is designed to complete useful work before asking the user for more input.

- It infers the intended Candidate solution from the conversation instead of asking merely because multiple alternatives were mentioned.
- It inspects available code, call sites, tests, configuration, and documentation before treating a gap as blocking.
- It asks only when the remaining uncertainty is material and plausible answers would change the Decision.
- It uses the smallest bounded verification appropriate to the proposal; safe local read-only checks do not require an extra approval step.
- It keeps the Challenge semantically read-only by default. If the user explicitly asks for review followed by implementation, the review and Decision come first, then the authorized implementation can continue as a separate phase.
- Explicit user instructions take precedence over skill guidelines.

## How it differs from related projects

`Are You Sure?` overlaps with projects that reduce agent over-engineering, but it operates at a different layer of the workflow.

| Project | Core question | Main intervention point | Primary output |
| --- | --- | --- | --- |
| **Are You Sure?** | **Should this solution be adopted?** | After a concrete proposal exists, before implementation | Evidence-backed Decision and revised recommendation |
| [**Ponytail**](https://github.com/DietrichGebert/ponytail) | **Given the task, what is the smallest correct implementation?** | During task understanding and implementation | Smaller implementation, diff, and dependency surface |
| [**Karpathy Guidelines**](https://github.com/multica-ai/andrej-karpathy-skills) | **How should the coding agent behave while working?** | Throughout coding, editing, and verification | More cautious, simple, surgical, and verifiable execution behavior |

### Compared with Ponytail

Ponytail is primarily about **implementation minimization**. It prefers existing code, the standard library, native platform capabilities, and already-installed dependencies before writing the minimum custom code.

`Are You Sure?` is about **solution evaluation**. It asks whether a specific Candidate solution is effective, whether its complexity is justified, and what consequences adoption would create. A complex proposal can legitimately receive **Retain** when the evidence supports it.

> **Ponytail asks: what is the simplest way to implement this?**  
> **Are You Sure? asks: should we adopt this solution at all?**

### Compared with Karpathy Guidelines

Karpathy Guidelines is a set of behavioral principles that applies throughout coding work. `Are You Sure?` is a narrower, explicit second-pass review after a concrete proposal exists.

> **Karpathy Guidelines asks: how should a coding agent work?**  
> **Are You Sure? asks: should this specific solution be adopted?**

The two are complementary: one guides execution behavior; the other independently challenges the proposed solution before adoption.

## Install

**Option A: Ask your agent (recommended)**

```text
Install the are-you-sure skill from https://github.com/nscTechArt/are-you-sure
```

**Option B: npx**

```bash
npx skills add nscTechArt/are-you-sure
```

**Option C: Manual copy**

Clone or download this repository and copy it into your agent's skills directory, for example `~/.agents/skills/are-you-sure`, `~/.claude/skills/are-you-sure`, or `.cursor/skills/are-you-sure`. The entrypoint is `SKILL.md`.

After installation, start a new agent task so the skill directory reloads.

## Usage

### Example prompts

```text
Use $are-you-sure to challenge your most recent technical recommendation.
```

```text
Before we implement that plan, run are-you-sure on it.
```

```text
Run are-you-sure on that plan, then implement the revised recommendation.
```

### When to use it

- The agent has produced a concrete solution that claims to solve the goal.
- The work is software-engineering or technical-project scoped.
- You want an independent challenge before adoption or implementation.

### When not to use it

- There is no concrete proposal yet and you only want open-ended exploration.
- The work is outside software engineering or technical-project scope.
- You only want execution and do not want the solution challenged.

### What you get back

1. **Decision** — Retain, Simplify, Modify, Replace, Reject, or Defer.
2. **Effectiveness / Simplicity / Consequences** — concise findings tied to decisive evidence.
3. **Revised recommendation** — the self-contained recommendation that remains after the challenge.

A **Defer** result is reserved for a named uncertainty that could actually change the outcome after available evidence has been exhausted; it is not the default response to routine ambiguity.

## How to know it is working

- A clear Candidate is reviewed without unnecessary clarification.
- Each decision-changing claim is tied to project evidence or marked as an explicit uncertainty.
- A credible lower-complexity baseline is considered rather than invented for the sake of comparison.
- The skill can return **Retain** when the proposal survives, and pushes back with **Simplify / Modify / Replace / Reject** when evidence warrants it.
- Verification stays proportional to the decision instead of expanding automatically.
- The final response is concise by default and does not repeat the same finding across dimensions.

Behavioral regression cases are documented in [`evals/behavior.md`](./evals/behavior.md).

## License

[MIT](./LICENSE)
