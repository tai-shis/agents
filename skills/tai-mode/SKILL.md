---
name: tai-mode
description: Entry point for non-trivial engineering work — read the task, match it to a playbook, delegate, verify with evidence, and report. The orchestration-loop dispatcher for this library.
requires: [skills/verify/SKILL.md]
---

# tai-mode

## Entry conditions

Reach for this whenever a task is more than a one-line edit: a bug fix, a feature, a refactor, an investigation, anything where "just do it" would mean skipping steps that matter. Once you're in it, plain follow-ups ("keep going," "also handle X") stay inside it — you don't need to re-invoke for every message in the same task. A message that clearly starts a *different* task should say so explicitly, so the playbook match happens again instead of forcing the new task through the old one.

## What happens to the task

1. **Classify.** Bug fix, feature, refactor, investigation-only, or "doesn't fit a known shape" — pick the closest, don't force a bad fit.
2. **Match a playbook**, i.e. the shape of steps + verification that classification implies:
   - *Investigation-only*: understand and report; do not implement. Stop at a root cause or an answer, not a fix.
   - *Bug fix*: reproduce first, so "fixed" is checkable; fix; reproduce again to confirm — see [verify](../verify/SKILL.md).
   - *Feature*: build against the smallest real version of the use case, then verify the actual behavior, not just that it compiles.
   - *Refactor*: behavior must not change — verification here means showing the *before* and *after* produce the same observable result, not just that tests still pass.
   - *Doesn't fit*: say so, propose a short custom sequence of steps for this specific task, and check it against the person before running far with it.
3. **Work the playbook.** Delegate sub-parts if the task decomposes cleanly into independent pieces; otherwise work it directly.
4. **Verify** per [verify](../verify/SKILL.md) before claiming anything is done. No exceptions for "it's a small change."
5. **Report** what changed, what evidence backs the "done" claim, and what's still open, if anything.

## Say the goal, not the ceremony

State what's wrong or wanted plus the context that matters — not step-by-step instructions for how to get there. `users get charged twice on retry. repro first, then fix and verify.` is enough; you don't need to spell out "read the code, find the bug, write a test." The playbook already sequences that.

Don't enumerate every skill to use by name as a matter of habit — the playbook match already picks the right sequence. Name a specific skill only when deliberately overriding what the playbook would otherwise choose.

## Long-running / open-ended work

If the task is "keep going until X," hand it to [loop](../loop/SKILL.md) rather than trying to plan every iteration up front inside a single pass of this skill.

## Failure handling

- Ambiguous scope or an unclear finish condition: ask, don't guess and proceed — this is the one place in the playbook where stopping to check is cheaper than unwinding wasted work later.
- Stuck after a genuine attempt: don't keep re-running the same failed approach. Say what was tried, what happened, and what you'd try differently — that's a valid stopping point, not a failure to hide.

## Completion criteria

The playbook's own verification step has run and produced evidence (see [verify](../verify/SKILL.md)), and that evidence is in the report. A plan that looks right is not the same as a result that's been shown to work.
