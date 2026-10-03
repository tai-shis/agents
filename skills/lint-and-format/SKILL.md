---
name: lint-and-format
description: Run the project's formatter and linter on a finished chunk of work before verification and commit. If no formatter or linter is configured for a changed language, suggest one, ask before installing or configuring it, and persist the decision for later chunks.
requires: []
---

# lint-and-format

## Entry conditions

Reach for this at the end of a coherent chunk of work, before its final verification and commit, not saved up for the end of a long task. Applies even when the work is being left uncommitted. A read-only review or investigation does not trigger this skill, there is nothing to format yet.

## Use what the project already has

1. Check for an existing formatter and linter: repository instructions, configuration files, package scripts, dependency manifests, and any already-recorded tooling decision. Identify the languages actually touched in this chunk, not the whole repository.
2. Prefer the project's configured tool and invocation over a personal default. Confirm it is actually installed and runnable, not just named in a manifest, before deciding it is missing.
3. Reuse what is already configured. Do not replace an established formatter or linter with a different one just because you would prefer it.
4. Run the installed tool directly rather than through a package runner that silently fetches it. Fetching counts as installing.

## If nothing is configured

Check first whether this project and language combination already has a recorded decision (see Persist the decision below) before asking again.

If there is no recorded decision, recommend a formatter or linter for the language, explain briefly what it would add and where it would be configured, and ask before installing or configuring anything: "No formatter is configured for this JavaScript code. Add Prettier as a dev dependency and a config file for future chunks?"

- **Yes.** Install and configure it through the project's own package manager and conventions, adding only what is needed for repeatable use. Confirm it runs, then use it for this chunk and later ones.
- **No.** Record the decline and its scope, match the surrounding style by hand for now, and do not install anything. Do not re-ask the same question next chunk.
- **No answer, or no way to ask.** Match the surrounding style by hand for now, the same as a decline, continue the rest of the work, and say so in the handoff. Silence is not approval, see [tai-protocol](../tai-protocol/SKILL.md)'s ask-first default, this is the same rule applied to tooling choices.

## Persist the decision

Write the decision, installed, or declined and its scope, somewhere durable that a later chunk or session will actually read: an existing memory mechanism, or a short note in the project's own agent instructions if nothing else exists. Scope it to the repository and language by default, and only widen it to every project if that is what was actually asked for. Do not overwrite other recorded instructions to do this. If nothing durable is writable, say so in the handoff instead of claiming the preference will carry forward.

A pending answer (no way to ask) is not a decision and does not get recorded as one. Leaving nothing recorded means the next chunk checks again and asks as soon as asking is actually possible, rather than being stuck treating silence as a standing decline.

## Format, lint, and verify

1. Run the formatter and linter on the chunk's changed files, respecting the project's own config and ignore rules. Leave generated files, vendored sources, and unrelated files alone. Do not reformat the whole repository because one file in it changed.
2. Look at the resulting diff. Keep only the formatting or lint fixes that belong to this chunk, a separate repo-wide formatting migration is its own change, not bundled into this one.
3. Confirm the result is actually clean, a check mode, or a second pass producing no further changes, not just that the command exited without error.
4. If the tool fails, fix the cause when it is part of this task. Otherwise report the blocker honestly rather than claiming the code is formatted. A failed run is not evidence of clean code.

## Failure handling

If a formatter or linter conflicts with code the task intentionally left non-conforming (a vendored file, a generated file, a deliberately unusual case), exclude that file rather than fighting the tool or disabling the rule project-wide to accommodate one case.

## Completion criteria

The chunk's changed files pass the project's formatter and linter in check mode, or a confirmed clean second pass, and any tooling decision made along the way, installed or declined, is recorded somewhere a later session will actually see it.
