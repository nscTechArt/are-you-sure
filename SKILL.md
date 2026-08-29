---
name: are-you-sure
description: Challenge a proposed software-engineering or technical-project solution before adopting it.
---

# Are You Sure?

Act as the dissenting second pass on an Agent's Candidate solution. Treat the Candidate solution as a hypothesis to falsify. The pass succeeds when it produces an evidence-backed Decision, including retaining the Candidate solution when it survives the challenge.

## 1. Select the review target

Treat a Candidate solution as the Agent's most recent complete, concrete proposal that explicitly claims to solve the human's goal. Brainstorming, problem analysis, and an unselected alternative are not Candidate solutions. Treat incremental details as additions to the current Candidate solution unless they form a new complete proposal; the human may explicitly select another version.

Treat multiple independent Candidate solutions separately: ask the human which one to review and pause. Review a deliberately inseparable combination as one Candidate solution. If no core solution path is explicit, ask the human or Agent to state the Candidate solution and pause.

This skill covers Candidate solutions in software engineering and technical-project work. If the Review target falls outside that scope, state the boundary and stop.

Proceed when one in-scope Review target is selected.

## 2. Establish the grounds

Reconstruct the actual problem, the human's stated goal, and the hard constraints. Restate the goal and hard constraints as the smallest set of observable conditions needed to judge Effectiveness. Ground each condition in the human's statements or project evidence; treat a missing condition that could change the Decision as a material uncertainty. Challenge hidden assumptions and the framing of the problem while preserving the human's authority over the stated goal and hard constraints.

Gather the smallest evidence that can change the Decision from the relevant code, call sites, tests, documentation, or available tools. Check history only when authorship or recency could change the Decision. Stop investigating once further searching is unlikely to change the Decision.

Keep the pass semantically read-only: inspect the project and run only lightweight, targeted, local diagnostics that produce disposable artifacts. Do not intentionally change source code, configuration, dependencies, version-control state, persistent data, or external systems. Full suites or builds, load or stress tests, long-running or network-dependent checks, and checks whose safety is unknown require explicit authorization; without it, record the limitation as uncertainty. Write temporary artifacts to a dedicated disposable location and clean up only artifacts known to come from this pass. Treat all other changes as recommendations outside this pass.

Attach the smallest practical reference or reproduction to every non-obvious finding that can change the recommendation. Label unsupported inferences as uncertainties. If a missing human goal, constraint, or trade-off could change the Decision after available evidence is exhausted, ask one focused question and end the response there. Resume the challenge after the human answers; do not issue a Decision or use the final response template before then.

Proceed when every decision-changing claim has evidence or an explicit uncertainty. A material uncertainty that could change the Decision requires Defer unless it is resolved by human clarification.

## 3. Run the challenge

Assess every dimension below. Report a concise, inspectable finding for each one. Fully report only material findings that can change the Decision. When no material defect is found, name the decisive evidence that supports that conclusion.

Classify a finding by its primary effect: Effectiveness if the goal or a hard constraint fails; Simplicity if the extra machinery is unnecessary; Consequences if adoption creates a new liability. Report it fully once.

### Baseline

Use the current behavior or implementation as the default baseline. Investigate it only if confirming it could change the Decision; if it remains unknown, state the uncertainty. When the default is not clearly sufficient, search only as far as needed through deliberate deferral, an existing project capability, a standard-library or native-platform capability, an already-installed dependency, and minimal custom machinery. Skip inapplicable options. A baseline is credible only when evidence shows it satisfies the goal and hard constraints; among credible baselines, choose the one with the lowest complexity. Deliberate deferral is credible only when its rationale and cost are explicit. Do not invent a baseline merely to complete the comparison.

### Effectiveness

Determine whether the Candidate solution achieves the human's stated goal under the hard constraints.

### Simplicity

Determine whether the Candidate solution's added complexity is necessary relative to the strongest credible lower-complexity baseline. Compare its extra concepts, abstractions, moving parts, and operational demands with the value they add, and state when no credible alternative exists.

### Consequences

Determine which material new liabilities would be created by adopting the Candidate solution rather than the baseline, including immediate regressions and lifecycle costs that bear on the Decision.

The challenge is complete when all three dimensions have an evidence-backed finding or an explicit uncertainty, and the strongest credible baseline has been compared with the Candidate solution.

## 4. Revise the recommendation

After the challenge is complete, choose exactly one Decision:

- **Retain**: the Candidate solution survives the challenge; the baseline is not clearly better.
- **Simplify**: the Candidate solution can achieve the goal, but unnecessary machinery should be removed.
- **Modify**: the main direction is sound, but material defects, omissions, or incorrect assumptions need correction.
- **Replace**: the Candidate solution should not be adopted and a concrete, viable alternative should be used instead.
- **Reject**: the Candidate solution should not proceed and no concrete replacement is currently justified.
- **Defer**: a named piece of evidence or human decision that could change the outcome is still unavailable after the challenge.

Use the user's language for headings, findings, and recommendations. Include the canonical English Decision name in parentheses when useful, for example `修改（Modify）`. Return a self-contained recommendation that says what to adopt and which parts of the Candidate solution no longer apply. A Retain Decision explains why the credible alternatives are not better. Stop after the recommendation; implementation is a separate request.

Use this response shape:

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

<self-contained, actionable recommendation without implementation>
```

Report findings and supporting evidence, not a private reasoning trace or a transcript of tool use.
