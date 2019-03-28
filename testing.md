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

[`py.test`](https://docs.pytest.org/en/latest/) is the default framework we use
for testing.


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

### When all else fails

Mocks **can** be ok when all other options are exhausted, or it is the pragmatic solution, or some other reason to ignore the bad things.

### But maybe don't mock things

A recurring pattern in our projects is a Backend Pattern, where you can swap in providers of functionality. Instead of mocking things that the code wasn't designed to do, try implementing the Backend Pattern, and swap in a dummy backend to stub out responses as fixtures. This provides most of the benefits of mocking, without sidestepping big swaths of code. You have more confidence that the majority of the feature was tested with the dummy backend. Then when you need to test a "live" backend, you can add use a few, targetted tests.

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
