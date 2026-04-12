---
name: prose
description: "Review or apply prose and markdown style conventions for Dogmatiq documentation. Use when writing, reviewing, or checking any documentation file for style compliance. Triggers: style review, check style, prose style, markdown style, check docs, documentation style, style guide, spelling, US English."
argument-hint: "the file or section to review, e.g., 'docs/glossary.md' or 'the Context section of ADR 0042'"
---

# Prose Style

Common conventions for all markdown documentation in Dogmatiq projects.
Domain-specific rules live in their respective skill files; this skill
covers the shared baseline.

## When to Use

- Writing new documentation and needing a style reference
- Reviewing an existing document for style compliance
- Checking a specific section flagged in a review

## Rules

### File naming

Use lowercase names for all documentation files (e.g. `glossary.md`,
`style.md`). Do not capitalize file names unless an external convention
requires it (e.g. `LICENSE`, `Makefile`).

### Line wrapping

Reflow paragraph text so that each line fits as many words as possible
within 80 characters.

### Punctuation

- Non-ASCII characters in code blocks and formulas are acceptable.
- Do not use curly quotes or curly apostrophes in prose. Use straight ASCII
  quotes and apostrophes.
- Use em dash — not hyphen(s) — as a parenthetical separator.
- Use an em dash for definitional appositions in both lists and paragraphs.
  Do not use colons, semicolons, or parentheses for this purpose.
- Consider restructuring sentences or paragraphs that contain many em dashes.
- Use semicolons sparingly:
  - for contrast (A; B does not).
  - for qualification (A; however B).
  - for logical consequence (A; therefore B).
- Prefer a period to a semicolon if the clauses read naturally as two separate
  sentences.
- If you see double hyphens in prose, replace them with an em dash when
  appropriate. Otherwise, suggest the correct punctuation.

### Spelling

Use US English spelling throughout. When in doubt, prefer the spelling used
by golang.org, the Go standard library documentation, or the AWS/GCP
documentation as canonical references.

Common differences to watch for:

| US (correct) | Non-US (incorrect) |
| ------------ | ------------------ |
| color        | colour             |
| center       | centre             |
| behavior     | behaviour          |
| serialize    | serialise          |
| initialize   | initialise         |
| analyze      | analyse            |
| canceled     | cancelled          |
| labeled      | labelled           |

### Reference links

- Use markdown reference-style links throughout.
- Collect link definitions at the bottom of the file, inside HTML comment
  delimiters that label each group (e.g. `<!-- anchors -->`,
  `<!-- references -->`).
- Keep each group sorted alphabetically by label.
- When a term appears in plural form in prose, add a separate plural
  reference link (e.g. `[instructions]: #instruction`) so the source reads
  naturally.
- Every reference link definition must be used at least once in the
  document. Remove unused definitions.
- Every reference link used in prose must have a matching definition. Do
  not leave broken references.

## Review Checklist

Work through every item when reviewing a document for style compliance.
For each issue found: quote the offending text, name the rule it breaks,
and suggest a corrected version.

### File naming

- [ ] File name is lowercase unless an external convention requires
      otherwise

### Line wrapping

- [ ] All paragraph lines wrap at or before 80 characters
- [ ] No lines are unnecessarily short (mid-sentence breaks)

### Characters

- [ ] No curly quotes or curly apostrophes in prose
- [ ] Em dashes used as parenthetical separators, not hyphens
- [ ] Em dashes not overused; restructured where prose is heavy with them

### Spelling

- [ ] All prose uses US English spelling
- [ ] No British/Australian spellings (e.g. colour, behaviour, serialise,
      cancelled, labelled)

### Reference links

- [ ] All links use reference style, not inline style
- [ ] Link definitions collected at the bottom of the file in labelled
      comment blocks
- [ ] Each comment block is sorted alphabetically by label
- [ ] Plural forms of linked terms have their own reference link
- [ ] Every definition is used at least once
- [ ] Every reference used in prose has a matching definition
