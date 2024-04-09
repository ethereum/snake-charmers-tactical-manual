# Testing

Testing is **essential** to the production of software with minimal flaws.  The
default should always be writing tests for the code you produce.

## Continuous Integration

Tests **must** always be automated using a service like Travis-CI or Circle-CI.

Testing also introduces overhead into our workflow.  If a test suite takes a
long time to run, it slows down our iteration cycle.  This means finding a
*pragmatic* balance between thorough testing, and the speed of our test suite,
as well as always iterating on our testing infrastructure.


## Testing Framework

[`pytest`](https://docs.pytest.org/en/latest/) is the default framework we use
for testing.


## Test Suite Structure

### Test Module Naming/Organization

Where possible, test suites should mirror the module structure of their
corresponding package.  For example, the
[`eth-abi`](https://github.com/ethereum/eth-abi) package has a submodule called
[`eth_abi.grammar`](https://github.com/ethereum/eth-abi/blob/main/eth_abi/grammar.py).
Accordingly, it has a testing module called
[`tests.grammar`](https://github.com/ethereum/eth-abi/blob/main/tests/grammar.py)
that contains testing routines for resources provided by the `eth_abi.grammar`
module.

Test suites may also include a `tests.end_to_end` module that defines
functional testing of a package's public API.  This might include common tasks
performed with a package that compose different individual components of that
package.

### Test Case Naming

Test case names should adhere to the following format where possible:

```
def test_<function or class name>_<short description of functionality being tested>:
    ...
```

For example, test cases for a function called `foo` might be named as follows:

```python
def test_foo_does_kung_fu():
    kung_fu = foo()

def test_foo_raises_value_error():
    with pytest.raises(ValueError):
        foo(...)

...
```

> **Note:** At the time of writing this section on test suite structure, many
> ethereum python packages do not follow all of these guidelines.  Therefore,
> this section is meant to describe the desired test suite structure for future
> packages and is also meant to inform refactoring efforts for existing
> packages.


## Using Mocks

The use of mock objects such as those provided by the
[`pytest-mock`](https://pypi.python.org/pypi/pytest-mock) library should be
considered an anti-pattern.

### Mocks are to be avoided

Because:

- Mock based tests tend to test the implementation and not the feature functionality
- It is very easy to have mock-based tests pass, when the actual code fails
- Mocking can be leaky if not done correctly: leaving objects monkey-patched after test cleanup, etc.

### Signal that your code is incorrectly structured

In many cases, if you find yourself needing to mock objects in order to write
your tests, this is a signal that your code may need to be refactored to make
functionality more modular and easier to test in isolation.

### How to refactor an implementation to avoid mocking

Consider the following steps when looking at your implementation if it seems the only easy way to test some function's logic is via mocking:

1. Prefer small, pure functions. They are easier to test if they just do one thing and only require the minimum amount of stuff required to do their job. Stateful inputs are harder to test as you have to recreate the context before you can see the effect of the routine on your parameters.
2. If (1) is not enough to expose just the functionality you want to test, you can introduce a function beyond what you may normally think is helpful to isolate the functionality you want to test.

An example:

```python
def foo(bar):
    baz = load_baz_from(bar)
    for i in range(ROUND_COUNT):
        some_value = compute_quux_given(i)
        quux = compute_quux_for(baz, some_value)
    return quux
```

We have a function `foo` that computes a value based on an input `bar` by executing an auxiliary routine for some number of rounds `ROUND_COUNT`. To test the auxillary routine (here the body of the `for` loop), we can extract it from `foo`.

```python
def _compute_quux(baz, i):
    some_value = compute_quux_given(i)
    return compute_quux_for(baz, some_value)

def foo(bar):
    baz = load_baz_from(bar)
    for i in range(ROUND_COUNT):
        quux = _compute_quux(baz, i)
    return quux
```

We can now test `_compute_quux` independently of `foo` even if it didn't seem useful to pull out this inner function when writing the first implementation. Note we prefix the inner function with a `_` primarily to indicate that this function is "private" to the module it is defined in.

3. If (2) is not enough to get at the functionality you want to test, you can also refactor so that the explicit behavior you want to check is a parameter to the function. A default argument can be supplied so that callers of the function have no additional burden but the option to customize the function is still available in a testing context.

```python
def foo(bar, quux_provider=_compute_quux):
    baz = load_baz_from(bar)
    for i in range(ROUND_COUNT):
        quux = quux_provider(baz, i)
    return quux

# at the call site:
the_thing = foo(bar)

# during testing:
constant_quux_provider = lambda _*: 2
the_thing = foo(bar, quux_provider=constant_quux_provider)
assert the_thing == the_expected_thing
```

### When all else fails

Mocks **can** be ok when all other options are exhausted, or it is the pragmatic solution, or some other reason to ignore the bad things.

### But maybe don't mock things

A recurring pattern in our projects is a Backend Pattern, where you can swap in providers of functionality. Instead of mocking things that the code wasn't designed to do, try implementing the Backend Pattern, and swap in a dummy backend to stub out responses as fixtures. This provides most of the benefits of mocking, without sidestepping big swaths of code. You have more confidence that the majority of the feature was tested with the dummy backend. Then when you need to test a "live" backend, you can add use a few, targeted tests.

## Tips and Tricks

### Segfault during testing

If pytest crashes with `Segmentation fault (core dumped)` it can be tricky to understand what happened, because you get no traceback printed. Two common causes are: infinite recursion that overflows the stack, and a compiled dependency bug. The first being more common.

#### Identifying an infinite recursion

One trick is to add this to your `tests/conftest.py`:
```py
import sys

def trace(frame, event, arg):                                                                   
    print("%s, %s:%d" % (event, frame.f_code.co_filename, frame.f_lineno))
    return trace                                                                                

sys.settrace(trace)
```

This will print out every line as it happens. You can then capture output to a file with `pytest ... --capture=no >trace.log` so that you don't assault your terminal screen. The final lines of the log should give you a hint where your infinite recursion is happening.

[source](https://stackoverflow.com/a/2663863/8412986)

#### Identifying a compiled dependency bug

On linux you can use gdb to backtrace the crash:
```sh
gdb python
(gdb) -m pytest tests
## wait for segfault ##
(gdb) backtrace
## stack trace of the c code
```

[source](https://stackoverflow.com/a/2664232/8412986)
