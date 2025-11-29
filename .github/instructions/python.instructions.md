---
description: Test-driven development guidelines for Python with pytest
applyTo: '**/*.py'
---

# Test-Driven Development for Python

## Core Principles

- **Red-Green-Refactor**: Always write a failing test first, make it pass with minimal code, then refactor.
- **Model-driven structure**: Treat PlantUML diagrams in `model/` as the source of truth. Every interface, class, attribute, and method must match the diagram exactly.
- **One class per file**: Each class from the model gets its own file named `<class_name>.py` in the language-specific directory (use snake_case for filenames).

## File Organization

- **Runtime code**: `python/src/` directory contains all implementation files
- **Test files**: `python/tests/` directory with `test_<class_name>.py` naming pattern
- **Entry point**: `python/src/main.py` orchestrates the application flow (no tests here)
- **Model source**: `model/` contains PlantUML diagrams embedded in markdown
- **Dependencies**: `python/requirements.txt` or `python/pyproject.toml` for package management

## Module Conventions

- Use Python packages with `__init__.py` files where needed
- Follow PEP 8 naming: snake_case for functions/variables/modules, PascalCase for classes
- Use type hints for method signatures: `def lock(self) -> None:`
- Import modules explicitly; group standard library, third-party, and local imports

## Test-Driven Workflow

1. **Read the model**: Identify the class, its attributes, methods, and relationships from PlantUML
2. **Write test first**: Create/update `python/tests/test_<class_name>.py` with a failing test
3. **Run tests**: Execute `pytest` or `python -m pytest` and verify failure
4. **Implement minimally**: Write just enough code in `python/src/<class_name>.py` to pass the test
5. **Verify**: Rerun tests to confirm they pass
6. **Refactor**: Clean up code while keeping tests green

## Test Structure

- Use pytest with descriptive function names: `def test_door_should_be_locked_after_lock_is_called():`
- Use pytest fixtures for setup: `@pytest.fixture` for reusable test data
- Common assertions: `assert x == y`, `assert x is not None`, `with pytest.raises(ValueError):`
- Group related tests in the same file; use classes (`class TestDoor:`) for organization
- Use parametrize for data-driven tests: `@pytest.mark.parametrize("input,expected", [...])`

## Model Mapping

- **Visibility**: `-` prefix → `_private` (single underscore), `+` prefix → `public` (no underscore)
- **Multiplicity**: `1..*`, `2..4` → use `list` or `set` with validation in `__init__` or adder methods
- **Associations**: Implement with private lists and public getter/adder methods
- **Return defensive copies**: Return `list(self._items)` or use `tuple(self._items)` to prevent external modification

## Error Handling

- Where the model is ambiguous, raise `ValueError`, `TypeError`, or custom exceptions with descriptive messages
- Document assumptions with docstrings: `"""Assumes unique door names; raises ValueError if duplicate"""`
- Prefer fail-fast validation in `__init__` and methods

## Code Quality

- **Formatting**: Follow PEP 8 (4-space indentation, 79-character line limit)
- **Linting**: Run `black .` for formatting, `flake8` or `ruff` for linting
- **Type checking**: Run `mypy src/` for static type analysis
- **Deprecated APIs**: Avoid using deprecated functions and modules; check `DeprecationWarning` messages and use modern alternatives
- **Verification**: Always run `pytest` before committing
- **Documentation**: Use docstrings for classes and public methods

## Running Commands

```bash
# Run all tests (from python/ directory)
cd python && pytest

# Run specific test file
cd python && pytest tests/test_door.py

# Run with coverage
cd python && pytest --cov=src --cov-report=html

# Run the application
cd python && python src/main.py

# Format and lint code
cd python && black .
cd python && flake8 src/ tests/
cd python && mypy src/
```

## Reporting

After each implementation cycle, summarize:
- Which test(s) were written
- What code was added/modified to pass them
- Any assumptions made or clarifications needed from the model
