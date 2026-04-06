---
name: build-and-test
description: "Build, test, lint, or run checks in a Dogmatiq project. All operations go through `make` regardless of language or toolchain. Use when: running tests, building the project, running precommit checks, checking whether CI will pass. Never invoke language-specific tools directly. Triggers: run tests, build, precommit, lint, make test, make build, run checks, how to test, how to build."
argument-hint: "optional target, e.g. 'test' or 'precommit'"
---

# Building and Testing Dogmatiq Projects

All build, test, lint, and precommit tasks in Dogmatiq projects are driven
by `make`. Never invoke language-specific tools directly (e.g. no `go test`,
`npm run`, `cargo test`). Use `make` targets exclusively.

## Key Principle

**Always use `make`.** The Makefile abstracts over language, toolchain, and
project structure. The same targets exist across all Dogmatiq projects
regardless of the language or framework in use.

## How the Makefiles Work

The makefiles are sourced from [github.com/make-files/makefiles] and vendored
into each project under `.makefiles/`. The root `Makefile` in each project
includes the appropriate package makefile(s) from that directory.

The `.makefiles/` directory must not be edited directly. To update it, run
`make makefiles`.

## Standard Targets

These targets are defined by the core makefile and are available in every
project. Language-specific makefiles add recipes to these targets via `::`.

| Target             | Purpose                                                                  |
| ------------------ | ------------------------------------------------------------------------ |
| _(none)_           | The most common development task; always equivalent to `test`            |
| `test`             | Run the full test suite (default goal)                                   |
| `lint`             | Run linters and static analysis                                          |
| `benchmark`        | Run benchmarks (if the project has any)                                  |
| `generate`         | Build any out-of-date generated files (e.g. Go code from `.proto` files) |
| `regenerate`       | Remove and rebuild all generated files                                   |
| `verify-generated` | Regenerate and fail if committed files differ (used in CI)               |
| `precommit`        | All checks to run before committing (see below)                          |
| `clean`            | Remove all generated and gitignored files                                |
| `clean-generated`  | Remove only files listed in `GENERATED_FILES`                            |
| `makefiles`        | Install or update the vendored `.makefiles/` directory                   |

## Go-Specific Targets

These additional targets are available when a project uses the Go makefile
package (`pkg/go/v1`).

| Target               | Purpose                                                      |
| -------------------- | ------------------------------------------------------------ |
| `build`              | Build debug binaries for the current host                    |
| `debug`              | Build debug binaries for all platforms in the build matrix   |
| `release`            | Build release (optimized) binaries for all platforms         |
| `archives`           | Build release zip archives                                   |
| `coverage`           | Produce an HTML coverage report                              |
| `coverage-open`      | Open the HTML coverage report in a browser                   |
| `test-until-failure` | Run tests in a loop until they fail (useful for flaky tests) |

## What `precommit` Does

`precommit` stacks recipes from multiple makefiles. For a typical Go project
it runs: generate, debug, release, test, and `go fmt ./...`. Run it before
pushing a branch to catch all issues CI would catch.

## Procedure

1. **During development** — Run `make` with no arguments. This runs `test`,
   which is the default goal in all projects.

2. **Before committing** — Run `make precommit` to run the full set of checks
   including linting, formatting, and builds.

3. **Discover available targets** — Inspect the `Makefile` in the project root
   and the included files under `.makefiles/` to see all targets. There is no
   `make help` target.

4. **Never bypass `make`** — Do not run `go test ./...`, `npm test`,
   `cargo test`, or any other language-specific command directly. Always go
   through `make` to ensure that environment setup, code generation, and other
   preconditions are handled correctly.

[github.com/make-files/makefiles]: https://github.com/make-files/makefiles
