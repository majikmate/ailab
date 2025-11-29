---
description: Test-driven development guidelines for C# with xUnit or NUnit
applyTo: '**/*.cs'
---

# Test-Driven Development for C#

## Core Principles

- **Red-Green-Refactor**: Always write a failing test first, make it pass with minimal code, then refactor.
- **Model-driven structure**: Treat PlantUML diagrams in `model/` as the source of truth. Every interface, class, attribute, and method must match the diagram exactly.
- **One class per file**: Each interface or class from the model gets its own file named `<ClassName>.cs` in the appropriate namespace directory.

## File Organization

- **Runtime code**: `csharp/src/<ProjectName>/` directory contains all implementation files
- **Test files**: `csharp/tests/<ProjectName>.Tests/` directory with `<ClassName>Tests.cs` naming pattern
- **Entry point**: `csharp/src/<ProjectName>/Program.cs` orchestrates the application flow (no tests here)
- **Model source**: `model/` contains PlantUML diagrams embedded in markdown
- **Project files**: `.csproj` files for dependency and build management

## Module Conventions

- Use proper namespace structure matching your project: `namespace ProjectName.Domain`
- Follow C# naming conventions: PascalCase for classes/methods/properties, camelCase for private fields with `_` prefix
- Declare visibility explicitly: `private`, `public`, `protected`, `internal`
- Use properties instead of public fields: `public string Name { get; private set; }`

## Test-Driven Workflow

1. **Read the model**: Identify the class/interface, its attributes, methods, and relationships from PlantUML
2. **Write test first**: Create/update `csharp/tests/<ProjectName>.Tests/<ClassName>Tests.cs` with a failing test
3. **Run tests**: Execute `dotnet test` and verify failure
4. **Implement minimally**: Write just enough code in `csharp/src/<ProjectName>/<ClassName>.cs` to pass the test
5. **Verify**: Rerun tests to confirm they pass
6. **Refactor**: Clean up code while keeping tests green

## Test Structure

### Using xUnit
- Use `[Fact]` for single test cases, `[Theory]` with `[InlineData]` for parameterized tests
- Name tests descriptively: `public void Door_ShouldBeLocked_AfterLockIsCalled() { ... }`
- Use xUnit assertions: `Assert.Equal()`, `Assert.Throws<T>()`, `Assert.True()`, `Assert.NotNull()`
- Group tests in classes; use nested classes with `public class` for related scenarios

### Using NUnit (alternative)
- Use `[Test]` attribute for test methods
- Use `[TestCase]` for parameterized tests
- Use NUnit assertions: `Assert.That()`, `Assert.Throws<T>()`, `Assert.IsTrue()`

## Model Mapping

- **Visibility**: `-` prefix → `private`, `+` prefix → `public`, `#` → `protected`
- **Multiplicity**: `1..*`, `2..4` → use `List<T>`, `IList<T>`, or `IReadOnlyList<T>` with validation in constructors/adders
- **Associations**: Implement with private collections and public getter/adder methods
- **Return defensive copies**: Return `_items.ToList()` or `_items.AsReadOnly()` to prevent external modification

## Error Handling

- Where the model is ambiguous, throw `ArgumentException`, `InvalidOperationException`, or custom exceptions with descriptive messages
- Document assumptions with XML documentation comments: `/// <summary>Assumes unique door names; throws ArgumentException if duplicate</summary>`
- Prefer fail-fast validation in constructors and methods using guard clauses

## Code Quality

- **Formatting**: Follow C# conventions (4-space indentation, Allman brace style or K&R)
- **Linting**: Configure `.editorconfig` for consistent style; use analyzers
- **Deprecated APIs**: Avoid using deprecated or obsolete APIs; use modern alternatives and check compiler warnings
- **Verification**: Always run `dotnet test` before committing
- **Documentation**: Use XML documentation (`///`) for public APIs and non-obvious business rules

## Running Commands

```bash
# Build and run tests (from csharp/ directory)
cd csharp && dotnet build
cd csharp && dotnet test

# Run tests with coverage
cd csharp && dotnet test /p:CollectCoverage=true /p:CoverageReportsFormat=opencover

# Run specific test class
cd csharp && dotnet test --filter "FullyQualifiedName~DoorTests"

# Run the application
cd csharp && dotnet run --project src/<ProjectName>/<ProjectName>.csproj

# Format code
cd csharp && dotnet format
```

## Reporting

After each implementation cycle, summarize:
- Which test(s) were written
- What code was added/modified to pass them
- Any assumptions made or clarifications needed from the model
