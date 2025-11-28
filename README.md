# AI Lab - Requirements-Based AI Code Generation

An example workspace demonstrating requirements-based AI code generation using
UML models and user stories formulated in GitHub issues.

## Overview

This AI lab showcases how to leverage AI agents to automatically generate
production-ready code from high-level requirements. The approach combines:

- **UML models** (PlantUML diagrams) that define the domain structure and
  contracts
- **User stories** captured as GitHub issues with acceptance criteria
- **Test-driven development** (TDD) workflow enforcing quality through
  Red-Green-Refactor cycles
- **AI-assisted code generation** that transforms requirements into working
  implementations across multiple programming languages

This is a complete example workspace showing how AI can bridge the gap between
business requirements and tested, working code.

## How It Works

1. **Model the domain** - Create UML class diagrams in PlantUML defining
   interfaces, classes, attributes, methods, and relationships
2. **Write user stories** - Formulate requirements as GitHub issues with clear
   acceptance criteria
3. **Invoke AI generation** - Use `@Generate <language>` to automatically
   generate tests and implementation
4. **Review and iterate** - AI generates code following TDD principles, you
   review and refine

## Supported Languages

- **TypeScript** (Deno runtime) - See `.github/instructions/ts.instructions.md`
- **Java** (JUnit, Gradle) - See `.github/instructions/java.instructions.md`
- **Python** (pytest) - See `.github/instructions/python.instructions.md`
- **C#** (xUnit/NUnit, dotnet) - See
  `.github/instructions/csharp.instructions.md`

## Project Structure

```
ailab/
├── .github/
│   ├── agents/
│   │   └── Generator.agent.md       # TDD Generator agent (tdd-generator mode)
│   ├── instructions/
│   │   ├── ts.instructions.md        # TypeScript TDD guidelines
│   │   ├── java.instructions.md      # Java TDD guidelines
│   │   ├── python.instructions.md    # Python TDD guidelines
│   │   └── csharp.instructions.md    # C# TDD guidelines
│   └── prompts/
│       └── Generate.prompt.md        # @Generate command for AI code generation
├── model/
│   └── model.md                      # PlantUML domain models
├── typescript/                       # TypeScript implementation
│   ├── src/                          # Runtime code
│   └── tests/                        # Test files
├── java/                             # Java implementation
│   ├── src/main/java/                # Runtime code
│   ├── src/test/java/                # Test files
│   └── build.gradle                  # Gradle build configuration
├── python/                           # Python implementation
│   ├── src/                          # Runtime code
│   └── tests/                        # Test files
└── csharp/                           # C# implementation
    ├── src/                          # Runtime code
    └── tests/                        # Test files
```

## AI-Driven Workflow

### 1. Define Requirements

**Model the Domain** - Create PlantUML diagrams in `model/`:

- Define interfaces and classes
- Specify attributes and methods with visibility
- Document relationships and multiplicities
- Add constraints and business rules

**Formulate User Stories** - Create GitHub issues with:

- Label: `userstory`
- Clear acceptance criteria (Given-When-Then or checklist format)
- References to relevant model elements
- Business perspective (not implementation details)

### 2. Generate Code

**Invoke AI Code Generation**:

```
@Generate typescript
```

Or for a specific issue:

```
@Generate #1 java
```

**What the AI Does**:

1. Scans GitHub issues labeled `userstory`
2. Loads PlantUML model for structural contracts
3. Reads language-specific TDD instructions
4. For each acceptance criterion:
   - Writes failing tests first
   - Implements minimal code to pass tests
   - Verifies and refactors
5. Reports progress and highlights assumptions

### 3. Review and Iterate

- Review generated tests and implementation
- Run tests to verify functionality
- Refine model or user story if needed
- Iterate until all acceptance criteria are met

## Running Tests and Code

### TypeScript (Deno)

```bash
cd typescript
deno test --allow-read --allow-write --allow-net
deno run --allow-read --allow-write --allow-net src/main.ts
deno fmt
```

### Java (Gradle)

```bash
cd java
gradle clean test
gradle run
gradle build
```

### Python

```bash
cd python
pytest
python -m src.main
```

### C#

```bash
cd csharp
dotnet test
dotnet run --project src
```

## Example Model

The current model implements a **Security Service** that checks if lockable
items (doors, windows) in houses and cars are properly secured:

- `Lockable` interface defines lock/unlock contract
- `Door` and `Window` implement `Lockable`
- `House` and `Car` aggregate lockable items
- `SecurityService` validates security across all items

See `model/model.md` for the complete PlantUML diagram.

## AI Agent Configuration

This workspace includes a custom **TDD Generator** agent that runs in
`tdd-generator` mode:

**Capabilities**:

- Automatically fetches GitHub issues labeled `userstory`
- Parses PlantUML models for domain structure
- Applies language-specific TDD patterns from `.github/instructions/`
- Generates tests before implementation (Red-Green-Refactor)
- Continuously runs tests and reports progress
- Documents assumptions and ambiguities

**Usage**:

```
@Generate typescript          # Process all user story issues in TypeScript
@Generate #1 java             # Process specific issue in Java
@Generate python              # Process all user story issues in Python
```

The agent follows a strict TDD workflow defined in
`.github/agents/Generator.agent.md` and uses language-specific instructions to
ensure idiomatic, tested code.

## Key Benefits

- **Requirements Traceability** - Direct link from user story → tests →
  implementation
- **Quality Assurance** - TDD ensures all code is tested and meets acceptance
  criteria
- **Multi-Language Support** - Same workflow across TypeScript, Java, Python,
  and C#
- **AI Acceleration** - Automate the tedious parts while maintaining quality
  standards
- **Living Documentation** - UML models and tests serve as executable
  specifications

## Extending the Lab

**Add New Languages**:

1. Create `.github/instructions/<language>.instructions.md` following the
   established format
2. Define file structure, test framework, and TDD workflow
3. Update README with language-specific commands
4. Update `.gitignore` with language-specific patterns

**Add New Features**:

1. Update PlantUML model in `model/model.md`
2. Create GitHub issue with `userstory` label and acceptance criteria
3. Run `@Generate <language>` to implement

## Example

See GitHub issue [#1](https://github.com/majikmate/ailab/issues/1) for a
complete user story example with acceptance criteria, and `model/model.md` for
the corresponding UML model.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file
for details.
