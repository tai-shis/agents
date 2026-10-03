---
name: better-interface
description: Combines all of the `better-*` skills into a single review across accessibility, layout, writing, typography, color and UI polish.
requires: [skills/better-ui/SKILL.md, skills/better-typography/SKILL.md, skills/better-colors/SKILL.md, skills/better-accessibility/SKILL.md, skills/better-layout/SKILL.md, skills/better-writing/SKILL.md]
---

# better-interface

Vendored, not authored here. The full skill lives at [vendor/better-interface/SKILL.md](../../vendor/better-interface/SKILL.md), unchanged from the source below. Fetch that file for the actual rules, process, and output format. This page only states where it came from.

This skill is orchestration only: it routes a review to the six `better-*` skills listed in `requires` above, collects their evidence, and issues one ranked verdict. It owns no domain rules of its own, which is why all six are unconditional dependencies here rather than inline mentions — the orchestration cannot run without them.

## Source

[jakubkrehel/skills](https://github.com/jakubkrehel/skills), MIT License, copyright Jakub Krehel. Listed at [jakub.kr/skills](https://jakub.kr/skills), the page this skill was pulled from. Mirrored here (not linked externally) so it resolves the same way as every other skill in this library: one fetch, no dependency on an outside host staying up.

## Supporting files

Also vendored under [vendor/better-interface/](../../vendor/better-interface/): `LICENSE`, `agents/openai.yaml` (an OpenAI Agents SDK variant of this same skill from the source project), and `review-format.md`, the reference the main skill links out to.

## Related in this family

This is one of eleven skills from the same source that reference each other by name in their own text. A bare mention of one of these names inside the vendored content means its sibling here, beyond the six already listed in `requires`: [interface-review](../interface-review/SKILL.md), [explain-interface](../explain-interface/SKILL.md), [break](../break/SKILL.md), [variant](../variant/SKILL.md).
