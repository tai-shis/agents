---
name: skill-creator
description: Author, revise, or retire a skill of your own in this library, defining its contract, writing it to the house structure, validating it against a real task before shipping, and recording anything deliberately left out of scope. Use when adding a skill, substantially changing one, or deciding a candidate doesn't belong.
requires: [skills/verify/SKILL.md]
---

# skill-creator

## Entry conditions

Reach for this when writing or substantially revising a skill of your own for this library. It does not apply to a skill taken from elsewhere and kept as-is: see [Vendoring](#vendoring) for that case instead.

## Contract first

Before writing a word of the body:

1. **State the trigger, the consumer, and the done-condition.** What task makes an agent reach for this skill, who's running it (this library's own agents, a harness fetching cold over HTTP), and what observable state means it worked.
2. **Check [index.json](../../index.json) for overlap.** If an existing skill already covers most of the ground, strengthen that skill instead of shipping a near-duplicate. A new skill earns its place only when the workflow is genuinely distinct.

## Structure rules

These aren't negotiable: a skill that violates one isn't ready to ship.

- Frontmatter is exactly `{name, description, requires}`, per [docs/loading.md](../../docs/loading.md). `requires` holds paths to *unconditional* dependencies only.
- Refer to capabilities, not tools: "search the codebase," never "use ripgrep."
- Single `SKILL.md` is the default. Split into `references/` or an atomic `rules/` directory (one topic per file) only when the body has cases that don't all apply on every invocation, not merely because it's grown long.
- A skill that produces an artifact (code, a commit, a file, a message) states an output contract: what it produces *and* what it explicitly does not do. "Writes a Conventional Commit message. Does not stage, commit, or push" is a contract; "writes good commits" is not.

## Writing guidelines

The prose is instructions an agent executes cold, not documentation someone skims. Default to these; deviate with a stated reason.

- Pair every prohibition with its fix: "don't do X" is incomplete without "do Y instead." When the obvious workaround still fails, say why: a rule that just bans a word invites a mechanical substitution that technically complies and changes nothing.
- Ban the pattern, not the instance. "No labelled-aside sentences" (`Label: content`, used as filler) outlasts a list of banned labels.
- Treat a tautological step as a bug. If restating the goal would satisfy the step as written, it's not a step: "verify it works" tells an agent nothing "make it work" didn't already say. Compare [verify](../verify/SKILL.md)'s "reproduce the original failure, then reproduce again after the fix," which specifies what to observe.
- When the skill's subject is itself phrasing or wording, show a Bad/Good pair, not just an adjective. "Terse" is an opinion; `"That password is too short"` → `"Choose a password with at least 8 characters"` is checkable.

## Vendoring

A skill imported from another library and kept as-is doesn't go through Contract or Writing guidelines above: it's someone else's instructions, not yours to rewrite. Wrap it in a thin `SKILL.md` that states its source and any license/attribution, and keep the original content under a clearly separated path. Only a skill you're authoring or substantially rewriting needs this skill's rules applied.

## Validate

Structural correctness (frontmatter parses, links resolve, `index.json` matches) proves the file is well-formed. It does not prove another agent follows the instructions successfully. That needs the same evidence standard as any other claim of "done," per [verify](../verify/SKILL.md):

1. Run the skill on a representative task using only its stated inputs.
2. Run a near-miss task (one that shares a keyword or surface similarity but shouldn't trigger the skill) to confirm it doesn't over-fire.
3. Run a scenario missing a stated prerequisite, to confirm the skill admits the gap instead of faking success.
4. Prefer a fresh agent's walkthrough as evidence over a same-session one: an agent that already knows what you meant to write is a weaker test of whether the words alone carry the instruction.

## Failure handling

If validation surfaces a gap, fix the skill and re-run the same three checks. Don't ship on the strength of the first pass that happened to work. If a candidate skill turns out not to earn its place (redundant, too narrow to reuse, or the task it served was one-off), don't force it in: record why in `docs/out-of-scope/<slug>.md` instead, so the decision doesn't get re-litigated next time the idea resurfaces.

## Completion criteria

The skill (new or changed) has a matching [index.json](../../index.json) entry, has passed all three validation checks with the evidence stated, and, if anything was deliberately cut from its scope, that cut is recorded in `docs/out-of-scope/`.
