# Proposed skills

Ideas for future skills in this library, not yet written. Not a commitment and not a catalog entry: run any of these through [skill-creator](../skills/skill-creator/SKILL.md) before it becomes one.

## Writing / wording

A personal voice-and-phrasing skill: Bad/Good comparison-table format (per [skill-creator](../skills/skill-creator/SKILL.md)'s writing guidelines), tied to Tai's own preferences once stated concretely: commit messages, PR descriptions, comments, general prose. Every surveyed example library independently has one of these (0xhckr's and matthew-hre's `better-writing`, mattpocock's glossary/changeset discipline), a strong signal it earns a place here too.

**Status:** waiting on concrete phrasing examples from Tai before drafting.

## Coding practices

Naming conventions and structural preferences (variable names, function shape, file organization). Likely an atomic `rules/` directory (one topic per file, per [skill-creator](../skills/skill-creator/SKILL.md)'s progressive-disclosure allowance) rather than one monolith, since this will grow unevenly across languages and domains (matthew-hre's `vercel-react-best-practices/rules/` is the model).

**Status:** waiting on concrete code examples from Tai before drafting.

## Deprioritized

- **Skill discovery / remote-skills equivalent.** matthew-hre's `find-skills` wraps a public marketplace CLI (`npx skills`) this library doesn't use. The closer analog is 0xhckr's `remote-skills` (catalog-fetch + dependency-resolution protocol), but most of that ground is already covered by [bootstrap/AGENTS.md](../bootstrap/AGENTS.md). Revisit only if the catalog grows large enough that matching a task to a skill from `index.json` alone stops being reliable.

## Other backlog (not a skill)

- **A way to customize Claude Code itself.** Tai mentioned this exists and wants it documented somewhere, but didn't say which mechanism (project `CLAUDE.md`, `settings.json`, hooks, slash commands, a plugin, something else). Needs a follow-up question before anyone acts on it. Logged here instead of guessed at.
