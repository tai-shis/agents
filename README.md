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

## Provenance

This library's shape (HTTP-loadable `SKILL.md`s, a JSON catalog, a separate loading-protocol doc) follows the pattern set by [0xhckr/agents](https://co.codes/0xhckr/agents) and its companion [hackr-nixos](https://co.codes/0xhckr/hackr-nixos) — referenced for structure, not copied; this repo's protocol, skill set, and nix wiring are its own. The orchestration-loop skills draw on the **pstack** methodology from [@poteto](https://x.com/poteto) (Lauren Tan): ["The Complete Guide to pstack Pt. 1"](https://x.com/poteto/article/2094457600259842065) and [Pt. 2](https://x.com/poteto/article/2097732320606507506).
