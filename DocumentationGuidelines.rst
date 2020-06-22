Documentation Guidelines
========================

This document outlines guidelines for how to generate and maintain documentation for open source projects maintained by `Obsidian Systems <https://obsidian.systems>`__.

Table of Contents
~~~~~~~~~~~~~~~~~

.. contents::

Repository Documentation
~~~~~~~~~~~~~~~~~~~~~~~~

README
^^^^^^

All open source projects must have a ``README`` document at the root of the repository. It must *at least* adhere to the following structure and content:

- Heading 1: name of the project
- Appropriate badges (like Hackage version, language, license, build status, etc.)
- Logo (if applicable)
- Elevator pitch - This should be no more than 3 sentences; the less the better. It must clearly communicate what benefit a user stands to gain by using this project.
- Table of contents (if applicable)
- Overview
  - A more detailed description of the project
  - Target audience - A section describing assumptions about the users
- Other content

FAQ
^^^

When there are more than a few of them, frequently asked questions should exist in their own ``FAQ`` document at the root of the repository and there must be a link to the document in the ``README``.

CONTRIBUTING
^^^^^^^^^^^^

All open source projects must contain a ``CONTRIBUTING`` document at the root of the repository describing how to develop the project (development environment), how to run tests, how to file bugs and feature requests, etc. The CONTRIBUTING document should link to the official `Obsidian Systems Open Source Contributing Guide <https://github.com/obsidiansystems/contributing-guide>`__. The ``README`` should link to the ``CONTRIBUTING`` document.

LICENSE
^^^^^^^

All open source projects must contain a LICENSE document at the root of the repository with the contents of a standard, well-known license. The go-to default is `BSD3 <https://opensource.org/licenses/BSD-3-Clause>`__.

GitHub
^^^^^^

Repository Description
''''''''''''''''''''''

For GitHub repositories, GitHub’s repository description field must match the elevator pitch in the ``README``.

Pull Requests
'''''''''''''

All open source projects should include a `Pull Request Template <https://help.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository>`__ including the following checklist items for reviewers and submitters:

- All new exported APIs are documented, including types, constructors, etc.
- Existing documentation for changed code is still correct and up-to-date.
- ``README``/``FAQ``/``CONTRIBUTING`` changes adhere to Obsidian Systems standards for open source documentation.

Inline Code Documentation
~~~~~~~~~~~~~~~~~~~~~~~~~

Inline documentation is documentation that exists along side of code, e.g. `haddocks <https://haskell-haddock.readthedocs.io/en/latest/index.html>`__.

Exported APIs
^^^^^^^^^^^^^

All exported entities (e.g. functions, types, constructors, classes, methods, record fields) must have documentation. Function arguments should *usually* be documented unless the type of the argument makes it’s use abundantly clear (i.e. a complete newcomer could not possibly misunderstand).

When a codebase does not adhere to this standard already, any *new* code must adhere. When modifying existing code, try to add documentation and make sure existing documentation remains up-to-date.

Rules
'''''

- API documentation should not refer to how/where something is used; instead, it should describe what the thing *is*. The principle here is "Things shouldn’t know their own names."
  - For example: Instead of "Helper function for ``f`` and ``g``, does blah blah" just use "Does blah blah".
- API documentation should not refer to how something is implemented; instead, it should just describe to what the thing *accomplishes*.

Comments
^^^^^^^^

Good comments are part of good documentation. Generally you should try to comment code for *at least* the following:

- Subtle or complex algorithms
- Subtle/non-obvious design choices
- Things that could be improved later (use ``FIXME`` or ``TODO`` markers in comments where appropriate)
- Things that are intentionally *not* exported and why that is the case

Avoid comments that merely reiterate what a line is already saying.

Tutorials/guides
~~~~~~~~~~~~~~~~

Tutorials and guides must:

- State early-on assumptions about the audience.
- Not build on concepts that have not been introduced previously in the document. If explaining the concept is out of scope for the tutorial/guide then at least do one of the following:
  - State it as assumed familiarity at the beginning of the document, ideally with links to learn more.
  - Provide a "bridging" explanation that gives enough context for the reader to at least make sense of the following content, if not with in-depth familiarity. You might use an analogy to another concept to bridge the gap, or use a loose equivalence with something the reader already understands (with some caveats that the equivalence is not precise).

Maintainership
~~~~~~~~~~~~~~

All open source projects maintained by Obsidian Systems must list their author/maintainer as "Obsidian Systems LLC" and use the contact email of "maintainer@obsidian.systems".

Backlinks
~~~~~~~~~

When at all possible, try to preserve non-permalink `backlink <https://en.wikipedia.org/wiki/Backlink>`__\ s to documentation. I.e. if you move documentation to a new location try to put a symlink or placeholder document in the old location so that links to it continue to exist, even for links not pointing to a specific commit (permalink). Ideally, fragments to headings in the document would continue to work as well, but that is hard to maintain so it is not a strict requirement.
