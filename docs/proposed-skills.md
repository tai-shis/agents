# Proposed skills

Ideas for future skills in this library, not yet written. Not a commitment and not a catalog entry: run any of these through [skill-creator](../skills/skill-creator/SKILL.md) before it becomes one.

## Writing / wording

A personal voice-and-phrasing skill: Bad/Good comparison-table format (per [skill-creator](../skills/skill-creator/SKILL.md)'s writing guidelines), tied to Tai's own preferences once stated concretely: commit messages, PR descriptions, comments, general prose. Every surveyed example library independently has one of these (0xhckr's and matthew-hre's `better-writing`, mattpocock's glossary/changeset discipline), a strong signal it earns a place here too.

**Status:** waiting on concrete phrasing examples from Tai before drafting.

## Coding practices

Mostly covered now. [coding-practices](../skills/coding-practices/SKILL.md) holds the language-agnostic rules (naming, function shape, file organization), and [tai-components](../skills/tai-components/SKILL.md) holds React and Next.js component conventions. What's left is conventions for other languages and stacks, written as their own skills or as an atomic `rules/` directory (one topic per file, per [skill-creator](../skills/skill-creator/SKILL.md)'s progressive-disclosure allowance) if they grow unevenly (matthew-hre's `vercel-react-best-practices/rules/` is the model).

**Status:** waiting on a language or stack that needs its own conventions.

## Not yet validated

These skills shipped without [skill-creator](../skills/skill-creator/SKILL.md)'s validation checks (representative task, near-miss task, missing prerequisite). Run those checks on first real use, fix any gaps found, and remove the skill from this list.

- [storybook](../skills/storybook/SKILL.md)
- [tai-ui](../skills/tai-ui/SKILL.md)
- [tai-components](../skills/tai-components/SKILL.md)
- [design-md](../skills/design-md/SKILL.md): partly validated. Fresh-agent walkthroughs covered every mode and the near-miss and missing-prerequisite checks, but its two external steps have never run: the linter (step 6) and the with-and-without comparison (step 7).
- [tai-setup](../skills/tai-setup/SKILL.md): partly validated. Its commands ran end to end on NixOS `x86_64-linux` on the shadcn and Postgres path, and the coss path ran through the icon rewrite and build. No fresh agent has followed it, and the near-miss and missing-prerequisite checks are unrun. Other systems in the flake and non-Postgres databases are untested.

## Deprioritized

- **Skill discovery / remote-skills equivalent.** matthew-hre's `find-skills` wraps a public marketplace CLI (`npx skills`) this library doesn't use. The closer analog is 0xhckr's `remote-skills` (catalog-fetch + dependency-resolution protocol), but most of that ground is already covered by [bootstrap/AGENTS.md](../bootstrap/AGENTS.md). Revisit only if the catalog grows large enough that matching a task to a skill from `index.json` alone stops being reliable.

## Other backlog (not a skill)

- **A way to customize Claude Code itself.** Tai mentioned this exists and wants it documented somewhere, but didn't say which mechanism (project `CLAUDE.md`, `settings.json`, hooks, slash commands, a plugin, something else). Needs a follow-up question before anyone acts on it. Logged here instead of guessed at.
- **Portless as the standard for local dev environments.** Tested and left out of [tai-setup](../skills/tai-setup/SKILL.md) for now. See [the out-of-scope note](out-of-scope/tai-setup-portless.md) for the findings and what would reopen it.
- **ADR generation — deferred, framed for personal use.** For personal/small-scale projects, skip formal ADR tooling: a decision log in a plain `docs/` folder, written arbitrarily as needed, is enough. Revisit this once there's an actual industry/team project in scope, where audience and stakes change what a decision record needs to do. Don't build ADR tooling before that need is real.
