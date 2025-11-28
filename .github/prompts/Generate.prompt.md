---
description: 'Generate code from GitHub user story using TDD workflow'
name: 'Generate'
argument-hint: 'language or #issue language (e.g., typescript or #1 typescript)'
agent: tdd-generator
---

# Generate Code from User Story

Implements GitHub user story issues using test-driven development.

## Usage

### Generate from all user stories
```
@Generate typescript
```

This will scan all open issues with the `userstory` label and implement them sequentially in TypeScript.

### Generate from specific issue
```
@Generate #1 typescript
```

This will implement only issue #1 in TypeScript.

## Parameters

1. **Issue selection** (optional) - Specific issue number like `#1`, or omit to process all `userstory` labeled issues
2. **Target language** (required) - One of: `typescript`, `java`, `python`, `csharp`

## What This Does

The TDD Generator agent will:
- Scan for issues labeled `userstory` (all open issues, or specific issue if provided)
- For each user story:
  - Fetch acceptance criteria from the issue
  - Load the PlantUML model from `model/`
  - Read language-specific instructions from `.github/instructions/<language>.instructions.md`
  - For each acceptance criterion:
    - Write failing tests
    - Implement minimal code to pass
    - Verify and refactor
  - Report progress and highlight any issues
  - Mark completed acceptance criteria in the issue

## Output Location

Code is generated in language-specific directories:
- TypeScript: `typescript/src/` and `typescript/tests/`
- Java: `java/src/main/java/` and `java/src/test/java/`
- Python: `python/src/` and `python/tests/`
- C#: `csharp/src/` and `csharp/tests/`