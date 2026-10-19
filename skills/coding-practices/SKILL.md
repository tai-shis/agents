---
name: coding-practices
description: Language-agnostic code design and structure conventions, function responsibility and naming, control flow shape, readability over cleverness, and when code should explain itself instead of needing a comment. Apply whenever writing or substantially editing code, in any language. Language-specific syntax, casing, and formatting are out of scope.
requires: []
---

# coding-practices

## Entry conditions

Reach for this whenever writing new code or substantially editing existing code, in any language. It governs structure and design decisions, not language syntax, casing, or formatting, those come from the language's own idioms, a linter or formatter, or a dedicated skill for that language or stack.

This is distinct from [tai-protocol](../tai-protocol/SKILL.md)'s Code comments section, which rules on what comments to write at edit time. This skill is about writing code that needs fewer comments in the first place.

## Function responsibility and naming

- A function does one thing. If describing it needs "and," it is two functions. This is judged at the function's own level: an orchestrator whose own body is a sequence of delegated calls is still doing one thing, coordinating, even though the steps it coordinates do several things between them. The rule is violated when a single function's own body, not a helper it calls out to, does more than one job.
- A parameter whose only job is selecting between different behaviors inside the function (a mode flag, a boolean that branches most of the body) is a sign the function has already stopped doing one thing. Once a function is growing flags to handle more cases, split the cases into separate, named functions instead of accumulating parameters on the one function.
- The name is the contract: it should describe everything the function visibly does, with nothing hidden. A reader should never be surprised by a side effect the name did not promise.
- Name a function for its purpose, what it is used for, not for the mechanism inside it. Exception: a genuinely complex function can lean on a more mechanism-revealing name when a purpose-only name would be too vague to be useful.
- This naming standard applies to variables and parameters too, not only functions: a name should describe what the value is or represents, specific enough that a reader does not have to trace its later usage to find out. Not knowing what a value represents or will be used for is a reason to find out before naming it, not a reason to reach for a generic placeholder (`data`, `temp`, `x`).
- Naming a function by purpose takes thought. Swapping in another verb that only narrates the body is the same mistake as describing the mechanism. Ask what the caller wants done and name that. If the only name you can reach restates the steps inside, you do not yet understand the function's purpose. Stop and work that out before naming it.
- Do not abbreviate, unless the abbreviation is established convention that a reader new to the codebase decodes instantly and that has exactly one plausible meaning in its scope. `err`, `req`, `res` in an HTTP handler, `id`, `url`, `db`, `env`, `args`, `ctx`, and `i` in a short, simple loop pass. An abbreviation invented locally or made by dropping letters (`usrMgr`, `cfgVal`, `btnLbl`) does not. An abbreviation with two readings (`res` as response or result) is spelled out when both readings are live in the surrounding code. Once loops walk named things, use the names instead of `i` and `j`.
- Booleans read as predicates (`is`, `has`, `should`, `can`). Functions start with a clear action (`get`, `list`, `create`, `update`, `remove`, `handle`). These are heuristics, not a fixed formula.
- Avoid these filler words in identifiers and comments: `resolve`, `narrow`, `canonical`, `invariant`, `seam`, and vague verbs like `process`, `manage`, `perform`, and `do`. They sound precise and say nothing specific. Use the concrete word for what is actually happening.
- If the same multi-step action is needed in more than one place (a repeated error-handling sequence, a repeated setup step), extract it into one named function and call it from each site, instead of duplicating the sequence everywhere it is needed.
- Extraction has limits. Do not extract a helper if the result makes the types or the control flow harder to follow. A helper with a single caller that adds only indirection gets inlined, unless the separation clearly improves readability.

### Orchestrator and helper delegation

A function that coordinates a multi-step procedure, several distinct actions in sequence (validate, then transform, then persist), should read as a short, flat sequence of calls to well-named helpers, not contain the step-by-step logic inline. Push the "how" down into small, single-purpose helpers; keep the "what" visible at the top level. A function already doing one thing, a single check or a single calculation, is not a multi-step procedure and does not need this treatment.

```
function get_absolute_path(input):
    if exists(input): return input
    if is_relative(input): return join_with_working_directory(input)
    return search_in_path_dirs(input)
```

Each branch delegates to one named helper. Reading the orchestrator alone tells you the whole algorithm; the mechanism lives in the helpers, each small enough to hold in your head at once.

## Splitting as scope grows

The same one-thing principle applies above the function level. Once a file or module has grown large enough that it is clearly doing more than one cohesive thing, or has simply grown too large to hold in mind at once, split it into multiple files along the natural divisions between its responsibilities. A section-label comment inside a file (`// Helpers`, `// State`) marks one of those divisions. Split the file there instead of labeling it. Group the result into a subdirectory once enough related files accumulate that a flat listing stops being easy to scan.

This is a judgment call on cohesion and size, not a fixed line count. Ask the same question asked of a function: does this unit still read as one thing, or has it quietly become several things sharing a file out of convenience?

## Structured data over ad-hoc shapes

When several related values travel together as a group, give that group a declared name and shape (a class, struct, interface, record, or whatever the language's equivalent is) instead of building it ad hoc as an anonymous bag of keys at the point of use. A named shape answers "what is this" at a glance; an anonymous one makes the reader reconstruct that answer from the literal keys used, every time it is read.

This does not mean every value needs a named type. A function returning a single unrelated value (a boolean, a number, a string) does not need a wrapper. It applies once a group of related values is being passed or stored together.

## Name constants, not magic numbers

A literal value with a special meaning, a retry count, a timeout, a status code, a size limit, gets a named constant instead of appearing inline at every site that uses it. The name carries the meaning a bare number can't; a reader shouldn't have to guess why `3` or `0.2` appears at that particular spot, or hunt down every occurrence to change it consistently.

This does not apply to a genuinely self-evident literal (`0` as an empty count, `1` as a single increment or decrement). It applies once a value represents a specific, meaningful threshold or setting, one that could plausibly change, or isn't obvious from context alone.

## Validate at the boundary, trust the interior

Check untrusted or external input where it enters the system (user input, a network or file call, an external API or LLM response) and let internal functions assume they're receiving what they expect, rather than re-validating the same thing everywhere it's passed around.

At a boundary where the incoming shape isn't guaranteed, an API request body, an LLM response, a parsed file, validate it against a declared schema (a library like Zod or Pydantic, or a format like JSON Schema, whatever the language's equivalent is) instead of trusting the shape informally and finding out it's wrong three functions later. This is the boundary case of [Structured data over ad-hoc shapes](#structured-data-over-ad-hoc-shapes): give incoming data a name and a checked shape right where it enters, not after it's already spread through the code.

## Testable boundaries

Keep business logic in pure functions with explicit inputs, and inject dependencies at I/O boundaries (the database, the network, the clock, the file system) instead of reaching for them inside the logic. Code shaped this way can be tested without setting up the outside world.

## Remove what no longer serves

When editing a flow, delete the dead branches, stale compatibility paths, redundant wrappers, and misleading names that no longer serve it. Leaving them behind makes the next reader work out which parts are still live.

## Control flow

Favor guard clauses, handle the exceptional or terminal case first and exit, over nesting the normal case inside an `if`/`else`. Keep the main path unindented.

Bad (the real logic is buried a level deep, and gets deeper with every added case):
```
function process(item):
    if item is not null:
        if item.is_valid():
            do_the_real_work(item)
```

Good (each exceptional case exits immediately; the real work sits flat and unindented):
```
function process(item):
    if item is null: return
    if not item.is_valid(): return
    do_the_real_work(item)
```

## Readability over cleverness

Prefer the version a tired reader can follow over the version that shows off a language feature. A trick that trades real readability for a marginal gain, fewer characters, a slightly faster path nobody asked for, is not worth it. "Clever" is fine when it is also the clearest option available. The test is readability, not whether a trick is possible.

## Code that explains itself

Write function and variable names, and the structure around them, so the code does not need a comment to explain what it does. Reaching for a comment to explain what a block does is more often a sign that the block needs a better name or should be extracted into its own function, than a sign a comment is needed.

Default to no comment. A comment earns its place only when one of these holds:

- The code is genuinely non-obvious and cannot be refactored to be obvious.
- The reason for the code cannot be inferred from the surrounding code or context: an external constraint, a subtle tradeoff, surprising behavior, a workaround for something outside this code, a rule the code silently depends on.

Never narrate what the code does, restate a name, or mark imports and exports. Doc tags that drive tooling (`@deprecated`, parameter docs on a public API) are fine where they have that effect. See [tai-protocol](../tai-protocol/SKILL.md)'s Code comments section for what that looks like at edit time.

## Out of scope

- Language-specific syntax, casing conventions, or idioms (naming case, bracket style, import order, and the like). Defer to the language's own conventions, a linter or formatter, or a dedicated skill for that language.
- Automatic formatting and lint setup or enforcement. That is a separate, mechanical concern from the design decisions above, see a dedicated skill for adding and configuring formatters and linters on a project.

## Completion criteria

Code written or changed under this skill has single-purpose functions named for what they do and nothing more, no flag parameters standing in for separate functions, files split along real divisions once they outgrow one cohesive thing, no section-label comments standing in for a split, no unneeded abbreviations or filler words in names, no single-use indirection or dead code left behind, business logic testable without the outside world, grouped data given a named shape instead of an ad hoc bag of keys, no unexplained magic numbers, boundary input checked against a declared schema before it spreads through the code, guard clauses instead of nesting for exceptional cases, and no comment doing a better name's job.
