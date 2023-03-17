# Versioning Policy

All Go projects in the `dogmatiq` organisation are Go **modules** and [Semantic
Versioning].

Some of those modules are **libraries** intended to be used as dependencies by
other projects.

At any given time, any **stable version** (as per the definition in [Semantic
Versioning]) should build under the officially supported [Go versions] at that
time. This means the latest version and the one prior.

This guarantee does not apply to Go modules that are not libraries, nor those
that are not yet considered stable.

<!-- references -->

[semantic versioning]: https://semver.org/
[go versions]: https://endoflife.date/go
