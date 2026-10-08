---
name: typesafe-ai
description: Builds AI-powered features with TypeSafe, small System One models that return typed judgments and probabilities code can combine. Use when a feature needs programmable common sense, such as routing, ranking, extraction, or verification.
requires: []
---

# typesafe-ai

Vendored, not authored here. The full skill lives at [vendor/typesafe-ai/SKILL.md](../../vendor/typesafe-ai/SKILL.md), unchanged from the source below. Fetch that file for the actual rules, process, and doc links. This page only states where it came from.

## Source

[typesafe-ai/skills](https://github.com/typesafe-ai/skills) at commit `65a39f393687675ce170e6094757de20370365b9`, MIT License, copyright TypeSafe AI. Listed at [docs.typesafe.ai/agent-skill](https://docs.typesafe.ai/agent-skill), the page this skill was pulled from. Mirrored here (not linked externally) so it resolves the same way as every other skill in this library: one fetch, no dependency on an outside host staying up.

## Scope

The vendored skill sends the agent to the live TypeSafe docs at `docs.typesafe.ai` for API details and cookbooks, so those pages still need network access. Its own description says an outdated copy can make an agent invent request or response fields. If that happens, re-vendor from the source above.

## Supporting files

Also vendored under [vendor/typesafe-ai/](../../vendor/typesafe-ai/): `LICENSE`. The source repo's Claude Code plugin files (`.claude-plugin/`) were left out, since this library loads skills over HTTP and does not install plugins.
