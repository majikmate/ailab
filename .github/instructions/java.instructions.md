---
description: Test-driven development guidelines for Java with JUnit
applyTo: '**/*.java'
---

# Test-Driven Development for Java

## Core Principles

- **Red-Green-Refactor**: Always write a failing test first, make it pass with minimal code, then refactor.
- **Model-driven structure**: Treat PlantUML diagrams in `model/` as the source of truth. Every interface, class, attribute, and method must match the diagram exactly.
- **One class per file**: Each interface or class from the model gets its own file named `<ClassName>.java` in the appropriate package directory.

## File Organization

- **Runtime code**: `java/src/main/java/<package>/` directory contains all implementation files
- **Test files**: `java/src/test/java/<package>/` directory with `<ClassName>Test.java` naming pattern
- **Entry point**: `java/src/main/java/<package>/Main.java` orchestrates the application flow (no tests here)
- **Model source**: `model/` contains PlantUML diagrams embedded in markdown
- **Build configuration**: `java/build.gradle` for dependency management

## Module Conventions

- Use proper Java package structure matching your organization (for example `com.example.project`)
- Follow Java naming conventions: PascalCase for classes, camelCase for methods/variables
- Declare visibility explicitly: `private`, `public`, `protected`, or package-private
- Import classes explicitly; avoid wildcard imports

## Test-Driven Workflow

1. **Read the model**: Identify the class/interface, its attributes, methods, and relationships from PlantUML
2. **Write test first**: Create/update `java/src/test/java/<package>/<ClassName>Test.java` with a failing test
3. **Run tests**: Execute `gradle test` and verify failure
4. **Implement minimally**: Write just enough code in `java/src/main/java/<package>/<ClassName>.java` to pass the test
5. **Verify**: Rerun tests to confirm they pass
6. **Refactor**: Clean up code while keeping tests green

## Test Structure

- Use JUnit 5 (Jupiter) annotations: `@Test`, `@BeforeEach`, `@AfterEach`
- Name tests descriptively: `@Test void doorShouldBeLockedAfterLockIsCalled() { ... }`
- Use JUnit assertions: `assertEquals()`, `assertThrows()`, `assertTrue()`, `assertNotNull()`
- Organize tests with `@Nested` classes for grouping related scenarios
- Use `@DisplayName` for human-readable test descriptions

## Model Mapping

- **Visibility**: `-` prefix → `private`, `+` prefix → `public`, `#` → `protected`
- **Multiplicity**: `1..*`, `2..4` → use `List<>` or `Set<>` with validation in constructors/adders
- **Associations**: Implement with private collections and public getter/adder methods
- **Return defensive copies**: Use `Collections.unmodifiableList()` or return new `ArrayList<>(items)` to prevent external modification

## Error Handling

- Where the model is ambiguous, throw `IllegalArgumentException` or `IllegalStateException` with descriptive messages
- Document assumptions with Javadoc comments: `/** Assumes unique door names; throws IllegalArgumentException if duplicate */`
- Prefer fail-fast validation in constructors and methods

## Gradle Build Configuration

Create `java/build.gradle` with the following structure:

```gradle
plugins {
    id 'java'
    id 'application'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    // JUnit 5
    testImplementation platform('org.junit:junit-bom:5.10.0')
    testImplementation 'org.junit.jupiter:junit-jupiter'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

application {
    mainClass = 'com.example.Main'
}

test {
    useJUnitPlatform()
    testLogging {
        events "passed", "skipped", "failed"
        exceptionFormat "full"
    }
}
```

## Code Quality

- **Formatting**: Follow standard Java conventions (4-space indentation, braces on same line)
- **Linting**: Run `gradle checkstyleMain` or configure your IDE formatter
- **Deprecated APIs**: Avoid using `@Deprecated` methods and classes; use modern alternatives and address compiler warnings
- **Verification**: Always run `gradle test` before committing
- **Documentation**: Use Javadoc for public APIs and non-obvious business rules

## Running Commands

```bash
# Gradle commands (run from java/ directory)
cd java && gradle clean test                 # Run all tests
cd java && gradle test --tests DoorTest      # Run specific test class
cd java && gradle run                        # Run application
cd java && gradle build                      # Build JAR
cd java && gradle check                      # Run all checks
```

## Reporting

After each implementation cycle, summarize:
- Which test(s) were written
- What code was added/modified to pass them
- Any assumptions made or clarifications needed from the model
