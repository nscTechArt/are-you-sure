---
name: are-you-sure
description: Challenge a proposed software-engineering or technical-project solution before adopting it.
disable-model-invocation: true
---

# Are You Sure?

Run the **Challenge**: a dissenting second pass on the Agent's Candidate solution. Treat the Candidate as a hypothesis to falsify. The Challenge succeeds when it produces an evidence-backed Decision, including Retain when the Candidate survives.

Something is **material** when it could change the Decision.

## Operating contract

The Challenge is semantically read-only: inspect and report, and leave source code, configuration, dependencies, version-control state, persistent data, and external systems as you found them. If the user asks to review and then implement, state the Decision first, then implement as a separate phase.

## 1. Select the Candidate

A **Candidate solution** (the Candidate) is the Agent's most recent complete, concrete proposal that claims to solve the user's goal. Brainstorming, problem analysis, and unselected alternatives are not Candidates. Incremental details belong to the current Candidate unless they form a new complete proposal.

When several proposals appear, select the Candidate by the strongest signal:

1. the solution the user explicitly selected, endorsed, referred to, or asked to implement;
2. the most recent complete proposal that clearly replaces an earlier one;
3. a deliberately inseparable combination, reviewed as one Candidate.

If the work is outside software engineering or technical-project scope, state the boundary and stop. This is the only exit without a Decision.

Step 1 is complete when exactly one Candidate is selected. If equally plausible Candidates remain and the choice is not material, select either; otherwise, go to Defer.

## 2. Establish the grounds

Before you reread the Candidate's rationale, reconstruct the actual problem, the user's goal, and the hard constraints as the smallest set of observable conditions needed to judge Effectiveness. Ground material conditions in the user's statements or project evidence, and infer routine, low-impact gaps from context. Goals, hard constraints, and meaningful trade-offs remain the user's call.

Question the problem framing, holding to the requirements the user stated or the project implies.

Gather the smallest body of material evidence from relevant code, call sites, tests, configuration, documentation, or tools. Seek falsifying evidence before supporting evidence. Check history only when authorship, regression timing, or recency is material. Attach the smallest practical reference or reproduction to each non-obvious material finding.

### Verification

Use the smallest verification that settles a material question.

- Existing tests, linters, builds, diagnostics, or reproductions may run without extra approval when they are safe, local, non-networked, and not expected to modify tracked files or persistent data.
- Prefer existing checks and small reproductions; write a new test only when it checks material behavior.
- After sufficient checks pass, broaden or repeat verification only when new changes, failures, unresolved concerns, or the Candidate's risk justify it.
- Run long-running, destructive, network-dependent, expensive, or safety-unknown checks only when the Decision needs them, and only with explicit authorization. If such a check is needed but not authorized, record a material uncertainty. Otherwise, skip it.

If independent subagents are available, use at most one bounded counterexample task, and only for a non-trivial review where it could surface material evidence or reduce anchoring.

Step 2 is complete when every material claim has evidence or an explicit uncertainty, and further investigation is unlikely to surface anything material.

## 3. Test the Candidate

Assess all three dimensions. Report each material finding once, under its primary effect:

- **Effectiveness**: the goal or a hard constraint fails.
- **Simplicity**: extra machinery is unnecessary.
- **Consequences**: adoption creates a new material liability.

When a dimension has no material defect, name the evidence that supports that conclusion.

### Baseline

Use current behavior or implementation as the default baseline. If it is not clearly sufficient, search only as far as needed through deliberate deferral, an existing project capability, a standard-library or native-platform capability, an already-installed dependency, and minimal custom machinery.

An option is credible only when evidence shows it can satisfy the goal and hard constraints. Among credible baselines, prefer the one with the lowest necessary complexity. Deliberate deferral is credible only when its rationale and cost are explicit.

### Effectiveness

Determine whether the Candidate achieves the user's goal under the hard constraints. Test assumptions whose failure would invalidate the proposal.

### Simplicity

Determine whether the Candidate's added complexity is necessary relative to the strongest credible lower-complexity baseline. Compare concepts, abstractions, moving parts, dependencies, and operational demands with the value they add. State when no credible simpler alternative exists.

### Consequences

Determine which material new liabilities adoption would create relative to the baseline, including regressions, coupling, operational burden, migration cost, security or reliability exposure, and lifecycle maintenance.

Step 3 is complete when all three dimensions have an evidence-backed finding or explicit uncertainty and the strongest credible baseline has been compared with the Candidate.

## 4. Decide and recommend

Choose exactly one Decision:

- **Retain**: the Candidate survives the Challenge; no correction is justified and the baseline is not clearly better.
- **Simplify**: the Candidate can achieve the goal, but unnecessary machinery should be removed without changing the core solution path.
- **Modify**: the main direction is sound, but material defects, omissions, or incorrect assumptions require correction.
- **Replace**: the Candidate should not be adopted, and a concrete, credible alternative (a baseline or another option) should be used instead.
- **Reject**: the Candidate should not proceed and no concrete, credible replacement exists.
- **Defer**: the Candidate, or a material fact, constraint, or user trade-off, is still missing.

When both Simplify and Modify apply, choose Modify and include the simplification in the revised recommendation.

### Defer and questions

Complete the review from the conversation and project evidence whenever you can, and finish the read-only investigation that could close a gap before you treat it as material. Defer only in these cases:

- No concrete Candidate can be identified: name the missing Candidate.
- Independent Candidates remain equally plausible and the choice among them is material: name the choice.
- A material uncertainty remains: name exactly what evidence, constraint, or user trade-off is missing, and explain how it could change the Decision.

When the user can close the gap, end with one focused question. When the user answers, resume the Challenge with the new information.

### Output

Localize headings and findings to the user's language, and keep the canonical English Decision name in parentheses, for example `修改（Modify）`.

For a completed Challenge, use this shape:

```markdown
## <localized review decision> (Decision)

**<localized outcome> (<Retain | Simplify | Modify | Replace | Reject | Defer>)**

## <localized challenge>

### <localized effectiveness>

<finding and its evidence>

### <localized simplicity>

<finding, baseline comparison, and its evidence>

### <localized consequences>

<finding and its evidence>

## <localized revised recommendation>

<self-contained, actionable recommendation>
```

When Defer comes before the three dimensions can be assessed, return only the Decision and a concise revised recommendation that names the gap, plus the question.

Keep each dimension to one concise paragraph by default, with each reference beside the claim it supports. A Retain recommendation explains why credible alternatives are not better. Report findings and evidence only.
