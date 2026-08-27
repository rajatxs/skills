# Grammar Refiner

Grammar Refiner is a Codex agent skill for polishing English text. It corrects grammar, spelling, punctuation, awkward phrasing, and accidental ambiguity while preserving the author’s meaning, tone, and intended level of formality.

## Behavior

- Returns only the revised text, without explanations or alternatives.
- Preserves meaningful structure such as paragraphs, bullets, numbering, and line breaks.
- Leaves already-correct text unchanged.
- Preserves names, numbers, URLs, placeholders, quoted text, and domain-specific terms unless an evident correction is required.
- Responds with `Only English text is supported.` when the input is not in English.

## Usage

Invoke the `grammer-refiner` skill and provide the English text to refine. The result is a concise, natural, and professional revision that stays faithful to the original.

The skill definition is available in [SKILL.md](./SKILL.md).
