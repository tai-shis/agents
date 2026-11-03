# tai-setup: Nix package and app suite

**Considered:** 2026-11-02
**Decision:** declined
**Why:** hackr-setup requires a default package, a default app, a `nix run .#NAME` task suite, and sandboxed checks. Tai chose a dev shell only for [tai-setup](../../skills/tai-setup/SKILL.md), with project commands kept in the package manifest. This avoids a second command system and the lockfile hash upkeep of packaging web apps with Nix.
**Revisit if:** a project is deployed through Nix or ships a CLI that people run with `nix run`. Add those outputs to that project when asked, and change the skill's default only if this keeps happening.
