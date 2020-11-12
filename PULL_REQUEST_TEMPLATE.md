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

- Read the contribution guidelines.
- Know what you are submitting.
- Review the entire diff line-by-line. If this is too hard, your PR is too big.

------------------------------------------------------------------------------->

#### What change does this introduce?

<!-- Example:

This PR adds support for "projection compaction", which is an operation on
projections that can be invoked by the engine to clean up any "unused" or
"oversized" projection data.

-->

#### What issues does this relate to?

<!-- Example:

Fixes #123
Partially addresses #456

-->

#### Why make this change?

<!-- Example:

By making this a first-class feature we encourage the developer to think about
the lifetime of their projection data, which might otherwise go unaddressed.

-->

#### What approach has been taken?

<!-- Example:

A `Compact()` method has been added to the `ProjectionMessageHandler` interface.

-->

#### Is there anything you are unsure about?

<!-- Example:

Should `Compact()` impose its own timeout?

-->
