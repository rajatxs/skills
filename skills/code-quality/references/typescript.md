# TypeScript Code Quality Standards

Use this reference when creating, modifying, reviewing, or refactoring TypeScript. Apply the repository’s TypeScript version, compiler options, formatter, linter, module system, and naming conventions first. This document supplements the JavaScript standards in [javascript.md](./javascript.md); TypeScript code should follow those runtime and design standards unless this reference is more specific.

## Agent guidance

- Inspect `tsconfig.json`, `package.json`, repository instructions, generated-code boundaries, and nearby modules before choosing a pattern.
- Treat types as part of the API. Preserve public signatures, serialized shapes, nullability, error contracts, and runtime behavior unless the request changes them.
- Make the smallest coherent change. Do not weaken compiler settings or add broad type assertions to make an error disappear.
- Keep compile-time guarantees aligned with runtime validation. TypeScript types do not validate network responses, user input, files, environment variables, or persisted data.
- Prefer a clear, precise type over a clever type. When a type becomes difficult to understand or maintain, simplify the design or extract a named type.

## Compiler and configuration

- Use the project’s configured TypeScript version and compiler options.
- Keep strict checking enabled when the project supports it, including strict null checks and no implicit `any`.
- Do not disable `strict`, `noImplicitAny`, `strictNullChecks`, or equivalent checks for a local fix.
- Avoid file-level escape hatches such as `// @ts-ignore`; use `// @ts-expect-error` only for a known, intentional error and explain why it is expected.
- Keep `noEmit` or build configuration consistent with the project’s type-checking and compilation pipeline.
- Run the repository’s type checker and relevant tests after changes. A successful type check does not replace runtime or integration tests.

## Types and naming

- Use descriptive names for types, interfaces, type parameters, and values.
- Use `PascalCase` for types, interfaces, classes, enums, and namespaces when the project uses them.
- Use `camelCase` for values, functions, and methods.
- Use `T`-style type parameters only for short, conventional abstractions; prefer descriptive names such as `TItem` or `TResponse` when multiple parameters are involved.
- Prefer type aliases for unions, intersections, mapped types, and function types.
- Use interfaces for extensible object contracts when declaration merging or `extends` is useful; do not choose interfaces and aliases mechanically.
- Avoid redundant names such as `IUser` unless the repository already uses that convention.

```ts
type RequestStatus = 'idle' | 'loading' | 'success' | 'error';

interface User {
    id: string;
    displayName: string;
    email?: string;
}
```

## Type safety

- Avoid `any`. Prefer a precise type, `unknown` for untrusted values, or a generic when the relationship between inputs and outputs matters.
- Narrow `unknown` with validation, type guards, or discriminated unions before using it.
- Do not use assertions (`as Type`) to bypass a type mismatch unless the runtime invariant is established and documented at that boundary.
- Prefer non-null checks and control-flow narrowing over non-null assertions (`!`).
- Model absence explicitly with `undefined` or `null` according to the project’s convention; do not conflate missing values with empty strings, zero, or false.
- Prefer literal unions or discriminated unions over loosely typed strings and boolean combinations when states are finite.
- Use exhaustive checks for discriminated unions where handling every variant is important.

```ts
type Result<T> = { status: 'success'; value: T } | { status: 'error'; error: Error };

function getValue<T>(result: Result<T>): T {
    if (result.status === 'success') {
        return result.value;
    }

    throw result.error;
}
```

## Functions and generics

- Annotate exported functions, public methods, callbacks at important boundaries, and complex internal functions where inference is not clear.
- Let TypeScript infer local variable types when the initializer makes the type obvious.
- Keep function parameters focused. Use an options object for several related or optional parameters.
- Use generics to preserve meaningful relationships between values; do not add generics merely to appear reusable.
- Constrain generics when operations require a capability or shape.
- Prefer overloads only when they provide a materially clearer API; use a union or generic when the implementation and call sites can express the relationship directly.
- Keep overload signatures consistent with the implementation and test each supported call shape.

```ts
function first<T>(items: readonly T[]): T | undefined {
    return items[0];
}

function createUser({
    name,
    email,
    isActive = true,
}: {
    name: string;
    email: string;
    isActive?: boolean;
}): User {
    return { id: crypto.randomUUID(), displayName: name, email };
}
```

## Objects, collections, and mutability

- Use `readonly` for values or properties that must not be reassigned through the current API.
- Accept `readonly` arrays or collections when a function does not mutate them.
- Prefer immutable updates when objects or arrays are shared across boundaries; mutate local state only when it is clear and safe.
- Use `Record<K, V>` only when arbitrary keys are valid. For fixed properties, define an object type.
- Use `Map` and `Set` when their semantics match the operation; do not replace object records solely for style.
- Avoid broad index signatures such as `[key: string]: unknown` when the valid keys are known.
- Use `satisfies` when a value should be checked against a type while retaining its inferred literal properties.

## Modules and public APIs

- Prefer the module system already used by the project and keep import boundaries consistent.
- Export only the types and values that form part of the intended public API.
- Use `import type` and `export type` when the project supports type-only imports and the distinction improves emitted code or clarity.
- Avoid circular dependencies, including cycles introduced through type-only imports when the toolchain treats them specially.
- Keep implementation details private. Do not export a type only to work around a local inference problem.
- Keep runtime validation at module or application boundaries separate from compile-time type declarations.

## Runtime boundaries and validation

- Treat API responses, parsed JSON, environment variables, storage, and user input as `unknown` until validated.
- Reuse the project’s established schema or validation library when one exists.
- Keep validation and the resulting type aligned; do not claim an unchecked value satisfies a domain type.
- Convert validated external data into the application’s internal shape at the boundary when the representations differ.
- Preserve useful error context and avoid exposing secrets or sensitive payloads in errors and logs.

## Enums, constants, and utility types

- Prefer literal unions for small finite sets when no runtime enum object is needed.
- Use enums only when the project benefits from their runtime representation or established convention.
- Avoid `const enum` when the build toolchain, library publishing, or isolated compilation makes inlining unsafe.
- Use built-in utility types such as `Pick`, `Omit`, `Partial`, `Required`, `Readonly`, and `Record` when they make the relationship clear.
- Do not stack utility types into opaque declarations; create a named domain type when the result is difficult to read.
- Avoid using `Partial<T>` for update payloads when some fields have different validation or mutability rules.

## Classes and decorators

- Prefer functions and plain objects unless a class provides meaningful encapsulated state, lifecycle, or polymorphism.
- Keep constructors lightweight and preserve class invariants after construction.
- Prefer composition over deep inheritance.
- Use `private` or ECMAScript `#private` fields according to project convention and runtime requirements.
- Use decorators only when supported and required by the project’s framework or compiler configuration; understand their runtime and metadata costs.

## Async code and errors

- Type asynchronous results explicitly at public boundaries when inference is not sufficient; use `Promise<T>` rather than an uninformative promise type.
- Handle rejected promises at the layer that can recover, report, or intentionally detach the work. Make fire-and-forget behavior explicit.
- Use `Promise.all` for independent operations and retain sequential execution when ordering, rate limiting, bounded concurrency, or dependencies require it.
- Prefer `Error` instances and typed error classification over strings or unchecked error assumptions.
- In `catch` blocks, treat the caught value as `unknown` and narrow it before reading custom properties.

```ts
try {
    await loadConfiguration();
} catch (error: unknown) {
    const message = error instanceof Error ? error.message : 'Unknown failure';
    throw new Error(`Failed to load configuration: ${message}`, {
        cause: error,
    });
}
```

## Comments and documentation

- Comment intent, constraints, invariants, or trade-offs—not syntax that the types and code already explain.
- Keep comments and types synchronized with implementation behavior.
- Use TSDoc for exported APIs when documentation is consumed by users, generated tooling, or an IDE.
- Document deliberate assertions, suppressions, unsafe casts, and compatibility workarounds at the point of use.

## Security and performance

- Never hard-code secrets, credentials, tokens, or environment-specific configuration.
- Validate and sanitize untrusted input before constructing commands, queries, HTML, URLs, or filesystem paths.
- Do not assume a TypeScript type provides runtime security or validation.
- Avoid dynamic code execution such as `eval` and `new Function`.
- Prefer readable code; optimize only for a demonstrated bottleneck and preserve type safety while doing so.
- Avoid unnecessary allocations and repeated expensive work in performance-sensitive paths.
- Do not introduce caching without a clear ownership and invalidation strategy.

## Review checklist

Before completing TypeScript work, verify that:

- The code follows repository conventions and the configured compiler options.
- Public APIs and serialized shapes have intentional, precise types.
- `any`, unsafe assertions, non-null assertions, and suppression comments are absent or justified at explicit boundaries.
- External data is validated at runtime before use.
- Unions are narrowed correctly and important variants are handled exhaustively.
- Async operations and caught errors are handled safely.
- Unnecessary exports, dependencies, mutable state, and dead code were avoided.
- Relevant formatting, lint, type-check, build, and test commands pass, or limitations are reported.
