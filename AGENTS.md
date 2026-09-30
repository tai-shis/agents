# tai-shis/agents — skill library

You are either (a) an agent loading skills *from* this library over HTTP, or (b) an agent doing development work *on* this library. Figure out which applies and read accordingly.

## (a) Loading skills from this library

Fetch the catalog and follow the protocol in [bootstrap/AGENTS.md](bootstrap/AGENTS.md):

```
GET https://co.codes/t:a/tai-shis/agents/index.json?ref=main
```

Most of the time you won't be reading this file to do that — it'll already be summarized in your own global instructions (deployed from the companion nix config's `cfg/agents/AGENTS.md`). This file exists for when you land here directly.

## (b) Working on this repo

This repo *is* the skill library — its own Markdown files, not app code. When adding or editing a skill:

1. Read [docs/loading.md](docs/loading.md) for the authoring contract (frontmatter shape, portability rules, format).
2. Read [docs/publishing.md](docs/publishing.md) before pushing.
3. Keep [index.json](index.json) in sync with whatever's under `skills/` — every skill referenced there needs an entry, and vice versa.
4. Skills must work when fetched cold over HTTP by an agent with no other context on this repo. Don't write a skill that assumes the reader has this whole repo checked out.

## Layout

- `index.json` — catalog (schema 1): the discovery entry point for consumers.
- `bootstrap/AGENTS.md` — the remote loading contract (protocol, not skill content).
- `docs/` — authoring and publishing guides for contributors.
- `skills/<name>/SKILL.md` — one skill per directory.

## Attribution

Skills here draw on the pstack methodology (verification-first agent work, mode-based dispatch) originated by [@poteto](https://x.com/poteto) (Lauren Tan) — adapted, not copied, for this library's own protocol and conventions.
