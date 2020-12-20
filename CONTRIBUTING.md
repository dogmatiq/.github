# Contributing

The **Dogmatiq** organization maintains open source software; contributions from
the community are encouraged and very much appreciated. Please take a moment to
read these guidelines before submitting changes.

Contributions to our projects are released under the MIT license, unless
stated otherwise by the project's LICENSE file.

## Building and testing

With few exceptions, all of our repositories include a Makefile designed for use
with GNU make. Running `make` with no arguments always runs the tests.

## Submitting a pull request

Before submitting your pull request, please open a GitHub issue on the relevant
project to discuss the change you'd like to make. If there is an existing issue
please discuss the changes there to clarify the approach that should be taken.

Some further actions you can take that will increase the likelihood of your pull
request being accepted include:

- Mimicking the code style and quality of the project.
- Including unit tests. Use the existing tools used throughout the project.
- Submitting small, focussed PRs. If you have multiple unrelated changes please
  submit them as separate PRs. Avoid correcting unrelated typos and code style
  violations within more substantial PRs. Review the diff line-by-line. If this
  is too hard, your PR is too big.
- Writing [good commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).
- Adding any user-facing API or behavioral changes to CHANGELOG.md under an
  `[Unreleased]` heading at the top of the file.
- Using a PR title that completes the sentence "If applied, this PR will ...".
- Completing each section of the pull-request template.

### Pull request template

This pull request template contains several prompts for information. Each prompt
is described below along with an example answer.

**Prompt:** *What change does this introduce?*

Briefly describe the change you've made in terms of the features or behavior it
introduces or modifies.

Example:
> This PR adds support for "projection compaction" which is an operation on
> projections that can be invoked by the engine to clean up any "unused" or
> "oversized" projection data.

**Prompt:** *Why make this change?*

Briefly describe the rationale behind making this change.

Example:
> By making this a first-class feature we encourage the developer to think about
> the lifetime of their projection data, which might otherwise go unaddressed.

**Prompt:** *Is there anything you are unsure about?*

This is a place to ask any specific questions about the changes you've made that
you'd like to see addressed by the project's maintainers before they begin
reviewing.

Consider submitting a draft PR if you require feedback about incomplete changes.

Example:
> Should `Compact()` implementations be required to impose their own timeout?

**Prompt:** *What issues does this relate to?*

Link to any GitHub issues that are relevant to these changes. Use the "fixes"
keyword if you believe an issue to be completely addressed by this PR.

Example:
> - Fixes #123
> - Partially addresses #456

## Resources

- [Dogmatiq Community Health Documents](https://github.com/dogmatiq/.github)
- [How to Contribute to Open Source](https://opensource.guide/how-to-contribute/)
- [Using GitHub Pull Requests](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/about-pull-requests)
