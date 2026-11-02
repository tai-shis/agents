# design-md: companion stylesheet

**Considered:** 2026-11-02
**Decision:** declined
**Why:** Vercel pairs its design.md with a public stylesheet of named classes and tokens, so agents outside its codebase build against fixed primitives. [design-md](../../skills/design-md/SKILL.md) instead names a repository's own components and tokens in the file. Generating, publishing, and versioning a CSS API would make the skill much larger, and it only helps agents working outside the repository.
**Revisit if:** DESIGN.md files need to drive one-off pages outside a codebase (reports, proposals), or comparison runs show agents still inventing typography and spacing with the tokens in hand.
