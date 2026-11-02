---
name: design-md
description: Create or revise a project's DESIGN.md, the file that tells coding agents how its UI should look and how its pages should be composed. Works from scratch through an interview, from an existing repository by extracting its design system, or from repeated design corrections. Not for building UI from an existing DESIGN.md, or for one-off color, type, or layout questions.
requires: [skills/verify/SKILL.md]
---

# design-md

## Entry conditions

Reach for this when the user asks to create a DESIGN.md, extract one from a repository, or change one, including because agents keep making the same design mistake.

It does not apply to:

- Building or changing UI in a project that already has a DESIGN.md. That is [tai-ui](../tai-ui/SKILL.md), which reads the file.
- Choosing a palette, type scale, or layout with no DESIGN.md involved. That is [better-colors](../better-colors/SKILL.md), [better-typography](../better-typography/SKILL.md), or [better-layout](../better-layout/SKILL.md).
- Reviewing a UI change. That is [interface-review](../interface-review/SKILL.md), on the user's explicit ask.
- A request to find out why agents ignore an existing file. Investigate that per [debug](../debug/SKILL.md), starting with whether the agent loaded the file at all. Report the cause, and change nothing unless the user asks. A request to fix a repeated mistake in the file is Revise, in step 4.
- A design document for software architecture, such as a service design. This file covers visual design only.

## The format

The output follows [Google's DESIGN.md spec](https://raw.githubusercontent.com/google-labs-code/design.md/main/docs/spec.md): YAML tokens that hold exact values, and prose sections in a fixed order. Fetch the current spec before writing. It is at version `alpha` and can change. Where it disagrees with this skill, follow the spec and name the difference in the report. The linter is documented in [its README](https://raw.githubusercontent.com/google-labs-code/design.md/main/README.md).

The prose standard comes from [Vercel's write-up](https://vercel.com/blog/how-our-agents-build-on-brand-pages-with-design-md): rules a reviewer can check, patterns rejected by name, and page structure driven by what the reader needs to do. Tokens alone leave composition to each agent's defaults. In Vercel's comparison, the file changed page structure and hierarchy as well as styling. You do not need to read the write-up or [Vercel's own file](https://vercel.com/design.md) to follow this skill. Vercel's file covers report websites for a large brand, so do not copy its rules or match its length.

## Steps

### 1. Pick the mode

**First, confirm the project has a visual interface.** Web, mobile, and desktop apps, and generated pages or reports, all count. If the project has none and none is planned, such as a data pipeline or a command-line tool with plain text output, say that a DESIGN.md has nothing to govern, and stop unless the user still wants one.

Then look for an existing DESIGN.md and UI code, and choose:

| What exists | What the user wants | Mode | Go to |
| --- | --- | --- | --- |
| A DESIGN.md | A token or rule changed, and they gave the new value or wording | Small edit | Step 4.5 |
| A DESIGN.md | Corrections encoded, or a section added, such as a page type or dark values | Revise | Step 4 |
| A DESIGN.md | To start over | Confirm overwriting it, then treat as if no file exists | |
| UI code, no DESIGN.md | To capture what the code does | From repo | Step 3 |
| UI code, no DESIGN.md | A new visual identity | From scratch | Step 2 |
| UI code, no DESIGN.md | Not stated | Ask which of the two | |
| No UI code yet | Either | From scratch | Step 2 |

If the user asks for "from repo" and there is no UI code to extract from, say so, name what you searched, and offer from scratch. Do not invent a design system and present it as extracted. If the repository holds more than one app or brand, ask which one the file covers.

Evidence: the mode, what was checked to choose it, and the file path. The default path is `DESIGN.md` at the project root. State it in your first message so the user can change it.

### 2. From scratch: interview, then directions

1. **Ask one grouped round of questions,** per [tai-protocol](../tai-protocol/SKILL.md). Skip anything the user has already answered.
   - What the product is, and who uses it.
   - The most common page type, and what its reader needs to do there.
   - Three words for how it should feel, and one thing it must not look like.
   - Two or three products or sites they like, and what they like about each.
   - Fixed constraints: an existing logo or brand colors, font licensing, light or dark themes, density, and the frontend stack.
2. **Write two or three directions in the chat.** Each one gets a name, a one-line idea, four or five colors with hex values and roles, a type pairing, corner radius, and density, and a sentence on how the main page type opens. The directions must differ in type, shape, and density, except where the user fixed one of those. Three color swaps of one layout are one direction. Compute contrast for each direction's text and surface pairs using the thresholds in step 6, and replace any color that fails. If a color the user fixed fails, keep it and say so.
3. **Wait for a choice.** Do not write the file before the user picks. If they combine directions, restate the combined version and confirm it. If they tell you to choose ("you pick", "do whatever"), recommend one, say why, and treat it as chosen. List it in the report as delegated. An empty or unrelated answer is not a choice.

Then go to step 5, using the chosen direction as the source for every value.

Evidence: the interview answers and the chosen direction, as stated in the conversation.

### 3. From repo: extract, propose, confirm

Extract first, so your questions are specific. This does not replace asking about the request itself if it is unclear.

1. **Find the declared system.** Search for theme and token sources: Tailwind config or `@theme` blocks, CSS custom properties, theme objects, `tokens.json`, and component library configuration.
2. **Measure actual use.** Search the UI code for colors, font sizes, font families, radii, spacing, and shadows, and resolve each one to its final value. Count uses, and note separately whether a value is declared. `text-sm` and `text-[14px]` are one value spelled two ways, and a class like `rounded-md` resolves through the project's theme, not the framework default.
3. **Read the core components.** Open the buttons, inputs, cards, and navigation, and note their file paths and how they take variants.
4. **Look at the running app, if it can run.** View the main pages at a narrow and a wide viewport, in each theme it supports. If it cannot run, say so in your message and in the report, because roles then come from code alone.
5. **Check contrast on the likely values,** using the thresholds in step 6, so failures reach the user before they confirm anything.
6. **Send one message** with the proposals and the questions together, then wait.

   **Proposals.** One table, with one row per job that a group of values does:

   | Role | Values found (count) | Example | Proposed | Why |
   | --- | --- | --- | --- | --- |
   | Body text | `#1F2937` (41), `#222` (6), `#2B2B2B` (2) | `src/ui/Card.tsx:14` | `#1F2937` | Declared token, most used |
   | Caption, dark **(your call)** | None | `src/theme.css:30` | None yet | The code sets no dark value, and `#71717A` on the dark surface fails at 3.9:1 |

   Mark a row **(your call)** when counts tie, when you are unsure two values do the same job, when a role has no value, or when a value fails contrast. Leave a role with no value empty unless the user asks you to propose one. Label a framework default as a default.

   **Questions.** Brand intent is not in the repository, so ask for what the code cannot show: what the product is and who uses it, how it should feel and what it must not look like, the main page types and what their readers need to do, and the corrections the user repeats most often. Do not infer intent from a color and state it as fact.

The file records the values the user confirmed. Whatever still differs in the code goes under Known Drift in step 5. Do not change application code.

Then go to step 5.

Evidence: the table, the user's confirmation, and whether the app was viewed.

### 4. Revise: turn corrections into rules

1. **Collect the corrections or additions** from the user, or from review comments and threads they point to. Read the current file, and keep a copy outside the project for step 6, unless version control already holds it.
2. **Route each correction to one home:**

   | The correction is | Where it goes |
   | --- | --- |
   | An exact value | A token |
   | A judgment about structure or hierarchy | A prose rule that a reviewer can check |
   | Something code can detect | The user's choice of a test or lint rule. Suggest it, and do not add it without asking. |
   | A defect in one output, seen once | Nowhere yet. Note it, and add a rule if it repeats. |
   | Already covered by a rule that agents ignored | A clearer rule, after the load question below |

   **The load question.** When a rule already covers the mistake, ask whether those agents were given the file, or look at a transcript if the user has one, and check whether the project's agent instructions point to it. If the agents did not load it, or nobody can tell, say that a rewrite may change nothing and suggest the pointer from step 6.6. Rewrite the rule only if the user still wants that.
3. **Show the proposed wording** as before and after, and wait for approval. Revise a matching rule instead of adding a second one. Put a rule about one page type in its Page Composition entry, and a rule for all pages under Do's and Don'ts.
4. **Apply the approved edits,** using the rules in step 5. Update Known Drift: remove entries the change settles, and add a place where the code now differs from the file.
5. **Small edits skip the steps above.** Keep a copy as in item 1. If the new value is a color, first compute contrast for every pair that uses it, including component pairs, using the thresholds in step 6.3. If a pair fails, show the failure and a proposed fix before editing, and leave the file unchanged until the user decides. Then change the token and every prose mention of it, and update Known Drift.

Then go to step 6.

Evidence: the routing table and the approved wording, or for a small edit, the contrast result.

### 5. Write the file

Draft the file following this step, get approval as described at the end of it, and only then save it. Use this order. Omit a section that does not apply. If an omitted section is a token section, list it under `omitted` in the front matter with a reason.

```markdown
---
version: alpha
name: <name>
description: <one sentence>
colors: ...
typography: ...
rounded: ...
spacing: ...
components: ...
---

## Overview
## Colors
## Typography
## Layout
## Elevation & Depth
## Shapes
## Components
## Do's and Don'ts
## Page Composition
## Known Drift
```

The spec fixes the order of its own sections and accepts added ones, so Page Composition and Known Drift go last.

**Tokens**

- Tokens hold the normative values. Any value the prose presents as part of the system must match a token, unless the schema cannot hold it. Rejected values named in a rule, such as "do not write `text-[15px]`", and values listed under Known Drift are exempt.
- Resolve values to literals. The spec has no CSS variable type, so write `"#1F2937"` in the token and name the source variable in the prose.
- Keep the repository's token names where they exist. The exception is `primary`, which the spec expects, so map the main brand color to it and name the source variable. Name new tokens by role, using the spec's recommended names where one fits.
- Dimensions use `px`, `em`, or `rem` only. `fontWeight` is a number. References look like `{colors.primary}`.
- The spec has no theme concept. Add dark values as their own tokens (`surface-dark`), and explain the pairing under Colors.
- A value the schema cannot hold goes in prose: shadows, component borders, and padding with two values.

**Prose**

Every line must change what an agent would do. Delete any line that only describes the brand in general terms. Overview may state the brand's feel, as the spec asks, but tie each word to a visible effect: "Quiet: `accent` appears only on the main action and the focus ring."

- **Write rules a reviewer can check.** If a reviewer cannot mark the rule pass or fail from a screenshot or the code, rewrite it. Adjectives read differently to different models. Use short imperative sentences.

  | Bad | Good |
  | --- | --- |
  | Make the table feel less cramped. | Let evidence tables use the full available width. |
  | Keep the layout clean. | Each section has one focal object, and supporting items share its left edge. |
  | Use color sparingly. | Use `tertiary` only for the single primary action on a screen. |

- **Name real code under Components.** In from-repo mode, list each core component's path and variants, and the token or class names it should use. Describe the confirmed target, and record any difference from today's code under Known Drift.
- **Reject patterns by name under Do's and Don'ts.** Name each pattern and pair it with what to do instead: "No cards nested inside cards. Separate groups with spacing." Draw from the user's corrections and the "must not look like" answer. Add a common generated-design pattern only when it conflicts with this brand, because some brands use uppercase labels or gradients on purpose.

**Page Composition**

Start with a short priority order for when rules compete, with accuracy of content first. Then write one entry for each main page type:

- **Reader and job:** who opens it, and what they need to do or decide.
- **First viewport:** what leads, such as the recommendation, the working tool, or the key comparison.
- **Supporting content:** where it goes, and what stays available without dominating.

**Known Drift** (whenever UI code exists)

List each place the code differs from the file on the date you wrote it, with its location: values, components, and existing pages that break a new rule. Open it with: "The rules above are the target. New code follows them. Do not fix these places unless asked." Without this section, the file looks like an accurate description of the code, and agents stop on each difference. If the whole UI predates the file, as with a new visual identity, say that once and list the differences by area instead of by line.

**Get approval, then save**

The draft holds decisions the user has not seen. These are design decisions, per [tai-protocol](../tai-protocol/SKILL.md). Send one message showing:

- Every token value the user has not confirmed. A direction from step 2 sets only colors, type pairing, radius, and density, so the type scale, spacing, and component values are new.
- Every rule, priority, and composition entry you wrote beyond what the user stated.
- Each rule the user gave that existing code breaks, with a proposed scope for it.

Wait for approval, revise as asked, then save the file.

Then go to step 6.

Evidence: the file exists at the path stated in step 1, and the user approved its values and rules.

### 6. Validate the file

1. **Run the linter** through the project's package runner, such as `npx @google/design.md lint DESIGN.md`. Output is JSON, and it exits with `1` on errors. Run it as a one-off. Do not add it to the project's dependencies. Record the package version in the report.
2. **Resolve the findings.** Errors must reach zero. For each warning, fix it or state why it stands. `orphaned-tokens` flags colors no component references, which can be correct for a color used only in prose. A contrast finding follows item 3. If a fix would change a value the user approved, show it to them first. After any change, run items 1 to 4 again, so the report quotes the final file.
3. **Check contrast by computing it.** The linter tests only component background and text pairs. Compute every other text and surface pair you defined, in each theme. A token with no dark value counts on the dark surface if it can appear there. Normal text needs 4.5:1. Large text (24px, or 18.66px and bold) needs 3:1, and so do control boundaries a user needs to identify the control, focus indicators, and state indicators. Decorative dividers and card edges are exempt.

   If a pair fails, show it to the user with a proposed fix and finish the other checks in this step. Do not change a confirmed value on your own. Record every failure that remains under Colors with its ratio, according to what the user did:

   - **Accepted it:** record it as accepted. Do not ask again about a failure the user already accepted.
   - **Deferred it:** record it, and tell agents to ask before using that pair.
   - **Did not reply:** do the same, mark it "Unresolved", and report the file as incomplete.
4. **Check the prose against the tokens.** Each hex value presented as part of the system must match a token. Rejected values and Known Drift entries are exempt.
5. **Compare versions in revise and small-edit modes,** with `npx @google/design.md diff <old> DESIGN.md`, and confirm the token changes are the ones approved.
6. **Check that agents will load it.** A DESIGN.md does nothing if no agent reads it. See whether the project's agent instructions, such as `AGENTS.md` or `CLAUDE.md`, point to it. If not, suggest a one-line pointer. Do not add it without asking.

Evidence: the command, its JSON output, the reason for each warning left in place, the contrast results, and the load check.

### 7. Compare with and without the file

A clean lint result shows the file is well-formed. This step is the evidence that it changes what agents build.

Run it for a new file, a new section, or a new page type. Skip it for a small edit, a revision of existing tokens or rules, or a rule added to an existing section, and say so in the report.

1. **Freeze one test.** Pick a page type from Page Composition. If the file has none, report the step as not run. Write one prompt with concrete inputs. Use the same prompt, inputs, model, and viewport for both runs. Take the first attempt, with no re-rolls.
2. **Generate both pages from fresh agents.** Give one the file. Withhold it from the other, including by building outside any directory that contains it. Keep test pages outside the project.
3. **Write a short rubric before looking at the results:** the supplied facts survived, the reader's job is clear from the first viewport, and the corrections the user named are resolved.
4. **Show both blind.** Screenshot each page at a wide and a narrow viewport. Label them A and B in random order, and ask the user to score them against the rubric before you say which is which.
5. **Act on the result.** Turn each failure into a change through step 4, then run the same test once more. Report it as one comparison. It is too small a sample to quote a percentage improvement.

Evidence: the frozen prompt, the screenshot paths, the rubric, and the user's scores.

## Output contract

Produces a DESIGN.md that follows the spec and adds Page Composition and, when UI code exists, Known Drift, plus a report covering lint results or the hand check, contrast results, any delegated or deferred decisions, whether the app was viewed, whether agents will load the file, and the comparison result.

Does not change application code, or, unless the user asks, the project's agent instructions or tests. Does not add dependencies, generate a stylesheet, set up a recurring feedback process, save a DESIGN.md before the user has approved its values and rules, or commit. Committing is a separate step.

## Failure handling

- **The linter cannot run** (no package runner, no network, or the harness or project does not allow running it). Do not report the file as linted or validated. Check by hand that the front matter parses as YAML with hex values quoted, every color is valid CSS, every `{reference}` resolves, the sections follow the spec's order, no heading or top-level key is repeated, a `primary` color and typography tokens exist, dimensions use valid units, and `fontWeight` is a number. State in the report what you did not check. In step 6.3, also compute contrast for component pairs, since no linter covered them. Report: "Lint not run, checked by hand," and give the command for the user to run. Without the linter, also skip the diff in step 6.5 and list the token changes by hand.
- **The spec cannot be fetched.** Say so, and use the structure in step 5. Flag in the report that the file was not checked against the current spec.
- **The linter rejects something this skill prescribes,** such as an added section. Follow the linter, move the content where it passes, and report the conflict.
- **The user does not answer the interview.** You can choose the feel, references, and direction if they tell you to, and list those as delegated. You cannot choose what the product is, who uses it, or what the main page is for, so ask once more for those. If they still do not answer, stop. Write no file, and report what is missing. For unanswered constraints, such as font licensing, take the least restrictive reading and state it in the approval message.
- **The user rejects the draft or stops replying before approval, at any point from step 1 on.** Save nothing, leave any existing file unchanged, and report what was left open.
- **A linter error needs a decision the user does not give.** Keep the saved file, and report it as incomplete, with the error.
- **The code has no system to extract:** no declared tokens, and no value dominates most roles. Say so, and show the table anyway. The user can confirm values from the table or switch to from scratch with the table as input.
- **The user declines from scratch after a project with no UI, or declines every option.** Stop, write no file, and report what was checked.
- **The comparison cannot run as specified.**
  - No way to delegate, but a browser is available: you have already read the file, so any baseline you build is contaminated. Run it only if the user wants to, and label it contaminated. It cannot support a claim that the file improves output.
  - No browser, or the user declines: report the step as not run, and do not claim the file improves output. Give the user the frozen prompt, the rubric, and the instruction to run one agent without access to the file, so they can run it.
- **The file makes no visible difference, or does worse, in the comparison.** Report that plainly. Check that the agent loaded the file and that the rules are specific enough, then revise through step 4.

## Completion criteria

Either the skill stopped at a stated reason with no file written, or all of these hold:

- The DESIGN.md exists at the path stated to the user.
- Every token value traces to a choice the user made, approved, or explicitly delegated, and the report lists any delegated value.
- The linter reports zero errors with each warning explained, or the report states that it did not run and what was checked by hand.
- The report says whether the spec was fetched.
- Every rule can be marked pass or fail from a screenshot or the code, and the user approved those they did not state themselves.
- Contrast is computed for each theme, with failures fixed or recorded in the file. A failure marked "Unresolved" means the report calls the file incomplete.
- The comparison has user scores from an uncontaminated run, or the report says why not.
