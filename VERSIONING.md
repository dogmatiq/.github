# Versioning Policy

All Go projects in the `dogmatiq` organisation are Go **modules** and use
[Semantic Versioning].

Some of the modules are **libraries** intended to be imported into other
projects. Any library release that is **stable** as per [Semantic Versioning] is
intended to build under the officially supported [Go versions] at that time.
That is, latest version and the one prior.

Dopping support for an unsupported Go version is not considered a breaking
change, and will not result in a new major version of Dogmatiq libraries.

This policy does not apply to Go modules that are not libraries, nor those that
are not yet considered stable. Such modules are free to target any Go version.

<!-- references -->

[semantic versioning]: https://semver.org/
[go versions]: https://endoflife.date/go
