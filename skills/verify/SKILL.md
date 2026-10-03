---
name: verify
description: Verification-first principle — never report a task done on the strength of a plan, a green build, or the model's own confidence; require evidence the change actually behaves as claimed.
requires: []
---

# verify

## Entry conditions

Reach for this before reporting *any* task complete — a bug fix, a feature, a refactor, a config change. It's a principle other skills (like [tai-mode](../tai-mode/SKILL.md) and [loop](../loop/SKILL.md)) lean on, not a standalone workflow you run in isolation.

## The core rule

A green test suite is not verification by itself. Tests alone prove the code paths *you already thought of* behave as expected — they don't prove the actual described behavior happened. Verification means you ran the real thing and observed the real result:

- A bug fix: reproduce the original failure first (so you know what "fixed" looks like), then reproduce again after the change and show it no longer fails — same repro steps, not a new happy-path check. Capture that reproduction as a regression test where the codebase supports one, so the check outlives this task instead of living only in this report.
- A UI/behavior change: drive the actual interface (or the actual CLI, the actual API call) and capture what happened — a screenshot, a response body, a log line — not a description of what you expect it to do.
- A data/migration change: inspect the actual record after the operation, not just the exit code of the command that ran it.

## Steps

1. Before claiming done, state what evidence would prove it — out loud, in the plan or the final report, not just in your head.
2. Produce that evidence by actually running the thing, not by re-reading the code and reasoning that it should work.
3. Attach or quote the evidence in whatever you report back — the command and its output, the file diff, the screenshot path.
4. If you can't produce the evidence (no test environment, no way to drive the UI, no access to the downstream system), say so explicitly instead of reporting success. "Verified by code review only — could not exercise the running feature" is an honest status; a bare "done" is not, if you didn't run it.

## Failure handling

If the evidence contradicts the claim (the repro still fails, the screenshot shows the old behavior), the task isn't done — go back and fix it. Don't soften the report to make a partial fix sound complete.

## Completion criteria

You have runtime evidence, specific to the actual claim being made, and you've included or referenced it in the report. Not: the code compiles, the tests pass, or "this should work."
