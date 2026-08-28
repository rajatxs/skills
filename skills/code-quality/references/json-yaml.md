# JSON and YAML Code Quality Standards

Use this reference when creating, modifying, reviewing, or refactoring JSON or YAML. Apply the repository's parser, schema, formatter, linter, version, and configuration conventions first; use these standards to resolve gaps, not to rewrite established local conventions.

## Agent guidance

- Inspect repository instructions, nearby configuration files, schemas, examples, parser versions, and validation scripts before editing.
- Identify whether the file is an API contract, persisted data, deployment configuration, generated output, fixture, or documentation example. Apply the stricter compatibility requirements appropriate to that role.
- Preserve keys, value types, defaults, ordering where meaningful, comments where supported, anchors, and unknown-field behavior unless the request changes them.
- Make the smallest coherent change. Do not reformat an entire document or reorder unrelated keys without a repository-supported reason.
- Never infer a new secret, endpoint, credential, permission, environment value, or production setting. Use the repository's established placeholder or secret-reference convention.
- Validate after editing with the project's parser, schema validator, formatter, linter, and relevant tests. Syntax validity alone does not establish that the configuration is safe or operational.

## Shared data-model standards

- Use descriptive, stable keys that match the owning schema or public contract exactly.
- Keep one representation for each concept. Do not alternate between `user_id`, `userId`, and similar spellings within the same contract unless an external API requires it.
- Preserve the distinction between a missing key, `null`, an empty string, an empty array, `false`, and numeric zero.
- Use the narrowest appropriate value type. Do not encode booleans, numbers, or arrays as strings merely because a consumer can parse them.
- Keep units explicit in key names or schema documentation, such as `timeoutSeconds` or `max_retries`; do not rely on an undocumented assumption.
- Use ISO 8601 or the repository's documented format for dates and times. State whether timestamps are UTC, include an offset, or are local time.
- Keep identifiers opaque and stable. Do not derive identifiers from mutable labels unless that is part of the contract.
- Avoid duplicate keys. Different parsers handle duplicates inconsistently, and a later value can silently override an earlier value.
- Keep sensitive values out of examples, fixtures, logs, comments, and committed configuration. Use clearly non-functional placeholders.
- Document required, optional, nullable, defaulted, and deprecated fields in the owning schema or documentation.

## JSON syntax and structure

- Produce standards-compliant JSON: double-quoted strings and keys, no comments, no trailing commas, and valid Unicode escapes.
- Use valid JSON literals: `true`, `false`, and `null`. Do not use `True`, `False`, `None`, `undefined`, or `NaN`.
- Do not rely on parser extensions such as comments, unquoted keys, single-quoted strings, or trailing commas.
- Escape control characters and quotes correctly. Do not manually concatenate JSON when a serializer can produce it safely.
- Keep object and array nesting understandable. Extract or redesign an overly deep structure when it materially harms validation and maintenance.
- Preserve array order when it represents priority, sequence, fallback order, or user-visible ordering. Do not sort arrays mechanically.
- Treat object key order as non-semantic unless the consumer, diff workflow, or documentation convention gives it meaning; keep a stable human-friendly order when editing.
- Avoid floating-point values for identifiers, currency, counters requiring exact precision, or other values whose rounding would change meaning.
- Keep JSON documents to one top-level value, normally an object or array, unless the consuming format explicitly permits another value.

```json
{
  "name": "example-service",
  "enabled": true,
  "timeoutSeconds": 30,
  "allowedOrigins": []
}
```

## YAML syntax and structure

- Use spaces for indentation; never use tabs for indentation.
- Keep indentation consistent within a document and match the repository's configured width.
- Quote values when YAML's implicit typing could change their meaning, including values resembling booleans, nulls, dates, numbers, version strings, identifiers with leading zeroes, or special scalars.
- Prefer plain scalars for unambiguous simple values and single quotes when literal text should not interpret escape sequences. Use double quotes when escapes are intentionally required.
- Use block sequences and mappings with clear indentation. Avoid dense flow syntax when it reduces readability or makes diffs harder to review.
- Use `---` and `...` only when multi-document boundaries are required by the consumer or repository convention.
- Keep comments useful and accurate. Do not depend on comments for data consumed by a parser.
- Avoid anchors, aliases, and merge keys unless they reduce real duplication and are supported by every consumer and validation tool.
- Keep aliases local and easy to trace. Do not create deeply chained or cyclic references.
- Be cautious with YAML 1.1 versus YAML 1.2 differences; confirm which schema the consumer uses before choosing values such as `on`, `off`, `yes`, `no`, or date-like strings.
- Do not use custom tags or parser-specific directives without documenting the required consumer.

```yaml
name: example-service
enabled: true
timeoutSeconds: 30
allowedOrigins: []
```

## Schemas and contracts

- Reuse the repository's canonical JSON Schema, OpenAPI, Kubernetes, CI, package, or application schema instead of duplicating it locally.
- Validate required properties, types, enums, formats, ranges, patterns, additional properties, and nested structures at the appropriate boundary.
- Keep examples and fixtures valid against the schema unless they intentionally demonstrate an invalid case and say so explicitly.
- Update schema, fixtures, generated types, documentation, and migration logic together when a contract changes.
- Treat adding a required field, changing a type, renaming a key, changing nullability, or changing enum values as a compatibility change. Check all producers and consumers first.
- Preserve unknown fields according to the contract. Do not silently remove them during a migration or normalization step.
- Distinguish configuration defaults from serialized values. A missing field should not be changed to an explicit default unless consumers treat them identically.

## Configuration and operational safety

- Keep environment-specific values in the repository's supported environment or secret-management mechanism.
- Do not commit private keys, access tokens, passwords, connection strings, personal data, or production credentials.
- Review changes to permissions, exposed ports, hosts, image tags, resource limits, replicas, network policies, and destructive commands as high-risk configuration changes.
- Prefer immutable, pinned versions where reproducibility and the repository's deployment policy require them. Do not update versions incidentally.
- Keep paths, URLs, ports, timeouts, retry limits, and resource quantities explicit and correctly typed for the consumer.
- Check whether a configuration value is interpreted by a shell, template engine, CI runner, container runtime, or application. Escape or quote it for that layer rather than assuming JSON/YAML parsing is the only boundary.
- Treat interpolated values and environment variables as untrusted input. Validate them before using them in commands, queries, URLs, file paths, or HTML.
- Do not assume an unquoted YAML value is safe merely because it parses. It may be reinterpreted by a later templating or shell layer.

## Generated files and transformations

- Determine whether the file is generated before editing. Change the source or generator when one exists.
- Do not hand-edit lockfiles, compiled manifests, snapshots, or generated clients unless the repository explicitly requires it.
- Preserve generator-managed headers and formatting.
- After changing a source contract, regenerate dependent artifacts using the repository's documented command and review the complete diff for unintended changes.
- Do not normalize line endings, key order, or quoting across unrelated generated files without an explicit migration.

## Formatting and readability

- Use the repository's formatter and configuration rather than personal formatting preferences.
- Keep one logical property per line for maintainable diffs unless the project consistently uses compact documents.
- Use a stable key order when the format is reviewed by humans or generated deterministically; follow local conventions such as metadata, identity, configuration, and nested resources.
- Keep line lengths within the repository's configured limit where practical, especially for YAML values and URLs. Use block scalars for long human-readable text when supported.
- Choose YAML block scalar styles deliberately: `|` preserves line breaks, while `>` folds them. Preserve a final newline when the formatter and repository expect one.
- Avoid large inline blobs, encoded payloads, and opaque one-line structures when a structured representation is possible.
- Keep examples minimal but complete enough to communicate required fields and valid types.

## Comments and documentation

- Explain intent, compatibility constraints, operational consequences, or non-obvious parser behavior—not the meaning of an obvious key.
- In YAML, keep comments adjacent to the value they explain and remember that comments are discarded by most parsers.
- JSON does not support comments. Put explanatory material in schema descriptions, documentation, or a separate example file.
- Keep comments synchronized with the actual default, version, environment, and behavior.
- Mark temporary workarounds with an owner or removal condition when the repository has an established convention for doing so.

## Validation checklist

- Parse the complete document with the same parser or toolchain used by the application.
- Run the repository's formatter, linter, schema validator, type generation, and focused tests when available.
- Check for duplicate keys, accidental type changes, invalid references, unresolved environment variables, and unsupported YAML features.
- Review the rendered or effective configuration when templating, inheritance, anchors, overlays, or environment substitution are involved.
- Test required environments or representative configurations without exposing secrets.
- Review changes to permissions, networking, credentials, deployment behavior, resource limits, and destructive operations separately from cosmetic formatting.
- Confirm that generated artifacts were updated through the correct source or generation command.
- Report checks that could not run and distinguish parser/schema validation from live deployment, service, or infrastructure validation.
