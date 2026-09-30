# Authoring a skill

A skill is one `SKILL.md` file under `skills/<name>/`, meant to be fetched cold over HTTP and followed by an agent that has no other context on this repo.

## Frontmatter

```yaml
---
name: skill-name
description: One sentence — what it's for and when to reach for it.
requires: []
---
```

- `name`, `description`, `requires` only. `requires` is a JSON-style array of `path`s to *unconditional* dependencies — skills this one always needs loaded alongside it. A dependency that's only needed sometimes gets linked inline in the body instead (`see [verify](../verify/SKILL.md) once you have something to check`), not forced into `requires`.
- Every skill here needs a matching entry in [`index.json`](../index.json) — `name`, `description`, and `path` should match the frontmatter exactly.

## Portability

Refer to *capabilities*, not specific tools: "search the codebase," not "use ripgrep"; "run the tests," not "invoke `pytest` via Bash." This library gets loaded by whatever harness is asking — Claude Code, an OpenCode agent, something else — and each has different tool names, model IDs, and slash commands. Don't hardcode any of that into a skill.

## Body

Write it as something an agent can execute, not prose someone reads once and forgets:

- **Entry conditions** — when does this skill apply.
- **Steps** — in order, concrete enough to act on.
- **Observable evidence** — what proves each step actually happened (a command's output, a file that now exists, a screenshot) — not "the agent believes it's done."
- **Failure handling** — what to do when a step doesn't produce that evidence.
- **Completion criteria** — the condition that ends the skill, stated so it can be checked, not just asserted.

Link to other skills or docs with explicit relative Markdown paths (`../verify/SKILL.md`, not just "the verify skill") — a reader fetching this file in isolation over HTTP needs to resolve the link itself.

## Before publishing a change

- Regenerate/check [`index.json`](../index.json) against `skills/` — no skill without a catalog entry, no catalog entry without a skill.
- For a structural change (frontmatter, paths, links): confirm every link in the file you touched still resolves.
- For a workflow change (the actual instructions a skill gives): don't just eyeball it — actually run the changed instructions against a representative task and note what happened. A skill that reads correctly but has never been exercised is a guess, not a verified change.
