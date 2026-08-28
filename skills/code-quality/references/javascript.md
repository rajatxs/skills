# JavaScript Code Quality Standards

Use this reference when creating, modifying, reviewing, or refactoring JavaScript. Apply the repository’s runtime, module system, formatter, linter, and naming conventions first; use these standards to resolve gaps, not to rewrite established local conventions.

## Agent guidance

- Inspect `package.json`, configuration files, nearby modules, and repository instructions before choosing syntax or tooling.
- Preserve public behavior, data shapes, error contracts, and side effects unless the request changes them.
- Prefer the smallest coherent change. Do not perform unrelated cleanup or introduce a style migration incidentally.
- Treat recommendations below as context-dependent. Choose readability and correctness over mechanical rule-following, and explain material trade-offs when reporting a review.

## Language Standards

- Use modern ECMAScript syntax supported by the project's target runtime.
- Prefer `const` by default.
- Use `let` only when reassignment is required.
- Never use `var`.
- Prefer strict equality with `===` and `!==`.
- Prefer template literals over string concatenation when interpolation is required.
- Prefer optional chaining (`?.`) and nullish coalescing (`??`) when they improve clarity.
- Avoid relying on implicit type coercion when explicit conversion is clearer.

## Variables and Naming

- Use descriptive and intention-revealing names.
- Use `camelCase` for variables and functions.
- Use `PascalCase` for classes and constructor-like functions.
- Use `UPPER_SNAKE_CASE` only for true constants with static values.
- Prefix boolean values with meaningful terms such as `is`, `has`, `can`, or `should`.
- Avoid unnecessary abbreviations.
- Avoid single-letter variable names except for obvious short-lived contexts such as indexes.

```js
const isAuthenticated = true;
const userCount = 10;

function calculateTotalPrice(items) {
    // ...
}
```

## Functions

- Keep functions small and focused on one responsibility.
- Prefer pure functions when practical.
- Prefer early returns over deeply nested conditionals.
- Avoid excessive positional parameters.
- Use an options object when a function requires several related arguments.
- Do not mutate function arguments unless mutation is intentional and documented.
- Use default parameters instead of manual fallback logic where appropriate.
- Prefer named functions when the function name improves debugging or readability.
- Prefer arrow functions for concise, single-line function expressions or callbacks; use function declarations for multi-line or more substantial function definitions.

```js
function getUserDisplayName(user) {
    if (!user) {
        return null;
    }

    return user.displayName ?? user.username;
}
```

For functions with several parameters:

```js
function createUser({ name, email, role, isActive = true }) {
    // ...
}
```

## Objects and Arrays

- Prefer object and array destructuring when it improves readability.
- Prefer object shorthand syntax.
- Prefer spread syntax for immutable updates when reasonable.
- Avoid unnecessary object or array cloning.
- Do not mutate shared objects unless mutation is part of the intended API.
- Prefer array methods such as `map`, `filter`, `find`, `some`, and `every` when they clearly express intent.
- Use traditional loops when they are simpler, more efficient, or require complex control flow.

```js
const { id, name } = user;

const updatedUser = {
    ...user,
    name: newName,
};
```

## Conditionals

- Keep conditional expressions easy to understand.
- Prefer guard clauses over excessive nesting.
- Avoid complex nested ternary expressions.
- Use ternary operators only for short, simple value selection.
- Extract complicated boolean expressions into descriptively named variables.
- Prefer `switch` only when it improves readability over repeated conditions.

Avoid:

```js
const status = loading ? 'loading' : error ? 'error' : ready ? 'ready' : 'idle';
```

Prefer:

```js
function getStatus({ loading, error, ready }) {
    if (loading) return 'loading';
    if (error) return 'error';
    if (ready) return 'ready';

    return 'idle';
}
```

## Equality and Type Conversion

- Use `===` and `!==` by default.
- Perform explicit conversions when changing types intentionally.
- Use `Number()`, `String()`, and `Boolean()` when they communicate intent clearly.
- Validate parsed numeric values when invalid input is possible.
- Avoid ambiguous truthiness checks when values such as `0`, `""`, or `false` are valid.

```js
const page = Number(input);

if (!Number.isFinite(page)) {
    throw new Error('Invalid page number');
}
```

## Null and Undefined

- Treat `null` and `undefined` deliberately.
- Prefer `??` instead of `||` when valid falsy values must be preserved.
- Do not use optional chaining to hide unexpected missing values.
- Validate required values at system boundaries.

```js
const retryCount = options.retryCount ?? 3;
```

## Error Handling

- Never silently ignore errors.
- Throw meaningful `Error` instances instead of strings.
- Add contextual information when rethrowing errors.
- Catch errors only when the current layer can recover, translate, log, or add useful context.
- Avoid broad `try...catch` blocks around unrelated operations.
- Do not expose sensitive implementation details in user-facing errors.
- Preserve the original error when supported by the runtime.

```js
try {
    await loadConfiguration();
} catch (error) {
    throw new Error('Failed to load application configuration', {
        cause: error,
    });
}
```

## Async Code

- Prefer `async`/`await` over long promise chains when it improves readability.
- Handle rejected promises at the boundary that can recover, report, or intentionally detach the work. Make intentional fire-and-forget behavior explicit.
- Do not use `await` inside loops when operations can safely run concurrently; retain sequential execution when ordering, rate limiting, bounded concurrency, or dependencies require it.
- Use `Promise.all()` for independent concurrent operations.
- Avoid wrapping an existing promise unnecessarily in `new Promise()`.

```js
const [user, permissions] = await Promise.all([getUser(userId), getPermissions(userId)]);
```

## Modules

- Prefer ES modules when supported by the project.
- Keep imports organized according to the project's formatter or linting conventions.
- Avoid circular dependencies.
- Export only what other modules need.
- Prefer named exports for reusable utilities unless the project consistently uses default exports.
- Keep module responsibilities focused.

```js
export function parseDate(value) {
    // ...
}
```

## Classes

- Prefer functions and plain objects when a class does not provide a meaningful benefit.
- Use classes when modeling stateful entities or behavior where encapsulation is useful.
- Keep constructors lightweight.
- Avoid deep inheritance hierarchies.
- Prefer composition over inheritance.
- Use private fields when internal state should not form part of the public API.

## Comments

- Write comments to explain intent, constraints, trade-offs, or non-obvious behavior.
- Do not comment obvious syntax.
- Keep comments synchronized with the implementation.
- Remove outdated comments and commented-out code.
- Use JSDoc when documenting a public API or when type information materially improves JavaScript tooling.

Avoid:

```js
// Increment count
count++;
```

Prefer:

```js
// Requests are retried once because the upstream service occasionally
// closes idle connections before responding.
const maxRetries = 1;
```

## Magic Values

- Avoid unexplained repeated literals.
- Extract meaningful values into descriptively named constants.
- Do not create constants for trivial values when doing so reduces readability.

```js
const MAX_LOGIN_ATTEMPTS = 5;
```

## Security

- Never embed passwords, API keys, tokens, or other secrets in source code.
- Treat external input as untrusted.
- Validate and sanitize input at system boundaries.
- Avoid dynamic code execution such as `eval()` and `new Function()`.
- Avoid constructing commands, queries, HTML, or URLs from untrusted input without appropriate escaping or parameterization.
- Do not expose sensitive data through logs or error messages.
- Prefer cryptographically secure APIs when randomness has security implications.

## Performance

- Prioritize readable code unless profiling identifies a real performance issue.
- Avoid repeated expensive operations inside loops.
- Avoid unnecessary allocations for performance-sensitive paths.
- Use appropriate data structures such as `Set` and `Map` when they better represent the operation.
- Do not introduce caching without a clear invalidation strategy.

## Dependencies

- Prefer built-in platform functionality when it provides a clear and maintainable solution.
- Do not add a dependency for trivial functionality.
- Reuse dependencies already present in the project where appropriate.
- Evaluate maintenance, security, bundle size, and runtime impact before adding a dependency.

## Dead Code

- Remove unused variables, imports, functions, and modules when they are truly unreachable or outside the intended public interface.
- Do not keep commented-out implementations in source files.
- Do not leave temporary debugging statements such as `console.log`; retain deliberate, appropriately scoped logging when the project requires it.
- Do not leave placeholder code unless explicitly required and clearly documented.

## Testing and Testability

- Structure code so important logic can be tested independently.
- Avoid unnecessary global state.
- Prefer dependency injection when external dependencies make testing difficult.
- Add or update tests when behavior changes, following the project’s test boundaries and fixture conventions.
- Test edge cases and failure paths in addition to successful paths.

## Formatting

- Follow the project's configured formatter and linter.
- Do not manually fight formatter output.
- Keep formatting consistent with surrounding code.
- Do not disable lint rules without a specific, documented reason.

## Avoid

Do not introduce the following without a strong technical justification:

- `var`
- `eval()`
- `new Function()`
- implicit globals
- deeply nested callbacks
- deeply nested conditionals
- complex nested ternaries
- unnecessary mutable state
- monkey patching
- prototype modification of built-in objects
- silent error swallowing
- duplicated logic
- premature abstractions
- premature optimization

## Review Checklist

Before completing JavaScript changes, verify that:

- The code follows existing project conventions.
- Variable and function names communicate intent.
- Functions have clear responsibilities.
- Error cases are handled appropriately.
- Async operations handle failures.
- External input is validated where necessary.
- No secrets or sensitive values are exposed.
- No unnecessary dependencies were introduced.
- Debugging and dead code have been removed.
- Relevant tests and static checks pass.
