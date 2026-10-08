---
name: debug
description: Hypothesis-driven root-cause investigation for a bug, test failure, crash, or unexpected output that doesn't have an obvious cause. Reproduce it, narrow to the first point where expected and actual behavior diverge, and confirm the cause before touching a fix. Not for adding a feature or change near a known bug.
requires: []
---

# debug

## Entry conditions

Reach for this when there's an observed failure, a bug report, a failing test, a crash, wrong output, that isn't already explained by reading the code. Not for implementing a change to a known, agreed spec, and not for recovering why a design decision was made historically, that's a different kind of investigation.

This is the depth behind [tai-mode](../tai-mode/SKILL.md)'s bug-fix playbook: that playbook says "reproduce first, fix, reproduce again." This skill is what happens inside "reproduce first" and between reproducing and fixing, once the cause isn't obvious on sight.

## Reproduce first

Get a reliable reproduction before investigating further. Reading code to work out how to build the repro is fine, but don't settle on a cause from reading alone. A cause found by reading code without a repro is a guess until it's checked against real behavior. If the failure is intermittent, capture the conditions that make it more likely (timing, input shape, concurrency, environment) rather than giving up on reproducing it or claiming false confidence that it's fixed later without one.

If you have no code, logs, or access to the failing system, there is nothing to reproduce yet. Ask for specific artifacts, such as the code or config involved, logs from a failing run and a passing run, and the conditions around when it fails, instead of guessing.

## State a hypothesis before changing anything

Say what you believe is wrong and what evidence would confirm or rule it out, before editing code. A hypothesis with no stated evidence is not checkable, and an edit made without one is a guess dressed up as a fix. If you have no evidence yet, list candidate hypotheses and label them inferences, along with what would discriminate between them.

There can be more than one independent cause. Keep each as its own hypothesis and confirm each separately.

## Narrow by evidence, not by reading harder

Find the first point in the actual execution where expected and actual behavior diverge, rather than reading the whole codebase hoping to spot the bug. Bisect the path: add logging or a breakpoint partway through, isolate the input, bisect across commits, or strip the problem down to the smallest case that still reproduces it. Each probe should discriminate between competing explanations, not just gather more general context.

If more than one cause still looks plausible, test the one probe that would tell them apart, instead of fixing all of them at once and hoping one was it.

Keep observed facts, supported inferences, and open questions distinct as the picture builds. A trace that shows what happened is a fact. An explanation of why it happened is an inference until confirmed. Don't let an inference harden into a fact without the confirming probe.

## Confirm the cause before fixing

The investigation is done when you can point to the specific line, state, or interaction where expected and actual behavior split, with the evidence that pins it there, not just a plausible story. Fix that location, or each location if there were several causes. If the fix doesn't touch the place the evidence pointed to, the fix and the diagnosis don't agree, form a new hypothesis and narrow again rather than patching the mismatch away.

## Failure handling

- "A real attempt" is a judgment call, not a fixed count or duration, the same way scope and cohesion are judgment calls elsewhere in this library. It means having actually varied the plausible factors (timing, input, environment), not having run the same thing twice and stopped.
- If there is no code, logs, or access to try anything on, that is not an attempt. Ask for the artifacts listed under Reproduce first, and stop until you have them.
- If a reliable repro genuinely can't be produced after a real attempt, say so explicitly, along with what's been tried and what conditions make the failure more likely, rather than proceeding on an unconfirmed guess.
- If root cause can't be pinned down after a genuine attempt, stop and report what's known, what's been ruled out, and what's still open, rather than shipping a fix that addresses a symptom instead of the cause.
- Don't keep re-running the same failed probe expecting a different read. A probe that didn't discriminate between hypotheses needs a different probe, not a repeat.

## Completion criteria

The root cause is stated with the evidence that locates it, the fix addresses that specific location, and the original reproduction no longer fails, see [verify](../verify/SKILL.md), if you have it loaded, for turning that into a regression test where the codebase supports one.
