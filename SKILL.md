---
name: are-you-sure
description: Challenge a proposed software-engineering or technical-project solution before adopting it.
---

# Are You Sure?

Act as the dissenting second pass on an Agent's Candidate solution. Treat the Candidate solution as a hypothesis to falsify. The pass succeeds when it produces an evidence-backed Decision, including retaining the Candidate solution when it survives the challenge.

## Operating contract

The user's instructions take precedence over guidelines in this skill. If explicit user instructions conflict with this skill, follow the user's instructions.

Infer the user's intent, review target, and routine scope details from the conversation and available project evidence. Bias toward completing the review rather than asking for clarification. Before asking a question, complete all authorized read-only investigation that could resolve it or make the remaining choice concrete. Do not ask permission for reversible read-only work, review, or safe local diagnostics that are already implied by the task.

Ask a question only when the remaining uncertainty is material: plausible answers would change the Decision or the recommendation. Do not ask non-blocking questions that would not change the outcome.

A Challenge pass is semantically read-only by default. Do not intentionally change source code, configuration, dependencies, version-control state, persistent data, or external systems while producing the review. If the user explicitly asks to review and then implement in the same task, finish the Challenge and state the Decision first, then follow the user's implementation instruction as a separate phase.

Keep the review proportional to the decision. Default to concise findings and expand only when evidence or trade-offs require it.

## 1. Select the review target

A **Candidate solution** is the Agent's most recent complete, concrete proposal that explicitly claims to solve the user's goal. Brainstorming, problem analysis, and unselected alternatives are not Candidate solutions. Treat incremental details as additions to the current Candidate solution unless they form a new complete proposal.

When several proposals appear in the conversation, infer the intended Review target from the strongest available signal, in this order:

1. the solution the user explicitly selected, endorsed, referred to, or asked to implement;
2. the most recent complete proposal that clearly replaces an earlier one;
3. a deliberately inseparable combination, reviewed as one Candidate solution.

Do not ask the user to choose merely because multiple alternatives were mentioned. If two or more independent Candidate solutions remain equally plausible after using the available context, treat the target as a material uncertainty and use **Defer** only if selecting among them could change the review.

If no concrete Candidate solution can be identified after inspecting the available conversation and project context, use **Defer** and name the missing review target. If the work is outside software engineering or technical-project scope, state the boundary and stop without inventing a Decision.

Proceed when one in-scope Review target is selected.

## 2. Establish the grounds

Reconstruct the actual problem, the user's stated goal, and the hard constraints. Express them as the smallest set of observable conditions needed to judge Effectiveness. Ground material conditions in the user's statements or project evidence. Infer routine, low-impact gaps when the context supports a reasonable interpretation; preserve the user's authority over goals, hard constraints, and meaningful trade-offs.

Challenge hidden assumptions and the framing of the problem, but do not manufacture requirements that the user did not state and the project does not imply.

Gather the smallest body of evidence likely to change the Decision from relevant code, call sites, tests, configuration, documentation, or available tools. Check history only when authorship, regression timing, or recency could change the Decision. Stop investigating when additional searching is unlikely to change the outcome.

### Verification

Use the smallest bounded verification appropriate to the Candidate solution and its risk.

- Safe, local, non-networked diagnostics, tests, linters, builds, or reproductions may be run without additional approval when they are proportionate to the review and do not intentionally modify persistent project state.
- Prefer existing checks and small reproductions. Do not create tests merely to mirror a reversible, low-impact implementation detail.
- Once sufficient checks pass, broaden or repeat verification only when new changes, failures, unresolved concerns, or the Candidate's risk profile justify it.
- Long-running, destructive, network-dependent, expensive, or safety-unknown checks require explicit authorization if they are necessary. If they are not necessary to decide, do not block on them; record the limitation only when it remains decision-relevant.
- Put disposable diagnostic artifacts in a dedicated temporary location when possible. Clean up only artifacts known to come from this pass.

If independent subagents are available, use one bounded parallel evidence or counterexample task only when it can materially improve a non-trivial review or reduce anchoring on the Candidate. Do not delegate routine reviews or duplicate work.

Attach the smallest practical reference or reproduction to every non-obvious finding that can change the recommendation. Label unsupported inferences as uncertainties.

Before asking the user anything, exhaust the available evidence that could answer the question. If a material uncertainty still remains, choose **Defer**, name exactly what evidence or user decision is missing, and explain how it could change the outcome. If the user can resolve it, ask one focused question at the end of the Defer recommendation.

Proceed when every decision-changing claim has evidence or an explicit uncertainty.

## 3. Run the challenge

Assess all three dimensions below. Fully report only material findings that can change the Decision. When no material defect is found, name the decisive evidence that supports that conclusion.

Classify each finding by its primary effect: **Effectiveness** if the goal or a hard constraint fails; **Simplicity** if extra machinery is unnecessary; **Consequences** if adoption creates a new material liability. Report a finding fully once rather than repeating it across dimensions.

### Baseline

Use the current behavior or implementation as the default baseline. Investigate it only when confirming it could change the Decision. If the current baseline is not clearly sufficient, search only as far as needed through deliberate deferral, an existing project capability, a standard-library or native-platform capability, an already-installed dependency, and minimal custom machinery. Skip inapplicable options.

A baseline is credible only when evidence shows it can satisfy the goal and hard constraints. Among credible baselines, prefer the one with the lowest necessary complexity. Deliberate deferral is credible only when its rationale and cost are explicit. Do not invent a baseline merely to complete the comparison.

### Effectiveness

Determine whether the Candidate solution achieves the user's stated goal under the hard constraints. Test the assumptions whose failure would invalidate the proposal.

### Simplicity

Determine whether the Candidate solution's added complexity is necessary relative to the strongest credible lower-complexity baseline. Compare extra concepts, abstractions, moving parts, dependencies, and operational demands with the value they add. State when no credible simpler alternative exists.

### Consequences

Determine which material new liabilities adoption would create relative to the baseline, including regressions, coupling, operational burden, migration cost, security or reliability exposure, and lifecycle maintenance that bear on the Decision.

The challenge is complete when all three dimensions have an evidence-backed finding or an explicit uncertainty, and the strongest credible baseline has been compared with the Candidate solution.

## 4. Revise the recommendation

Choose exactly one Decision:

- **Retain**: the Candidate solution survives the challenge; no material change is justified and the baseline is not clearly better.
- **Simplify**: the Candidate solution can achieve the goal, but unnecessary machinery should be removed without changing the core solution path.
- **Modify**: the main direction is sound, but material defects, omissions, or incorrect assumptions require correction.
- **Replace**: the Candidate solution should not be adopted and a concrete, viable alternative should be used instead.
- **Reject**: the Candidate solution should not proceed and no concrete replacement is currently justified.
- **Defer**: a named piece of evidence, review target, constraint, or user trade-off that could change the outcome remains unavailable after the available investigation is exhausted.

Use the user's language for headings, findings, and recommendations. Include the canonical English Decision name in parentheses when useful, for example `修改（Modify）`.

Return a self-contained recommendation that says what to adopt and which parts of the Candidate solution no longer apply. A Retain Decision explains why credible alternatives are not better. A Defer Decision names the exact blocker and, when the user can resolve it, ends with one focused question.

Use this response shape for the review portion:

```markdown
## <localized review decision> (Decision)

**<localized outcome> (<Retain | Simplify | Modify | Replace | Reject | Defer>)**

## <localized challenge>

### <localized effectiveness>

<finding and decisive evidence>

### <localized simplicity>

<finding, baseline comparison, and decisive evidence>

### <localized consequences>

<finding and decisive evidence>

## <localized revised recommendation>

<self-contained, actionable recommendation; for Defer, include the exact blocker and one focused question when applicable>
```

Default to one concise paragraph per challenge dimension. Expand only material findings whose evidence or trade-offs require explanation. Integrate references with the claims they support. Report findings and evidence, not a private reasoning trace or a transcript of tool use.

If the user explicitly requested implementation after the review, complete the review portion first and then carry out that authorized work separately.