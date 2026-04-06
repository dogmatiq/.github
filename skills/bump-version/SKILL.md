---
name: bump-version
description: "Bump the version in a Dogmatiq CHANGELOG.md. Renames [Unreleased] to the new versioned release heading with today's date, inserts a fresh empty [Unreleased] section above it, and adds the version reference link in sorted order at the bottom. Use when: bump version, cut a release, tag a release, promote unreleased changelog entries. Triggers: bump version, new release, release v, cut release."
argument-hint: "optional target version, e.g. 'v1.6.0' -- inferred from semver if omitted"
---

# Bump Version in CHANGELOG.md

Promotes the `[Unreleased]` section to a versioned release entry in a
Keep-a-Changelog-style `CHANGELOG.md`.

## When to Use

- Preparing a release and need to stamp the unreleased section with a version
  and date
- Tagging a new version

## Procedure

### 1. Determine the last released version

Run `git describe --tags --abbrev=0` to find the most recent tag.

### 2. Infer the new version (if not supplied)

Read the `[Unreleased]` section and apply semver rules:

| Highest change type in [Unreleased]     | Bump  |
|-----------------------------------------|-------|
| Breaking changes (`[BC]`)               | major |
| New features (Added section)            | minor |
| Bug fixes / changes only (no new API)   | patch |

Confirm the inferred version with the user before proceeding if it is at all
ambiguous.

### 3. Make all edits in a single `multi_replace_string_in_file` call

Three changes are required:

**Change 1 -- Rename the heading.**
Replace `## [Unreleased]` with the new versioned heading using today's date:

```markdown
## [1.6.0] - 2026-04-07
```

**Change 2 -- Insert a fresh `[Unreleased]` section above the new heading.**
Leave it with no sub-sections; those are added as changes accumulate:

```markdown
## [Unreleased]

## [1.6.0] - 2026-04-07
```

**Change 3 -- Add the version reference link at the bottom of the file.**
Insert it immediately after the previous version's link, keeping the list
sorted. Do NOT insert it near the `[unreleased]:` link:

```markdown
[1.5.1]: https://github.com/dogmatiq/ferrite/releases/tag/v1.5.1
[1.6.0]: https://github.com/dogmatiq/ferrite/releases/tag/v1.6.0
```

### 4. Verify -- no duplicate links

After editing, grep for the new version string and confirm it appears exactly
twice: once as a heading and once as a reference link definition.

If it appears more than twice, a duplicate reference link was introduced
(usually near `[unreleased]:`). Remove the extra occurrence.

## Key Constraints

- The `[unreleased]:` link at the bottom does NOT change -- it always points to
  the repo root (e.g. `https://github.com/dogmatiq/ferrite`), not to a tag.
- Reference links must remain sorted numerically within the version group.
- Use today's date in `YYYY-MM-DD` format.
- Do not add empty sub-section headings (`### Added`, `### Changed`, etc.) to
  the new `[Unreleased]` block.
