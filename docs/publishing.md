# Publishing changes

This repo is plain `git`, pushed to `https://git.co.codes/tai-shis/agents.git` (already the `origin` remote). No `jj` — just ordinary git.

## One-time setup (already done for this checkout)

The `co` CLI (`co login`, then `co link tai-shis/agents` or an equivalent clone) wires a per-host git credential helper for `https://git.co.codes`, so `git push` works without a separate token. Check it with:

```
co doctor
```

`git       ok (HTTPS credential helper ready)` means you're set.

## Workflow

1. Make the change under `skills/`, `docs/`, or the root/bootstrap `AGENTS.md` files.
2. If you touched `skills/`, update [`index.json`](../index.json) to match (see [loading.md](loading.md)).
3. Review the diff — `git status`, `git diff` — before staging. Public repo: don't commit anything you wouldn't want fetched by a stranger's agent a minute later.
4. Commit with a message describing *why*, not just *what changed* (the diff already shows what).
5. `git push origin main`.
6. **Confirm publication, don't assume it** — a local commit is not evidence the library updated. Fetch the file back from co.codes and check it matches:

   ```
   curl -sf "https://co.codes/t:a/tai-shis/agents/<path>?ref=main"
   ```

   or re-fetch `index.json` the same way for a catalog change. If the pushed content doesn't come back, treat the change as unpublished — report the local commit, the destination, and what step failed.

## If push is rejected

Someone (or another agent) pushed to `main` since your last fetch. Pull, resolve, re-verify with the same fetch-back check — don't force-push over a public branch.
