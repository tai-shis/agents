---
name: tai-setup
description: Set up a new repository, or explicitly establish this setup in an existing one, with a Nix flake dev shell and direnv, then the default web and backend stack where the project needs it, covering Next.js, shadcn or coss, Phosphor icons, Storybook, Vitest, Playwright, Zod, and Drizzle. Scope the project and ask about open choices first. Not for ordinary work in a repository that is already set up.
requires: [skills/verify/SKILL.md]
---

# tai-setup

## Entry conditions

Reach for this when bootstrapping a new project, or when explicitly asked to bring an existing repository onto this setup.

It does not apply to:

- A bug fix or feature in a repository that already has a flake. A flake existing is not a reason to run this skill.
- Adding one tool to a working project. Storybook alone is [storybook](../storybook/SKILL.md), and a formatter or linter alone is [lint-and-format](../lint-and-format/SKILL.md).

## Scope first

Read the request, the repository instructions, and any existing files. Check version control state before changing an existing directory. Then send one grouped list of what is still open, reusing anything already answered, and wait for answers. Each row states its default, so the person can accept it in one word.

| Decision | Default | Ask |
|----------|---------|-----|
| Name, location, and first runnable behavior | None | Always, when not stated |
| Which layers apply (see below) | Inferred from the deliverable | When the deliverable is unclear |
| Framework and package manager | Next.js and pnpm for web | Confirm in the list |
| UI library | None | Always for web. See UI library |
| Database | None | Always when the backend layer applies |
| Analytics | None installed | Always for web. See Analytics |
| Formatter and linter | ESLint from the Next.js template | Hand off to [lint-and-format](../lint-and-format/SKILL.md) |
| Remote host, visibility, and license | None | Only when part of the task. Never choose a license silently |

Agreeing to this list is the approval for everything it names, including Storybook and the test tools. Do not ask again per tool. Anything not on the list still needs its own question.

Record the agreed choices in the project's agent instructions, so [storybook](../storybook/SKILL.md) and [lint-and-format](../lint-and-format/SKILL.md) find a decision and do not ask again.

## Layers

| Layer | Applies when | Contains |
|-------|-------------|----------|
| Base | Always, in any language | Git, flake dev shell, direnv, ignore rules, README |
| Web | The deliverable has a React UI | Next.js, UI library, Phosphor, Storybook, Vitest, Playwright, analytics decision |
| Backend | The project has server logic or stored data | Zod, Drizzle, database decision |

Set up only the layers that apply. A Python CLI gets the base layer with its own toolchain and no web defaults. For another stack in the web layer, say the defaults do not cover it and ask.

In an existing repository, preserve history and user files, add only what is missing, and do not migrate an established tool (lucide to Phosphor, Jest to Vitest) unless asked.

## Base layer

1. Check that Nix with flakes, direnv, and nix-direnv are available. If one is missing, report it and ask. Do not install host tools or edit the machine's configuration without asking.
2. Run `git init` for a new repository. Use `main` as the branch.
3. Write `flake.nix` with a dev shell only: the toolchain, a formatter output, and nothing else. Do not add a Nix package, a `nix run` task suite, or a shell hook that starts servers, runs migrations, or installs dependencies. Project commands live in the package manifest. Add a package or apps only when asked. Replace the description and packages to fit the project:

   ```nix
   {
     description = "<project> development environment";

     inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";

     outputs =
       { nixpkgs, ... }:
       let
         systems = [
           "x86_64-linux"
           "aarch64-linux"
           "x86_64-darwin"
           "aarch64-darwin"
         ];
         forAllSystems = nixpkgs.lib.genAttrs systems;
       in
       {
         devShells = forAllSystems (
           system:
           let
             pkgs = import nixpkgs { inherit system; };
           in
           {
             default = pkgs.mkShell {
               packages = [
                 pkgs.nodejs_24
                 pkgs.pnpm
               ];
             };
           }
         );
         formatter = forAllSystems (system: nixpkgs.legacyPackages.${system}.nixfmt);
       };
   }
   ```

4. Write `.envrc` containing `use flake`, and commit it to version control with the rest of the setup. Ignore `.direnv/`, `result`, and `result-*`. Local secrets go in ignored files, with a value-free `.env.example`. Next.js's template ignores `.env*`, so add `!.env.example`.
5. Stage the new files with `git add`. Flakes only see files git tracks, so an unstaged `flake.nix` or source file looks missing. Do not use `--impure` to get around this.
6. Run `nix flake lock`, then `direnv allow`. Run every later command through the direnv environment, with `direnv exec . <command>` when the shell has not loaded it.

## Web layer

### Next.js

`create-next-app` writes its own files, so run it in a temporary subdirectory and move the result up. Append its `.gitignore` to the existing one. Do not overwrite it.

```sh
mkdir -p .scaffold-tmp
pnpm dlx create-next-app@latest .scaffold-tmp/app --ts --tailwind --eslint --app --src-dir --import-alias "@/*" --use-pnpm --turbopack --yes --skip-install --disable-git
```

Set the real name in the package manifest, then install. Add `NEXT_TELEMETRY_DISABLED = "1";` to the dev shell.

### UI library

Ask which to start from, using what the project needs.

- **shadcn.** The broad default, with more themes and blocks, and it converts its own icons to Phosphor.
- **coss.** Built on Base UI, denser, and opinionated toward production apps. It installs 50+ components and sets Inter and Geist Mono as the fonts.

They are not exclusive. coss installs through the shadcn CLI, so a project can add the `@coss` registry later.

**shadcn path:**

```sh
pnpm dlx shadcn@latest init --defaults --yes
```

Set `"iconLibrary": "phosphor"` in `components.json` before adding components. Then add what the first screen needs with `pnpm dlx shadcn@latest add <names> --yes`.

Check the font wiring in `globals.css`. Init can write `--font-sans: var(--font-sans);`, which refers to itself, so the app silently renders in the browser's serif default while every build and test passes. Point it at the variable the layout defines, such as `--font-sans: var(--font-geist-sans);`.

**coss path:**

```sh
pnpm dlx shadcn@latest init @coss/style --yes
```

Set `"iconLibrary": "phosphor"` in `components.json`. coss ignores it, so continue with the rewrite below.

### Phosphor

Icons are `@phosphor-icons/react`. Neither CLI finishes this on its own: shadcn writes Phosphor imports without installing the package and leaves `lucide-react` installed, and coss hardcodes `lucide-react` in its components.

```sh
pnpm add @phosphor-icons/react
pnpm remove lucide-react
```

**On the coss path**, rewrite the imports. Imports can span several lines, so search for the module name and not for a whole import statement. Rewrite each icon name to its Phosphor equivalent, then repeat until both checks in the last step of this section pass. A fixed rename list will miss names, so let the type checker find them.

| lucide | Phosphor |
|--------|----------|
| `ChevronDownIcon`, `ChevronUpIcon`, `ChevronLeftIcon` | `CaretDownIcon`, `CaretUpIcon`, `CaretLeftIcon` |
| `ChevronRight`, `ChevronRightIcon` | `CaretRightIcon` |
| `ChevronsUpDownIcon` | `CaretUpDownIcon` |
| `Loader2Icon`, `LoaderCircleIcon` | `CircleNotchIcon` |
| `MoreHorizontal`, `MoreHorizontalIcon` | `DotsThreeIcon` |
| `PanelLeftIcon` | `SidebarSimpleIcon` |
| `SearchIcon` | `MagnifyingGlassIcon` |
| `CircleAlertIcon`, `TriangleAlertIcon` | `WarningCircleIcon`, `WarningIcon` |
| `CircleCheckIcon` | `CheckCircleIcon` |

Adding a coss component later brings lucide back into that file. Repeat the rewrite after every `@coss` add, and say so in the project's agent instructions.

**Import path.** The root import fails `next build` with `createContext is not a function` in any file that renders on the server. This includes library files with no `"use client"` directive, such as coss's `spinner.tsx`.

| File | Import from |
|------|-------------|
| Starts with `"use client"` | `@phosphor-icons/react` |
| Anything else | `@phosphor-icons/react/ssr` |

**Check.** No `lucide` match remains in the source or the package manifest, and `pnpm exec next typegen && pnpm exec tsc --noEmit` passes. Run `typegen` first, because a fresh project otherwise fails on `LayoutProps`.

### Vitest and Playwright

Playwright's browsers come from Nix. A browser downloaded by `playwright install` does not run on NixOS. Add these to the dev shell, then reload it:

```nix
PLAYWRIGHT_BROWSERS_PATH = "${pkgs.playwright-driver.browsers.override {
  withFirefox = false;
  withWebkit = false;
}}";
PLAYWRIGHT_SKIP_VALIDATE_HOST_REQUIREMENTS = "true";
PLAYWRIGHT_DRIVER_VERSION = pkgs.playwright-driver.version;
```

Pin the npm package to exactly that version. A caret range or a different version fails with `Executable doesn't exist`, and the same failure appears after a `flake.lock` update moves the driver. Re-pin when it does.

```sh
pnpm add -D vitest
pnpm add -D -E "@playwright/test@$PLAYWRIGHT_DRIVER_VERSION"
```

Put unit tests in `tests/` and match `tests/**/*.test.{ts,tsx}` in the Vitest config. Put end-to-end specs in `e2e/`. Write one test of each kind against real behavior of the first screen, not a placeholder that asserts `true`.

Read the port from `PORT` in the Playwright config for `baseURL` and `webServer`. Do not hardcode a port. Another project's dev server may hold it, and Playwright then stops with `is already used`. If a port is taken, choose another one, and do not stop a process you did not start.

### Storybook

Follow [storybook](../storybook/SKILL.md) for stories, titles, and the preview. Setup needs these steps on top of it:

```sh
pnpm create storybook@latest --yes --features docs test --package-manager pnpm --disable-telemetry --no-dev
```

The browser install at the end of this command reports an error on NixOS, which is expected. Then:

1. Add `esbuild: true` under `allowBuilds` in `pnpm-workspace.yaml`. Storybook brings in esbuild, and until its build script is approved, pnpm commands including `pnpm install --frozen-lockfile` exit non-zero with `ERR_PNPM_IGNORED_BUILDS`, even when they changed the manifest. Decide any other package pnpm reports on its own, and do not approve everything at once.
2. Pin Playwright, because init adds it as `latest`: `pnpm add -D -E "playwright@$PLAYWRIGHT_DRIVER_VERSION"`.
3. Remove `@chromatic-com/storybook` from the manifest and from `.storybook/main.ts`, unless Chromatic was approved. Init adds it without asking.
4. Delete the generated example stories and `debug-storybook.log`.
5. Point the `stories` patterns in `.storybook/main.ts` at `../stories/**`, and import the app's global stylesheet in `.storybook/preview.tsx`.
6. Add `storybook-static` to the ignore file and to ESLint's `globalIgnores`. Otherwise lint scans the built output and reports hundreds of errors.

### Analytics

Install nothing by default. Ask during scoping, and suggest PostHog for a product app, the host's built-in analytics for a site, and nothing for a prototype. If one is chosen, read its key from the environment and leave the key out of the repository. Creating an account is the person's step.

## Backend layer

Zod and Drizzle are the defaults. Ask which database to use. For Postgres:

```sh
pnpm add zod drizzle-orm postgres
pnpm add -D drizzle-kit
```

For another database, use the driver and `dialect` from Drizzle's current documentation.

- Write `drizzle.config.ts`, a first schema in `src/db/schema.ts`, and generate a migration with `pnpm exec drizzle-kit generate`. Generating does not need a running database. Only a first table the project actually needs belongs in the schema, so ask if none is known.
- Parse external input with Zod where it enters the system, including `process.env` in one module such as `src/lib/env.ts`. Check the installed Zod major version before writing schemas, because the API differs between majors.
- Add the database client to the dev shell if the project needs it. Do not start a database or run migrations from a shell hook. Do not point a migration at a database you did not create without asking.

## Verify

Per [verify](../verify/SKILL.md), run each applicable command through the direnv environment, from a clean state, and report its result. A parsed flake is not a working environment.

| Layer | Command | Evidence |
|-------|---------|----------|
| Base | `nix flake check` | Passes, and the output notes which systems it skipped |
| Base | `direnv exec . sh -c 'command -v node pnpm'` | Paths are in the Nix store |
| Base | `git ls-files` and `git status --ignored` | `.envrc`, `flake.nix`, and `flake.lock` tracked. `.direnv/` and build output ignored |
| Web | `pnpm install --frozen-lockfile` | Exits 0 |
| Web | `pnpm lint` | Exits 0 |
| Web | `pnpm exec next typegen && pnpm exec tsc --noEmit` | Exits 0 |
| Web | `pnpm exec vitest run` | Unit tests and the Storybook project both pass |
| Web | `PORT=<free port> pnpm exec playwright test` | Passes against the running app |
| Web | `pnpm build` and `pnpm build-storybook` | Both exit 0 |
| Web | Open the running app in a browser and read the body's computed `font-family` | Screenshot of the first screen with a Phosphor icon showing, and the family is the project's font, not a browser default such as Times New Roman |
| Backend | `pnpm exec drizzle-kit generate` | A migration file exists. Say that it was not applied unless it was |

Other systems in the flake's list are unverified until run on them. Report them as untested.

Stop any process you started. Then write the README: prerequisites, `direnv allow`, the commands above, and the recorded choices.

## Output contract

Produces a repository that can be developed and checked: the flake, `.envrc`, ignore rules, a project skeleton, dependency manifests and lockfiles, working tests, recorded decisions, and a README.

Does not commit, create a hosted repository, push, deploy, choose a license, create third-party accounts, apply a migration to an existing database, change the machine's configuration, or put a secret in the flake, the repository, or the Nix store.

## Failure handling

- **A prerequisite is missing or a step is blocked.** Keep the files, and report the exact command, its output, and the next action. Mark that part incomplete, even if the rest works.
- **A CLI behaves differently from this skill.** These tools change often. Trust the installed version's output and documentation, then adapt the step. Report the difference so the skill can be updated.
- **`nix develop` works but direnv does not.** Direnv setup is incomplete. Report the missing hook or nix-direnv integration. Make sure a success did not come from a cached shell after an evaluation failure.
- **A check fails only because of generated files.** Exclude the generated path. Do not disable the rule or the check for the whole project.

## Completion criteria

Every agreed layer is in place, each applicable row in Verify has its evidence in the report, the choices are recorded in the project's agent instructions, processes you started are stopped, and anything unverified or still undecided is named in the handoff.
