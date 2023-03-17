# Versioning Policy

All Go projects in the `dogmatiq` organisation are Go **modules** and use
[Semantic Versioning].

### Libraries

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

<!-- references -->

[semantic versioning]: https://semver.org/
[go versions]: https://endoflife.date/go
