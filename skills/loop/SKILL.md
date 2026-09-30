---
name: loop
description: Iterate on a task against a stated finish condition instead of a fixed step count — check the condition, make one justified change, verify it, keep or discard, repeat, log every decision.
requires: [skills/verify/SKILL.md]
---

# loop

## Entry conditions

Use this when a task is open-ended enough that you can't fully plan it up front — an autonomous run, a long refactor, a "keep going until X is true" instruction — rather than a task with a known, fixed sequence of steps.

## Steps

Before the first iteration, get the finish condition stated in checkable terms — not "make it better," but something you can look at and answer yes/no: a test suite that passes, a metric under a threshold, a specific behavior reproduced correctly N times.

Each iteration:

1. **Check the finish condition.** If it's met, stop — don't keep going out of momentum.
2. **Make one justified change.** One change per iteration, not a batch — if it needs to be discarded, you want to know exactly what to discard.
3. **Verify the real result** — see [verify](../verify/SKILL.md). Not "the build succeeded"; actually check the thing the change was supposed to fix or improve.
4. **Keep or discard.** If verification contradicts the intent of the change, discard it (revert, don't leave it half-applied) rather than pressing forward on top of a change you don't trust.
5. **Record the decision** before moving to the next iteration: what you tried, why, what the verification showed, and what you did about it (kept/discarded). A few lines is enough — the point is that the next iteration (or the person reading the log later) doesn't have to reconstruct your reasoning from the diff alone.

## Failure handling

- If three consecutive iterations fail to make progress toward the finish condition, stop and report rather than continuing to churn — that's a sign the approach, not the current change, is wrong.
- If the finish condition itself turns out to be unreachable or ambiguous as stated, stop and say so rather than redefining it unilaterally partway through.

## Completion criteria

The stated finish condition is met and verified, or you've stopped and reported why it couldn't be reached — never a silent exit after running out of turns.
