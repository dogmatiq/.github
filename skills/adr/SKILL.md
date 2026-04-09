---
name: adr
description:
  "Write, draft, review, or edit an Architecture Decision Record (ADR)
  for this repository. Use when recording a new architectural decision, revising
  an existing ADR, or checking an ADR against project style rules. Triggers: write
  ADR, draft ADR, new ADR, create ADR, review ADR, architecture decision record."
argument-hint: "the decision to document, e.g., 'use gRPC for inter-service communication'"
---

# Write ADR

Guides drafting, reviewing, and editing Architecture Decision Records for the
repository. All ADRs follow the Nygard format with the project-specific
style rules defined in this skill.

The reference exemplar for this project is
`docs/adr/0002-rendezvous-hashing-for-workload-assignment.md`.

For voice and tone, refer to these ADRs from the dogma repository:

- [0009-immutable-keys.md]
- [0020-identifier-constraints.md]
- [0022-remove-crud-application-support.md]
- [0023-message-order-guarantees.md]
- [0026-event-stream-based-projection-occ.md]

## When to Use

- Drafting a new ADR for a technical or architectural decision
- Reviewing an existing ADR for style, correctness, or completeness
- Editing an ADR to fix issues flagged in a review

## Assets

- [ADR template]

## ADR Status

The status value and relationship annotations are independent. The status
describes the state of this ADR; the annotations describe its relationships to
other ADRs. Both appear in the `## Status` section, with annotations as bullets
below the status value.

### Status values

| Value        | Meaning                                                                                |
| ------------ | -------------------------------------------------------------------------------------- |
| `Proposed`   | Drafted, not yet accepted; transient state during review                               |
| `Accepted`   | In force; includes explicit decisions _not_ to do something                            |
| `Deprecated` | No longer applicable; no replacement exists                                            |
| `Superseded` | No longer in force; replaced by one or more ADRs (named via `- Superseded by` bullets) |

### Admonitions for inactive ADRs

ADRs with status `Proposed`, `Superseded`, or `Deprecated` must include a
GitHub-style admonition immediately after the `## Status` section (before
`## Context`). Copy the admonition blocks exactly as shown below, including
whitespace and line breaks, because GitHub can render them incorrectly
otherwise:

**Proposed:**

```
> [!NOTE]
> This decision has not yet been accepted and is subject to change.
```

**Superseded:**

```
> [!WARNING]
> This decision has been superseded. Refer to the replacement ADR(s) listed
> above.
```

**Deprecated:**

```
> [!WARNING]
> This decision has been deprecated and is no longer applicable.
```

### Relationship annotations

Always written as bullets. Always add the counterpart annotation to each linked
ADR. Multiple annotations of the same kind are allowed.

| This ADR                         | Linked ADR                          | Meaning                                                                                              |
| -------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `- Supersedes [N. Title][ADR-N]` | `- Superseded by [N. Title][ADR-N]` | This ADR replaces the linked one; the linked ADR's status becomes `Superseded`                       |
| `- Amends [N. Title][ADR-N]`     | `- Amended by [N. Title][ADR-N]`    | This ADR corrects or invalidates part of the linked ADR without replacing it; both remain `Accepted` |
| `- References [N. Title][ADR-N]` | `- Referenced by [N. Title][ADR-N]` | This ADR explicitly cites the linked ADR in its prose for context only; no status change             |

Use these rules when deciding between the two non-supersession link styles:

- Use `- Amends ...` / `- Amended by ...` only when the new ADR corrects or
  invalidates something in the original ADR.
- Use `- References ...` / `- Referenced by ...` only when the prose of this
  ADR actually cites the linked ADR — not merely because two ADRs are
  thematically connected.

## Amending an Existing ADR

When an amendment ADR is written:

- Do not include a "Dismissed alternatives" subsection. The original design is
  what is being revised — it is not an alternative. Any acknowledgment of its
  properties, including things it did well, belongs in Context.

When updating the original ADR to reflect the amendment:

- Add the `- Amended by [N. Title][ADR-N]` annotation to its Status section as
  normal.
- In the body of the original ADR, add a GitHub admonition at each location the
  amendment touches — not only in the Status section. Link the admonition to
  the amending ADR. Choose the admonition level based on the magnitude of the
  change at that site:
  - `[!NOTE]` — minor correction or clarification
  - `[!IMPORTANT]` — a substantive part of the design has changed
  - `[!WARNING]` — the original content should not be relied upon without
    reading the amendment

## Drafting a New ADR

Before starting, settle the scope: if two decisions are direct consequences of
each other, record them together in one ADR. Splitting forces cross-references
to reconstruct what is really one choice. Conversely, if a concern is genuinely
separable — it sits independently of the other decisions — record it in its own
ADR and call out the boundary explicitly in each.

### 1. Determine the file number

List the files in `docs/adr/`. The next ADR gets the next sequential number,
zero-padded to four digits. File name pattern: `NNNN-title-with-dashes.md`.

Numbers are assigned at draft time and are provisional. If two concurrent drafts
claim the same number, the one that merges second must renumber. Abandoned
drafts are deleted; do not leave placeholder files or gaps.

### 2. Set the status

New ADRs start as `Proposed`. Do not change the status during drafting. See
[ADR Status] for allowed values and relationship annotations.

### 3. Write the title

The title is a short noun phrase that identifies the decision. It does not need
to fully articulate the decision as a statement — naming the thing being adopted
or implemented is enough. It is not a question or a problem statement.

- Good: "Rendezvous hashing" (names the thing adopted)
- Good: "Rendezvous hashing for workload assignment" (also fine when extra context helps)
- Bad: "How should we assign workloads?"

**Negative decisions:** If the team explicitly decides _not_ to do something,
write and accept a normal ADR for it. The title is still a decision noun phrase:
"No Redis use in cache-layer", "Avoid gRPC for inter-service communication".
Context explains the reasoning; Consequences describe what not having X means
going forward. Do not use a special status for this — it is a first-class
`Accepted` decision.

### 4. Draft the three sections

Copy [ADR template] and fill it in.

Follow the [style guide] for line wrapping, character, and reference link
conventions.

**Context** — one to three paragraphs:

- State the problem or constraint that motivates the decision.
- Introduce every term that will appear in the Decision section as prose, woven
  into the narrative. If "candidate" appears in Decision, it must be defined
  here first. Do not introduce terms as a glossary list or bullet definitions.
- Do not propose or evaluate solutions in this section. This includes verdict
  or evaluative phrases such as "does not justify the costs" — those belong in
  Decision.
- Any project-specific terms used as though they are settled vocabulary must be
  either explained in place, traced to an existing ADR with a citation link, or
  replaced with plain language.

**Decision** — explain what we will do and why:

- Prefer first-person plural ("We will...", "We need...", "We considered...")
  to keep the tone conversational. It is not required in every sentence —
  readability comes first — but it should set the overall voice and steer away
  from overly formal, specification-like, or academic language.
- Ordinary English words used in their ordinary sense — "acceptance",
  "submission", "commitment" — are not terms of art. Express the concept
  directly ("the engine writes the command envelope") rather than naming the
  act ("the acceptance step").
- Include a "Dismissed alternatives" subsection when alternatives were seriously
  considered. Acknowledge genuine advantages before explaining why each was
  rejected. Be specific about the reasons.
- Any pseudocode must use the exact terms from the prose.

**Consequences**:

- Describe the results of the decision. These should be grounded in what was
  actually decided, but need not read like a specification — informal
  observations about what might be beneficial, or suggestions worth carrying
  forward, are welcome. Write conversationally.
- Scope each consequence to what this ADR specifically changed. Avoid claiming
  effects across the broader system when only a narrow part was touched.
- When a consequence is that something is unconstrained, or that a constraint
  is lifted, name that absence of constraint directly. "We are free to..." is
  one natural form of this. Do not use it to express a design preference or
  intent — state those directly.
- Do not use "future ADRs will..." phrasing.
- If (and only if) this ADR introduces new glossary terms, call them out
  explicitly. Example: "This ADR introduces two terms to the glossary:
  rendezvous hashing and self-affinity." If no new terms are introduced, omit
  any mention of the glossary.
- Do not cite rejected or superseded ADRs as the basis for a decision.

### 5. Add diagrams (if needed)

Include a [Mermaid] diagram — especially a sequence diagram — when a
procedure, protocol, or multi-party interaction described in the prose would
be clearer with a visual representation. A diagram should reinforce existing
prose, not replace it: if the prose could be removed without loss after adding
the diagram, the prose was not doing its job.

When adding a diagram:

- Place it immediately after the prose it illustrates, not at the end of a
  section.
- Keep every actor, message, state, and step in the diagram consistent with
  the surrounding prose. Mismatches are silent contradictions.
- Do not add diagrams speculatively. Add one only when the prose is already
  written and the diagram genuinely reduces cognitive load.

### 6. Add references

- Link external concepts to a well-regarded source on first use. Prefer
  Wikipedia for general concepts; use a more authoritative source (RFC, spec,
  official docs) when one exists.
- Link code identifiers to pkg.go.dev only when they are defined in other
  repositories.
- Link RFCs to rfc-editor.org.
- Use markdown reference-style links. Collect them at the bottom of the file
  inside a `<!-- references -->` comment block. Keep the list alphabetized.
- Label inter-ADR reference links as `[ADR-N]` with no zero-padding
  (e.g. `[ADR-2]`, not `[ADR-0002]`). Use the two-part `[display text][ADR-N]`
  form when the display text should be the full title, as in relationship
  annotations.

### 7. Update the index

Run `make docs/adr/README.md` to regenerate the ADR index.

### 8. Run the pre-flight checklist

Work through every item in the [Style Checklist].

## Reviewing an Existing ADR

Read the ADR, work through the [Style Checklist], and report all issues found.
For each issue: quote the offending text, name the rule it breaks, and suggest
a corrected version.

## Accepting an ADR

Before changing an ADR's status from `Proposed` to `Accepted`, scan the
glossary for any definitions the decision affects:

- **Stale definitions** — entries that described the old design and no longer
  accurately reflect what the ADR has established.
- **Missing entries** — terms introduced by the ADR that do not yet have
  glossary entries. These should already be called out in the Consequences
  section.

Update the glossary in the same commit that changes the status. Use the
[glossary skill] to make any changes.

## Style Checklist

### Structure

- [ ] Status value is one of the four allowed values; no other text on
      the same line
- [ ] Relationship annotations are bullets below the status value,
      using only the six allowed verbs
- [ ] Every relationship annotation has its counterpart added to the linked ADR
- [ ] For amendment ADRs: body of the original ADR has admonitions at each
      amended location, with the level reflecting the magnitude of the change
      (see [Amending an Existing ADR])
- [ ] `Proposed`, `Superseded`, and `Deprecated` ADRs include the
      exact required admonition after `## Status`, including whitespace
      and line breaks
- [ ] Title is a decision noun phrase, not a question or problem statement
- [ ] Exactly three top-level sections: Context, Decision, Consequences
- [ ] No extra top-level sections (no "Options", "Alternatives", "Background")
- [ ] File named `NNNN-title-with-dashes.md`
- [ ] One decision per ADR; split if scope has crept
- [ ] Date is today's date (new ADRs only)
- [ ] Index regenerated by running `make docs/adr/README.md`

### Voice and characters

- [ ] Follows the [style guide] (line wrapping, characters, reference links)
- [ ] First-person plural preferred throughout; not mandatory in every sentence
- [ ] Conversational tone, not academic or specification-like
- [ ] Context contains no evaluative or verdict language

### Terminology

- [ ] Every term introduced before first use
- [ ] Consistent terminology throughout; no synonym alternation
- [ ] No unexplained jargon; written for average programmers
- [ ] Project-specific terms used as settled vocabulary are explained in place,
      cited to an ADR, or replaced with plain language
- [ ] Terms from established disciplines (DDD especially) used with their
      proper meanings, not casually
- [ ] Dogma ecosystem terms (command, aggregate, process, etc.) used without
      redefinition
- [ ] Ordinary English words not treated as defined terms of art

### Claims and evidence

- [ ] Performance claims quantified ("in the order of nanoseconds") or hedged
- [ ] No specific figures cited without a citable source
- [ ] Tradeoffs represented honestly; dismissed alternatives not strawmanned

### References

- [ ] External concepts linked to a well-regarded source on first use
      (Wikipedia, RFC, spec, or official docs)
- [ ] Code identifiers from other repositories linked to pkg.go.dev
- [ ] RFCs linked to rfc-editor.org
- [ ] `References`/`Referenced by` only when the prose actually cites the
      linked ADR

### Scope boundaries

- [ ] No undecided concepts or unfinished implementation details referenced
- [ ] Consequences grounded in the decision; informal observations welcome,
      not aspirational claims
- [ ] Each consequence scoped to what this ADR specifically changed
- [ ] No-constraint signals (e.g. "we are free to...") only where a constraint
      is genuinely absent or lifted, not to express design intent
- [ ] No superseded or deprecated ADR is cited as the basis for a decision
- [ ] No "future ADRs will..." phrasing

### Dismissed alternatives (if present)

- [ ] Subsection appears under Decision, not as a top-level section
- [ ] Amendment ADRs do not include this subsection
- [ ] Genuine advantages acknowledged before reasons for rejection
- [ ] Reasons are specific, not vague ("adds complexity without benefit here
      because...")

### Diagrams (if present)

- [ ] Each diagram immediately follows the prose it illustrates
- [ ] Every actor, message, and step matches the surrounding prose exactly
- [ ] Prose remains meaningful without the diagram
- [ ] Rendered as a fenced code block with the `mermaid` language tag

### Pseudocode (if present)

- [ ] Uses the same terms as surrounding prose (not synonyms or abbreviations)
- [ ] Minimal and readable

### Glossary

- [ ] New glossary terms called out in Consequences; if none, glossary not
      mentioned
- [ ] On acceptance: glossary reviewed for stale definitions and missing
      entries (see [Accepting an ADR])

<!-- references -->

[0009-immutable-keys.md]: https://github.com/dogmatiq/dogma/blob/main/docs/adr/0009-immutable-keys.md
[0020-identifier-constraints.md]: https://github.com/dogmatiq/dogma/blob/main/docs/adr/0020-identifier-constraints.md
[0022-remove-crud-application-support.md]: https://github.com/dogmatiq/dogma/blob/main/docs/adr/0022-remove-crud-application-support.md
[0023-message-order-guarantees.md]: https://github.com/dogmatiq/dogma/blob/main/docs/adr/0023-message-order-guarantees.md
[0026-event-stream-based-projection-occ.md]: https://github.com/dogmatiq/dogma/blob/main/docs/adr/0026-event-stream-based-projection-occ.md
[Accepting an ADR]: #accepting-an-adr
[ADR Status]: #adr-status
[ADR template]: ./assets/adr-template.md
[glossary skill]: ../glossary/SKILL.md
[Mermaid]: https://mermaid.js.org
[Style Checklist]: #style-checklist
[style guide]: ../prose/SKILL.md
