# Code Quality

Code Quality is a Codex agent skill for writing, reviewing, and refactoring production source code and structured configuration. It improves correctness, maintainability, consistency, and project-aligned best practices while keeping changes focused on the requested scope.

## What it does

- Inspects repository guidance, surrounding code, and available development scripts before editing.
- Preserves stable behavior and interfaces, including APIs, persisted data, error contracts, configuration, and user-visible behavior.
- Favors clear, focused code, simple control flow, descriptive names, explicit error handling, and existing project patterns.
- Avoids unnecessary dependencies, speculative abstractions, hard-coded secrets, and unrelated cleanup.
- Preserves backward compatibility unless a breaking change is explicitly requested.
- Selects and applies the matching language or format reference before implementation or review.
- Applies all relevant references when a change spans multiple formats, such as TypeScript, JSX, and CSS.

## Reference documents

Use the reference that matches the files being changed or reviewed:

- [JavaScript standards](./references/javascript.md)
- [TypeScript standards](./references/typescript.md)
- [HTML and CSS standards](./references/html-css.md)
- [JSON and YAML standards](./references/json-yaml.md)

The references cover language- and format-specific practices, including accessibility, schema and contract compatibility, configuration safety, security, formatting, and validation. Repository conventions and explicit user requirements take precedence when they conflict with a reference, while preserving its safety and compatibility intent.

## Review focus

Code reviews prioritize correctness and behavioral regressions over style. Depending on the change, the skill checks boundary conditions, nullability, input validation, authorization, resource cleanup, concurrency, performance-sensitive paths, and failure handling. Findings distinguish definite defects from optional suggestions and include file and line references when possible.

## Validation

Validation is proportional to the change: focused parser, schema, formatter, lint, type, build, or test checks run first, followed by broader checks when appropriate. Any check that cannot run is reported with the reason. Static validation is distinguished from browser, runtime, deployment, infrastructure, and assistive-technology validation.

See the complete agent instructions in [SKILL.md](./SKILL.md).
