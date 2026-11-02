---
name: tai-ui
description: Workflow for implementing a UI requirement in an existing codebase, such as a new feature, screen, or flow, or a substantial change to existing UI, in any frontend stack. Work out what the UI needs, break it into levels from primitives up to the page, map those onto what already exists, build or extend the pieces from the smallest up, integrate them with real data and navigation, and verify the whole flow. Applies the better-* standards while writing and offers the user-invoked break, variant, and interface-review at checkpoints. Not for small tweaks to existing UI.
requires: [skills/verify/SKILL.md]
---

# tai-ui

## Entry conditions

Reach for this when a requirement needs UI work: a new feature, screen, or flow, or a substantial change to existing UI (new states, new structure, a redesign). It is the UI-specific playbook for a feature in [tai-mode](../tai-mode/SKILL.md).

It does not apply to:

- Small tweaks to existing UI: a color fix, a spacing change, a copy edit. Make the change, follow [storybook](../storybook/SKILL.md) if the component has stories, and verify per [verify](../verify/SKILL.md).
- Reviewing a change someone else made. That is [interface-review](../interface-review/SKILL.md), on the user's explicit ask.
- Explaining how an existing interface was built. That is [explain-interface](../explain-interface/SKILL.md), on the user's explicit ask.

## User-invoked skills

[break](../break/SKILL.md), [variant](../variant/SKILL.md), and [interface-review](../interface-review/SKILL.md) run only when the user explicitly asks for them. This skill never runs them on its own. At the checkpoints below, offer the skill in one sentence that says what it would do, then wait. Run it only on a yes. A no, or no answer, means continue without it and note in the report that it was offered and not run.

Offer each one at most once per piece of UI, so the user is not asked the same question twice.

## Steps

### 1. Understand the requirement

1. State in one or two sentences what the user of the app needs to do or see, where in the app it happens, and how they get there.
2. Work out what the UI needs to meet it:
   - **Screens and entry points.** Which pages, routes, dialogs, or panels, and what leads the user to them (a nav link, a button, a redirect).
   - **Data.** Where it comes from, its shape, and what is shown while it loads, when it is empty, and when it fails.
   - **Interactions.** What the user can do, and what changes as a result.
   - **States and constraints.** Permissions, validation, small screens, and anything else that changes what renders.
3. If the requirement leaves out something the build depends on, ask, per [tai-protocol](../tai-protocol/SKILL.md). Do not fill a gap in behavior or design with a guess.

Evidence: the restated requirement and the list of what the UI needs, with open questions, stated before reading deep into the code.

### 2. Map it onto the codebase

1. **Find where it goes.** Locate the route, page, or feature module it belongs in, and look at how neighboring features are built: folder layout, data fetching, state management, and error handling. Follow those patterns.
2. **Find what exists.** Search for components, primitives, design tokens, hooks, and data utilities that already do part of the job, and for a similar feature to use as a model.
3. **Break the UI into pieces from the top down, and give each one a level:**
   - **Page:** one route, filled with real content.
   - **Layout:** page structure shared by two or more pages, with slots and no real content. With one page, keep the structure in the page and skip this level.
   - **Section:** a distinct section of the interface, such as a site header or an invoice table.
   - **Composite:** a few primitives or native elements doing one job, such as a labeled form field.
   - **Primitive:** one wrapped native element or UI library component with no domain knowledge, such as a button or an input.

   Levels are a way to reason about the pieces. Apart from primitives, which live together in one place, they do not decide folders. In a React or Next.js project, [tai-components](../tai-components/SKILL.md) has the full rules. If a level is borderline, choose the lowest one that fits and move on.
4. **Look for reuse at the highest level that could fit,** then go down. Reusing a section saves more work than reusing the primitives inside it.
5. **Classify each piece:**
   - **Reuse:** an existing piece works as is.
   - **Extend:** an existing piece needs a new prop, variant, or state. List its other consumers, since the change reaches them too.
   - **New:** nothing suitable exists.
   - **Remove:** existing UI this replaces. Removing anything the requirement does not mention needs the user's confirmation.

Do not add a piece the requirement does not use. Move a piece somewhere shared when another feature needs it, not in advance.

Evidence: a short table of pieces with their level and classification, and for each extended piece, its other consumers.

### 3. Plan

- If there is more than one reasonable way to do it (where state lives, extending a shared component or adding a new one, how the data flows), surface a plan first, per [plan-first](../plan-first/SKILL.md).
- Order the work bottom-up by level: primitives, composites, sections, a layout if pages share structure, then the page.
- **Checkpoint: variant.** If the user has not settled how it should look and two or more directions are reasonable, offer [variant](../variant/SKILL.md) to build the options side by side. Do not pick a direction for them on a design question.

### 4. Build the pieces

1. **Choose where to build, and build in level order,** up to and including any layout. If [storybook](../storybook/SKILL.md) applies to the stack, build each new or extended piece there first, with its stories, and follow that skill's setup flow if Storybook is missing. Otherwise build in the running app.
2. **Structure the code.** In a React or Next.js project, write each piece per [tai-components](../tai-components/SKILL.md). Otherwise follow the project's own component patterns found in step 2.
3. **Apply the standards while writing.** Load the `better-*` skills for the parts the work touches and follow them as you write, not as a cleanup pass afterwards:
   - [better-accessibility](../better-accessibility/SKILL.md): always, for any new or changed UI.
   - [better-layout](../better-layout/SKILL.md): new structure, grouping, or reading order.
   - [better-typography](../better-typography/SKILL.md): new text styles, sizes, or truncation.
   - [better-colors](../better-colors/SKILL.md): new colors, tokens, or states that change color.
   - [better-ui](../better-ui/SKILL.md): surfaces, radii, icons, hit areas, and polish.
   - [better-writing](../better-writing/SKILL.md): any user-facing text.
4. **Use the project's tokens and primitives.** Do not hardcode a color, size, or spacing value the design system already defines. If a needed value does not exist, say so and ask whether to add a token.
5. **Keep extended pieces backward compatible.** An extended shared component keeps working for its existing consumers unless the plan says otherwise.

Evidence: the list of `better-*` skills loaded and why, and any token or primitive gap raised.

### 5. Integrate

1. **Build the page.** Compose the layout or sections with real content, and wire them into the app following the patterns found in step 2: route or page, data fetching, state, and event handling.
2. **Connect the entry points** so a user can reach the new UI the way the requirement says (a nav link, a button, a redirect).
3. **Handle every data state with real behavior:** loading, empty, error, and no permission, as listed in step 1. A happy path with placeholder handling for the rest is not integrated.
4. **Remove what it replaces,** as classified in step 2. Do not leave dead routes, components, or styles behind.

Evidence: the app runs and the new UI is reachable from its entry point.

### 6. Verify

1. **Drive the flow end to end** in the running app as the user would, starting from the entry point, per [verify](../verify/SKILL.md). Capture screenshots at a narrow and a wide viewport, and of each data state that can be produced (loading, empty, error).
2. **Verify the pieces in isolation** per [storybook](../storybook/SKILL.md) where it applies.
3. **Check the other consumers** of every extended piece. Open at least one place each is used and confirm it still renders and behaves as before.
4. **Fix each problem at the lowest level that owns it.** If real content shows a composite wraps badly, change the composite, not the page that uses it. Then re-check the page.
5. **Checkpoint: break.** If a piece renders content that varies in production (user text, lists, numbers, images, optional fields), offer [break](../break/SKILL.md) to stress-test it with worst-case content.

### 7. Final check and report

1. **Checkpoint: interface-review.** Before calling the work done, offer [interface-review](../interface-review/SKILL.md) on the change.
2. Report:
   - The requirement as understood, and anything still open.
   - The pieces table from step 2, updated with what actually happened.
   - The `better-*` skills applied.
   - The verification evidence: the flow driven, commands and output, screenshot paths, and what each screenshot shows.
   - Which checkpoint skills were offered, and whether each ran. For any that ran, the findings and what was fixed.

## Output contract

Produces the implemented UI change integrated into the app, stories for new and extended pieces where [storybook](../storybook/SKILL.md) applies, and a report with verification evidence.

Does not run break, variant, or interface-review without the user's explicit yes, invent behavior or a visual direction the requirement left open, add design tokens or remove UI outside the requirement without asking, or commit. Committing is a separate step.

## Failure handling

- **The requirement conflicts with how the app already works** (an existing flow, a permission model, a data contract). Stop and describe the conflict. Do not resolve it silently in either direction.
- **The data the UI needs does not exist yet** (no endpoint, no field). Say so. Build against a clearly marked fixture only if the user agrees, and list it as open in the report.
- **A `better-*` rule conflicts with the project's design system.** Follow the design system, and name the conflict in the report so the user can decide whether to change it.
- **A piece cannot be built in isolation.** Build it in the app, and say why in the report.
- **A page needs an override to look right.** Treat it as a defect in the lower piece and fix it there. If that change is out of scope, for example because it reaches consumers the requirement does not cover, list it as open in the report.
- **A checkpoint skill finds a problem.** Fix it, re-run the verification from step 6 for the affected part, and list the fix in the report.
- **Verification cannot run** (no browser, no way to start the app). Say which step could not run and why. Do not report the work as verified.

## Completion criteria

The requirement is met in the running app and reachable from its entry point, every data state behaves as specified, each piece has a level in the pieces table, existing pieces were reused or extended where they fit and their other consumers still work, problems found with real content were fixed at the lowest level that owns them or listed as open, the relevant `better-*` skills were applied while writing, the flow is verified end to end with screenshot evidence, each checkpoint skill that applied was offered, and the report lists all of it.
