---
name: tai-components
description: Conventions for structuring and writing React and Next.js components styled with Tailwind, covering file layout, one job per component, props API design, state ownership, server and client components, and styling with tokens. Apply whenever writing or substantially editing a component, small edits included. Not for other frameworks or for visual design rules, which belong to the better-* skills.
requires: [skills/coding-practices/SKILL.md]
---

# tai-components

## Entry conditions

Reach for this whenever writing a React component or changing its structure, props, or state, in a React or Next.js project. It applies to small edits too: adding one prop is a props API decision.

It builds on [coding-practices](../coding-practices/SKILL.md), which still applies in full. This skill covers what is specific to components.

It does not cover:

- How the UI looks or behaves for users (layout, type, color, accessibility rules). That is the `better-*` skills, for example [better-accessibility](../better-accessibility/SKILL.md).
- Writing stories. That is [storybook](../storybook/SKILL.md).
- The order of work for a whole feature. That is [tai-ui](../tai-ui/SKILL.md).
- Other frameworks (Svelte, Vue, Angular). Say the skill does not cover the stack and follow the project's own patterns.

**The project wins.** Where the project already has a clear convention for anything below, follow the project and do not migrate it unasked. These rules are the default for new projects and for gaps.

## Files and location

- Components live in `components/` under the app directory: `app/components/`, or `src/app/components/` if the project uses `src/`.
- `components/ui/` holds primitives only (`Button`, `Dialog`, `Input`). It is the default install location for UI library components (shadcn, coss, and similar), so keep it for those and for primitives written in the same role. Leave library-installed files where the library's tooling put them.
- Everything else goes in `components/<subcategory>/`, named for the feature or category it belongs to, and nested further when a category grows: `components/billing/InvoiceTable.tsx`, `components/billing/invoices/InvoiceRow.tsx`.
- Stories live in `stories/components/` at the project root, and tests in `tests/components/`. Both mirror the path under `components/`: `components/billing/InvoiceTable.tsx` has `stories/components/billing/InvoiceTable.stories.tsx` and `tests/components/billing/InvoiceTable.test.tsx`.
- One exported component per file, and the file is named after it in PascalCase (`InvoiceTable.tsx`). Library-installed files in `components/ui/` keep the library's naming. A small subcomponent used only by that file can stay in it, unexported.
- When a component moves or is deleted, move or delete its story and test with it.

## One job per component

- A component does one job, per [coding-practices](../coding-practices/SKILL.md)' one-thing rule. If describing what it renders needs "and," split it.
- Keep data apart from rendering. Presentational components get everything through props and do not fetch, read global stores, or know about routes. Fetching happens in a page, a route-level server component, or a hook, which passes data down.
- A component whose render is mostly branches for loading, empty, and error handles each with an early return, so the main render stays flat.

## Props API

- Type props as `<Component>Props` and export the type, so callers and stories can use it.
- **Variants are one string union prop, not several booleans.** Booleans are for true on and off states only (`disabled`, `loading`, `open`). This is coding-practices' no-flag-parameters rule, applied to props.
- **Compose instead of configuring.** Accept `children` or a named slot prop (a prop that takes a React node) instead of a growing list of props that only pass content through.
- **Wrap native elements transparently.** A component that renders one native element extends that element's props, spreads the rest onto it, merges `className`, and passes `ref` through. How `ref` is passed depends on the React version (a plain prop in recent versions, `forwardRef` in older ones), so check the installed version.
- Event props are named `onX` (`onSelect`, `onClose`). Handlers defined inside a component are named `handleX`.
- Give optional props their defaults in the parameter destructuring, not inside the body.

| Bad | Good |
|-----|------|
| `<Button primary small>` | `<Button variant="primary" size="sm">` |
| `<Card title="Plan" subtitle="Monthly" icon={<Star />} actionLabel="Upgrade" onAction={upgrade} />` | `<Card><CardHeader icon={<Star />}>Plan</CardHeader><Button onClick={upgrade}>Upgrade</Button></Card>` |
| `function Input({ label }: { label: string })` with no way to pass `name`, `type`, or `aria-*` | `function Input({ label, className, ...props }: InputProps)` where `InputProps` extends the native input props, and `props` is spread onto `<input>` |

## State

- Keep state in the lowest component that needs it. Lift it only when a sibling or parent needs it too.
- Do not store a value in state that can be computed from props or other state. Compute it during render.
- Do not use an effect to copy props into state or to react to a state change inside the component. Use effects only to sync with something outside React (a subscription, a timer, a browser API).
- Input-like components (anything with a value the user changes) support controlled use, `value` with `onChange`. Add uncontrolled use, `defaultValue`, when callers need it.
- Move logic that is more than a few lines out of the component body into a custom hook (`useX`) or a plain function, so the body reads as data in and markup out.

## Server and client components (Next.js)

- Components are server components by default.
- Add `"use client"` only to the smallest component that needs interactivity, state, effects, or a browser API, not to its parent or the whole page.
- Props that cross from a server component to a client component must be serializable. Pass data, not functions or class instances, except server actions.
- Code that must stay on the server (secrets, database access) never goes in a client component or a module a client component imports.

## Styling with Tailwind

- Style with utility classes in the component. No inline `style` objects, except for values computed at runtime (a progress width, a position).
- Use the project's tokens through the Tailwind theme. Do not use arbitrary values (`text-[13px]`, `bg-[#1a1a1a]`) when a theme value exists. If the needed value has no token, say so and ask whether to add one.
- Merge the caller's `className` last with the project's class-merge helper (usually named `cn`, built on `clsx` and `tailwind-merge`), so callers can override styles without conflicts.
- Map variants to classes in one place: the project's variant helper (such as `cva`) if it has one, otherwise a plain object keyed by variant. Do not add a styling dependency without asking.
- Use the semantic element for the job (`button` for actions, `a` for navigation, `ul` for lists) before adding roles or handlers to a `div`. The full rules are in [better-accessibility](../better-accessibility/SKILL.md).

## Output contract

Produces component code that follows these conventions, in the locations above or the project's own.

Does not migrate existing components to these conventions unasked, add dependencies (a variant helper, a class-merge library) without asking, add design tokens without asking, or commit.

## Failure handling

- **The project's convention conflicts with a rule here.** Follow the project. Name the difference in the report only if it caused a problem.
- **An existing component needs restructuring to take a change cleanly** (it fetches its own data, or it has grown boolean variant flags). Make the change in the existing shape, and suggest the restructure separately instead of doing it as part of an unrelated change.
- **A rule here and a `better-*` skill seem to conflict.** The `better-*` skill decides what the user sees. This skill decides how the code is organized to produce it.

## Completion criteria

Every component written or changed is in the right location with its story and test paths mirrored, does one job, keeps data fetching out of presentational components, uses string unions for variants and composition for content, passes `className`, `ref`, and native props through where it wraps a native element, keeps state low and not derived, marks only the smallest needed components as client components, and styles with theme tokens and a merged `className`, unless the project's own conventions say otherwise.
