---
name: code-quality
description: Improve or review production code for correctness, maintainability, consistency, and project-aligned best practices without expanding the requested scope.
version: 1.0.0
---

# Code Quality

Use this skill when writing, reviewing, or refactoring source code or structured configuration. Adapt its guidance to the repository, language, framework, and risk of the change. Apply the relevant reference document in addition to the general guidance below.

## Workflow

1. Inspect the surrounding code, repository guidance, and existing scripts before making changes.
2. Identify the applicable language or format and read its reference document before implementation or review.
3. Identify the behavior and interfaces that must remain stable, including public APIs, persisted data, error contracts, serialized shapes, configuration, and user-visible behavior.
4. Make the smallest coherent change that satisfies the request. Keep unrelated cleanup out of the diff.
5. Use the project’s established patterns, dependencies, formatter, linter, type checker, schema validator, and test conventions.
6. Validate proportionally: run focused checks first, then broader checks when the change warrants them. Report checks that could not run and why.

## Implementation standards

- Prefer clear, focused functions and modules with simple control flow and early returns.
- Use names that communicate intent; avoid unexplained abbreviations, magic values, and speculative abstractions.
- Remove unused code only when it is part of the requested change or required to keep the affected path correct.
- Handle expected failures explicitly and preserve useful error context. Never silently swallow errors.
- Preserve backward compatibility unless a breaking change is explicitly requested. Consider callers, serialization, persistence, and configuration boundaries.
- Never hard-code secrets, credentials, tokens, or environment-specific configuration.
- Add comments only for non-obvious reasons, constraints, invariants, or tradeoffs; do not narrate self-explanatory code.
- Prefer existing dependencies and patterns. Add a dependency only when its benefit justifies its maintenance and security cost.

## Review priorities

When reviewing, prioritize correctness and behavioral regressions over style. Check boundary conditions, nullability, input validation, authorization, resource cleanup, concurrency, performance-sensitive paths, and failure handling when relevant to the change. Distinguish definite defects from suggestions, and include file and line references when reporting findings.

When requirements or conventions conflict, preserve the explicit user request and document the remaining tradeoff rather than silently choosing a broader behavior.

## Reference selection

Read and apply the matching reference document for the files being changed or reviewed:

- JavaScript: [references/javascript.md](./references/javascript.md)
- TypeScript: [references/typescript.md](./references/typescript.md)
- HTML and CSS: [references/html-css.md](./references/html-css.md)
- JSON and YAML: [references/json-yaml.md](./references/json-yaml.md)

When a change spans multiple formats, apply every relevant reference. For example, a TypeScript component with JSX and CSS should use the TypeScript and HTML/CSS references; a JavaScript module that reads YAML should use the JavaScript and JSON/YAML references.

The repository’s local conventions and explicit user requirements take precedence when they conflict with a reference. Preserve the reference’s safety, compatibility, accessibility, and validation intent, and document a material trade-off rather than silently weakening it.
