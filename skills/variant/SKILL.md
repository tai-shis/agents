---
name: variant
description: Builds multiple variants of a component you're working on and helps you iterate and pick one.
requires: []
---

# variant

Vendored, not authored here. The full skill lives at [vendor/variant/SKILL.md](../../vendor/variant/SKILL.md), unchanged from the source below. Fetch that file for the actual rules, process, and output format. This page only states where it came from.

## Invocation note

The source marks this skill user-invoked only (`disable-model-invocation: true` in its native Claude Code frontmatter) — it runs only when explicitly asked for, not auto-triggered by conversation content. This library's frontmatter contract doesn't carry that field (see [docs/loading.md](../../docs/loading.md)), so nothing here enforces it mechanically. Treat it as a standing instruction: reach for this on an explicit ask to produce component variants, not because "options" or "alternatives" showed up in a message.

## Source

[jakubkrehel/skills](https://github.com/jakubkrehel/skills), MIT License, copyright Jakub Krehel. Listed at [jakub.kr/skills](https://jakub.kr/skills), the page this skill was pulled from. Mirrored here (not linked externally) so it resolves the same way as every other skill in this library: one fetch, no dependency on an outside host staying up.

## Supporting files

Also vendored under [vendor/variant/](../../vendor/variant/): `LICENSE`, `agents/openai.yaml` (an OpenAI Agents SDK variant of this same skill from the source project), and `picker.md`, the reference the main skill links out to.

## Related in this family

This is one of eleven skills from the same source that reference each other by name in their own text. A bare mention of one of these names inside the vendored content means its sibling here: [better-ui](../better-ui/SKILL.md), [better-typography](../better-typography/SKILL.md), [better-colors](../better-colors/SKILL.md), [better-accessibility](../better-accessibility/SKILL.md), [better-layout](../better-layout/SKILL.md), [better-writing](../better-writing/SKILL.md), [better-interface](../better-interface/SKILL.md), [interface-review](../interface-review/SKILL.md), [explain-interface](../explain-interface/SKILL.md), [break](../break/SKILL.md).
