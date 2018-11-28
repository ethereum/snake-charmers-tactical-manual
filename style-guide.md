# Style Guide

This document defines a style guide for python code.  It includes both
objective rules as well as some subjective preferences.


# RFC2119

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.


## What code does this apply to

This applies to both the library code and the test modules.

> The code within the test modules may be treated with slightly looser
> standards than the main library code.


## Why would you want to use this

### Opinionated consistency

When multiple people are working on the same body of code, it is important that
they write code that conforms to a similar style.  It often doesn't matter as
much *which* style, but rather that they conform to *one* style.

A good term for this is **opinionated consistency**.  It's more important that
the codebase be consistently styled and thus, there needs to be a definition of
what that style is.

## Philosophy

TODO


# Linting

## PEP8

All code **must** conform to [PEP8](https://www.python.org/dev/peps/pep-0008/)
with the following exceptions.

* Line length of 100 (instead of the default 80).


## Flake8

All code **must** pass linting using the `flake8` command line tool with all ignore
rules disabled.

```bash
flake8 --ignore="" path-to-lint/
```


### Exceptions

Code that do not pass ``flake8`` linting rules but which is either required for
some reason or would be *worse* by conforming to the linter can be excluded
using a `noqa` comment which excludes the specific rule.


```python
# in __init__.py
from something import (  # noqa: F401
    Something,
)
```


The `noqa` comment should not be used in the catch-all format:

```
# don't do this
a = a+b  # noqa
```

# Subjective Style Preferences

The following style preferences cover areas where PEP8 does not establish a
single way in which code should be formatted.


## Whitespace

No empty lines at the top of modules.


## Import Statements

### Overall Ordering

All imports should be ordered as follows:

1. Standard Library
    * Ordered alphabetically
2. Installed Libraries
    * Ordered by approximate popularity
        * The `requests` library is more popular than `web3.py` library
3. Local Library
    * Ordered hierarchically
        * The `my_module.core` should be above `my_module.utils.encoding`
    * Ordered alphabetically within hierarchy
        * The `my_module.utils.alphabet` should come before `my_module.utils.zebra`

See example below with comment annotations.

```python
# standard library
import collections
import functools
import os
import sys

# installed libraries
import requests

from web3 import (
    Web3,
)

# local
from my_module import (
    something,
)
from my_module.subsystem import (
    others,
)

from my_module.utils.encoding import (
    encode_thing,
)
```

### Individual Ordering

Within a single import statement, the imported items should be sorted alphabetically

```python
from something import (
    aardvark,
    baboon,
    monkey,
    zebra,
)
```

### Formatting

Single imports **may** be done in a single line.

```python
from something import Thing
```

Multiple imports **must** be split across multiple lines, one per line.

```python
from something import (
    ThingOne,
    ThingTwo,
)
```

### Whitespace

Standard library imports which import the top-level module do not need whitespace.

```python
import collections
import functools
```

Standard library imports which import things from the module **should** be separated with a single line of whitespace.

```python
import collections

from datetime import (
    datetime,
)

import functools
```

Installed library imports **must** be separated by a single line of whitespace.

```python
import collections
import functools

import requests  # 3rd party

import pyethereum  # 3rd party

from local_module import (
 ...
)
```

Local library imports **should** be separated by a single line of whitespace based on hierarchy.


```python
# top level module import
from my_module import (
    MainThing,
)

# 1st level module import
from my_module.submodule import (
    SubThing,
)

# 2nd level module import
from my_module.utils.encoding import (
    encode_thing,
)
from my_module.utils.formatting import (
    format_thing,
)

# local import
from .helpers import (
    do_thing,
)
```


### Avoid `from x import y` for most standard library imports

For most standard library usage, you should access the necessary
functions/classes/etc from the module itself.  

```python
# good
import functools

do_specific_thing = functools.partial(do_thing, x=3)

# less-good
from functools import partial

do_specific_thing = partial(do_thing, x=3)
```

This reduces cognitive overhead when reading code since a reader may already
**know** what `functools.partial` does.  In the case where `partial` is
imported on its own, the reader must go find that import to verify that the
`partial` function is in fact `functools.partial`.

Exceptions to this rule:

#### The `abc.abstractmethod` decorator 

This decorator should be imported directly as it is arguably easier to read 
than using it off of the `abc` namespace.

```python
class MyClass:
    @abc.abstractmethod  # harder to read
    def my_method(self):
        ...
        
    @abstractmethod  # easier to read
    def my_other_method(self):
        ...
```


#### Don't use `from datetime import datetime`

You **should not** import the `datetime` class from the `datetime` module.
Both the class and module share the same name.  By importing the `datetime`
class, you introduce ambiguity for a reader as to whether the `datetime`
variable they are looking at is the class or the module.

```python
# good
import datetime

x = datetime.datetime(year=2017, month=10, day=1)

# bad
from datetime import datetime
x = datetime(year=2017, month=10, day=1)
```


### Relative and Absolute imports

Relative imports **should** only be used for importing from the same module
level.  Relative imports are mentally difficult to traverse and are not
portable if the module is moved.

```python
# good
from .helpers import (
    do_helpful_thing,
)

# less-good
from ..helpers import (  # should instead use absolute import path.
    do_helpful_thing,
)
```


## Naming Conventions


### Looping over dictionaries

You **should** use the names `key` and `value`

```python
for key, value in thing.items():
    ...
```


### Looping over iterables


When writing generic loops, you **should** use the name ``value``.

```python
for value in things_to_process:
    ...
```

In cases where the term ``value`` is not available, you **should** use the term
``item``

```python
for key, value in thing.items():
    for item in value:
        ...
```


### Confusing Plurals

For short names, the simple plural form is preferred.

```python
for user in users:
    ...
```


For longer names, avoid using plural names that only differ by one letter.

```python
for lockfile_path in lockfile_paths:
    ...
```

When using longer names, consider adding a prefix or suffix to help
differentiate the variable names.

```python
for lockfile_path in lockfile_paths_to_process:
    ...
```


## Function and Method Names


### Prefixes

The following guideline **should** be used when naming functions or methods.
This provides logical consistency across methods to give future readers of the
code a cue as to what this method likely does (or more importantly, does not
do).

* `get_` **should** be used for simple retrievals of data.
* `compute_` **should** be used for cases where something is being computed.
* `fetch_` **should** be used for retrievals that are external, such as via an HTTP request, or other IO.


## Multi-line statements

We have traditionally avoided use of forward slash style multi-line statements.


```python
result = something_long \
```

### Dangling Commas

Multiline statements should use dangling commas.  The reason for this is that
it reduces the size of the diff when new items are added.

```python
# good
x = [
    item_a,
    item_b,
    item_c,  # <--- dangling comma
]

# bad
x = [
    item_a,
    item_b,
    item_c  # <--- doesn't have a dangling comma
]
```


### Long Strings

You **should** use the following pattern for long strings.

```python
raise ValueError(
    "This exception message is exceptionally long and thus it "
    "requires that the message be broken up across multiple lines "
    "so that the linting gods do not smite us."
)
```


### Formatting log messages

You **should** use the following string format pattern for log messages, since this pattern is not calculated **unless** it is logged.

```python
logger.info("%s went %s wrong", 'Something', 'very')
```


### Complex statements

Statements with lots of operators can be broken up across multiple lines more
easily if the entire statement is wrapped in parenthesis.  This allows for line
breaks that would otherwise not be allowed.

```python
result = (
    (variable_a + variable_b) * something_kind_of_long /
    some_divisor
)
```


### Comprehensions

When a list/dict/etc comprehension exceeds the 100 character line lenght it
**should** be split across multiple lines with the `statement/for/in/if`
each on their own lines.

```python
result = [
    item
    for item
    in value
]

# or with an `if` statement
result = [
    item
    for item
    in value
    if item is not None
]
```

It is also acceptable to combine the `for` and `in` lines for simple comprehensions

```python
result = [
    item
    for item in value
]
```


### One thing per line

Any statement being split across multiple lines **should** include *one thing*
per line.  Multiline statements are inherently difficult to parse.  By keeping
each line down to *one thing* it reduces this difficulty.

```python
result = get_things(
    argument_a,
    argument_b,
    argument_c,
    argument_d,
)
```

### First thing on its own line

Any statement being split across multiple lines **should** move the first *thing*
to the next line, indented one deeper than the previous line.  For example, changes
to function names **should not** change indentation level of all arguments. Reading
the diff is faster and less error prone when only important lines change.

```python
# should
result = get_things(
    argument_a,
    argument_b,
    argument_c,
    argument_d,
)

# should not
result = get_things(argument_a,
                    argument_b,
                    argument_c,
                    argument_d,
                    )
```

### Closing parenthesis/bracket/brace

The closing parenthesis/bracket/brace for a multiline statement **should** be
on it's own line, at the same level of indentation as the beginning of the
statement.  This provides an easy visual cue for where the multi-line statement
begins and ends.


```python
# good
result = get_things(
    ...
)

# less-good
result = get_things(
    ...)
```


### Function Signatures

When a function signature exceeds the 100 character limit, it should be split
across multiple lines with a single argument per line.  The closing parenthesis
for the function arguments should be on the same line as the final argument.

```python
def long_signature(
        thing_a,
        thing_b,
        thing_c=None,
        thing_d=None):
    pass
```

# Typing

Since all new code is required to come with type hints ([PEP 484](https://www.python.org/dev/peps/pep-0484/)), the following aims to provide answers to some common questions around working with types and `mypy`.

## Choosing the right type

When working with types, we often have to choose between multiple different types that all may seem like the right candidate for the code in question.

### Function variance

As a rule of thumb, the **parameters** of a function **should** be typed to accept the *most flexible* type that the function can work with.

```python
# good
def triple_len(seq: Sequence[T]) -> int:
    return len(seq) * 3

res_1 = triple_len(["some", "irrelevant", "data"])
res_2 = triple_len(("some", "irrelevant", "data",))

# bad
def triple_len(seq: List[T]) -> int:
    return len(seq) * 3

res_1 = triple_len(["some", "irrelevant", "data"])
# Doesn't work if `triple_len` is limited to `List[T]`
# Argument 1 to "triple_len" has incompatible type "Tuple[str, str, str]"; expected "List[<nothing>]"

# res_2 = triple_len(("some", "irrelevant", "data"))
```

In contrast, the **return type** of a function **should not** be more abstract than the type already known in the function body.

```python
# good
def create_things() -> Tuple[str, ...]:
    return ("some", "irrelevant", "data",)

things = create_things()
res = triple_len(things)

# bad
def create_things() -> Iterable[str]:
    return ("some", "irrelevant", "data",)

things = create_things()
# Doesn't work if `things` is falsly assumed to *only* qualify `Iterable[str]`
# Type error: Argument 1 to "triple_len" has incompatible type "Iterable[str]"; expected "Sequence[<nothing>]"

# res = triple_len(things)
```
