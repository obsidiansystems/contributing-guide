**********************************
Obsidian Systems Coding Guidelines
**********************************

This document outlines guidelines for how to write and maintain code for open source projects maintained by `Obsidian Systems <https://obsidian.systems>`__.

.. contents:: Table of Contents

Principles
----------

- Do not assume developers are using particular editors or editor plugins.

  - Don't apply formatting rules that require O(n) work for the developer to fix up after an O(1) change.

Nix
---

Bans
~~~~

- Impure expressions:

  - Usage of ``builtins.fetchGit`` or similar without a ``rev`` and ``sha256`` specified is prohibited.
  - Usage of ``nixpkgs.fetchgit`` must not use ``leaveDotGit = true;`` or ``deepClone = true``.
  - Nix paths to locations that are not in the same repository or a submodule of the same repository.
  - Absolute paths in the form of Nix paths or strings are not allowed at all unless they refer to to files on a NixOS image used for deployment.
  - Exceptions:

    - Usage of ``created = "now"`` in ``nixpkgs.dockerTools.buildLayeredImage`` is allowed.
    - macOS compatibility features that require impurity.

Haskell
-------

.. _bans-1:

Bans
~~~~

- GHC extensions:

  - No ``RecordWildCards``

    - Bind-sites that don't mention the name of their binders mean that whoever is reading the code needs to already know the names of the binders; this makes the scoping much harder to figure out.

  - No ``NamedFieldPuns``

    - ``NamedFieldPuns`` shadow other names with differently-typed bindings. This makes it much harder to type check an expression in your head.

- Never use existing partial functions, such as ``Prelud.head``; instead, in the case of missing functionality, write an incomplete case or a pattern match that may fail; also, prefer to evaluate these potentially-failing patterns strictly. Using these approaches makes partial-function-related errors much easier to track down.

  - Even better: Instead of missing cases in a pattern match, add an explicit use of ``error``, throw an ``IO`` exception, or use a function from this library so that the error is thrown with its source location: https://hackage.haskell.org/package/loch-th-0.2.1/docs/Debug-Trace-LocationTH.html
  - Here are some specific functions that should never be used:

    - ``Data.Map.(!)``
    - ``Data.Maybe.fromJust``
    - ``Data.Text.Encoding.decodeUtf8``
       - Use ``decodeUtf8With lenientDecode`` instead.
    - ``Prelude.(!!)``
    - ``Prelude.head``
    - ``Prelude.init``
    - ``Prelude.last``
    - ``Prelude.tail``

  - Everything on `this list <https://wiki.haskell.org/List_of_partial_functions>`__, except the the ones marked "Only partial for infinite lists" are OK to use.

- Don't use ``undefined``; use ``error`` with a message explaining exactly why the case is impossible.

  - If you can use template haskell, consider using the ``placeholders`` or ``loch-th`` package.

Style
~~~~~

- Rule #1: Make your code fit in. Consistency (within a given function, file, or project) is more important than any particular style.
- Whitespace

  - Indentation

    - Indentation should use two-space tabs.
    - Prefer to indent things by a fixed amount rather than aligning with the previous line.

      - When a ``do`` block spans multiple lines there should be a linefeed between ``do`` and the first statement of the ``do`` block.

        - Wrong:

          .. code:: haskell

             func = do action1
                       action2

        - Right:

          .. code:: haskell

             func = do
               action1
               action2

  - Line feeds.

    - Use Unix line feeds exclusively.
    - Files should end with exactly one linefeed after the last non-whitespace line.
    - Double blank lines (i.e. ``"\n\n\n"``) should be avoided.

      - People tend to do it differently, and it doesn't really convey any information. If someone wants to break out larger sections, a big header in a comment generally should be added instead.

  - Alignment

    - Don't vertically align with whitespace. This requires the developer to potentially reformat every line when changing one line.

  - Be careful to ensure that there are not extra spaces between things.

- Commas

  - Pairs should have a single space after the comma, e.g. ``(a, b)`` instead of ``(a,b)`` or ``(a ,b)``.
  - Commas should always have whitespace (space or linefeed) after them; they should only have whitespace before them when they are the first non-whitespace character on the line.

- Parentheses

  - Extra parentheses are encouraged when using uncommon operators whose precedence may not be obvious to everyone.
  - Extra parentheses should not be used around function application syntax. This applies to both term and type level expressions.

- ``where``

  - ``where`` clauses on functions should always have the ``where`` keyword starting on a new line.

    - ``where`` should be indented 2 spaces.
    - Its contents should start on the following line and be indented 2 additional spaces compared with the ``where``.

  - There should be a space before ``where`` in module export list.

- Type signatures

  - Top-level definitions should have type signatures.
  - There should always be whitespace after the ``.`` in a ``forall`` clause, but never a space before, if it's all on one line.

    - When breaking ``forall`` over lines, treat ``.`` as a ``->`` and align accordingly, with an extra space following.

  - Class contexts with a single constraint should never be parenthesized.
  - When building a long list, record, or tuple, the opening brace, the commas, and the closing brace should all be on separate lines, all in the same column as each other.

- Operator preferences

  - Always use ``(<>)`` to append ``String``\ s, never ``(++)``. Using ``(++)`` for ``[a]`` where ``a`` is not known to be ``Char`` is fine.
  - ``(.)`` and ``($)`` should be used instead of parentheses, where applicable.
  - Prefer ``$`` over ``.`` when they are equivalent.
  - Operators should generally be separated from their arguments by whitespace.
  - In an operator section, there should not be a space between the parenthesis and the operator.

- Function definitions

  - Don't put a newline before the ``=`` when defining a function.
  - When defining a function, prefer using ``case`` (including lambda ``case``) over top-level clauses; e.g.:

    - Worse:

      .. code:: haskell

         showMaybeCtor (Just _) = "Just"
         showMaybeCtor Nothing = "Nothing"

    - Better:

      .. code:: haskell

         showMaybeCtor m = case m of
           Just _ -> "Just"
           Nothing -> "Nothing"

    - Best:

      .. code:: haskell

         showMaybeCtor = \case
           Just _ -> "Just"
           Nothing -> "Nothing"

Imports
^^^^^^^

- The following modules should always be imported ``qualified`` as shown or with an explicit import list:

  - ``Data.ByteString as BS``
  - ``Data.ByteString.Lazy as LBS``
  - ``Data.Text as T``

    - ``Data.Text (Text)``

  - ``Data.Map`` or ``Data.Map.Monoidal as Map``

    - ``Data.Map (Map)`` or ``Data.Map.Monoidal (MonoidalMap)``

  - ``Data.Set as Set``

    - ``Data.Set (Set)``

Naming
^^^^^^

- The field of a ``newtype MyNewtype``, if it is named, should be named ``unMyNewtype``.
- Fields of a record named ``MyRecord`` should be named like ``_myRecord_fieldName``.
- Constructors of a sum type named ``MySumType`` should be named like ``MySumType_ConstructorName``.
- Don't prefix variables with ``_`` to suppress unused variable binding warnings.

  - Instead, you should actually not bind it; add a comment if you feel it's necessary to explain why you aren't binding it
  - Exception: if the variable is used under some CPP flags but not used under others, using ``_`` is reasonable.

- Avoid using primed variable names like ``e'``.

Compiler
~~~~~~~~

- ``-Wall`` should be enabled, and no warnings should exist.

  - If you are editing a file that already has warnings, it is not incumbent on you to fix all the warnings; however, you should not add new warnings, and you should fix warnings if possible in code you are modifying.
  - If there are unreasonable or unavoidable warnings, disable them specifically in a pragma, and add a comment above the pragma explaining why they're unreasonable/unavoidable in this specific case. There should never be a pragma disabling a warning without a comment.

Aspirational Rules
~~~~~~~~~~~~~~~~~~

These are rules that we will not enforce right now, but we would like to be able to enforce at some point in the future.

- Derive everything that GHC can derive for you, ``Eq``, ``Ord``, ``Show``, ``Functor``, ``Foldable``, ``Traversable``, ``Generic``, ``Data``, ``Typeable``, ``NFData``. (``Read`` if you're feeling spicy)

Draft of language extensions which should be on by default (in cabal file):

- ``AllowAmbiguousTypes``
- ``DeriveDataTypeable``
- ``DeriveFoldable``
- ``DeriveFunctor``
- ``DeriveGeneric``
- ``DeriveTraversable``
- ``EmptyDataDeriving``
- ``FlexibleContexts``
- ``FlexibleInstances``
- ``FunctionalDependencies``
- ``GADTs``
- ``GeneralizedNewtypeDeriving``
- ``LambdaCase``
- ``MultiParamTypeClasses``
- ``OverloadedStrings``
- ``RankNTypes``

  - This may interfere with GHC's ability to determine which typeclass contexts are redundant/simplifiable

- ``ScopedTypeVariables``
- ``StandaloneDeriving``
- ``TypeApplications``
- ``TypeFamilies``

SQL
---

- Never use ``SELECT *`` (with an actual asterisk) or anything similar in checked-in code.
- Never use `natural joins <https://en.wikipedia.org/wiki/Join_%28SQL%29#Natural_join>`__.
- All tables must have a primary key.
- Always use foreign keys when possible.
- PostgreSQL

  - Don't create new datatypes in the database (domains, enums, etc.)

    - It's reasonable in theory, but Postgres's support (for migrations, etc.) is not very complete.

JavaScript
----------

- Never use ``==`` or ``!=``. Use ``===`` and ``!==`` instead.

General Engineering
-------------------

- Every project must be able to be built from source, in full, with a single invocation of ``nix-build``.
- Build products may not be checked into source control.
- Do not blindly follow `Postel's law <https://en.wikipedia.org/wiki/Robustness_principle>`__.

  - When we control both sides of an interface, be strict about what you accept (as well as what you emit)
  - When we do not control the user of the interface, consider whether inaccessibility or miscommunication is more dangerous.

    - If a transaction being rejected is not so bad, then be strict.
    - If a transaction going through incorrectly is not so bad, then be lenient.

  - Do not add lenient interpretations that are non-obvious or potentially ambiguous.
  - If possible, add warnings whenever leniency is actually used, so that nonconforming clients can be detected and corrected.

- Do not try to avoid "boolean blindness" by writing versions of ``Bool`` with different constructor names.

  - *Every* variable depends on its context to understand its meaning; you cannot solve this by simply changing constructor names.
  - Consider instead *why* you feel there's a possibility for confusion in a particular case. Perhaps you should use a more constructive type, such as ``Maybe SomeWitness`` rather than ``Bool``.

Web
~~~

- When using `Obelisk <https://github.com/obsidiansystems/obelisk>`__, use Obelisk's built-in static file serving rather than using `CDN <https://en.wikipedia.org/wiki/Content_delivery_network>`__\ s.

  - If the asset is necessary for the app to function correctly, then the external CDN is an additional point of failure (i.e. if the Obelisk server is down OR the external CDN is down, the app will fail.).
  - External CDNs often track users and/or sites, which we should avoid by default
  - If a CDN is needed for geographic locality or other performance reasons, then it is probably needed for *all* static assets. Rather than leaning on ad-hoc CDNs for this, we should have a unified CDN for the app.
