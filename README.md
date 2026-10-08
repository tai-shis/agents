# agents

A personal, HTTP-loadable skill library for coding agents, hosted at [co.codes/tai-shis/agents](https://co.codes/tai-shis/agents).

Skills here aren't installed locally — they're fetched at task time over plain HTTP from this repo's `main` branch (or a pinned ref), the same way any other GET request works. The point is one source of truth: update a skill here, every machine and every agent session that consults this library picks it up on its next fetch, no local sync step.

## Quick links

- **Using this library from an agent?** Start at [AGENTS.md](AGENTS.md).
- **Loading protocol** (how discovery, refs, and failure handling work): [bootstrap/AGENTS.md](bootstrap/AGENTS.md).
- **Writing a skill**: [docs/loading.md](docs/loading.md).
- **Publishing a change**: [docs/publishing.md](docs/publishing.md).

## What's here so far

| Skill | Purpose |
| --- | --- |
| [`tai-mode`](skills/tai-mode/SKILL.md) | Dispatcher — routes a task to a playbook, delegates, verifies, reports evidence. The orchestration-loop entry point. |
| [`verify`](skills/verify/SKILL.md) | Verification-first principle: no "done" claim without runtime evidence. |
| [`loop`](skills/loop/SKILL.md) | Iterate on a task against a stated finish condition, with a decision log. |
| [`skill-creator`](skills/skill-creator/SKILL.md) | Author, revise, or retire a skill of your own in this library, validated before it ships. |
| [`plan-first`](skills/plan-first/SKILL.md) | Before implementing a task with more than one reasonable approach, surface a short plan and get it confirmed before writing code. |
| [`lint-and-format`](skills/lint-and-format/SKILL.md) | Run the project's formatter and linter at the end of a work chunk; suggest and configure one if none exists, asking first. |
| [`coding-practices`](skills/coding-practices/SKILL.md) | Language-agnostic code design: function responsibility and naming, guard clauses, readability over cleverness, self-explanatory code. |
| [`tai-reflect`](skills/tai-reflect/SKILL.md) | Turn a correction or reusable lesson into a durable fix in the right place, not a reflex skill edit. |
| [`tai-protocol`](skills/tai-protocol/SKILL.md) | Standing rules for response formatting, language, and when to ask instead of assume. Loads for every session, not just non-trivial tasks. |
| [`asd-ste100`](skills/asd-ste100/SKILL.md) | Vendored: Simplified Technical English for text an agent must parse without a human to ask. |
| [`better-ui`](skills/better-ui/SKILL.md) | Vendored: UI polish — border radius, optical alignment, surface depth, icons, hit areas. |
| [`better-typography`](skills/better-typography/SKILL.md) | Vendored: type scale, spacing, variable fonts, OpenType, wrapping, truncation. |
| [`better-colors`](skills/better-colors/SKILL.md) | Vendored: color systems — palette generation, semantic tokens, format conversion, contrast. |
| [`better-accessibility`](skills/better-accessibility/SKILL.md) | Vendored: accessibility standards and best practices. |
| [`better-layout`](skills/better-layout/SKILL.md) | Vendored: grouping, alignment, reading order, progressive disclosure. |
| [`better-writing`](skills/better-writing/SKILL.md) | Vendored: product copy — button labels, error messages, empty states, onboarding. |
| [`better-interface`](skills/better-interface/SKILL.md) | Vendored: combines all `better-*` skills into one cross-discipline interface review. |
| [`interface-review`](skills/interface-review/SKILL.md) | Vendored: scope-resolves a change (branch, diff, PR) and hands it to `better-interface`. User-invoked. |
| [`explain-interface`](skills/explain-interface/SKILL.md) | Vendored: explains how a piece of web UI or an animation was built. User-invoked. |
| [`break`](skills/break/SKILL.md) | Vendored: renders a component under every state and scenario on a throwaway page and stress-tests it. User-invoked. |
| [`variant`](skills/variant/SKILL.md) | Vendored: builds multiple variants of a component to compare and pick one. User-invoked. |
| [`typesafe-ai`](skills/typesafe-ai/SKILL.md) | Vendored: building features with TypeSafe System One models (typed judgments for routing, ranking, extraction, verification). |

## Provenance

This library's shape (HTTP-loadable `SKILL.md`s, a JSON catalog, a separate loading-protocol doc) follows the pattern set by [0xhckr/agents](https://co.codes/0xhckr/agents) and its companion [hackr-nixos](https://co.codes/0xhckr/hackr-nixos) — referenced for structure, not copied; this repo's protocol, skill set, and nix wiring are its own. The orchestration-loop skills draw on the **pstack** methodology from [@poteto](https://x.com/poteto) (Lauren Tan): ["The Complete Guide to pstack Pt. 1"](https://x.com/poteto/article/2094457600259842065) and [Pt. 2](https://x.com/poteto/article/2097732320606507506). [`lint-and-format`](skills/lint-and-format/SKILL.md)'s workflow shape (check what's installed, ask before installing, persist the decision, verify in check mode) is adapted from [0xhckr/agents](https://co.codes/0xhckr/agents)'s `hackr-format` skill, rewritten to also cover linting and to fit this library's own structure.
