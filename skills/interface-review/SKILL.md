---
name: interface-review
description: Reviews your work across multiple categories like UI, typography, layout, color, writing and accessibility and gives you a detailed analysis of the findings.
requires: [skills/better-interface/SKILL.md]
---

# interface-review

Vendored, not authored here. The full skill lives at [vendor/interface-review/SKILL.md](../../vendor/interface-review/SKILL.md), unchanged from the source below. Fetch that file for the actual rules, process, and output format. This page only states where it came from.

This skill owns scope resolution for a change (a branch, a diff, a pull request) and nothing past that: it always hands the resolved review off to [better-interface](../better-interface/SKILL.md) for severity, consolidation, and the verdict, which is why that's listed as an unconditional dependency above rather than an inline mention.

## Invocation note

The source marks this skill user-invoked only (`disable-model-invocation: true` in its native Claude Code frontmatter) — it runs only when explicitly asked for, not auto-triggered by conversation content. This library's frontmatter contract doesn't carry that field (see [docs/loading.md](../../docs/loading.md)), so nothing here enforces it mechanically. Treat it as a standing instruction: reach for this on an explicit ask to review a change's interface, not because "review" or "diff" showed up in a message.

## Source

[jakubkrehel/skills](https://github.com/jakubkrehel/skills), MIT License, copyright Jakub Krehel. Listed at [jakub.kr/skills](https://jakub.kr/skills), the page this skill was pulled from. Mirrored here (not linked externally) so it resolves the same way as every other skill in this library: one fetch, no dependency on an outside host staying up.

## Supporting files

Also vendored under [vendor/interface-review/](../../vendor/interface-review/): `LICENSE`, `agents/openai.yaml` (an OpenAI Agents SDK variant of this same skill from the source project), and the reference files the main skill links out to: `removed-signals.md`, `scope-resolution.md`.

## Related in this family

This is one of eleven skills from the same source that reference each other by name in their own text. A bare mention of one of these names inside the vendored content means its sibling here, beyond `better-interface` already listed in `requires`: [better-ui](../better-ui/SKILL.md), [better-typography](../better-typography/SKILL.md), [better-colors](../better-colors/SKILL.md), [better-accessibility](../better-accessibility/SKILL.md), [better-layout](../better-layout/SKILL.md), [better-writing](../better-writing/SKILL.md), [explain-interface](../explain-interface/SKILL.md), [break](../break/SKILL.md), [variant](../variant/SKILL.md).

Also worth knowing: this skill is read-only by its own rule (no checkout, no working-tree mutation), which fits alongside [verify](../verify/SKILL.md)'s evidence standard, but it is not a dependency of verify and verify is not a dependency of it.
