---
name: testing
description: "Write, review, or improve Go tests in a Dogmatiq project. Use when: writing new tests, reviewing test quality, fixing flaky tests, choosing between t.Fatal and t.Error, using cmp for diffs, Ginkgo vs standard Go tests, property testing with rapid. Triggers: write tests, add tests, test coverage, test patterns, test conventions, testing best practices, fix flaky test, property test, rapid."
---

# Go Testing Conventions for Dogmatiq Projects

## Framework Rules

- Use the standard `testing` package exclusively.
- **Never introduce Ginkgo, Gomega, testify, or any BDD/assertion library
  in new tests.** Ginkgo was used in older projects and still exists in some
  codebases.
  - Do not convert existing Ginkgo tests to standard Go tests unless the
    developer explicitly asks for it.
  - If a project already uses Ginkgo, ask the developer which style to use
    before writing any new tests.
- Use `github.com/google/go-cmp/cmp` only when a diff of two values is
  genuinely useful. Do not reach for it just to avoid writing a condition.
- **`pgregory.net/rapid` is encouraged for property-based testing.** See
  [references/rapid.md](./references/rapid.md).

### Dot imports

Prefer a dot import (`import . "package"`) for the package under test. It
reduces noise when every call in the file refers to the same package:

```go
import (
    . "github.com/dogmatiq/dogma"
)
```

Fall back to a named import only if the dot import causes a naming collision.
Do not use dot imports for third-party packages other than the package under
test.

## Assertions

### Prefer `t.Fatal` over `t.Error`

- `t.Fatal` (and `t.Fatalf`, `t.FailNow`) stops the test immediately, which
  prevents misleading cascade failures.
- Use `t.Error`/`t.Errorf` only when all of the following are true:
  - The remaining assertions are genuinely independent — no later assertion
    can panic or produce misleading output if an earlier one fails.
  - Seeing all failures simultaneously provides meaningfully better
    diagnostic value than stopping at the first.
  - The test is explicitly structured around reporting multiple results
    (e.g., validating every field of a large output struct).
- When in doubt, use `t.Fatal`.

### Error paths

When handling an unexpected error, call `t.Fatal(err)` directly with no
wrapping or embellishment:

```go
// Correct — brief and unambiguous
conn, err := db.Open(ctx)
if err != nil {
    t.Fatal(err)
}

// Avoid — unnecessary noise when the error speaks for itself
if err != nil {
    t.Fatalf("unexpected error opening DB: %v", err)
}
```

Add context only when the error path is itself the thing being tested and
you need to distinguish which of several failures occurred.

### Using `cmp` for diffs

Use `cmp.Diff` when comparing structs or multi-line strings where a textual
diff aids diagnosis:

```go
if diff := cmp.Diff(want, got); diff != "" {
    t.Fatalf("unexpected result (-want +got):\n%s", diff)
}
```

Do not use `cmp` for simple scalar comparisons; use a plain `if want != got`
with `t.Fatalf` instead.

## Test Structure and Naming

### Package names

All test files must use the `_test` package suffix (e.g. `package foo_test`,
not `package foo`). Never use the non-`_test` package form to gain access to
unexported identifiers.

**Never read unexported fields, call unexported functions, or otherwise
introspect internal state from a test.** If the observable effect of something
cannot be verified through the package's public API, one of two things is true:

- The behavior under test is not actually needed and should be removed, or
- The implementation is incomplete — skip the test with a TODO and come back
  to it once the public surface exists:

```go
t.Skip("TODO: expose observable effect before testing")
```

If the developer explicitly requests white-box access, prefer `export_test.go`
(a file built only during tests that re-exports unexported identifiers to the
`_test` package) over switching test files to `package foo`. Never apply either
technique without being asked.

### File naming

When creating new test files, place them alongside the code they test:
`widget.go` → `widget_test.go`. For large test suites, group by behavior
into separate `_test.go` files (e.g., `widget_create_test.go`,
`widget_update_test.go`).

When modifying existing tests, follow the file naming conventions already
used in that project — do not rename existing test files to match these
conventions. Minimal diffs are preferred; renaming files or reorganizing
test suites adds noise that obscures the actual change.

### Function naming

Follow standard Go conventions:

- `TestXxx` — unit or integration test
- `BenchmarkXxx` — benchmark (run via `make benchmark`)
- `ExampleXxx` — runnable example (appears in godoc)

Name top-level test functions after the symbol or concept under test:
`TestWidget`, `TestParseID`, `TestNoSnapshotBehavior`.

### Test hierarchy with `t.Run`

Use `t.Run` to express structure within a test function. The three recognized
levels map to a BDD-style hierarchy:

**Level 1 — group by method or aspect** using `"func MethodName()"`:

```go
func TestWidget(t *testing.T) {
    t.Run("func Update()", func(t *testing.T) {
        // ...
    })
}
```

If the top-level function covers only a single method, the grouping level can
be omitted and the remaining levels shift up by one.

**Level 2 — describe context or precondition** using a `"when "` prefix:

```go
t.Run("func Update()", func(t *testing.T) {
    t.Run("when the name is empty", func(t *testing.T) {
        // ...
    })
})
```

Omit this level when there is no meaningful precondition to express — go
straight to `"it "` behavior subtests.

**Level 3 — describe expected behavior** using an `"it "` prefix in the
present tense:

```go
t.Run("when the name is empty", func(t *testing.T) {
    t.Run("it returns an error", func(t *testing.T) { ... })
})
```

`"it "` is always the innermost `t.Run` level. Do not use it for method
groupings, context levels, or table case names.

**Table case names** are plain descriptive phrases without any prefix:
`"valid"`, `"empty name"`, `"nil input"`, `"duplicate identity"`.

Do not nest `t.Run` calls beyond what is needed to express the structure
clearly. Prefer flatter tests over deep nesting.

## Table-Driven Tests

Use table-driven tests when the same logic is exercised with multiple inputs.
Use exported field names in the anonymous struct — this produces readable
output when the test framework prints the case:

```go
func TestParseID(t *testing.T) {
    cases := []struct {
        Name    string
        Input   string
        Want    ID
        WantErr bool
    }{
        {
            Name:  "valid ID",
            Input: "abc-123",
            Want:  ID{Value: "abc-123"},
        },
        {
            Name:    "empty input",
            Input:   "",
            WantErr: true,
        },
    }

    for _, tc := range cases {
        t.Run(tc.Name, func(t *testing.T) {
            got, err := ParseID(tc.Input)
            if tc.WantErr {
                if err == nil {
                    t.Fatal("expected an error, got nil")
                }
                return
            }
            if err != nil {
                t.Fatal(err)
            }
            if diff := cmp.Diff(tc.Want, got); diff != "" {
                t.Fatalf("unexpected result (-want +got):\n%s", diff)
            }
        })
    }
}
```

Use `t.Run` for each case to produce scoped subtest names and enable targeted
re-runs (`go test -run TestParseID/empty_input`).

## Test Helpers

Write test helpers sparingly. A helper is only justified when:

- The same setup pattern recurs across multiple tests, **and**
- It can be expressed using domain language that makes the calling test
  clearer to read.

For one-off setup, inline the code directly in the test. An unexplained helper
that is only called once adds indirection without benefit.

### Mark helpers with `t.Helper()`

Always call `t.Helper()` at the top of any function that calls `t.Fatal` or
`t.Error`. This causes failure output to point to the call site, not inside
the helper. Accept `testing.TB` (not `*testing.T`) so helpers can be called
from both tests and benchmarks. Register cleanup with `t.Cleanup` rather than
`defer` — it runs after the test (including subtests) even on failure.

```go
func mustOpenDB(t testing.TB, dsn string) *sql.DB {
    t.Helper()
    db, err := sql.Open("pgx", dsn)
    if err != nil {
        t.Fatal(err)
    }
    t.Cleanup(db.Close)
    return db
}
```

## Avoiding Flaky Tests

- **Prefer `synctest` for time-dependent and concurrent tests.** Go 1.24's
  `testing/synctest` package runs tests in an isolated bubble with a fake
  clock. Within a `synctest.Run` block, `time.Sleep` and timer-based code
  advance deterministically — there is no real wall-clock delay and no
  race between goroutines and the clock. Use it whenever the code under
  test involves timers, tickers, deadlines, or goroutine coordination.
- **Outside a `synctest` bubble, avoid `time.Sleep`.** Poll with a timeout
  context or a retry helper instead.
- **No assumptions about goroutine scheduling order** outside a `synctest`
  bubble. Use channels or synchronization primitives to signal completion.
- **No global state mutation.** Avoid package-level vars that tests modify.
  Do not call `t.Parallel()` unless you have been asked to write parallel
  tests — it often makes test output harder to read and is not required by
  default.
- **No hardcoded ports or file paths.** Use `t.TempDir()` for temporary
  directories; let the OS assign ports.
- **Deterministic input.** If randomness is needed, seed from a fixed value
  or log the seed so failures are reproducible.

## Test Doubles

When a test needs a controllable implementation of an interface, use a
**stub** — never a mock library. Before writing one, check whether the project
already has suitable stubs in a `stubs` package — many Dogmatiq modules expose
one for their own types.

For the conventions on how to write stubs, see
[references/stubs.md](./references/stubs.md).

## Running Tests

Use `make` — never invoke `go test` directly. See the
[build-and-test skill](../build-and-test/SKILL.md) for available targets.
