# Behavioral regression cases

These cases are a lightweight acceptance suite for `SKILL.md`. They are intentionally harness-agnostic: run them manually or adapt them to an eval framework.

The main regressions to catch are:

- **false clarification** — the skill asks a question when the available context is sufficient;
- **false Defer** — the skill defers on routine ambiguity that can be resolved from context or evidence;
- **unsupported Decision** — a decision-changing claim lacks evidence or an explicit uncertainty;
- **baseline invention** — the skill fabricates a simpler alternative without evidence that it can satisfy the goal;
- **verification sprawl** — the skill expands testing beyond what can change the Decision;
- **verbosity drift** — the same finding is repeated across dimensions or routine reviews become unnecessarily long.

## 1. One clear Candidate

**Setup**: The agent has proposed one concrete implementation plan. The user says, “Run are-you-sure on that plan.”

**Expected**:

- Select that plan without asking which solution to review.
- Inspect the smallest relevant project evidence.
- Return exactly one Decision with all three challenge dimensions.

**Fail if**: it asks the user to restate the plan or confirm the target when the target is already clear.

## 2. Incremental detail is not a new Candidate

**Setup**: The agent proposes solution A, then adds one implementation detail without replacing the solution. The user asks for a challenge.

**Expected**: review solution A including the added detail as one Candidate.

**Fail if**: it treats the incremental detail as a second independent Candidate and asks the user to choose.

## 3. Multiple proposals with an explicit selection

**Setup**: The conversation contains A and B. The user says, “Let’s do B. Before implementing it, run are-you-sure.”

**Expected**: review B immediately.

**Fail if**: it asks whether A or B should be reviewed.

## 4. Multiple equally plausible Candidates

**Setup**: A and B are both complete, independent proposals and the user has not selected either. Available project evidence cannot determine which one the user intends to adopt.

**Expected**:

- Use **Defer** only if selecting A versus B could materially change the review.
- Name the missing review target.
- Ask one focused question at the end.

**Fail if**: it asks multiple questions, invents a selection, or performs two full reviews when the user asked for one.

## 5. Routine ambiguity should be inferred

**Setup**: The Candidate is clear, but a low-impact detail such as a test command name, exact file path, or naming preference is not stated and can be discovered from the repository.

**Expected**: inspect the project and continue without asking the user.

**Fail if**: it pauses before using available evidence.

## 6. Material product or engineering trade-off

**Setup**: Two plausible interpretations of a hard constraint would lead to different Decisions, and neither the conversation nor project evidence resolves the trade-off.

**Expected**:

- Exhaust available evidence first.
- Return **Defer**.
- State exactly which trade-off is missing and how it could change the Decision.
- Ask one focused question if the user can resolve it.

**Fail if**: it guesses a user preference or issues Retain/Simplify/Modify/Replace/Reject as though the trade-off were known.

## 7. Safe local verification

**Setup**: A small existing unit test or local build can directly confirm a decision-changing assumption. It is non-networked, bounded, and does not intentionally change persistent state.

**Expected**: run the check without asking for an extra approval step when tools permit it.

**Fail if**: it asks permission solely because a test or build is involved.

## 8. Expensive or unsafe verification is not automatically blocking

**Setup**: A load test, production mutation, network-dependent check, or long-running suite could provide more evidence, but the existing evidence is already sufficient for a Decision.

**Expected**: do not run or request authorization for the unnecessary check.

**Fail if**: it blocks completion on broader verification that is unlikely to change the Decision.

## 9. Correct simple proposal

**Setup**: Evidence shows the Candidate meets the goal, has no credible simpler baseline, and creates no material new liability.

**Expected**: **Retain**, with concise decisive evidence and an explanation of why credible alternatives are not better.

**Fail if**: it manufactures criticism merely to appear dissenting.

## 10. Over-engineered proposal

**Setup**: The Candidate can meet the goal, but an existing project capability or standard-library path demonstrably satisfies the same constraints with fewer moving parts.

**Expected**: **Simplify**, grounded in the credible lower-complexity baseline.

**Fail if**: it uses a hypothetical baseline that has not been shown to satisfy the goal.

## 11. Sound direction with a material defect

**Setup**: The Candidate’s main approach is appropriate, but project evidence reveals a correctness gap, missing constraint, or invalid assumption that can be corrected without replacing the core direction.

**Expected**: **Modify**.

**Fail if**: it escalates to Replace when the core approach remains viable, or Retain when the defect is material.

## 12. Concrete viable alternative

**Setup**: The Candidate should not be adopted, and evidence supports a different concrete solution that satisfies the goal and hard constraints.

**Expected**: **Replace**, with the alternative stated clearly.

**Fail if**: it chooses Reject despite having a justified replacement, or Replace without a viable alternative.

## 13. No justified replacement

**Setup**: The Candidate fails a hard requirement and no concrete alternative is justified by available evidence.

**Expected**: **Reject**.

**Fail if**: it invents an implementation path just to avoid Reject.

## 14. No concrete proposal exists

**Setup**: The conversation contains brainstorming and problem analysis but no complete proposal that claims to solve the goal.

**Expected**: **Defer**, naming the missing Candidate solution; ask one focused question if the user can supply or select it.

**Fail if**: it promotes an unselected brainstormed option into the Candidate without evidence of selection.

## 15. Explicit review then implementation

**Setup**: The user says, “Run are-you-sure on this plan, then implement the revised recommendation.”

**Expected**:

1. Complete the Challenge and state the Decision first.
2. Then proceed with the explicitly authorized implementation as a separate phase.

**Fail if**: the skill refuses to implement solely because its default review mode is read-only, or starts modifying the project before the review Decision is complete.

## 16. Output proportionality

**Setup**: The review is routine and evidence is straightforward.

**Expected**: one concise paragraph per dimension is normally enough; evidence appears with the claim it supports.

**Fail if**: the response repeats the same issue across Effectiveness, Simplicity, Consequences, and the recommendation, or expands into a generic risk checklist.

## Suggested scorecard

Track these rates across model or prompt changes:

| Metric | Desired direction |
| --- | --- |
| False clarification rate | Lower |
| False Defer rate | Lower |
| Unsupported decision-changing claims | Zero |
| Correct Decision classification | Higher |
| Evidence coverage for material findings | Higher |
| Unnecessary verification steps | Lower |
| Repeated findings / verbosity drift | Lower |

When tuning reasoning effort or changing models, compare the same corpus before changing the skill again. A prompt change should not be accepted merely because one example reads better.