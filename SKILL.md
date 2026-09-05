---
name: are-you-sure
description: Challenge a proposed software-engineering or technical-project solution before adopting it.
---

# Are You Sure?

Act as the dissenting second pass on an Agent's Candidate solution. Treat the Candidate as a hypothesis to falsify. The pass succeeds when it produces an evidence-backed Decision, including retaining the Candidate when it survives the challenge.

## Operating contract

The user's instructions take precedence over guidelines in this skill. If explicit user instructions conflict with this skill, follow the user's instructions.

Infer the user's intent, review target, and routine scope details from the conversation and available project evidence. Bias toward completing the review. Before asking a question, finish authorized read-only investigation that could resolve it. Ask only when the remaining uncertainty is material: plausible answers would change the Decision or recommendation.

A Challenge pass is semantically read-only by default. Do not intentionally change source code, configuration, dependencies, version-control state, persistent data, or external systems while producing the review. If the user explicitly asks to review and then implement, finish the Challenge and state the Decision first, then carry out the authorized implementation as a separate phase.

Keep the review proportional to the decision. Prefer concise findings and expand only when evidence or trade-offs require it.

## 1. Select the review target

A **Candidate solution** is the Agent's most recent complete, concrete proposal that claims to solve the user's goal. Brainstorming, problem analysis, and unselected alternatives are not Candidates. Treat incremental details as additions to the current Candidate unless they form a new complete proposal.

When several proposals appear, infer the Review target from the strongest signal:

1. the solution the user explicitly selected, endorsed, referred to, or asked to implement;
2. the most recent complete proposal that clearly replaces an earlier one;
3. a deliberately inseparable combination, reviewed as one Candidate.

Do not ask the user to choose merely because multiple alternatives were mentioned. If independent Candidates remain equally plausible after using the available context, use **Defer** only when choosing among them could change the review.

If no concrete Candidate can be identified after inspecting the available context, use **Defer** and name the missing review target. If the work is outside software engineering or technical-project scope, state the boundary and stop without inventing a Decision.

## 2. Establish the grounds

Reconstruct the actual problem, the user's goal, and the hard constraints as the smallest set of observable conditions needed to judge Effectiveness. Ground material conditions in the user's statements or project evidence. Infer routine, low-impact gaps when context supports a reasonable interpretation; preserve the user's authority over goals, hard constraints, and meaningful trade-offs.

Challenge hidden assumptions and problem framing without manufacturing requirements that the user did not state and the project does not imply.

Gather the smallest body of evidence likely to change the Decision from relevant code, call sites, tests, configuration, documentation, or tools. Check history only when authorship, regression timing, or recency could change the outcome. Stop when further investigation is unlikely to change the Decision.

### Verification

Use the smallest bounded verification appropriate to the Candidate and its risk.

- Safe, local, non-networked diagnostics, existing tests, linters, builds, or reproductions may run without extra approval when proportionate and when they do not intentionally modify persistent project state.
- Prefer existing checks and small reproductions. Do not create tests merely to mirror a reversible, low-impact implementation detail.
- Once sufficient checks pass, broaden or repeat verification only when new changes, failures, unresolved concerns, or the Candidate's risk justify it.
- Long-running, destructive, network-dependent, expensive, or safety-unknown checks require explicit authorization only when they are necessary to decide. Otherwise, do not block on them.
- Keep disposable diagnostic artifacts isolated when possible and clean up only artifacts known to come from this pass.

If independent subagents are available, use one bounded parallel evidence or counterexample task only when it can materially improve a non-trivial review or reduce anchoring. Do not delegate routine reviews or duplicate work.

Attach the smallest practical reference or reproduction to each non-obvious finding that can change the recommendation. Label unsupported inferences as uncertainties.

If a material uncertainty remains after available evidence is exhausted, choose **Defer**, name exactly what evidence, target, constraint, or user trade-off is missing, and explain how it could change the outcome. If the user can resolve it, ask one focused question at the end. Do not ask non-blocking questions.

## 3. Run the challenge

Assess all three dimensions. Fully report only material findings that can change the Decision. When no material defect is found, name the decisive evidence supporting that conclusion.

Classify each finding once by its primary effect: **Effectiveness** if the goal or a hard constraint fails; **Simplicity** if extra machinery is unnecessary; **Consequences** if adoption creates a new material liability.

### Baseline

Use current behavior or implementation as the default baseline. If it is not clearly sufficient, search only as far as needed through deliberate deferral, an existing project capability, a standard-library or native-platform capability, an already-installed dependency, and minimal custom machinery. Skip inapplicable options.

A baseline is credible only when evidence shows it can satisfy the goal and hard constraints. Among credible baselines, prefer the one with the lowest necessary complexity. Deliberate deferral is credible only when its rationale and cost are explicit. Do not invent a baseline merely to complete the comparison.

### Effectiveness

Determine whether the Candidate achieves the user's goal under the hard constraints. Test assumptions whose failure would invalidate the proposal.

### Simplicity

Determine whether the Candidate's added complexity is necessary relative to the strongest credible lower-complexity baseline. Compare concepts, abstractions, moving parts, dependencies, and operational demands with the value they add. State when no credible simpler alternative exists.

### Consequences

Determine which material new liabilities adoption would create relative to the baseline, including regressions, coupling, operational burden, migration cost, security or reliability exposure, and lifecycle maintenance that bear on the Decision.

The Challenge is complete when all three dimensions have an evidence-backed finding or explicit uncertainty and the strongest credible baseline has been compared with the Candidate.

## 4. Decide and recommend

Choose exactly one Decision:

- **Retain**: the Candidate survives the challenge; no material change is justified and the baseline is not clearly better.
- **Simplify**: the Candidate can achieve the goal, but unnecessary machinery should be removed without changing the core solution path.
- **Modify**: the main direction is sound, but material defects, omissions, or incorrect assumptions require correction.
- **Replace**: the Candidate should not be adopted and a concrete, viable alternative should be used instead.
- **Reject**: the Candidate should not proceed and no concrete replacement is currently justified.
- **Defer**: named evidence, a review target, a constraint, or a user trade-off that could change the outcome remains unavailable after available investigation is exhausted.

Use the user's language for headings, findings, and recommendations. Include the canonical English Decision name in parentheses when useful, for example `修改（Modify）`.

For a completed Challenge, use this review shape:

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

<self-contained, actionable recommendation>
```

If **Defer** occurs before the three challenge dimensions can be meaningfully assessed, do not fabricate empty findings. Return only the Decision and a concise revised recommendation naming the blocker, plus one focused question when the user can resolve it.

A Retain recommendation explains why credible alternatives are not better. A Defer recommendation explains how the missing information could change the outcome. Default to one concise paragraph per challenge dimension, integrate references with the claims they support, and do not repeat the same finding across dimensions. Report findings and evidence, not a private reasoning trace or transcript of tool use.

If the user explicitly requested implementation after the review, complete the review first and then carry out that authorized work separately.
