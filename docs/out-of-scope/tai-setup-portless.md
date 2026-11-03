# tai-setup: portless as a default

**Considered:** 2026-11-02
**Decision:** declined for now
**Why:** Tai chose to leave it out of [tai-setup](../../skills/tai-setup/SKILL.md) after testing. Portless 0.15.13 is pre-1.0 and not in nixpkgs, and `portless trust` does not support NixOS. Its default HTTPS mode needs sudo, so an agent with no terminal cannot start it, and unprivileged mode needs a sysctl on the machine. `PORTLESS_PORT=1355 PORTLESS_HTTPS=0` did work unattended with Next.js 16, including separate URLs for linked worktrees, but hot reload through the proxy was never confirmed in a browser.
**Revisit if:** parallel agent runs on one project start colliding on ports in practice. Start with a browser check of hot reload.
