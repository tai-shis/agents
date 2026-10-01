---
name: tai-reflect
description: Turn a correction, repeated friction, or a clearly reusable success into a durable improvement in the right place, instead of a reflex edit to a global skill. Use after being corrected, after a recurring mistake, after a review or verification failure that exposed a gap, or after a substantial task with a method worth keeping.
requires: []
---

# tai-reflect

## Entry conditions

Trigger on:

- An explicit request to reflect or improve how a task was handled.
- A correction, or repeated manual steering toward the same thing.
- A review or verification failure that exposed a reusable gap, not a one-off mistake.
- A substantial successful task with a clearly reusable method.

Don't reflect after every trivial task, and don't treat a single surprising result as sufficient reason to add a global instruction: that's how a skill accumulates rules that only ever applied once.

## Find the failure (or the win)

1. State the concrete moment: what was believed, what actually happened, the consequence, and the earliest observation that would have changed the action.
2. Look for a counterexample before generalizing: a case where the behavior you're about to codify was already correct. If you find one, the lesson is narrower than it first looked.

## Choose the durable home

Work down this list and stop at the first level that actually controls the problem. Don't reach for a skill edit when a cheaper fix would hold:

1. **Impossible by construction**: when the work is in a codebase, fix the type, schema, API shape, or data model so the mistake can't recur. This beats any instruction, because it doesn't depend on being read.
2. **Mechanical enforcement**: a test, lint rule, schema check, or CI gate that catches it without anyone having to remember a rule.
3. **Guidance in this library**: amend an existing skill at the narrowest scope that reaches every consumer, using [skill-creator](../skill-creator/SKILL.md). Add a new skill only for a genuinely distinct, reusable workflow, not a one-line caveat on an existing one.
4. **Project-local note**: knowledge specific to one codebase or task doesn't belong in a shared skill at all; it goes in that project's own docs.
5. **One-off**: no durable change earns its place. Say so and move on.

## Apply and verify

For a change at level 3, follow [skill-creator](../skill-creator/SKILL.md) end to end, including its validation step. A reflection that edits a skill without validating it is the same unverified "should work" claim [verify](../verify/SKILL.md) rejects everywhere else.

Replay the original trigger scenario against the change, plus one nearby case where the prior behavior was already correct. If the nearby case breaks, the change overcorrected: narrow it rather than keeping a fix that trades one failure for another.

## Failure handling

If no generalizable lesson survives the counterexample check, say so explicitly instead of editing something to fit one example. If the durable home is level 3 but you don't have write access to apply it, leave the proposed change and its evidence stated precisely, without claiming it's been made.

## Completion criteria

The observed failure or win is stated with its evidence, a durable home has been chosen with reasoning for why cheaper levels don't suffice, and either the change has been applied and replayed, or an explicit "no change warranted" / "proposed, not applied" stands in its place.
