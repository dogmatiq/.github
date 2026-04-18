# Writing Test Stubs

Stubs are concrete implementations of interfaces used in tests. They live in
a non-test package so they can be imported by tests in other packages.

## Mocks vs stubs

Dogmatiq projects use **stubs**, not mocks. Do not use a mocking library such
as `gomock`, `testify/mock`, or `moq`. Write stub structs by hand following
the conventions below.

## Package placement

Place stubs in a dedicated non-test package, separate from the packages they
support. A common pattern is `<module>/enginetest/stubs`. The package must
**not** use the `_test` suffix — it needs to be importable by external test
packages.

```
mymodule/
  enginetest/
    stubs/
      widget.go
      doc.go
```

Include a `doc.go` with a brief package comment:

```go
// Package stubs provides test implementations of the interfaces defined
// by the parent module.
package stubs
```

## Struct structure

Each stub is a struct with one exported func field per interface method.
Assert interface compliance immediately after the struct declaration.

```go
// WidgetStub is a test implementation of [Widget].
type WidgetStub struct {
    ConfigureFunc func(WidgetConfigurer)
    HandleFunc    func(WidgetScope, Request) error
}

var _ Widget = &WidgetStub{}
```

- Name the struct `<Interface>Stub`.
- Name each field `<Method>Func`.
- The `var _ Interface = &Stub{}` line produces a compile error if the stub
  drifts from the interface — always include it.

## Method implementations

Each method checks its func field and delegates when set.

```go
func (s *WidgetStub) Configure(c WidgetConfigurer) {
    if s.ConfigureFunc != nil {
        s.ConfigureFunc(c)
    }
}

func (s *WidgetStub) Handle(sc WidgetScope, req Request) error {
    if s.HandleFunc == nil {
        panic(dogma.UnexpectedMessage)
    }
    return s.HandleFunc(sc, req)
}
```

- For message-handling methods that should not be called without the func
  field set, panic with `dogma.UnexpectedMessage`.
- For infrastructure or configuration methods where no-op is safe, do nothing
  when the field is nil.

## Generic stubs

Use generics when the stub must represent multiple distinct types differing
only by a type parameter. Provide named marker types:

```go
type (
    TypeA string
    TypeB string
    // ...
)

type CommandStub[T any] struct {
    Content         T
    ValidationError string
}
```

The marker types serve as unique Go types for routing and type assertions in
tests.

## Pre-built instances

For commonly used values, provide package-level variables:

```go
// CommandA1 is a command of type [CommandStub[TypeA]] with content "A1".
var CommandA1 = &CommandStub[TypeA]{Content: "A1"}
```

This lets tests reference `stubs.CommandA1` without constructing values
inline.
