---
name: storybook
description: Build and verify React and Next.js UI components in Storybook, isolated from the running app. Write one story per reachable state, keep the preview matching the app, cover behavior with interaction tests, and check appearance against visual regression baselines. If the project has no Storybook, suggest it, ask before setting it up, and persist the decision. Not for other frameworks or for pages that only work with live app data.
requires: [skills/verify/SKILL.md]
---

# storybook

## Entry conditions

Reach for this when creating or changing a React component that renders UI, in a plain React or Next.js project. It applies to small components too, a button variant counts.

It does not apply to:

- Other frameworks (Svelte, Vue, Angular). Say the skill does not cover the stack and verify the component in the running app instead, per [verify](../verify/SKILL.md).
- Full pages or route-level components wired to server data, auth, or routing. Verify those in the running app. Their presentational children still get stories.
- Stress testing a finished component with worst-case content inside the app. That is [break](../break/SKILL.md). Exploring design alternatives in place is [variant](../variant/SKILL.md).

## Use what the project already has

1. Look for an existing setup: a `.storybook/` directory, Storybook dependencies and scripts in the package manifest, existing `*.stories.*` files, and any recorded decision (see Persist the decision below). Note the installed Storybook version, the framework package (`@storybook/nextjs`, `@storybook/nextjs-vite`, or `@storybook/react-vite`), how interaction tests run, and which visual regression tool is configured, if any.
2. Follow the project's existing conventions (story location, title naming, decorators, fixtures) over the defaults in this skill.
3. Confirm the setup works: start or build Storybook and load one existing story. A dependency in the manifest does not prove Storybook runs.
4. Import paths and config options change between Storybook major versions. Check the installed version and use that version's docs. Do not write config or imports from memory.

## If Storybook is not set up

Check for a recorded decision for this repository first. If there is none, recommend Storybook, say what it adds (a `.storybook/` config directory, a few dev dependencies, and `storybook` and `build-storybook` scripts), and ask before installing anything.

- **Yes.** Run Storybook's official init command for the current version through the project's own package manager. It detects React or Next.js and picks the framework package. Delete the example stories the init generates. Then do Make the preview match the app below, and confirm by loading a story of a real component.
- **No.** Record the decline, verify components in the running app instead, and do not ask again in this repository.
- **No answer, or no way to ask.** Verify in the running app for now and say so in the handoff. Do not record silence as a decline, so the question comes up again once asking is possible.

Interaction testing and visual regression are separate decisions with the same flow. Someone can approve Storybook and decline visual regression. Ask about each one only when it is missing and the work needs it.

## Persist the decision

Write each decision (Storybook, interaction testing, visual regression tool), approved or declined, somewhere a later session will read: an existing memory mechanism, or a short note in the project's own agent instructions if nothing else exists. Scope it to the repository. Do not overwrite other recorded instructions. If nothing durable is writable, say so in the handoff instead of claiming the decision will carry forward.

## Make the preview match the app

A story that renders differently from the app gives false evidence. The preview must give components the same environment the app does.

- **Global styles.** Import the app's global stylesheet (the Tailwind entry, the CSS reset, `globals.css`) in the preview config.
- **Fonts.** Load the same fonts the app loads. A component rendered in a fallback font has different spacing and wrapping.
- **Providers.** Wrap stories in the providers the components need (theme, i18n, data client, store) with a global decorator. Give data clients mocked or fixture data. Stories never call a real network or backend.
- **Next.js.** Use the Next.js framework package. It handles `next/image`, `next/font`, and the router and navigation hooks. Set the route, params, or pathname for a story through the framework package's story parameters instead of mocking the Next.js modules by hand.
- **Server components.** An async server component, or one that imports server-only code, does not render in the browser. Write the story against the client or presentational component and pass data in as props. If a component fetches its own data and has no presentational part, say so and suggest splitting one out. Do not restructure the component without asking.

Evidence: open a story of an existing component and the same component in the running app. Fonts, colors, and spacing match.

## Write stories

- Put the story file next to the component (`Button.stories.tsx` beside `Button.tsx`), unless the project puts stories elsewhere.
- Use Component Story Format 3: a default export `meta` object with `component`, and one named export per story with `args`. Type them with `satisfies Meta<typeof Component>` and `StoryObj<typeof meta>`.
- Write one story per state the component can actually reach. Read the props, slots, and internal states to decide which ones: default, each variant, disabled, loading, empty, error, and long or missing content where the props allow it. Do not add a story for a state the props cannot produce.
- Name each story after its state, so the story list reads as a state inventory.
- Pass values through `args`, not hardcoded JSX, so controls work and stories stay small.
- Keep fixture data deterministic: fixed dates and fixed IDs. No `Math.random()`, `Date.now()`, or live data. Visual regression depends on it.

| Bad | Good |
|-----|------|
| `export const Story2 = { render: () => <Button disabled>Save</Button> }` | `export const Disabled: Story = { args: { disabled: true, children: "Save" } }` |
| A `Default` story only, for a button with five variants | One story per variant, plus `Disabled`, `Loading`, and `LongLabel` |
| Fixture `createdAt: new Date()` | Fixture `createdAt: new Date("2025-01-15T10:00:00Z")` |

## Interaction tests

Add a `play` function to stories of components with behavior: forms, menus, dialogs, toggles, anything that changes on input.

- Do what a user does and assert the visible result. Query by role, label, or text, not by class name or test ID, unless nothing accessible identifies the element.
- Pass callback props as spies (`fn()`) through `args` and assert the expected calls.
- Use the testing utilities the installed Storybook version exports. The package name changed between major versions, so check before importing.
- Run the tests from the command line with the runner the project uses (the Vitest addon in recent versions, the test runner in older ones). Watching the interactions panel is not a test run.

## Visual regression

Use the project's configured tool. If there is none, ask (see If Storybook is not set up). Recommend Playwright screenshot assertions against built Storybook stories as the default: local, free, and no account needed. Mention Chromatic as the hosted option, which needs an account and costs money above its free tier.

For the Playwright approach, build static Storybook, serve it, and screenshot each story at its isolated URL, `iframe.html?id=<story-id>&viewMode=story`. Read story IDs from the built Storybook's `index.json` instead of guessing them from titles.

Screenshots must be deterministic. Otherwise every run is noise.

- Disable animations and transitions, and wait for fonts to finish loading before capturing.
- Use a fixed viewport size per screenshot.
- Use the deterministic fixtures from Write stories.
- Generate baselines in the same environment that checks them. Font rendering differs between operating systems, so if CI checks the screenshots, generate baselines in the same container or image CI uses.

Baselines are committed to the repository.

- **New story.** Generate its baseline and look at the image before committing it. A baseline nobody looked at can lock in a broken render.
- **Diff on a run.** Open the diff image. If the change is intended and the diff shows only that change, update that story's baseline only. If the change is not intended, it is a regression, so fix the component.
- Never bulk-update baselines to make a failing run pass, and never raise the diff threshold globally to hide flakiness. Find the nondeterministic input and fix it.

## Verify

Per [verify](../verify/SKILL.md), report this evidence:

1. Static Storybook builds with no errors. Quote the command and the end of its output.
2. Each new or changed story was opened in a browser and screenshotted. List the screenshot paths, and say what each one shows.
3. Interaction tests pass from the command line. Quote the summary.
4. The visual regression run passes, or every diff is listed with a reason it is intended and its baseline was updated individually.
5. If the change affects how the component sits in a page (size, spacing, overflow), it was also checked in the running app. Storybook does not prove layout in context.

If a step cannot run (no browser available, no tooling approved), say which step and why. Do not report it as passed.

## Output contract

Produces stories, `play` functions, preview config, visual regression baselines, and, after approval, Storybook and test tooling setup.

Does not install or configure anything without approval, update a baseline nobody looked at, restructure a component to make it easier to story without asking, call real services from a story, or commit. Committing is a separate step.

## Failure handling

- **A story renders blank or throws.** Usually a missing provider, global style, or Next.js parameter. Fix it in the preview config or the story, not by changing the component.
- **Storybook does not start after setup.** Report the error. Do not silently remove addons or config until it starts.
- **Screenshots differ between identical runs.** Treat it as nondeterminism (animation, font loading, data, environment). Find and fix the source.
- **The component cannot render in isolation.** Say so, verify it in the running app, and suggest the presentational split described in Make the preview match the app.

## Completion criteria

Every new or changed component has stories for each state it can reach, static Storybook builds, interaction tests pass, visual regression passes or every diff is reviewed and justified, screenshot evidence is in the report, and any tooling decision made along the way is recorded where a later session will see it.
