<!------------------------------------------------------------------------------

A GOOD PULL REQUEST:

- Is small.
- Contains a single change that makes sense in isolation.
- Makes the code better.


A GOOD PULL REQUEST TITLE:

- Completes the sentence "If applied, this PR will ...".
- Is less than 50 characters.

Example: Allow for compaction of projections.


BEFORE YOU SUBMIT A PULL REQUEST:

- Read the contribution guidelines at https://github.com/dogmatiq/.github/CONTRIBUTING.md.
- Know what you are submitting.
- Review the entire diff line-by-line. If this is too hard, your PR is too big.

------------------------------------------------------------------------------->

#### What change does this introduce?

<!--

Briefly describe the change you've made in terms of the features or behavior it
introduces or modifies.

-- EXAMPLE:

This PR adds support for "projection compaction" which is an operation on
projections that can be invoked by the engine to clean up any "unused" or
"oversized" projection data.

-->

#### What issues does this relate to?

<!--

Link to any GitHub issues that are relevant to these changes. Use the "fixes"
keyword if you believe an issue to be completely addressed by this PR.

-- EXAMPLE:

Fixes #123
Partially addresses #456

-->

#### Why make this change?

<!--

Briefly describe the rationale behind making this change.

-- EXAMPLE

By making this a first-class feature we encourage the developer to think about
the lifetime of their projection data, which might otherwise go unaddressed.

-->

#### Is there anything you are unsure about?

<!--

This is a place to ask any specific questions about the changes you've made that
you'd like to see addressed by the project's maintainers before they begin
reviewing.

Consider submitting a draft PR if you require feedback about incomplete changes.

-- EXAMPLE

Should `Compact()` implementations be required to impose their own timeout?

-->
