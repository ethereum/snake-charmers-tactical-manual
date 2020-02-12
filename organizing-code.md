# Organizing Library Code

We use the following conventions for organizing our libraries.


All of these examples assume that `./` is the *root* of the git repository.

We use `./<library>` to denote the root directory of the python module for a given library.


## Documentation

Documentation should be under `./docs`


## Tests

Tests should be under `./tests`


### `asyncio` & `trio`

See: https://github.com/pytest-dev/pytest-asyncio/issues/124

Currently `pytest-asyncio` doesn't play nicely with `trio`.  If you need to
have a test suite that uses both of these it is best to separate them at the
top level using separate directorie `./tests-trio` and `./tests-asyncio`.

## Non-public utility functions

For a simple singular python module containing utility functions.

- `./<library>/_utils.py`

For a more complex set of shared utility functions.

- `./<library>/_utils/`
  - `./<library>/_utils/__init__.py`
  - `./<library>/_utils/filesystem.py`
  - `./<library>/_utils/numbers.py`

For utility functions that are specific to a sub-section of the library

- `./<library>/some_module/_utils.py`


One *convention* that we use for *utils* modules is that they should **rarely**
import things from elsewhere in the library to prevent circular imports.  In
some cases this cannot be avoided, in which case the import should be inlined
into the function body with a comment indicating *why* the import isn't part of
the top level module imports.


## Constants

For constant values that don't belong in any specific module we use `./<library>/constants.py`

Or for constants that are specific to some sub-section of the library: `./<library>/some_module/constants.py`


## Typing

For type aliases or `typing.NewType` values that don't belong in any specific module we use `./<library>/typing.py`

Or for ones that are specific to some sub-section of the library: `./<library>/some_module/typing.py`


## Non-core functionality

- `./<library>/tools/`

This section encapsulates both functionality that isn't part of the *core*
library but may be useful to end users, as well as functionality that is only
used for things like the test suite.

The convention is that you **may not** import things from `library.tools`
anywhere in the rest of the library code.
