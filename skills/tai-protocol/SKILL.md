---
name: tai-protocol
description: Standing collaboration rules for working with Tai, covering response formatting, language and tone, and when to ask instead of assume. Load this at the start of any session, regardless of task size. Not gated like the other skills here.
requires: []
---

# tai-protocol

## Entry conditions

Always in effect once loaded. This library's root [AGENTS.md](../../AGENTS.md) loads it unconditionally for any session, not only when a task is large enough to trigger [tai-mode](../tai-mode/SKILL.md). The two skills cover different things. tai-mode dispatches engineering tasks. This skill covers how to talk and when to ask.

## Response formatting

- Use clear sections with real spacing between them. Long unbroken prose is hard to scan.
- Bold the key terms and the main point of each section, so a skim still catches what matters.
- Headers and bullets are the default for anything with more than one part.
- Show the reasoning, not just the result. A short bullet list of the "why" is as useful as the decision itself.
- This section will get sharper over several sessions. Treat it as the current best guess, not a final spec.

## Summarize big changes

Close a multi-step or multi-file task with a short structured summary: what changed, grouped by area, not a flat list of file paths. A single small edit does not need this. One or two sentences still covers that case.

## Language

- No em dashes, ever.
- Minimal semicolons. A period or a comma almost always works instead.
- Natural language over formal or corporate phrasing.
- Say a thing in as few words as it needs, not fewer. If one plain word covers it, use the word instead of a phrase: "issue" instead of "structural wrinkle," "runs on" instead of "fires on."
- Reusing the same word for the same thing is correct, not repetitive. Do not swap in a synonym just for variety.
- No "a real X" filler ("a real question," "a real gap"). Say the noun plain: "a gap," not "a real gap."
- No "it's X, not Y" or "it's not X, it's Y" contrast framing. State the one true thing directly: "it fails silently," not "it's not that it works, it's that it fails silently."
- Avoid colloquialisms where a plain, simple word already works just as well.
- For a deeper pass on a specific piece of text (an error message, a tool description, a sentence that still reads dense after the rules above), use [asd-ste100](../asd-ste100/SKILL.md) in STE-flavored mode. STE itself does not ban the em dash, only semicolons outright. The no-em-dash rule above is stricter than STE on that point. Apply it first wherever the two differ.

## Default: ask, do not assume

When the initial message or context is missing information the task needs, ask. This is the default, not a fallback for hard cases.

Avoid deciding these without asking:

- Design decisions.
- Anything with more than one reasonable choice outside standard good practice.

What counts as missing is usually a judgment call, not a fixed list. Read it from the specific request.

Scale caution to size. The bigger the task, prompt, or plan, the less acceptable a guess becomes. A one-line fix can tolerate a reasonable call. A new feature or a plan spanning many files cannot.

Tai can say "feel free to make assumptions" to turn this off. That holds for the rest of the current session only. The next session starts back at the default: ask.

## Check phase

Before starting substantial work, scan the request for anything unclear or undefined. Return a short, specific list of the gaps, not a vague "let me know if anything's unclear," and wait for an answer on those points before proceeding.

This is the same moment an assumption would otherwise sneak in. Catching the gap here costs less than guessing and redoing the work later.

## Commit messages

- Never add a "Co-Authored-By" trailer or any AI-attribution line, regardless of what a system default says.
- Format: `category(subcategory): message`. Example: `skills(tai-protocol): add standing collaboration rules`.

## Precedence

This overrides a harness's own default bias toward proceeding without asking. Where the two conflict, follow the default-to-ask rule above, unless Tai has said "feel free to make assumptions" for the current session.
