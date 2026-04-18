# Property Testing with rapid

Use `pgregory.net/rapid` for property-based tests — cases where the logic
under test should hold for any input from a large domain, rather than a fixed
set of examples.

## Basic structure

Wrap the property in `rapid.Check`:

```go
func TestParseID_RoundTrips(t *testing.T) {
    rapid.Check(t, func(t *rapid.T) {
        id := rapid.StringN(1, 64, -1).Draw(t, "id")
        got, err := Parse(Stringify(id))
        if err != nil {
            t.Fatalf("unexpected error: %v", err)
        }
        if got != id {
            t.Fatalf("round-trip failed: got %q, want %q", got, id)
        }
    })
}
```

- `rapid.Check` runs the function many times with random inputs and shrinks
  failing cases automatically.
- Assertions inside `rapid.Check` use `t.Fatalf` and `t.Fatal` in the same
  way as ordinary tests — `t` here is a `*rapid.T`, which satisfies the same
  interface.

## Stateful / action-sequence tests

Use `t.Repeat` to test a stateful subject against a sequence of random
operations:

```go
rapid.Check(t, func(t *rapid.T) {
    var subject MyCollection

    t.Repeat(map[string]func(*rapid.T){
        "insert": func(t *rapid.T) {
            v := rapid.Int().Draw(t, "value")
            subject.Insert(v)
        },
        "delete": func(t *rapid.T) {
            // ...
        },
    })
})
```

Each key in the map is a human-readable label used in failure output; the
value is the action function.

## Drawing values

Call `.Draw(t, "label")` on any generator to produce a value. The label
appears in failure output to identify which draw produced the bad value:

```go
name := rapid.StringN(1, 64, -1).Draw(t, "name")
count := rapid.IntRange(1, 100).Draw(t, "count")
flag := rapid.Bool().Draw(t, "flag")
```

Common built-in generators: `rapid.String()`, `rapid.StringN(min, max, maxRunes)`,
`rapid.Int()`, `rapid.IntRange(min, max)`, `rapid.Bool()`,
`rapid.SampledFrom(slice)`.

## Custom generators

For domain types, write a generator function using `rapid.Custom`:

```go
func arbWidget() *rapid.Generator[Widget] {
    return rapid.Custom(func(t *rapid.T) Widget {
        return Widget{
            Name:  rapid.StringN(1, 64, -1).Draw(t, "name"),
            Count: rapid.IntRange(0, 100).Draw(t, "count"),
        }
    })
}
```

Place custom generators for a package in a file named `rapid_test.go`. If
generators are shared across multiple packages, extract them into a dedicated
non-test package (e.g. `x/xrapid/`) so they can be imported without a test
build tag.
