---
description: Test-driven development guidelines for TypeScript with Deno runtime
applyTo: '**/*.ts'
---

# Test-Driven Development for TypeScript (Deno)

## Core Principles

- **Red-Green-Refactor**: Always write a failing test first, make it pass with minimal code, then refactor.
- **Model-driven structure**: Treat PlantUML diagrams in `model/` as the source of truth. Every interface, class, attribute, and method must match the diagram exactly.
- **One class per file**: Each interface or class from the model gets its own file named `<ClassName>.ts` in the language-specific directory.

## File Organization

- **Runtime code**: `typescript/src/` directory contains all implementation files
- **Test files**: `typescript/tests/` directory with `<ClassName>.test.ts` naming pattern
- **Entry point**: `typescript/src/main.ts` orchestrates the application flow (no tests here)
- **Model source**: `model/` contains PlantUML diagrams embedded in markdown

## Module Conventions

- Use Deno ES modules with explicit `.ts` extensions in imports
- Export every class/interface that needs to be consumed by other modules or tests
- Import from relative paths: `import { Door } from '../Door.ts';`

## Test-Driven Workflow

1. **Read the model**: Identify the class/interface, its attributes, methods, and relationships from PlantUML
2. **Write test first**: Create/update `typescript/tests/<ClassName>.test.ts` with a failing test
3. **Run tests**: Execute `deno test --allow-read --allow-write --allow-net` and verify failure
4. **Implement minimally**: Write just enough code in `typescript/src/<ClassName>.ts` to pass the test
5. **Verify**: Rerun tests to confirm they pass
6. **Refactor**: Clean up code while keeping tests green

## Test Structure

- Use `Deno.test()` with descriptive names: `Deno.test("Door should be locked after lock() is called", () => { ... })`
- Import assertions from `https://deno.land/std/testing/asserts.ts`
- Common assertions: `assertEquals()`, `assertThrows()`, `assert()`
- Group related tests in the same file but keep each test focused and independent

## Model Mapping

- **Visibility**: `-` prefix → `private`, `+` prefix → `public`
- **Multiplicity**: `1..*`, `2..4` → use arrays with validation in constructors/adders
- **Associations**: Implement with private arrays and public getter/adder methods
- **Return defensive copies**: Never expose internal arrays directly; return `[...this.items]`

## Error Handling

- Where the model is ambiguous, throw clear runtime errors with descriptive messages
- Document assumptions in comments: `// Assuming unique door names; throw if duplicate`
- Prefer fail-fast validation in constructors and methods

## Code Quality

- **Formatting**: Follow `deno.jsonc` settings (4-space indentation)
- **Linting**: Run `deno fmt` after changes
- **Deprecated APIs**: Avoid using deprecated Deno or TypeScript APIs; use `deno lint` to catch deprecated usage
- **Iteration**: Use `for...of` loops when iterating over model collections (arrays): `for (const item of items) { ... }`
- **Verification**: Always run `deno test --allow-read --allow-write --allow-net` before committing
- **Documentation**: Add brief comments for non-obvious business rules

## Running Commands

```bash
# Run all tests
cd typescript && deno test --allow-read --allow-write --allow-net

# Run specific test file
cd typescript && deno test --allow-read --allow-write --allow-net tests/Door.test.ts

# Run the application
cd typescript && deno run --allow-read --allow-write --allow-net src/main.ts

# Format code
deno fmt
```

## Reporting

After each implementation cycle, summarize:
- Which test(s) were written
- What code was added/modified to pass them
- Any assumptions made or clarifications needed from the model 