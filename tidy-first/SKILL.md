name: tidy-first
description: This skill should be used when the user asks to "refactor", "clean up code", "restructure", "rename", "extract method", "improve code quality", or when separating structural changes from behavioral changes. Enforces Tidy First approach to keep commits clean and focused.

# Tidy First

## The Rule

Never mix structural and behavioral changes in the same commit.

- **Structural (tidying)**: rearranging code without changing what it does
- **Behavioral (features/fixes)**: changing what the code does

Each commit is purely one or the other. Structural changes come first.

## Workflow

1. Look at the code where the change will go
2. If structure makes the change difficult, tidy first — commit the tidying
3. Then make the behavioral change — commit it separately
4. If structure is already clean, skip tidying and go straight to the behavioral change

## Tidyings Catalog

**Guard clauses** — Function wraps its body in a conditional. Invert and return early. Flag if a function already has too many guards.

**Dead code** — Unreachable branches, unused variables, commented-out blocks. Delete it.

**Asymmetric patterns** — Similar operations handled in different ways. Normalize to one pattern.

**Tangled declarations** — Variable declared far from first use, or declared and initialized separately. Move them together.

**Opaque expressions** — Complex condition or calculation inline. Extract to a named variable that communicates intent.

**Magic literals** — Meaningful number or string without context. Extract to a named constant. Skip when meaning is obvious.

**Long unbroken blocks** — Function body with no visual structure. Add blank lines between logical steps, or extract a cohesive chunk to a helper.

**Hidden inputs** — Function reads from globals, env vars, or broad config dicts. Make inputs explicit parameters.

**Redundant comments** — Comments restating what the code says. Delete them.

## Common Chaining Paths

- Guard clause -> explaining variable/helper for the condition
- Chunk statements -> extract helper
- Explaining variables/constants -> delete redundant comments
- Extract helper -> guard clause or explaining variables inside the helper

## Scoping Rules

- Only tidy code relevant to the current change. Do not tidy unrelated code "while you're here."
- A tidying takes minutes, not hours. If it's growing into a large effort, it's refactoring — file it separately.
- Stop tidying once the behavioral change becomes easy.

## When NOT to Tidy First

- Structure is already clean for the change at hand — just make the change.
- Fixing an urgent bug — fix first (minimal change), tidy after if needed.
- The code is unlikely to change again — leave it alone.
- The tidying doesn't connect to work in this PR — note it, don't do it.

## Validating a Tidying Commit

A pure tidying commit must not change behavior. Verify:

- Tests pass before and after with no modifications to tests
- No new branches, conditions, or return values
- No changes to function signatures visible to callers (unless that IS the tidying, e.g., renaming)
- Diff is explainable purely as "moved/renamed/extracted/deleted" with no logic changes
