***********************************
Obsidian Systems Contributing Guide
***********************************

This document describes the standard contributing guide for all open source projects maintained by `Obsidian Systems <https://obsidian.systems>`_.

All open source contributions are encouraged and appreciated!

.. contents:: Table of Contents

Opening Issues
--------------

Before opening an issue, please check whether your issue has already been reported. Assuming it has not:

- Describe the issue you're encountering or the suggestion you're making
- Include any relevant steps to reproduce or code samples you can. It's always easier for us to debug if we have something that demonstrates the error.
- Let us know what version of the project you were using. If you're using a GitHub checkout, provide the git hash. If a release version is available, provide that that.

Submitting Changes
------------------

Most pull requests should target the ``develop`` branch. ``master`` is the release branch. ``develop`` is periodically merged into ``master`` after a period of testing.

Guidelines for Commit Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Summary Line
^^^^^^^^^^^^

The summary line of your commit message should summarize the changes being made. Commit messages should be written in the imperative mood and should describe what happens when the commit is applied. If your commit modifies one of the in-tree haskell packages (found in ``./lib``), please prefix your commit summary with the name of the package being modified.

One way to think about it is that your commit message should be able to complete the sentence: “When applied, this commit will…”

Note on bumping dependencies
''''''''''''''''''''''''''''

Commits that update a dependency should include some information about why the dependency was updated in the commit message.

Body
^^^^

For breaking changes, new features, refactors, or other major changes, the body of the commit message should describe the motivation behind the change in greater detail and may include references to the issue tracker. The body shouldn't repeat code/comments from the diff.

Guidelines for Pull Requests
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Wherever possible, pull requests should add a single feature or fix a single bug. Pull requests should not bundle several unrelated changes. When including refactors or clean-ups, try to put those changes in their own commits.

Code Quality
~~~~~~~~~~~~

Warnings
^^^^^^^^

Your pull request should add no new warnings to the project. It should also generally not disable any warnings.

Build and Test
^^^^^^^^^^^^^^

Make sure the project builds and that the tests pass! This will generally also be checked by CI before merge, but trying it yourself first means you'll catch problems earlier and your contribution can be merged that much sooner!

Refer to the project's ``README`` or ``CONTRIBUTING`` document to learn how to run the tests.

Documentation
~~~~~~~~~~~~~

In the code
^^^^^^^^^^^

We're always striving to improve documentation. Please include `haddock <https://haskell-haddock.readthedocs.io/en/latest/index.html>`__ documentation for any added code, and update the documentation for any code you modify.

In the ``ChangeLog``
^^^^^^^^^^^^^^^^^^^^

When your change:

- Adds a feature
- Deprecates something
- Includes a breaking change
- Fixes a bug

add a sentence or two to your pull request description that would be appropriate to include in the ``ChangeLog``. The reviewer will add that description in the ``ChangeLog`` after merging your pull request.

In the ``README``
^^^^^^^^^^^^^^^^^

The ``README`` is the first place a lot of people look for information about the repository. Update any parts of the ``README`` that are affected by your pull request.
