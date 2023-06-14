# Versioning Policy

All Go projects in the `dogmatiq` organisation are Go **modules** and use
[Semantic Versioning].

## Libraries

Some of the modules are **libraries** designed to be imported into other
modules.

Any library release that is considered **stable** as per [Semantic Versioning]
is intended to build successfully under the officially supported [Go versions]
at the time of the release. That is, the latest version and the one prior.

Increasing the Go version requirement to exclude a Go version that has become
unsupported is not considered a breaking change, and will not result in a new
major version of the library.

This policy does not apply to Go modules that are not libraries, nor those that
are not yet considered stable. Such modules are free to target any Go version.

### Experimental Features

Some libraries may include **experimental features** that are not yet
considered stable, even if the Go module has a stable semantic version.

Experimental features are not subject to the same stability guarantees as the
rest of the library. They may be removed or changed at any time.

Experimental features are identified by the following conventions:

- Experimental **packages** shall be placed within a directory named `exp`,
  mirroring the approach taken by Go itself.

- Experimental **types**, **functions**, **constants**, and other package-level
  elements within an otherwise stable package shall be identified by a leading
  `X` in their name, such as `XParseString()`.

- Documentation shall include a warning to the effect of:
  > Warning: This is an experimental feature. It is not yet considered stable
  > and may be removed or changed at any time.

<!-- references -->

[semantic versioning]: https://semver.org/
[go versions]: https://endoflife.date/go
