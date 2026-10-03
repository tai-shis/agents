---
name: plan-first
description: Before implementing a task with more than one reasonable approach, surface a short plan (approach, areas touched, step order) and get it confirmed before writing code. Use for a new feature, a refactor, a schema or architecture choice, or anything where picking the wrong approach costs real rework.
requires: []
---

# plan-first

## Entry conditions

Reach for this before starting any task where the *approach* itself has more than one reasonable shape — a new feature touching multiple files or areas, a schema or architecture choice, a refactor with more than one viable strategy. Skip it for a task with one obvious way to do it: a one-line fix, a well-specified bug repro, a change where no reasonable person would pick a different approach.

This is distinct from [tai-protocol](../tai-protocol/SKILL.md)'s check phase, which front-loads missing information before a task starts at all. This skill assumes that's already happened for the task as a whole, but a narrower, approach-specific gap can still surface while stating tradeoffs or steps (see steps 2 and 4) — one the check phase had no way to catch because it's only visible once the approach itself is being worked out.

## Output contract

Produces: a short plan, stated before implementation begins — the approach, the areas or files it touches, and the order of steps.

Does not: start implementing, ask about requirements gaps (see [tai-protocol](../tai-protocol/SKILL.md)'s check phase for that), or substitute for verifying the result once work is done (see [verify](../verify/SKILL.md)).

## Steps

1. **State the goal in one line.** Something the finished plan can be checked against later.
2. **List the viable approaches**, if there's more than one, each with a one-line tradeoff. Two or three options with real tradeoffs; not an exhaustive survey. Test whether you actually need missing facts (platform, stack, scale, existing architecture) before claiming you do: if the one-line tradeoff would change depending on the answer, you need it; if you can state the tradeoff the same way regardless of the answer, you don't. When you do need it, stop here: name the specific missing facts and ask for them instead of listing generic placeholder approaches.
3. **Pick one and say why** — or, if the tradeoff is genuinely close, present the options and ask which to take rather than picking for the person. Default to the approach that satisfies the stated use case now, not one that adds flexibility or structure for a hypothetical future case nobody asked for. If a more flexible approach is tempting, raise that tradeoff explicitly here rather than building it in by default.
4. **Break the chosen approach into ordered steps**: what gets touched, in what order, and what observable result marks each step done. "Add the field, migrate existing rows, update the three call sites, confirm the old query still returns the same rows" — not "update the schema." Same rule as step 2: if this can't be made concrete without facts you don't have, stop and name what's missing rather than writing a vague placeholder step.
5. **Surface the plan and wait** for confirmation or redirection before writing code. A plan that reads right to the agent is not the same as a plan the person has actually seen and agreed to.

## Failure handling

- If the person redirects the plan, revise it and surface the revision before proceeding. Don't fold the redirection into the first line of code as an improvisation.
- If the actual shape of the problem turns out to differ from the plan mid-execution (a file doesn't exist, an assumption was wrong), stop and re-surface the revised plan instead of quietly improvising past it.
- If the task turns out to be open-ended rather than a fixed sequence once you start, hand the remaining work to [loop](../loop/SKILL.md) instead of forcing it back into a single plan.

## Completion criteria

The person has seen the plan and either confirmed it or redirected it, and implementation proceeds from the confirmed version, not the first draft.
