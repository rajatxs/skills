---
name: code-quality
description: Improve or review production code for correctness, maintainability, consistency, and project-aligned best practices without expanding the requested scope.
version: 1.0.0
---

# Code Quality

Use this skill when writing, reviewing, or refactoring source code. Adapt its guidance to the repository, language, framework, and risk of the change.

## Workflow

1. Inspect the surrounding code, repository guidance, and existing scripts before making changes.
2. Identify the behavior and interfaces that must remain stable, including public APIs, persisted data, error contracts, and user-visible behavior.
3. Make the smallest coherent change that satisfies the request. Keep unrelated cleanup out of the diff.
4. Use the project’s established patterns, dependencies, formatter, linter, type checker, and test conventions.
5. Validate proportionally: run focused checks first, then broader checks when the change warrants them. Report checks that could not run and why.

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
