# Code Quality

Code Quality is a Codex agent skill for writing, reviewing, and refactoring production source code. It improves correctness, maintainability, consistency, and project-aligned best practices while keeping changes focused on the requested scope.

## What it does

- Inspects repository guidance, surrounding code, and available development scripts before editing.
- Preserves stable behavior and interfaces, including APIs, persisted data, error contracts, configuration, and user-visible behavior.
- Favors clear, focused code, simple control flow, descriptive names, explicit error handling, and existing project patterns.
- Avoids unnecessary dependencies, speculative abstractions, hard-coded secrets, and unrelated cleanup.
- Preserves backward compatibility unless a breaking change is explicitly requested.

## Review focus

Code reviews prioritize correctness and behavioral regressions over style. Depending on the change, the skill checks boundary conditions, nullability, input validation, authorization, resource cleanup, concurrency, performance-sensitive paths, and failure handling. Findings distinguish definite defects from optional suggestions and include file and line references when possible.

## Validation

Validation is proportional to the change: focused checks run first, followed by broader format, lint, type, build, or test checks when appropriate. Any check that cannot run is reported with the reason.

See the complete agent instructions in [SKILL.md](./SKILL.md).
