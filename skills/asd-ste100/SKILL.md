---
name: asd-ste100
description: "Use when English text must be parsed without a human to resolve ambiguity: tool descriptions, error messages, inter-agent instructions, system prompts, status reports, or any text that reads dense, hedged, or easy to misparse. Triggers: disambiguate, STE100 rewrite, apply Simplified Technical English, plain-language rewrite, controlled-language rewrite. Not for creative or marketing copy."
requires: []
---

# asd-ste100

Vendored, not authored here. The full skill lives at [vendor/asd-ste100/SKILL.md](../../vendor/asd-ste100/SKILL.md), unchanged from the source below. Fetch that file for the actual rules, process, and output format. This page only states where it came from.

## Source

[danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill), MIT License, copyright Dustin Yuchen Teng. Mirrored here (not linked externally) so it resolves the same way as every other skill in this library: one fetch, no dependency on an outside host staying up.

## Supporting files

Also vendored under [vendor/asd-ste100/](../../vendor/asd-ste100/): `README.md`, `LICENSE`, `references/writing-rules.md`, `examples/before-after.md`, `examples/linter-edge-cases.md`, and `scripts/ste-lint.py` (a dependency-free Python linter for the mechanical structural rules: semicolons, sentence length, phrasal verbs, nominalization, marketing adjectives, synonym rotation, passive voice, compound tenses).

## Where this fits in this library

[tai-protocol](../tai-protocol/SKILL.md) references this skill's STE-flavored mode for a deeper pass on a specific piece of text. One difference to keep straight: STE itself does not ban the em dash, only semicolons outright. Tai's own rule in `tai-protocol` is stricter than STE on that one point. Apply Tai's rule first wherever the two differ.
