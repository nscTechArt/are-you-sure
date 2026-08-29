# Are You Sure

This context defines the language for a human-invoked, second-pass judgment that challenges an Agent's proposed solution in software engineering and technical project work.

## Language

**Candidate solution**:
The Agent's concrete proposal that claims to solve the human's goal. Brainstorming, problem analysis, and an unselected alternative are not Candidate solutions.
_Avoid_: Final solution, accepted solution

**Review target**:
The Candidate solution under a Challenge pass.
_Avoid_: Entire conversation, every prior suggestion

**Challenge pass**:
A dissenting attempt to falsify a Candidate solution, including its hidden assumptions and problem framing.
_Avoid_: Reflection, self-review, risk summary

**Challenge dimensions**:
The three required views of a Challenge pass: Effectiveness, Simplicity, and Consequences.
_Avoid_: Pros and cons, generic risk checklist

**Evidence**:
A fact from the project or tools that supports a Challenge finding.
_Avoid_: Intuition, plausible claim, unsupported concern

**Baseline**:
The lowest-complexity credible option that still meets the goal and hard constraints. The current behavior or implementation is the default.
_Avoid_: Straw-man alternative, hypothetical rewrite, Baseline alternative

**Material uncertainty**:
A missing fact, goal, constraint, or human trade-off whose plausible answers would change the Decision.
_Avoid_: Missing detail, generic doubt

**Semantic read-only**:
A Challenge pass that inspects without intentionally mutating the project, aside from disposable diagnostic artifacts.
_Avoid_: Strict filesystem read-only, implementation work

**Revised recommendation**:
The self-contained advice that remains after the Decision, without implementing it.
_Avoid_: Original answer, reflection output

**Decision**:
The closed outcome of a Challenge pass: Retain, Simplify, Modify, Replace, Reject, or Defer.
_Avoid_: Balanced summary, overall thoughts, Review decision
