# Naming Conventions

Here are some go-to conventions for the names of things.


## Variables


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

for user in all_users:
    ...
```


### Booleans

When naming boolean instance attributes, it is often a good idea to use the `is_` prefix.

```python
class Plugin
  def __init__(self, enable_discovery: bool) -> None:
    self.is_discovery_enabled = enable_discovery
```

If the attribute were to be named `enable_discovery` it might be confused for a method and thus a call to `if self.enable_discovery` would always evaluate *truthy*.


It is also best to avoid *negative* boolean properties since they end up producing double negatives in code.


```python
# avoid this
is_discovery_disabled

if not is_discovery_disabled:
    ...
```

It is difficult to parse the `if` statement above since it contains a double
negative.  If you regularly need to check the negative case it is recommended
to have both a positive and negative flag.

```python
is_discover_enabled = ...
is_discover_disabled = not is_discover_enabled

if is_discover_enabled:
    ...
elif is_discover_disabled:
    ...
```



## Function and Method Names


### Prefixes

The following guideline **should** be used when naming functions or methods.
This provides logical consistency across methods to give future readers of the
code a cue as to what this method likely does (or more importantly, does not
do).

- `get_`:
  - **should** be used for simple retrievals of data.  Generally assumed to not have side effects.
- `fetch_`:
  - **should** be used for retrievals that may have some sort of side effect such as lazy creation, making an HTTP request, or other IO.
- `compute_`:
  - **should** be used for cases where something is being computed or derived from some other data set.  Can be used to signal that this call might be computationally expensive.
- `construct_` or `build_`:
  - Similar to `compute_` but focused more on creation of some other first class object that needs to be computed or derived from other data.
- `from_thing`:
  - Typically used for `classmethod` implementations which act as convenience wrappers around instantiating an object from some other data type when we want to avoid coupling the objects `__init__` with that other data type.

- `validate_`:
  - A method that either returns `None` or raise an exception if something is wrong.


## General comments on naming

Strive to choose names that clearly communicate intent; you want to essentially use the "smallest" concept you can while still transmitting the relevant information to future readers (including yourself).

Of particular importance is naming things based on what they provide, not what they do or details of how they do it. First an example, then further rationale:

```python
# bad
## implies a package name of `enum` that doesn't tell
## the consumer of a codebase much about what is inside.
from enum.signature_enum import SignatureEnum

from datastructures.header_helpers import HeaderCounter

signature_domain = SignatureEnum.some_application_domain()
header_counter = HeaderCounter()
# use signature_domain and header_counter...



# better
from signatures.domains import SignatureDomain
from headers.counter import HeaderCounter

signature_domain = SignatureDomain.for_some_application()
header_counter = HeaderCounter()
# use signature_domain and header_counter...
```

This guideline tends to lend higher semantic value to a given name so that it plugs into the surrounding conceptual framework of the codebase. In the second example, we stick to the domain of this application with an enumeration for "signature domains" and some utility to help count "headers". The relevant concepts are those at the level of the application.  This contrasts with the first example that withholds this context and instead communicates lower-level implementation details (e.g. the fact that we have an ``enum`` implementation of the set of domains or that `HeaderCounter` belongs in package that bundles together what we may consider `data structures`.)


## CLI flags

When writing CLI tools we use the following conventions:


Flags should use `-` separators between words.


```bash
# yes
$ greet --full-name

# no
$ greet --fullname
```

Flags should have a double `--` prefix for the *full* non-abbreviated flag

```bash
# yes
$ greet --name piper

# no
$ greet -name piper
```

Flags should have a single `-` prefix for the short abbreviated flag

```bash
# yes
$ greet --name piper
$ greet -n piper

# no
$ greet --n piper
```
